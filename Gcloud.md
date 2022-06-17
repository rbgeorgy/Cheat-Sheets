# Gcloud

### Сертификаты:

```powershell
gcloud beta compute ssl-certificates create <your-cert-name> --domains <your-host>
```

```powershell
gcloud beta compute ssl-certificates describe <your-cert-name> --domains <your-host>
```

```powershell
gcloud compute ssl-certificates list
```

### Войти как сервисный аккаунт

```powershell
gcloud auth activate-service-account --project=<project-name> --key-file=bar.json
```

### Проекты:
```powershell
gcloud config get-value project

gcloud projects list

gcloud config set project
```

### Добавить ssh ключ к виртуалке:
```powershell
gcloud compute os-login ssh-keys add --key-file=key.pub --project=<project-name> --ttl=1000000m
```

### Как изменить политику CORS для бакета?
```powershell
gsutil cors set cors-json-file.json gs://<bucket-name>
```
cors-json-file.json content:
```json
[
    {
      "origin": ["*"],
      "responseHeader": ["Content-Type"],
      "method": ["GET"],
      "maxAgeSeconds": 10
    }
]
```