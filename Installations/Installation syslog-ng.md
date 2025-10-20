# Installation de Syslog-ng

## Ajouter le mot de passe elasticsearch

### Encoder un mot de passe en Base64
Utiliser le mot de passe utilisé pour elasticsearch
```bash
echo -n "motdepasse" | base64
```
Copie la sortie du mot de passe encodé dans le fichier de configuration :
```
syslog.conf
```
## Copier le fichier de configuration

```bash
sudo apt install syslog-ng -y
sudo cp syslog.conf /etc/syslog-ng/conf.d/ids.conf
```


Félicitations ! 🎉 Vous avez maintenant installé et configuré Syslog-ng sur votre machine virtuelle. Maintenant votre architecture de sécurité est complète.