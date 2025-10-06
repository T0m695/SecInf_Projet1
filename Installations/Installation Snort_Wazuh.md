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
## Installation de Wazuh indexer
- Maintenant que Snort est configuré, nous allons installer Wazuh indexer pour stocker les logs de Snort. Pour cela, il faut tout d'abord installer curl avec la commande :
```sudo apt install curl```
- Maintenant, il faut lancer cette commande pour récuperer le script d'installation de Wazuh indexer ainsi que récuperer le fichier de configuration [TODO!!](https://www.snort.org/downloads/community/community-rules.tar.gz) :

```curl -sO https://packages.wazuh.com/4.13/wazuh-install.sh```
> ⚠️ Attention, c'est -sO avec un o majuscule a la fin, non un zéro ! et Curl va installer le script dans le dossier courant, donc faites attention a bien être dans le dossier que vous voulez !
- Toujours dans le meme terminal, lancez le script d'installation avec la commande :
```sudo bash wazuh-install.sh --generate-config-files```