# 📡 Installation de ElasticSearch
## Prérequis
Lorsque votre machine virtuelle est prête,ouvrez un terminal et mettez à jour le système avec la commande :
```sudo apt update && sudo apt upgrade -y```

## Installation de ElasticSearch
Dans un premier temps, il faut installer les dépendances. Installez le paquet nécessaire pour les connexions HTTPS avec cette commande:
```sudo apt install apt-transport-https wget curl gnupg -y```

Ensuite, Ajoutez la clé GPG d’Elasticsearch qui permet à votre système de vérifier l’authenticité des paquets téléchargé avec cette commande :
```wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -```

Ajoutez le dépôt Elasticsearch pour la version 7.x à vos sources APT : 
```echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee /etc/apt/sources.list.d/elastic-7.x.list```

Enfin, mettez à jour les paquets et installer ElasticSearch : 
``` sudo apt update ```
``` sudo apt install elasticsearch -y ```

## 📡 Configuration de ElasticSearch
Une fois l'installation faite, vous devez configurer ElasticSearch. Assurez vous d'avoir le fichier **elasticsearch.yml** présent en tapant la commande ```ls -l /etc/elasticsearch/```.

Si le fichier est bien présent, ouvrez le fichier avec la commande :
```sudo nano /etc/elasticsearch/elasticsearch.yml```

Modifier son contenu comme ci-dessous:

```yaml
cluster.name: my-cluster
node.name: node-1
path.data: /var/lib/elasticsearch
path.logs: /var/log/elasticsearch
network.host: localhost
http.port: 9200
discovery.seed_hosts: ["127.0.0.1"]
cluster.initial_master_nodes: ["node-1"]
```

Attention, **network.host: localhost** permet à Elasticsearch d’être accessible seulement depuis votre machine locale.

## Démarrer et tester ElasticSearch
lancez ElasticSearch avec les commandes suivantes :
``` bash
sudo systemctl daemon-reload
sudo systemctl enable elasticsearch
sudo systemctl start elasticsearch
```
Verifiez que ElasticSearch est bien lancé avec la commande :
```sudo systemctl status elasticsearch```
Vous deviez alors voir **Active: active (running)**

Testez ElasticSearch avec la commande :
```curl -X GET "localhost:9200"```

Vous devriez alors voir une réponse JSON du style :
``` JSON
{
  "name" : "node-1",
  "cluster_name" : "my-cluster",
  "version" : { "number" : "7.x.x", ... },
  ...
}
```
