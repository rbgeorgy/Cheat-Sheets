# Kubernetes

## Работа с подами:

Получить поды в namespace:
```powershell
kubectl get pods -n <some-namespace>
```

Удалить зависшие поды
```powershell
kubectl delete pod --field-selector="status.phase==Failed" -n <some-namespace>
#or

kubectl get pods --namespace=<some-namespace> | grep Evicted | awk '{print $1 " --namespace=<some-namespace>"}' | xargs kubectl delete pod
```

Подключиться к поду
```powershell
kubectl exec -it <some-pod> --namespace=<some-namespace> -- /bin/bash
```

Получить последние события kubernetes engine о поде:

```powershell
kubectl get events --all-namespaces --sort-by='.lastTimestamp'  | grep -i $podname

#or

kubectl get events --all-namespaces --sort-by='.lastTimestamp'  | findstr $podname
```

Скопировать локальный файл на под:
```powershell
kubectl cp /tmp/foo <some-namespace>/<some-pod>:/tmp/bar
```

Скопировать файл с пода на локальную машину:
```powershell
kubectl cp <some-namespace>/<some-pod>:/tmp/bar <some-local-path> # в винде копируется только на диск C
```

## Контекст

```powershell
kubectl config get-contexts
```

```powershell
kubectl config use-context <context-name>
```

```powershell
kubectl create namespace <namespace-name>
```

## Скрипты

Запустить под по префиксу имени и неймспейсу

```powershell
$Namespace = "<namespace-name>"
kubectl delete pod --field-selector="status.phase==Failed" -n $Namespace
$TargetPod = "<pod-prefix-name>"
$KubectlGetPodsCommand = "kubectl get pods --namespace=" + $Namespace
$FindNodePattern = "^" + $TargetPod + "[A-Za-z0-9\-]+"

$PodName = Invoke-Expression $KubectlGetPodsCommand | Select-String -Pattern $FindNodePattern | Select-Object -Expand Matches | Select-Object -Expand Value
$KubectlExecContainerCommand = "kubectl exec -it " + $PodName +" /bin/bash --namespace=" + $Namespace

Invoke-Expression $KubectlExecContainerCommand
```