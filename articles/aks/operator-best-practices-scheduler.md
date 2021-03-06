---
title: 'Рекомендации для оператора: основные функции планировщика в службах AKS'
description: Рекомендации для оператора кластера по использованию основных функций планировщика, таких как квоты ресурсов и бюджеты неработоспособности pod в "Службе Azure Kubernetes" (AKS)
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: conceptual
ms.date: 11/26/2018
ms.author: iainfou
ms.openlocfilehash: abdce8b63a035fe55f4bd37acc5012237bd499da
ms.sourcegitcommit: c61c98a7a79d7bb9d301c654d0f01ac6f9bb9ce5
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/27/2018
ms.locfileid: "52430569"
---
# <a name="best-practices-for-basic-scheduler-features-in-azure-kubernetes-service-aks"></a>Рекомендации по основным функциям планировщика в "Службе Azure Kubernetes" (AKS)

При управлении кластерами в Cлужбе Azure Kubernetes (AKS) часто возникает необходимость изолировать команды и рабочие нагрузки. Планировщик Kubernetes предоставляет возможности, которые позволяют управлять распределением вычислительных ресурсов или ограничивать последствия событий технического обслуживания.

В этой статье с рекомендациями главное внимание уделяется основным возможностям планирования Kubernetes для операторов кластера. В этой статье раскрываются следующие темы:

> [!div class="checklist"]
> * Использование квот ресурсов для предоставления фиксированного объема ресурсов для команд или рабочих нагрузок.
> * Ограничение влияния запланированного обслуживания с помощью бюджетов неработоспособности pod.
> * Проверка отсутствующих запросов ресурсов pod и ограничений с помощью средства `kube-advisor`.

## <a name="enforce-resource-quotas"></a>Применение квот ресурсов

**Советы и рекомендации**: планирование и применение квот ресурсов на уровне пространства имен. Если pod не определяют запросы ресурсов и ограничения, отклоните развертывание. Наблюдайте за использованием ресурсов и при необходимости изменяйте квоты.

Ограничения и запросы на получение ресурсов помещаются в спецификацию pod. Эти ограничения используются в планировщике Kubernetes во время развертывания для поиска доступного узла в кластере. Эти ограничения и запросы работают на уровне отдельных pod. Дополнительные сведения о том, как определить эти значения, см. в статье [Определение ограничений и запросов на получение ресурсов pod][resource-limits].

Чтобы обеспечить возможность резервировать и ограничивать ресурсы для команды или проекта разработки, следует использовать *квоты ресурсов*. Эти квоты определяются в пространстве имен и могут использоваться для установки на следующей основе:

* **Вычислительные ресурсы**, например ЦП, память и GPU.
* **Ресурсы хранилища**, включая общее число томов или объем дискового пространства для класса заданного хранилища.
* **Число объектов**, таких как максимальное количество секретов, служб или заданий, которые могут быть созданы.

Kubernetes не перегружает ресурсы. Когда общее количество запросов ресурсов или ограничений проходит присвоенную квоту, последующие развертывания не выполняются.

При определении квоты ресурсов все pod, созданные в пространстве имен, должны содержать ограничения или запросы в спецификациях pod. Если они не содержат таких значений, можно отклонить развертывание. Вместо этого вы можете [настроить запросы по умолчанию и ограничения для пространства имен][configure-default-quotas].

В следующем примере манифест YAML с именем *dev-app-team-quotas.yaml* устанавливает жесткое ограничение (максимальные значения составляют *10* для ЦП, *20Gi* для памяти и *10* для pod):

```yaml
apiVersion: v1
kind: ResourceQuota
metadata:
  name: dev-app-team
spec:
  hard:
    cpu: "10"
    memory: 20Gi
    pods: "10"
```

Эта квота ресурсов может применяться путем указания пространства имен, например *dev-apps*:

```console
kubectl apply -f dev-app-team-quotas.yaml --namespace dev-apps
```

Обратитесь к разработчикам и владельцам приложений, чтобы понять их потребности и применить соответствующие квоты ресурсов.

Дополнительные сведения о доступных объектах ресурсов, областях и приоритетах см. в разделе [Квоты ресурсов в Kubernetes][k8s-resource-quotas].

## <a name="plan-for-availability-using-pod-disruption-budgets"></a>Планирование доступности с помощью бюджетов неработоспособности pod

**Советы и рекомендации**: чтобы поддерживать доступность приложений, определите бюджеты неработоспособности pod (PDB), обеспечив доступность минимального количества pod в кластере.

Существует два события прерывания работы, из-за которых удаляются pod:

* *Непреднамеренные прерывания работы* — это события, которые обычно неподконтрольны оператору кластера или владельцу приложения.
  * К таким непреднамеренным прерываниям работы относятся сбой оборудования на физическом компьютере, критическое состояние ядра или удаление виртуальной машины узла.
* *Преднамеренные прерывания работы* — это события, запрошенные оператором кластера или владельцем приложения.
  * К таким преднамеренным прерываниям работы относятся обновления кластера, применение обновленного шаблона развертывания или случайное удаление pod.

Последствия непреднамеренных прерываний работы можно смягчить, используя несколько реплик pod в развертывании. Запуск нескольких узлов в кластере AKS также помогает устранить последствия непреднамеренных прерываний работы. Для преднамеренных прерываний работы Kubernetes предоставляет *бюджеты неработоспособности pod*, с помощью которых оператор кластера может определить минимальное число доступных или максимальное число недоступных ресурсов. Эти бюджеты неработоспособности pod позволяют планировать реагирование развертываний или наборов реплик в случае преднамеренного прерывания работы.

Если нужно обновить кластер или шаблон развертывания, планировщик Kubernetes обеспечивает планирование дополнительных pod на других узлах до преднамеренного прерывания работы. Планировщик ожидает перезагрузки узла до успешного планирования определенного числа pod на других узлах в кластере.

Давайте рассмотрим в качестве примера набор реплик с пятью pod, на которых выполняется NGINX. Эти pod в наборе реплик имеют присвоенную метку `app: nginx-frontend`. Во время события преднамеренного прерывания, например обновления кластера, необходимо убедиться, что по крайней мере три pod продолжают работать. Следующий манифест YAML для объекта *PodDisruptionBudget* определяет такие требования:

```yaml
apiVersion: policy/v1beta1
kind: PodDisruptionBudget
metadata:
   name: nginx-pdb
spec:
   minAvailable: 3
   selector:
   matchLabels:
      app: nginx-frontend
```

Можно также определить процент, например *60 %*, что позволит автоматически компенсировать набор реплик, увеличив количество pod.

Можно определить максимальное число недоступных экземпляров в наборе реплик. В этом случае также можно определить процент максимального количества недоступных pod. Следующий манифест YAML бюджета неработоспособности pod определяет, что не более двух pod в реплике набора будут недоступны:

```yaml
apiVersion: policy/v1beta1
kind: PodDisruptionBudget
metadata:
   name: nginx-pdb
spec:
   maxUnavailable: 2
   selector:
   matchLabels:
      app: nginx-frontend
```

После определения бюджета неработоспособности pod нужно создать его в кластере AKS, как и в случае с любым другим объектом Kubernetes:

```console
kubectl apply -f nginx-pdb.yaml
```

Обратитесь к разработчикам и владельцам приложений, чтобы понять их потребности и применить соответствующие бюджеты неработоспособности pod.

Дополнительные сведения об использовании бюджетов неработоспособности pod см. в статье [Указание бюджета неработоспособности для приложения][k8s-pdbs].

## <a name="regularly-check-for-cluster-issues-with-kube-advisor"></a>Регулярная проверка кластера на отсутствие проблем с помощью kube-advisor

**Рекомендация.** Регулярно запускайте последнюю версию `kube-advisor` для поиска проблем в кластере. Если вы применяете квоты ресурсов в существующем кластере AKS, сначала запустите `kube-advisor`, чтобы найти контейнеры pod без запросов и ограничений ресурсов.

Средство [kube-advisor][kube-advisor] сканирует кластер Kubernetes и сообщает о выявленных проблемах. Полезная проверка — поиск контейнеров pod без установленных запросов и ограничений ресурсов.

В кластере AKS с большим количеством команд разработчиков и приложений может быть сложно отслеживать контейнеры pod без установленных запросов и ограничений ресурсов. Рекомендуем регулярно запускать `kube-advisor` для кластеров AKS, особенно в том случае, если не назначены квоты ресурсов пространствам имен.

## <a name="next-steps"></a>Дополнительная информация

В этой статье главное внимание уделяется основным возможностям планировщика Kubernetes. Дополнительную информацию об операциях кластера в AKS см. в рекомендациях на такие темы:

* [Мультитенантность и изоляция кластера][aks-best-practices-cluster-isolation]
* [Дополнительные функции планировщика Kubernetes][aks-best-practices-advanced-scheduler]
* [Аутентификация и авторизация][aks-best-practices-identity]

<!-- EXTERNAL LINKS -->
[k8s-resource-quotas]: https://kubernetes.io/docs/concepts/policy/resource-quotas/
[configure-default-quotas]: https://kubernetes.io/docs/tasks/administer-cluster/manage-resources/memory-default-namespace/
[kube-advisor]: https://github.com/Azure/kube-advisor
[k8s-pdbs]: https://kubernetes.io/docs/tasks/run-application/configure-pdb/

<!-- INTERNAL LINKS -->
[resource-limits]: developer-best-practices-resource-management.md#define-pod-resource-requests-and-limits
[aks-best-practices-cluster-isolation]: operator-best-practices-cluster-isolation.md
[aks-best-practices-advanced-scheduler]: operator-best-practices-advanced-scheduler.md
[aks-best-practices-identity]: operator-best-practices-identity.md
