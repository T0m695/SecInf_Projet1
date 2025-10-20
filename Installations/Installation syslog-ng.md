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
