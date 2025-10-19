# 📡 Installation de Wazuh Manager
## Installation de Wazuh Manager
- Pour installer Wazuh Manager, nous allons d'abord télécherger le fichier d'installation avec la commande :
```bash
curl -sO https://packages.wazuh.com/4.9/wazuh-install.sh && sudo bash ./wazuh-install.sh -a
```
- cela risque de prendre un certain temps, car le script va installer toutes les dépendances nécessaires pour Wazuh Manager.
- Une fois l'installation terminée, vous allez avoir le mot de passe de la session admin Wazuh.
> ⚠️ Notez bien ce mot de passe, car vous en aurez besoin pour vous connecter a l'interface web de Wazuh et ca sera le seul moyen d'accéder à votre instance Wazuh.
## Configuration de Wazuh Manager
- Maintenant que Wazuh Manager est installé, nous allons le configurer pour qu'il puisse recevoir les logs des agents Wazuh installés sur les machines clientes.
- Allez sur l'adresse que vous avez configurée pour Wazuh Manager (par défaut, c'est l'adresse IP de votre machine virtuelle serveur) via un navigateur web (Chrome risque d'afficher que la connexion n'est pas sécurisée, il faut accepter le risque pour continuer).
- Vous devriez voir la page de connexion de Wazuh. Connectez-vous avec l'utilisateur **admin** et le mot de passe que vous avez noté lors de l'installation.
- Apres quelques instants de vérification des APIs, vous devriez arriver sur le tableau de bord principal de Wazuh.
- Maintenant, dès que vous reliez un agent Wazuh (installé sur une machine cliente) a ce serveur Wazuh Manager, vous devriez voir les logs arriver dans l'interface web.


Bravo ! 🎉 Vous avez maintenant installé et configuré Wazuh Manager sur votre machine virtuelle, maintenant vous pouvez continuer sur [l'installation d'ElasticSearch et Kibana](./Installation%20ElasticSearch_Kibana.md).