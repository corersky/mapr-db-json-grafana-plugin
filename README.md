# MapR-DB JSON Grafana Plugin
A plugin to visualize MapR-DB JSON tables data into Grafana

## Installation

### Grafana

You can install Grafana on dev machine or on one of the cluster nodes. If you are installing Grafana on cluster node, use the following command:
```
$ sudo yum install mapr-grafana
```

In case of installing Grafana on dev machine, you can install [Grafana Labs Grafana](https://grafana.com/grafana/download/4.4.2). We have to check if plugin compatible with other versions of Grafana, so for now ensure that version of Grafana is `4.4.2`:
For Ubuntu/Debian:
```
wget https://s3-us-west-2.amazonaws.com/grafana-releases/release/grafana_4.4.2_amd64.deb 
sudo dpkg -i grafana_4.4.2_amd64.deb 
```

For Redhat/Centos:
```
wget https://s3-us-west-2.amazonaws.com/grafana-releases/release/grafana-4.4.2-1.x86_64.rpm 
sudo yum localinstall grafana-4.4.2-1.x86_64.rpm 
```

### MapR-DB JSON Datasource

1. Clone plugin repository:

```
$ git clone https://github.com/mapr-demos/mapr-db-json-grafana-plugin.git
```

2. Build the plugin:

```
$ cd mapr-db-json-grafana-plugin

$ mvn clean package
```

3. Install the plugin:

Copy the content of ` ` direcotory to Grafana dataource plugins direcory and restart the Grafana.

For dev machine installation:
```
$ sudo mkdir /usr/share/grafana/public/app/plugins/datasource/mapr-db-json-grafana-plugin

$ sudo cp -R frontend/dist/ /usr/share/grafana/public/app/plugins/datasource/mapr-db-json-grafana-plugin

$ sudo service grafana-server restart
```

For node installation:
```
$ mkdir /opt/mapr/grafana/grafana-4.4.2/usr/share/grafana/public/app/plugins/datasource/mapr-db-json-grafana-plugin

$ sudo cp -R frontend/dist/ /opt/mapr/grafana/grafana-4.4.2/usr/share/grafana/public/app/plugins/datasource/mapr-db-json-grafana-plugin

$ maprcli node services -action restart -nodes `hostname` -name grafana

```

### Datasource Backend

MapR-DB JSON Grafana Plugin consists of frontend and backend parts, so we have to start backend service. It can be started on one of the nodes of the cluster or on the dev machine, where MapR Client is installed and configured. To start backend service use following command:
```
$ cd mapr-db-json-grafana-plugin/backend

$ mvn spring-boot:run
```

## Adding MapR-DB JSON Datasource

After completing the steps above navigate to `http://grafanahost:3000` in your browser. Assuming that Grafana is installed on `localhost`:

* Ensure that MapR-DB-JSON Plugin present at [http://localhost:3000/plugins?type=datasource](http://localhost:3000/plugins?type=datasource)

* Add MapR-DB-JSON Datasource at [http://localhost:3000/datasources/new](http://localhost:3000/datasources/new) page

At `Http settings` specify URI of datasource backend and choose `direct` access type.
Note: `proxy` access type currently is not supported.

* Click 'Add' button. The following message should appear:

```
Success
Data source is working
```

* Now you are able to choose newly added MapR-DB JSON Datasource while creating [Table](http://docs.grafana.org/features/panels/table_panel/) and [Graph](http://docs.grafana.org/features/panels/graph/#graph-panel) Panels
