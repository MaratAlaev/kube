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

------

### Задание 1. Подготовить Helm-чарт для приложения

1. Необходимо упаковать приложение в чарт для деплоя в разные окружения. 
2. Каждый компонент приложения деплоится отдельным deployment’ом или statefulset’ом.
3. В переменных чарта измените образ приложения для изменения версии.

------
### Задание 2. Запустить две версии в разных неймспейсах

1. Подготовив чарт, необходимо его проверить. Запуститe несколько копий приложения.
2. Одну версию в namespace=app1, вторую версию в том же неймспейсе, третью версию в namespace=app2.
3. Продемонстрируйте результат.

   
![image](https://github.com/MaratAlaev/kube/assets/46092593/59bdd3ff-b26e-4afc-abb4-8039e50eb842)
![image](https://github.com/MaratAlaev/kube/assets/46092593/7edec02b-618f-47a5-9bd8-13959ab94878)
![image](https://github.com/MaratAlaev/kube/assets/46092593/2833894d-647f-47fa-98ac-86a3640721c6)
![image](https://github.com/MaratAlaev/kube/assets/46092593/2c62f0ce-c7ce-4cda-947e-bd82d4400b3d)
![image](https://github.com/MaratAlaev/kube/assets/46092593/dd82fa90-14e7-4524-93a7-9ae73a246202)
![image](https://github.com/MaratAlaev/kube/assets/46092593/4579cd84-1e5c-42ac-881c-ebf6687f7baa)





### Правила приёма работы

1. Домашняя работа оформляется в своём Git репозитории в файле README.md. Выполненное домашнее задание пришлите ссылкой на .md-файл в вашем репозитории.
2. Файл README.md должен содержать скриншоты вывода необходимых команд `kubectl`, `helm`, а также скриншоты результатов.
3. Репозиторий должен содержать тексты манифестов или ссылки на них в файле README.md.
