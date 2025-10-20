
# Installation d’Elasticsearch
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


Copier ensuite le fichier de configuration :
```bash
sudo cp elasticsearch.yml /etc/elasticsearch/elasticsearch.yml
```

---

# Installation de Kibana


```bash
sudo apt-get update && sudo apt-get install kibana -y
```
Dans le dossier :
```
/usr/share/elasticsearch
```
Génèrer le token d’enrôlement Kibana :
```bash
bin/elasticsearch-create-enrollment-token -s kibana
```

### Démarrage et configuration

- Lancer **Elasticsearch** et **Kibana**.
- Accèder à **http://localhost:5601** pour la configuration du token.

Pour réinitialiser le mot de passe `elastic` :
```bash
bin/elasticsearch-reset-password -u elastic
```
