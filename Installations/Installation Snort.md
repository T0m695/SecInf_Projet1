# Installation de Snort

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

## Installation de Wazuh Agent
- Maintenant que Snort est bien configuré sur cette machine, nous allons installer Wazuh Agent pour envoyer les logs de Snort au serveur principal Wazuh de notre architecture Sécuritée.
- Pour cela, on va suivre la démarche de la documentation officielle de Wazuh pour installer l'agent sur Ubuntu : [Wazuh Agent Installation on Linux](https://documentation.wazuh.com/current/installation-guide/wazuh-agent/wazuh-agent-package-linux.html)
- Il faut installer la clée GPG de Wazuh avec la commande :
```sudo curl -s https://packages.wazuh.com/key/GPG-KEY-WAZUH | gpg --no-default-keyring --keyring gnupg-ring:/usr/share/keyrings/wazuh.gpg --import && chmod 644 /usr/share/keyrings/wazuh.gpg```
- Ensuite, ajoutez le dépôt Wazuh a vos sources APT avec la commande :
```sudo echo "deb [signed-by=/usr/share/keyrings/wazuh.gpg] https://packages.wazuh.com/4.x/apt/ stable main" | tee -a /etc/apt/sources.list.d/wazuh.list```
- Mettez a jour les paquets et installez Wazuh Agent avec les commandes :
```sudo apt update```
```WAZUH_MANAGER="10.0.0.2" apt-get install wazuh-agent```
> ⚠️ Pensez a remplacer l'adresse IP 10.0.0.2 par l'adresse IP de votre serveur central de sécuritée Wazuh
- Une fois tout installé, il faut juste lancer l'agent Wazuh avec la commande :
```
systemctl daemon-reload
systemctl enable wazuh-agent
systemctl start wazuh-agent
```

- Bravo ! 🎉 Vous avez maintenant installé Snort et Wazuh sur votre machine virtuelle. Une fois toutes vos machines configurées, vous pouvez maintenant continuer sur [l'Installation de Wazuh Central sur le serveur principal](./Installation%20Wazuh%20Manager.md).