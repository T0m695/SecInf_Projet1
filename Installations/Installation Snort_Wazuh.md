# 📡 Installation de Snort/Wazuh
## Prérequis
- Une fois la virtual machine lancée, lancer le terminal et vérifiez qu'Ubuntu est bien a jour avec la commande :
```sudo apt update```
- Puis lancez le proccesus d'installation de Snort avec la commande :
```sudo apt install snort -y```
- Durant l'installation, il vous sera demandé de rentrer l'adresse IP de votre interface réseau, rentrez l'adresse IP que vous avez configuré lors de l'installation d'Ubuntu (si vous ne l'avez pas fait, vous pouvez la trouver avec la commande ```ip a```).
- Une fois l'installation terminée, vous pouvez vérifier que Snort est bien installé avec la commande :
```snort -V```, si vous avez une version qui s'affiche, c'est que Snort est bien installé.
## Configuration de Snort
Par défaut Snort n'est pas configuré, nous allons donc le configurer avec le fichier de configuration dans le repository. 
- Prenez le fichier de configuration [snort.conf](TODO) et placez le dans le dossier ```/etc/snort/```.
> ℹ️ On peut voir dans ce fichier différents réglages tels que ```output alert_syslog: LOG_AUTH LOG_ALERT``` qui dit a Snort d'envoyer les messages d'authentification et autres alertes au syslog.
>
> Snort stocke les logs en format pcap qui est un format binaire souvent lu par des logiciels comme Wireshark, mais on a modifié ce paramètre via ```output alert_fast: snort.alert``` pour renvoyer les logs dans un fichier texte
- Une fois que Snort est configuré pour lancer les alertes au SIEM, il faut relancer Snort pour appliquer les réglages avec la commande :
```sudo systemctl restart snort```
## Configuration des règles Snort
- Maintenant nous allons configurer les règles d'analyse de traffic réseau pour Snort. Placez les regles [TODO!!](https://www.snort.org/downloads/community/community-rules.tar.gz) dans le dossier ```/etc/snort/rules/```.
> ℹ️ Ce qu'on a ajouté c'est principalement la règle ```alert icmp any any -> any any (msg:"ICMP connection attempt:"; sid:1000010; rev:1;)``` qui permet de détecter les tentatives de connexion de n'importe quelle source et destination ICMP sur le réseau.
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