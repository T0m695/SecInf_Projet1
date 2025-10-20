# Installation de Snort, Syslog-ng, Elasticsearch et Kibana

## Installation de Snort

```bash
sudo apt-get update
sudo apt install snort -y
```

### Ajouter des règles
Ajouter le contenu du fichier local.rules ou le remplacer :
```bash
cp local.rules /etc/snort/rules/local.rules
```
### Modifier la configuration de Snort
Modifier le fichier snort.lua afin de sortir les logs dans le fichier /var/log/snort/alerte_json.txt avec un formatage json

Dans le fichier :
```
/etc/snort/snort.lua
```
Ajoute la section suivante :
```lua
alert_json = {
    file = true,
    limit = 100,
}
```
Dans la section `ip {}`, ajoute également :
```lua
include = '/etc/snort/rules/local.rules',
```

**OU** copier directement le fichier :
```bash
cp snort.lua /etc/snort/snort.lua
```

### Lancer Snort
```bash
sudo snort -A alert_json -c /etc/snort/snort.lua -i eth0 -l /var/log/snort
```


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



## Installation de Syslog-ng

```bash
sudo apt install syslog-ng -y
sudo cp syslog.conf /etc/syslog-ng/conf.d/ids.conf
```

### Encoder un mot de passe en Base64
Utiliser le mot de passe utilisé pour elasticsearch
```bash
echo -n "motdepasse" | base64
```
Copie la sortie du mot de passe encodé dans le fichier de configuration :
```
/etc/syslog-ng/conf.d/ids.conf
```

---
