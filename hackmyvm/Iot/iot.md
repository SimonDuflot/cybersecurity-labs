# URL
https://hackmyvm.eu/machines/machine.php?vm=Iot
# Conceptes
utilisation du service MQTT
environnement local virtuel pour tester
serveur local pour transférer fichier/script dans la VM

# Méthodes utilisés
nmap -> on sait que les ports 22 et 1883 sont ouverts
On utilise un outil disponible depuis github: 
```
https://github.com/bapowell/python-mqtt-client-shell
```

Pour utiliser l'outil on doit installer des dépendances tierces. A faire dans un environnement virtuel.
```
pip install paho-mqtt
pip install --upgrade pip setuptools
```

Après avoir éxecuter le script on utilise les commandes suivantes, dans l'ordre:
```
python mqtt_client_shell.py
logging off
connection
host <IP_ADDRESS>
connect
subscribe #
```

Après avoir attendu quelques secondes, nous receptionnons un message contenant les identifiants nécessaires pour se connecter via SSH

