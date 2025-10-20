
## Installation d’Elasticsearch

> 🔗 Référence : documentation officielle Elastic

```bash
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo gpg --dearmor -o /usr/share/keyrings/elasticsearch-keyring.gpg
sudo apt-get install apt-transport-https -y
echo "deb [signed-by=/usr/share/keyrings/elasticsearch-keyring.gpg] https://artifacts.elastic.co/packages/9.x/apt stable main" | sudo tee /etc/apt/sources.list.d/elastic-9.x.list
```
Copier ensuite le fichier de configuration :
```bash
sudo cp elasticsearch.yml /etc/elasticsearch/elasticsearch.yml
```

---

## Installation de Kibana

> 🔗 Référence : documentation officielle Elastic

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
