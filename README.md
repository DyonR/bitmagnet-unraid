# bitmagnet on Unraid guide

This guide will explain how to setup a minimal bitmagnet environment on your Unraid OS.  
The minimal setup consists of 2 containers; PostgreSQL and bitmagnet.  

Minimal setup **excludes** the following containers from the original docker-compose.yml:

```
gluetun  
grafana
grafana-agent
prometheus
loki
pyroscope
postgres-exporter
```

## Docker Containers setup

In order to get bitmagnet up- and running on Unraid, 1 container from the Apps section must be adjusted, and template must be added manually.

### PostgreSQL

1. In your Unraid WebUI go to the tab *Apps*.  
2. Search for `postgresql15`, you shall see an result from `jj9987` titled ***postgresql15***.  
3. Press install button, and make the following changes:  
   a. Make sure that *Basic view* is switched to *Advanced view* in the top right.  
   b. Change the *Name* to something else, I named it `bitmagnet-postgresql`.  
   c. Since PostgreSQL 16 will be used instead of 15, I changed the number in the *Overview* section.  
   d. Change the *Repository* from `postgres:15` to `postgres:16-alpine`. bitmagnet's own docker-compose.yml file uses this PostgreSQL container, hence the change.  
   e. Enter the remaining fields for `POSTGRES_PASSWORD`, `POSTGRES_USER`, `POSTGRES_DB` and `Database Storage Path`. Note that bitmagnet expects the following values by default: `POSTGRES_USER`:`postgres` and `POSTGRES_DB`:`bitmagnet`.  
   f. **APPLY** the settings to start the `postgres` container.  

### bitmagnet

Currently, there is no bitmagnet template for Unraid available. In the future it may be published to the official *Apps*-section in Unraid, but for now it's a bit more manual.

1. In the Unraid WebUI, open *Terminal* and enter the following:  
   `curl #URl_TO_THE_TEMPLATE# -o /boot/config/plugins/dockerMan/templates-user/my-bitmagnet.xml`
2. Go to the *Docker* tab and at the bottom select ***ADD CONTAINER***.  
3. At the *Template* -> select a template -> User Template -> *bitmagnet*. The template will now load in the WebUI.  
4. Configure the variables to your liking, a few things to know:  
   a. *Post Arguments* has 3 'keys' enabled, `--keys=http_server`, `--keys=queue_server` and `--keys=dht_crawler`, this is default. If you don't wish to run the `dht_crawler`, you can remove that key.  
   b. Under the *Show more settings ...* menu (above the APPLY/DONE/SAVE button), there are advanced settings located to configure bitmagnet more to your liking.
   c. Follow [this guide of Synology](https://kb.synology.com/en-au/DSM/tutorial/How_to_apply_for_a_personal_API_key_to_get_video_info) to obtain a TMDB API key.
  
