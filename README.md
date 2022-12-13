# Задание к занятию «Kubernetes (к8s)»

1. Ответы на вопросы
-  Что такое k8s?

Kubernetes (K8s) — это открытое программное обеспечение для автоматизации развёртывания, масштабирования и управления контейнеризированными приложениями.

- В чём преимущество контейнеризации над виртуализацией?

Контейнеризация дает преимущество над виртуализацией, т.к. имеет низкие накладные расходы из-за отсутствия необходимости для каждого контейнера использовать свою полноценную OS и ядро, а также эмуляцию устройств (что справедливо для виртуализации).

- В чём состоит принцип самоконтроля k8s?

K8s содержит в себе API-server и абстракцию control plane. Узел могут автоматически  регистрироваться в control plane, после чего они проверются на валидность и здоровье (все необходимые сервисы запущены, узел может запускать поды). Также автоматически определяются доступные на узле ресрусы, проводится постоянная проверка доступности посредством механизма Heartbeat. Также кластер может быть поделен на зоны доступности с автоматической балансировкой приложения в зависимости от состояния зоны (процент доступных узлов).

- Как вы думаете, зачем Вам понимать принципы деплоя в k8s?

Этот продукт достаточно популярен на больших проектах. Например можно развертывать отказустойчивый инференс модели, либо в принципе построить все ML пайплайны в K8s с помощью Kubeflow

- Какое из средств управления секретами наиболее распространено в использовании совместно с k8s?

Kubernetes Secrets Store CSI Driver

- Какие типы нод есть в k8s, каковы их базовые функции?

Control plane содержит множество компонентов, основные из которых:

- kube-apiserver - control plane и API сервер. Главный компонент кластера
- etcd - распределенное высоконадежное хранилище для метаданных кластера
- kube-scheduler - планировщик запуска подов на узлах
- разного рода служебные контроллеры

Основные компоненты на рядовых узлах:

- kubelet - средство управления контейнерами
- kube-proxy - средство предоставления абстракции Service, отвечает за сетевое взаимодействие
- Container runtime - движок запуска контейнеров (containerd, CRI-O)

2. Результат запуска деплоймента
```
$ kubectl get deployments
NAME          READY   UP-TO-DATE   AVAILABLE   AGE
netology-ml   2/2     2            2           47s

$ kubectl describe deployment netology-ml
Name:                   netology-ml
Namespace:              default
CreationTimestamp:      Tue, 13 Dec 2022 13:00:04 +0000
Labels:                 <none>
Annotations:            deployment.kubernetes.io/revision: 1
Selector:               app=tomcat
Replicas:               2 desired | 2 updated | 2 total | 2 available | 0 unavailable
StrategyType:           RollingUpdate
MinReadySeconds:        0
RollingUpdateStrategy:  0 max unavailable, 1 max surge
Pod Template:
  Labels:  app=tomcat
  Containers:
   tomcat:
    Image:        tomcat:8.5.69-jdk8-openjdk-slim-buster
    Port:         8080/TCP
    Host Port:    0/TCP
    Environment:  <none>
    Mounts:       <none>
  Volumes:        <none>
Conditions:
  Type           Status  Reason
  ----           ------  ------
  Available      True    MinimumReplicasAvailable
  Progressing    True    NewReplicaSetAvailable
OldReplicaSets:  <none>
NewReplicaSet:   netology-ml-7d795cb54 (2/2 replicas created)
Events:
  Type    Reason             Age   From                   Message
  ----    ------             ----  ----                   -------
  Normal  ScalingReplicaSet  64s   deployment-controller  Scaled up replica set netology-ml-7d795cb54 to 2
```
