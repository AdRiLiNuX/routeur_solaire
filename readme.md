# Réseautonome - Routeur offgrid

## Sommaire
* Paramétrage
  * Fonctionnalités optionnelles du système
  * Paramétrage pour l'algorithme
  * Paramétrage minimum pour le fonctionnement du serveur web
  * Paramétrage pour le fonctionnement MQTT
* Flash sur ESP
  * Via platformIO
  * Via Arduino IDE
* Développement


## Paramétrage
### Fonctionnalités optionnelles du système
Les champs décrits ci-dessous permette d'activer ou non certaines options
Ils sont présent dans le fichier settings.h et si l'option ne vous interesse pas, il faut les mettre en commentaire (commencer la ligne par '//' : //#define  MesureTemperature)

| Champs      |     Valeur |
| ------------- | ---------: |
| MesureTemperature      |       Permet la lecture de la sonde de dallas température  |
| Pzem04t      |       utilise un pzem004 pour la mesure de puissance dans le ballon  |
| Sortie2      |       Indique la connexion d'un deuxième Triac  |
| WifiMqtt      |       Communication via Mqtt (récupération et envoie de données)  |
| WifiServer      |     Serveur Web pour visualiser les données systèmes et modifier les valeurs de l'algorithme |
| EcranOled      |     Affichage de certaines info sur un afficheur Oled |
| simulation      |     Simulation de données pour des tests |

### Paramétrage pour l'algorithme
| Champs      |    Fichier | default |  description |
| ------------- | :---------: |---------: |---------:|
| zeropince  |   settings.h    | -100 | valeur mesurer à zéro |
| coeffPince  |   settings.h   |  0.0294 | Calculer le coefficient |
| coeffTension  |   settings.h    | 0.0177533 | diviseur de tension |
| seuilDemarrageBatterie  |   settings.h   | 56 | seuil de mise en marche de la regulation dans le ballon |
| toleranceNegative  |   settings.h    | 0.5 | autorisation de 300mA négative au moment de la charge complète |
| utilisation2Sorties  |   settings.h   | false | validation de la sortie 2eme gradateur |
| temperatureBasculementSortie2  |   settings.h    | 60 | température pour démarrer la regul sur la sortie 2 |
| temperatureRetourSortie1  |   settings.h   | 45 |température pour rebasculer sur le premier gradateur  |
| relaisStatique  |   settings.h    | false | Indique si un relais statique est utilisé |
| seuilMarche  |   settings.h   | 50 | température ou tension de déclenchement du relais |
| seuilArret  |   settings.h    | 45 | température ou tension d'arret du relais |
| tensionOuTemperature  |   settings.h   | "V" | Indique si le seuil est en "V"olts ou en "D"egrés|
| correctionTemperature  |   settings.h    | -2.3 | |
| basculementMode  |   settings.h   | "T" | // Choix du mode de basculement : T->température, P-> Puissance zero|


### Paramétrage minimum pour le fonctionnement du serveur web
| Champs      |    Fichier | default |  description |
| ------------- | :---------: |---------: |---------:|
| ssid  |   settings.h    | "" |Identifiant "ssid de la  box internet |
| password  |   settings.h   | "" | Mot de passe de la box internet  |
| SAP  |   src.ino   | false | serveur en point d'accès (en dehors du réseau domestique -> ssid: "routeur_esp32"; password : "adminesp32" |

### Paramétrage pour le fonctionnement MQTT
| Champs      |    Fichier |  default |  description |
| ------------- | :---------: |---------:|---------:|
| mqttServer  |   settings.h    | "192.168.1.28" | ip du serveur mqtt |
| mqttPort  |   settings.h    |  1883 | port du serveur mqtt |
| mqttUser  |   settings.h    |  "" | utilisateur mqtt si nécessaire |
| mqttPassword  |   settings.h    |  "" | mot "de passe mqtt si nécessaire |
| mqttopic  |   settings.h    | "sensor/solar"  |  |
| mqttopicInput  |   settings.h    |  "output/solar" | |
| mqttopicParam1  |   settings.h    |  "param/solar1" | |
| mqttopicParam2  |   settings.h    |   "param/solar2" | |
| mqttopicParam3  |   settings.h    | "param/solar3"  | |
| mqttopicPzem1  |   settings.h    |  "sensor/Pzem1" | |


## Flash sur ESP

### PlatformIO
#### Installation VSCodium
https://github.com/VSCodium/vscodium/releases

#### Plugin PlatformIO
Depuis vscodium : 
file -> préference -> extension, rechercher et installer platformIO IDE

#### Paramétrage 
Modifier les informations dans le fichier platformio.ini
| Champs      |     default |  description |
| ------------- |:---------:|---------:|
|platform|espressif32| plateforme cible|
|board|esp32dev|carte principale|
|monitor_speed|115200|vitesse de lecture|
|upload_port|COM3| port COM sur lequel est branché l'ESP|

#### Serveur web dans le spiff
Via l'onglet Plateformio qui est disponible sur la gauche de vscodium (la barre latérale, l'icone qui ressemble à une fourmi) :  
Cliquer sur "Upload File System image"

#### Application  
Depuis la barre d'action rapide en base de VSCodium : cliquer sur la fleche "PlatformIO: Upload"

#### Monitor
Depuis la barre d'action rapide en base de VSCodium : cliquer sur la fleche "PlatformIO: Serial Monitor"

Ceci permet de voir les infos et logs de l'ESP (Ip du serveur, mesures prises, publication mqtt ....)


### Arduino IDE
#### Libraries nécessaires
ArduinoJson 
OneWire 
DallasTemperature 
PubSubClient 
EspSoftwareSerial 
ESP8266_SSD1306 


Ouvrir le fichier src.ino

#### Serveur web dans le spiff
Il faut installer l'outil d'envoi de fichier système. 
télécharger le zip présent ici : https://github.com/me-no-dev/arduino-esp32fs-plugin/releases/ 
Extraire le zip dans le dossier 'tools' d'installation Arduino (C:\Program Files (x86)\Arduino\tools) 
ce qui donnera C:\Program Files (x86)\Arduino\tools\ESP32FS\tool\esp32fs.jar

Redémarrer Arduino IDE et cliquer sur Outils -> ESP32 Sketch Data Upload

#### Application  
Depuis la barre d'action rapide, en dessous des menus : cliquer sur la fleche "Téléverser"

#### Monitor
Depuis la barre d'action rapide, en dessous des menus, complétement à droiote : cliquer sur la loupe "Moniteur série"
Appuyer sur le bouton "Téléverser"

## Développement
Pour changer des fonctionnalités, vous pouvez éditer le code.
Si vous souhaitez éditer le code du serveur web, il faut modifier les fichiers présents dans le dossier "dataCode" puis, une fois les changements effectués, minifier le code dans le dossier "data".
Outils en ligne pour minimifier le code : 
html / css / js : http://minifycode.com/javascript-minifier/
json : https://www.webtoolkitonline.com/json-minifier.html