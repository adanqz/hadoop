#Hadoop Docker
###Versiones de Hadoop Soportadas
Consulta las ramas del repositorio para las versiones de Hadoop soportadas.

#Inicio Rápido
Para desplegar un clúster HDFS de ejemplo, ejecuta:

```
  docker-compose up
```
Ejecuta un trabajo de conteo de palabras de ejemplo:

```
  make wordcount
```

O despliega en swarm:

```
docker stack deploy -c docker-compose-v3.yml hadoop
```

`docker-compose` crea una red de Docker que se puede encontrar ejecutando `docker network list`, e.g. `dockerhadoop_default`

Ejecuta docker network inspect en la red (por ejemplo, dockerhadoop_default) para encontrar la IP donde se publican las interfaces de Hadoop. Accede a estas interfaces con las siguientes URL:

* Namenode: http://<dockerhadoop_IP_address>:9870/dfshealth.html#tab-overview
* Servidor de historial: http://<dockerhadoop_IP_address>:8188/applicationhistory
* Datanode: http://<dockerhadoop_IP_address>:9864/
* Nodemanager: http://<dockerhadoop_IP_address>:8042/node
* Administrador de recursos: http://<dockerhadoop_IP_address>:8088/


## Configurar Variables de Entorno
Los parámetros de configuración se pueden especificar en el archivo hadoop.env o como variables ambientales para servicios específicos (por ejemplo, namenode, datanode, etc.):


```
  CORE_CONF_fs_defaultFS=hdfs://namenode:8020
```

CORE_CONF corresponde a core-site.xml. fs_defaultFS=hdfs://namenode:8020 se transformará en:


```
  <property><name>fs.defaultFS</name><value>hdfs://namenode:8020</value></property>
```
Para definir un guion dentro de un parámetro de configuración, utiliza guiones bajos triples, como YARN_CONF_yarn_log___aggregation___enable=true (yarn-site.xml):

```
  <property><name>yarn.log-aggregation-enable</name><value>true</value></property>
```

Las configuraciones disponibles son:

/etc/hadoop/core-site.xml CORE_CONF
/etc/hadoop/hdfs-site.xml HDFS_CONF
/etc/hadoop/yarn-site.xml YARN_CONF
/etc/hadoop/httpfs-site.xml HTTPFS_CONF
/etc/hadoop/kms-site.xml KMS_CONF
/etc/hadoop/mapred-site.xml MAPRED_CONF
Si necesitas extender algún otro archivo de configuración, consulta el script bash base/entrypoint.sh
