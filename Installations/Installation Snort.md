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
