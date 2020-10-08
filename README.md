# KongMap #
Kongmap is a free visualization tool which allows you to view and edit configurations of
your Kong API Gateway Clusters, including Routes, Services, and Plugins/Policies. The tool is 
 being offered for installation via Docker and Kubernetes at this time.  

![GitHub](https://img.shields.io/github/license/yesinteractive/kong-map?style=for-the-badge)

## Features ## 

#### Cluster View ####
Allows an admin to view a dynamic map of their Kong API Gateway clusters and visually see relationships between
Workspaces (for Kong Enterprise), Services, Routes (Endpoints), and Plugins (Policies). Clicking on any entity displays
details of the entity and related links. Plugins can be toggled from view. 


![alt text](https://github.com/yesinteractive/kong-map/blob/main/screenshots/kongmap-home.png?raw=true "kongmap")

#### Endpoint Analyzer ####
View details of an API Endpoint (Route). The analyzer shows the Service attached to the endpoint/route as well as provides
a breakdown of all plugins/policies in order of execution attached to the route/endpoint. For Kong Enterprise users,
all entities can be viewed directly via a link to Kong Manager.

![alt text](https://github.com/yesinteractive/kong-map/blob/main/screenshots/kongmap-endpoint.png?raw=true "kongmap")


#### Declarative Configuration Viewer/Editor ####
KongMap is deployed with a browser based version of Kong's CLI tool, decK. Here you can view, edit, and export Kong declarative configurations for your open source 
and Enterprise clusters via YAML. Declarative
configuration editing can be disabled by KongMap configuration, or managed via RBAC permissions if using Kong Enterprise. 

![alt text](https://github.com/yesinteractive/kong-map/blob/main/screenshots/kongmap-deck.png?raw=true "kongmap")

## Compatibility ## 
KongMap supports both Kong Open Source and Kong Enterprise Clusters greater than version 1.5.

## Docker Installation ##

Docker image is Alpine 3.11 based running PHP 7.3 on Apache. The container exposes both ports 80 an 443 with a self signed certificated. 

#### 1. Export Cluster Configurations to Variable ####

The connections to your Kong clusters are defined via JSON. The below example illustrates adding two Kong clusters to KongMap:

```json
{
  "my enterprise cluster": {
    "kong_admin_api_url": "http://kongapi_url:8001",
    "kong_edit_config": "true",
    "kong_ent": "true",
    "kong_ent_token": "admin",
    "kong_ent_token_name": "kong-admin-token",
    "kong_ent_manager_url": "http://<kongmanager_url:8002>"
  },
  "my kong open source cluster": {
    "kong_admin_api_url": "http://kongapi_url:8001",
    "kong_edit_config": "true",
    "kong_ent": "false",
    "kong_ent_token": "null",
    "kong_ent_token_name": "null",
    "kong_ent_manager_url": "null"
  }
}
  ```

Notice the `kong_ent` configurations. Enable and configure this if the cluster you are configuring is Kong Cluster. If you do not, only the Default workspace
will be visible in your Kong Enterprise Cluster.

Export the config to a variable:

```shell
 export KONG_CLUSTERS='{  "my enterprise cluster": {    "kong_admin_api_url": "http://kongapi_url:8001",    "kong_edit_config": "true",   "kong_ent": "true",    "kong_ent_token": "admin",    "kong_ent_token_name": "kong-admin-token",    "kong_ent_manager_url": "http://kongmanager_url:8002"  }}'
  ```

#### 2. Start Container ####

Run the container with the following command. Set the ports to your preferred exposed ports. The example below exposes KongMap on ports 8100 and 8143. Notice the `KONGMAP_URL` variable. Set this optional variable if you have a need to set all KongMap URL's to a specific domain or URL.

```
$ docker run -d \
  -e "KONGMAP_CLUSTER_JSON=$KONG_CLUSTERS" \
  -e "KONGMAP_URL=http://url_to_kongmap" \
  -p 8100:80 \
  -p 8143:443 \
  yesinteractive/kongmap
```


Full documentation available online here: [https://github.com/yesinteractive/kong-map/](https://github.com/yesinteractive/kong-map/)

#### 3. Authentication ####

If you want to enable authentication to KongMap's UI, it is recommended to run Kongmap behind your Kong Gateway and implement any authentication
policies you feel is appropriate (OIDC, OAUTH2, Basic Auth, etc.) at the gateway.

## Feedback and Issues ##

If you have questions, feedback or want to submit issues, please do so here: [https://github.com/yesinteractive/kong-map/issues](https://github.com/yesinteractive/kong-map/issues).