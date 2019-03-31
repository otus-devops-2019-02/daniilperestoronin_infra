## Homework №6 (Cloud test app)

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