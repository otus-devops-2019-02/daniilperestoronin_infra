# daniilperestoronin_infra

daniilperestoronin Infra repository

## Homework №6 (Cloud test app)

```bash
testapp_IP = 35.228.182.233
testapp_port = 9292
```

## Homework №5 (VPN)

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
