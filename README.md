# 🧵 Threads Lite — DevOps проект

[![Terraform](https://img.shields.io/badge/Terraform-7B42BC?style=for-the-badge&logo=terraform&logoColor=white)](https://www.terraform.io/)
[![Kubernetes](https://img.shields.io/badge/kubernetes-326CE5?style=for-the-badge&logo=kubernetes&logoColor=white)](https://kubernetes.io/)
[![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)](https://www.docker.com/)
[![Yandex Cloud](https://img.shields.io/badge/Yandex_Cloud-5282FF?style=for-the-badge&logo=yandexcloud&logoColor=white)](https://cloud.yandex.com/)
[![GitHub Actions](https://img.shields.io/badge/GitHub_Actions-2088FF?style=for-the-badge&logo=github-actions&logoColor=white)](https://github.com/features/actions)
[![Prometheus](https://img.shields.io/badge/Prometheus-E6522C?style=for-the-badge&logo=prometheus&logoColor=white)](https://prometheus.io/)
[![Grafana](https://img.shields.io/badge/Grafana-F46800?style=for-the-badge&logo=grafana&logoColor=white)](https://grafana.com/)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?style=for-the-badge&logo=postgresql&logoColor=white)](https://www.postgresql.org/)
[![Patroni](https://img.shields.io/badge/Patroni-FF6C37?style=for-the-badge&logo=postgresql&logoColor=white)](https://patroni.readthedocs.io/)
[![RabbitMQ](https://img.shields.io/badge/RabbitMQ-FF6600?style=for-the-badge&logo=rabbitmq&logoColor=white)](https://www.rabbitmq.com/)
[![.NET](https://img.shields.io/badge/.NET-512BD4?style=for-the-badge&logo=dotnet&logoColor=white)](https://dotnet.microsoft.com/)

---

## 📋 О проекте

**DevOps-решение для развертывания микросервисного приложения в облаке**

**Ключевые особенности:**
- Отказоустойчивый кластер PostgreSQL под управлением **Patroni** в Kubernetes
- Асинхронное взаимодействие микросервисов через **RabbitMQ**
- Микросервисы на **ASP.NET Core**
- Полная наблюдаемость (метрики, логи, трассировка)
- Инфраструктура как код на **Terraform**
- Автоматический CI/CD через **GitHub Actions**

## 🛠 Технологический стек

### **1. Инфраструктура как код (IaC)**
| Инструмент | Назначение |
|------------|------------|
| **Terraform** | Создание всей облачной инфраструктуры (сети, кластер K8s, S3, балансировщики) |
| **Yandex Cloud Provider** | Управление ресурсами Yandex Cloud |
| **Terraform Backend (S3)** | Хранение состояния инфраструктуры в объектном хранилище |

### **2. Облачная платформа**
| Сервис | Назначение |
|--------|------------|
| **Managed Kubernetes** | Оркестрация контейнеров |
| **Object Storage (S3)** | Хранение медиафайлов |
| **Virtual Private Cloud (VPC)** | Сетевая изоляция |
| **Load Balancer** | Распределение трафика |

### **3. Контейнеризация и оркестрация**
| Инструмент | Назначение |
|------------|------------|
| **Docker** | Упаковка микросервисов |
| **Kubernetes** | Оркестрация, самовосстановление, масштабирование |
| **Helm / Kustomize** | Управление манифестами |
| **StatefulSet** | Базы данных с сохранением состояния |
| **PersistentVolume (PV/PVC)** | Постоянное хранение данных в облаке |

### **4. Микросервисы (ASP.NET Core)**
| Сервис | Технологии | Назначение |
|--------|------------|------------|
| **Auth Service** | ASP.NET Core, JWT | Регистрация, логин, проверка токенов |
| **Posts Service** | ASP.NET Core | Создание постов, лента |
| **Media Service** | Minio / S3 | Загрузка и хранение медиа (опционально) |
| **Notification Service** | ASP.NET Core + RabbitMQ | Отправка уведомлений (email/push) |
| **Analytics Service** | ASP.NET Core + RabbitMQ | Сбор статистики (лайки, просмотры) |

### **RabbitMQ (Брокер сообщений)**

| Компонент | Описание |
|-----------|----------|
| **Назначение** | Асинхронное взаимодействие между микросервисами |
| **Тип в K8s** | StatefulSet (для сохранения очередей при перезапусках) |
| **Реплики** | 3 пода в кластере (high availability) |
| **Хранение** | PersistentVolume для сохранения очередей |
| **Сценарии использования** | 1. При создании поста → уведомление подписчикам<br>2. При лайке → обновление счетчиков<br>3. Загрузка медиа → обработка в фоне |


---

## 🗄️ Базы данных и хранилища

### **PostgreSQL + Patroni (Отказоустойчивый кластер)**

| Компонент | Описание |
|-----------|----------|
| **Patroni** | Управляет кластером PostgreSQL: выбирает мастера, настраивает репликацию, выполняет failover |
| **etcd** | Распределенное хранилище для координации Patroni (или Kubernetes API) |
| **Архитектура** | 3 пода PostgreSQL в StatefulSet (1 мастер + 2 реплики) для каждого сервиса |
| **Хранение** | Каждый под имеет свой PersistentVolume в Yandex Cloud |
| **Автоматический failover** | При падении мастера Patroni автоматически повышает реплику |
