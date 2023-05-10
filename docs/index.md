---
layout: default
---
<!-- markdownlint-disable -->
<style>
/* The below `img` style sets the default CSS styling for all images hereafter in this markdown
file. */
img
{
    /* Default display value is `inline-block`. Set it to `block` to prevent surrounding text from
    wrapping around the image. Instead, `block` format will force the text to be above or below the
    image, but never to the sides. */
    display:block; 
    float:none; 
    margin-left:auto;
    margin-right:auto;
}
</style>
<!-- markdownlint-enable -->
# Local Grafana Deployment

## Requirements

* Grafana OSS 9.x.x
* Docker 20.10+ (linux)
* docker-compose 1.29+ (linux)

## Instructions

**Note:** This guide assumes that you have a Grafana already [installed][grafana-install-docs]
and running in a supported environment but for demonstration purposes the guide will provide
instructions for the Docker-based deployment as it is more portable and cross-platform.

### Getting Grafana up and running (Linux only)

1. First download the docker-compose file from [here](/files/docker-compose.yaml).

2. On a terminal opened in the same directory where you download the docker-compose.yaml file you
can simply type: `docker compose up -d grafana`
3. Go to the browser and open the page [http://localhost:3000](http://localhost:3000).
On the login page enter the default admin credentials
User: admin, Password: admin
4. For increased security Grafana prompts for setting a new admin password, please do so or skip it.

### Installing the required plugins/datasources

To integrate with the Edge Analytics API Grafana needs a plugin that is able to consume JSON APIs.
There are many plugins available to do so but the plugin
[JSON API Data Source for Grafana][grafana-json-datasource] is officially maintained by Grafana Labs
and it perfectly covers our needs.

To install the plugin from the UI you just have to (other installation methods described
[here][grafana-json-datasource-install]):

1. Navigate to the *Configuration > Plugins* section.
![Configuration > Plugins](/images/plugins.png)
2. Search for the term “JSON API” so you should get the plugin developed by Marcus Olsson.
![JSON API Plugin](/images/json-api-plugin.png)
3. Click on the *Install* button.
![JSON API install](/images/json-api-install.png)
4. Click on the *Create a JSON API* data source button.
![Create datasource](/images/create-datasource.png)
5. Configure the data source with the following values:
   1. Name: Dremel API
   2. URL: [https://api.system73.com/analytics](https://api.system73.com/analytics)
   3. Add a header on *Custom HTTP Headers* with the following:
      1. Header: `Authorization`
      2. Value: `Bearer <edge_analytics_api_key>`
![Datasource setup](/images/datasource-setup.png)
6. Click on the *Save & test* button (ignore the warning message) and click on the Back button.

### Importing Edge Analytics dashboards

You can import the Edge Analytics dashboard that we have made available [here](/files/ea-basic-dashboard.json).

To import it:

1. Click on the sidebar’s *Dashboards > Import*
![Dashboards > Import](/images/dashboard-import.png)
2. You can either upload the dashboard file or simply copy & paste the file context. Since we are
using a container without specific host-mounted volumes it is better to simply copy its content.
![Import via JSON](/images/dashboard-load.png)
3. Click on the *Load button*
4. You can choose the dashboard uid, tittle, folder as you want but make sure to select the
*Dremel API* as the dashboard datasource in the drop-down menu.
![Dremel API datasource](/images/dashboard-datasource.png)

5. Click on the *Import* button
6. After that you just need to select the correct region and the Edge Intelligence Id that you have
been assigned.
![Edge Analytics customer id](/images/analytics-id.png)

[grafana-install-docs]: https://grafana.com/docs/grafana/latest/?pg=oss-graf&plcmt=quick-links#installing-grafana
[grafana-json-datasource]: https://grafana.github.io/grafana-json-datasource/
[grafana-json-datasource-install]: https://grafana.github.io/grafana-json-datasource/installation
