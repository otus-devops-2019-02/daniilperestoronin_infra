# daniilperestoronin_infra

daniilperestoronin Infra repository

## Homework №8 (Ansible-2)

- Использованы плейбуки, хендлеры и шаблоны для конфигурации
окружения и деплоя тестового приложения. 
    - Подход один плейбук, один сценарий (play)
    - Аналогично один плейбук, но много сценариев
    - И много плейбуков.
- Изменен провижн образов Packer на Ansible-плейбуки

## Homework №7 (Ansible-1)

Вопрос: выполните ```ansible app -m command -a 'rm -rf ~/reddit' ``` и проверьте еще раз выполнение плейбука. Что изменилось и почему? 

Ответ: Командой выше удаляется репозиторий, при повторном выполнении плейбука репозиторий клонируется,
поэтому возвращается ```changed=1```

## Homework №6 (Terraform-2)

- Созданы модули app, db, vpc
- Проверена работа vpc
- Созданы окружения stage и prod
- Создан storage-bucket.tf, проверено с помошью вэб-консоли что бакеты создались и доступны

## Homework №5 (Terraform-1)

### В процессе сделано:

- Настроен шаблон терраформа для поднятия виртуальных машин
- Определены внешние переменные для шаблона
- Настроена передача ключей в метаданные проекта
- Настроен http-балансировщик

## Homework №4 (Packer)

### В процессе сделано:
 - Создан шаблон для создания image с установленными Ruby, Bundler и Mongodb (ubuntu16.json)
 - На основе шаблона ubuntu16.json запущена виртуальная машина и установлено приложение reddit-app ( http://35.228.233.244:9292/)
 - Параметризирован созданный шаблон ubuntu16.json
 - * Создан шаблон для создания image с установленными Ruby, Bundler, Mongodb и установленным тестовым приложением (immutable.json)
 - * Подготовлен скрипт create-redditvm.sh для создания виртуальной машины при помощи gcloud

### Проверить работоспособность:
 - По ссылке http://35.228.233.244:9292/

## Homework №3 (Cloud test app)

```bash
testapp_IP = 35.228.182.233
testapp_port = 9292
```

### gcloud with startup_script
```bash
gcloud compute instances create reddit-app --boot-disk-size=10GB \
--image-family ubuntu-1604-lts --image-project=ubuntu-os-cloud \
--machine-type=g1-small --tags puma-server --restart-on-failure \
--metadata-from-file startup-script=startup_script.sh \ 
--zone europe-north1-b
```

### gcloud firewall-rules
```bash
gcloud compute firewall-rules create default-puma-server --allow=tcp:9292 --target-tags puma-server
```

## Homework №2 (VPN)

```bash
bastion_IP = 35.228.179.12
someinternalhost_IP = 10.166.0.3
```

1. Подключение к someinternalhost в одну команду: 
    ````bash
    ssh -i ~/.ssh/appuser -At appuser@35.228.179.12 ssh 10.166.0.3
    ````
2. Для подключения из консоли командой ssh someinternalhost и ssh bastion
необходимо внести следующие изменения(создать в случае необходимости) в файл ~/.ssh/config на локальном компьютере:
    ```bash
    Host bastion
        HostName 35.228.179.12
        User appuser
    
    Host someinternalhost 10.166.0.3
        User appuser
        ProxyCommand ssh -i ~/.ssh/appuser -A bastion nc -w 180 %h %p
    ```
