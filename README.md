# About
Docker image to fetch Let's Encrypt SSL certificate using DNS acme challenge via Selectel DNS. Following projects were used:

* https://github.com/lukas2511/dehydrated
* https://github.com/sugdyzhekov/dehydrated-selectel-dns-hook-script

# Prerequisites

* Add domain to [Selectel](https://selectel.ru)
* Get token from https://support.selectel.ru/keys/

# How to use

Dry run:
```
docker run -e CA="https://acme-staging.api.letsencrypt.org/directory" \
    -e SELECTEL_TOKEN='XXXXX' \
    -v $(pwd)/certificates:/workbench sugdyzhekov/dehydrated-selectel-dns \
    -d example.com -d www.example.com
```

Run
```
docker run -e -e SELECTEL_TOKEN='XXXXX' \
    -v $(pwd)/certificates:/workbench sugdyzhekov/dehydrated-selectel-dns \
    -d example.com -d www.example.com
```

Check `certifcates` directory to obtain your certificates. You may repeat command to renew certificate. 
Feel free to add it in your cron task list.

## Systemd 
You may want to schedule update invocation. If your system uses `systemd` then you may use unit files from the project 
repository

* `update-certificates.service`
* `update-certificates.timer`

### Setup
1. Create `/etc/selectel.token` file:
```
SELECTEL_TOKEN=XXXXX
```
2. Copy `update-certificates.service` & `update-certificates.timer` to `/etc/systemd/system` directory
3. Edit `update-certificates.service` and specify your domains into the last row
4. Activate timer:
```
systemctl enable update-certificates.timer
```
