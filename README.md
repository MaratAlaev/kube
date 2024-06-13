# Домашнее задание к занятию «Управление доступом»

### Цель задания

В тестовой среде Kubernetes нужно предоставить ограниченный доступ пользователю.

------

### Чеклист готовности к домашнему заданию

1. Установлено k8s-решение, например MicroK8S.
2. Установленный локальный kubectl.
3. Редактор YAML-файлов с подключённым github-репозиторием.

------

### Инструменты / дополнительные материалы, которые пригодятся для выполнения задания

1. [Описание](https://kubernetes.io/docs/reference/access-authn-authz/rbac/) RBAC.
2. [Пользователи и авторизация RBAC в Kubernetes](https://habr.com/ru/company/flant/blog/470503/).
3. [RBAC with Kubernetes in Minikube](https://medium.com/@HoussemDellai/rbac-with-kubernetes-in-minikube-4deed658ea7b).

------

### Задание 1. Создайте конфигурацию для подключения пользователя

1. Создайте и подпишите SSL-сертификат для подключения к кластеру.
2. Настройте конфигурационный файл kubectl для подключения.
3. Создайте роли и все необходимые настройки для пользователя.
4. Предусмотрите права пользователя. Пользователь может просматривать логи подов и их конфигурацию (`kubectl logs pod <pod_id>`, `kubectl describe pod <pod_id>`).
5. Предоставьте манифесты и скриншоты и/или вывод необходимых команд.

------

С доступом к CA

```bash
       openssl genrsa -out marat.key 2048
       openssl req -new -key marat.key -out marat.csr -subj "/CN=marat/O=ops"
       openssl x509 -req -in marat.csr -CA /var/snap/microk8s/current/certs/ca.crt -CAkey /var/snap/microk8s/current/certs/ca.key -CAcreateserial -out marat.crt -days  20
       microk8s kubectl config set-credentials marat --client-certificate marat.crt --client-key marat.key --embed-certs=true

       kubectl config set-context marat_context --cluster=microk8s-cluster --user=marat
       kubectl config  use-context marat_context
```

Без доступа к CA

```bash
       openssl genrsa -out marat2.key 2048
       openssl req -new -key marat2.key -out marat.csr -subj "/CN=marat2/O=ops"
       cat marat2.csr  | base64
```


```yaml

apiVersion: certificates.k8s.io/v1
kind: CertificateSigningRequest
metadata:
  name: my-csr
spec:
  groups:
  - system:authenticated
  request: | 
    LS0tLS1CRUdJTiBDRVJUSUZJQ0FURSBSRVFVRVNULS0tLS0KTUlJQ1pEQ0NBVXdDQVFBd0h6RVBN
    QTBHQTFVRUF3d0diV0Z5WVhReU1Rd3dDZ1lEVlFRS0RBTnZjSE13Z2dFaQpNQTBHQ1NxR1NJYjNE
    UUVCQVFVQUE0SUJEd0F3Z2dFS0FvSUJBUURRTWJqN3BVQ1ozbWd4UXB3b0w2RFlFNmRHCjlvWXQz
    KzBCNHNMUERCUjVESE1HOWl4amJhVjJDSngwTis5NzR4aDhobjFjT1h1VGUwYTllU0MycmI5UkRj
    L1AKY1ozNGk0QjFPOGU1ODllRTBEWXZEZkxmZ3NRbm5OeW94TVQrWnE3cnVRN1dKZlA1YzhXOS9k
    Mnlob2hDUktxVwpiaWR6S0MyZU9KdFBYRnF5WDArYzJnM2gvWC9ENmo3Q0FGQnVqZGkrNlI0MzFm
    QXl6TkRxSnNZcCs5SmdIengzCmh5NEd1Rzd6NmY3TmNoaTRMcEIrQnp3YWhBRVE0UVl4eUp1ekJG
    Uy9aeEVSZVdraHY2MkNrbURHSWZIWDBzVUoKY0ttVUE0T2N2U3JHVmhjNTBUWFFrMlZDemtPZ2Ex
    MkhacXlic0VQb3RpeTFUKzZWT2wrV2c0QjRBVkczQWdNQgpBQUdnQURBTkJna3Foa2lHOXcwQkFR
    c0ZBQU9DQVFFQVBKRi9ycGFML2dvbG0vS3IzaEwxanZZcG5WbUNPdFVyCjFkeUJObkZ2cmcwcVFx
    RHhqNDh1V0YwODJvbnp6T0ZoYkZITGtNMi8zdCtSUk5YZmFCRmVlM21TZWRnNi9MQ3AKMDVsTFNQ
    NUNVT0dmZzQ4NmM3UStxWFZ0VlkzVXY4ajR6MmNNS29VSlZmaXBySnQ0TE5Lck1aSWpjZDFjaXpL
    awpiQW9TMC9TNGF5dnByUkZCVEJOb2REMlIzNXZCUjRucDBJdS9KSHZ2Z2dISXZFLzYxdmtCR0V1
    T0VZakFabElSCkl4WkVZNXFsTXZOZTdBbEQ0UXd6NjBUK2pMbU14YmM4TmFEWlo2aXVOK0N5SjQy
    TlVJZXFwV0tkaHpnSnp3WWIKVmFreUpsMWpoSy9rRm5YRVM1K3dYWGtadHk4T0xnNks5OFZUVE9h
    L1B2ZEluTHFyandCZm93PT0KLS0tLS1FTkQgQ0VSVElGSUNBVEUgUkVRVUVTVC0tLS0tCg==
  usages:
  - client auth
  signerName: kubernetes.io/kube-apiserver-client

```

```bash
kubectl certificate approve my-csr
kubectl get csr my-csr -o jsonpath={.status.certificate} | base64 --decode > marat2.crt


microk8s kubectl config set-credentials marat2 --client-certificate marat2.crt --client-key marat2.key --embed-certs=true

kubectl config set-context marat_context2 --cluster=microk8s-cluster --user=marat2
kubectl config  use-context marat_context2
```

Роли

![image](https://github.com/MaratAlaev/kube/assets/46092593/1fbf72fa-71b9-4d25-b2f6-09f57639e062)

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: default
  name: read-only
rules:
- apiGroups: [""]
  resources: ["pods", "pods/log"]
  verbs: ["get", "list", "watch"]

---

apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: read-only-binding
  namespace: default
subjects:
- kind: User
  name: marat
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: Role
  name: read-only
  apiGroup: rbac.authorization.k8s.io
```

![image](https://github.com/MaratAlaev/kube/assets/46092593/89f2716a-d062-4ca1-86c4-e65b52130fac)


### Правила приёма работы

1. Домашняя работа оформляется в своём Git-репозитории в файле README.md. Выполненное домашнее задание пришлите ссылкой на .md-файл в вашем репозитории.
2. Файл README.md должен содержать скриншоты вывода необходимых команд `kubectl`, скриншоты результатов.
3. Репозиторий должен содержать тексты манифестов или ссылки на них в файле README.md.

------
