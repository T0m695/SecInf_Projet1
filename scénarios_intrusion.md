# Les 5 scénarios d'intrusion
Dans ce fichier, vous retrouverez 5 exemples de scénarios d'intrusion qui pourraient déclencher une alerte snort, ainsi que les règles associées à configurer dans le fichier **local.rules**.

Pour executer les commandes pour déclencher des alertes, il faut définir la variable TARGET qui représente l'adresse IP de votre VM. Tapez dans votre terminal ``` TARGET = <VOTRE ADRESSE IP>```

## Scénario 1: Inclusion de fichier local (LFI)
Une LFI (Local File Inclusion) se produit quand un site web accepte un nom de fichier en entrée et ouvre un fichier local du serveur sans vérifier ce que l’utilisateur a demandé. Ainsi l’attaquant profite de cela pour lire des fichiers sensibles comme par exemple /etc/passwd, fichiers de configuration, clés, etc.

Cette attaque est très courante sur les serveurs web qui sont mal configurés et permet d’obtenir des informations sensibles du système.

REGLE SNORT:
La règle recherche la chaîne /etc/passwd (ou des motifs ../..) dans l’URI. Si trouvée, alerte LFI.
``` bash
alert tcp any any -> any 80 (msg:"TEST Local File Inclusion attempt"; flow:to_server,established; content:"/etc/passwd"; nocase; http_uri; sid:1000003; rev:1;)
```

Vous pouvez déclencher une alerte en executant:
``` bash
curl -v "http://$TARGET/index.php?page=../../../../etc/passwd"
```

## Scénario 2: Exécution d'un code à distance (RCE) avec PHP
La RCE (Remote Code Execution) est une vulnérabilité permettant à un attaquant d’exécuter des commandes ou du code sur un serveur à distance. Donc l'attaquant peut demander au serveur d’exécuter des opérations qu’il ne devrait pas faire, ce qui peut conduire à la prise de contrôle de la machine.

L'attaquant envoie une requête HTTP avec un paramètre injecté pour exécuter du code PHP à distance **<?php**.

REGLE SNORT:
La règle cherche la présence de <?php ou d’autres signatures d’un code injecté dans l’URI ou le corps de la requête POST.
``` bash
alert tcp any any -> any 80 (msg:"TEST PHP code injection attempt"; flow:to_server,established; content:"<?php"; nocase; http_uri; sid:1000004; rev:1;)
```

Vous pouvez déclencher une alerte en executant:
``` bash
curl -v "http://$TARGET/upload.php?file=<?php system('id'); ?>"
```
## Scénario 3: Injection SQL 
L'attaquant tente d’exploiter une vulnérabilité SQL dans une page web en envoyant une requête avec **' OR '1'='1** dans un paramètre GET.

Un site web reçoit une requête où l’attaquant insère du texte spécial **' OR '1'='1** dans un champ (par exemple: formulaire de connexion). Si le site n’a pas bien protégé la base de données, la requête peut tromper la base et retourner ou modifier des données (par exemple: utilisateurs, mots de passe).

Ce type de requête est l'une des source d'attaques web les plus courantes pour voler et/ou modifier des données potentiellement sensibles. De plus, c'est un exemple pratique à mettre en place dans un environnement de test.

REGLE SNORT:
La règle Snort recherche la chaîne ' OR '1'='1 dans l’URI de la requête web. Si la chaine est trouvée, Snort génère une alerte indiquant une tentative d’injection SQL.
``` bash
alert tcp any any -> any 80 (msg:"TEST SQL Injection attempt"; flow:to_server,established; content:"' OR '1'='1"; nocase; http_uri; sid:1000001; rev:1;)
```
Vous pouvez déclencher une alerte en executant:
``` bash
curl -v --get --data-urlencode "user=admin" --data-urlencode "pass=' OR '1'='1" "http://$TARGET/login.php"
```

## Scénario 4: Scan de ports ou reconnaissance réseau
Un attaquant scanne le serveur pour identifier quels ports sont ouverts (HTTP, SSH, FTP, etc.), et ainsi peut choisir l’attaque suivante (par exemple: se concentrer sur SSH si ouvert).

La reconnaissance est souvent la première étape d’une attaque pour comprendre comment mettre en place un scénario d'attaque. C'est pourquoi détecter les scans permet de prévenir les attaques les plus sérieuses.

REGLE SNORT:
La règle utilise l’option threshold pour détecter X tentatives SYN venant d’une même IP vers plusieurs ports en Y secondes. Cela déclenche une alerte Port Scan Detected.
``` bash
alert tcp any any -> 192.168.1.200 1:65535 (msg:"Port Scan Detected"; flags:S; threshold:type both, track by_src, count 10, seconds 60; sid:100004; rev:1;)
```

Vous pouvez déclencher une alerte en executant:
``` bash
sudo nmap -sS -p1-2000 $TARGET
```
## Scénario 5: Attaque par cross-site scripting (XSS)
Le Cross‑Site Scripting (XSS) est une vulnérabilité web où l'attaquant injecte du code JavaScript malveillant dans une page consultée par d'autres utilisateurs, ^pour voler des sessions, manipuler l'affichage ou exécuter des actions en se faisant passer pour la victime.

L'attaquant envoie une requête contenant ```<script>alert(1)</script>``` dans un formulaire web pour injecter du script côté client.

XSS est courant sur les applications web et permet de voler des sessions ou manipuler le comportement du site.

REGLE SNORT:
La règle recherche des motifs ````<script> ```` ou  ````alert( ```` dans l’URI / corps d’une requête POST/GET et déclenche une alerte XSS.
``` bash
alert tcp any any -> 192.168.1.200 80 (msg:"Cross-Site Scripting Attempt"; flow:to_server,established; content:"<script>alert(1)</script>"; http_uri; nocase; sid:100005; rev:1;)
```

Vous pouvez déclencher une alerte en executant:
``` bash
curl -v --get --data-urlencode "q=<script>alert(1)</script>" "http://$TARGET/search.php"
```
