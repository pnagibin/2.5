# Домашнее задание к занятию «Helm»

### Цель задания

В тестовой среде Kubernetes необходимо установить и обновить приложения с помощью Helm.

------

### Чеклист готовности к домашнему заданию

1. Установленное k8s-решение, например, MicroK8S.
2. Установленный локальный kubectl.
3. Установленный локальный Helm.
4. Редактор YAML-файлов с подключенным репозиторием GitHub.

------

### Инструменты и дополнительные материалы, которые пригодятся для выполнения задания

1. [Инструкция](https://helm.sh/docs/intro/install/) по установке Helm. [Helm completion](https://helm.sh/docs/helm/helm_completion/).

### Устанавливаю Helm

```
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
chmod 700 get_helm.sh
./get_helm.sh
```

------

### Задание 1. Подготовить Helm-чарт для приложения

1. Необходимо упаковать приложение в чарт для деплоя в разные окружения. 
2. Каждый компонент приложения деплоится отдельным deployment’ом или statefulset’ом.
3. В переменных чарта измените образ приложения для изменения версии.

### Решение:

Устанавливаю nginx через Chart

```
helm upgrade --install --atomic ngninx nginx/ --namespace netology --set image.tag=1.23.0
export POD_NAME=$(kubectl get pods --namespace netology -l "app.kubernetes.io/name=nginx,app.kubernetes.io/instance=ngninx" -o jsonpath="{.items[0].metadata.name}")
echo $POD_NAME
export CONTAINER_PORT=$(kubectl get pod --namespace netology $POD_NAME -o jsonpath="{.spec.containers[0].ports[0].containerPort}")
kubectl --namespace netology port-forward $POD_NAME 8080:$CONTAINER_PORT
```

![alt text](image.png)

![alt text](image-1.png)

![alt text](image-2.png)

Меняю версию приложения и развертываю повторно

```
helm upgrade --install --atomic ngninx nginx/ --namespace netology --set image.tag=1.24.0
```
![alt text](image-3.png)

![alt text](image-4.png)

![alt text](image-5.png)
------
### Задание 2. Запустить две версии в разных неймспейсах

1. Подготовив чарт, необходимо его проверить. Запуститe несколько копий приложения.
2. Одну версию в namespace=app1, вторую версию в том же неймспейсе, третью версию в namespace=app2.
3. Продемонстрируйте результат.

### Правила приёма работы

1. Домашняя работа оформляется в своём Git репозитории в файле README.md. Выполненное домашнее задание пришлите ссылкой на .md-файл в вашем репозитории.
2. Файл README.md должен содержать скриншоты вывода необходимых команд `kubectl`, `helm`, а также скриншоты результатов.
3. Репозиторий должен содержать тексты манифестов или ссылки на них в файле README.md.

### Решение

![alt text](image-6.png)

![alt text](image-7.png)
