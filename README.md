# LAB 12 : Bypass de la Détection de Root Android avec Medusa

Objectif: réaliser, pas à pas (niveau débutant), un bypass de la détection de root Android en utilisant l’outil Medusa (ensemble d’outils/scripts d’instrumentation, généralement basés sur Frida), puis valider que l’application ne « voit » plus le root.

Prérequis simples

<img width="647" height="253" alt="image" src="https://github.com/user-attachments/assets/cae1cca2-0b6c-4459-8c96-135665f5921c" />

Étape 1 — Préparer l’environnement Android et Frida

Installer Frida côté PC:

<img width="576" height="139" alt="image" src="https://github.com/user-attachments/assets/f28b95c0-4683-4624-996a-b2dc99b7f89b" />

Installer ADB et vérifier l’appareil:

<img width="2173" height="724" alt="image" src="https://github.com/user-attachments/assets/1038deeb-9df1-4d66-ae77-a1bad30cbbf8" />

<img width="2173" height="724" alt="image" src="https://github.com/user-attachments/assets/327ec34c-6870-42e7-a00d-e468e70ef0e3" />

Démarrer frida-server sur l’appareil
Depuis les releases Frida, téléchargez frida-server-<même_version>-android-<arch>.xz, décompressez puis poussez-le:
adb push frida-server /data/local/tmp/
adb shell chmod 755 /data/local/tmp/frida-server
adb shell "/data/local/tmp/frida-server -l 0.0.0.0"   # laissez tourner (ou utilisez nohup pour le background)
Option de redirection de ports (selon appareil):
adb forward tcp:27042 tcp:27042
adb forward tcp:27043 tcp:27043
Vérification:
frida-ps -Uai

Étape 2 — Installer Medusa (outil d’instrumentation)

<img width="638" height="419" alt="image" src="https://github.com/user-attachments/assets/37369689-22c3-43d4-94c7-948bb5dad1b6" />

Vérifiez l’aide/CLI de Medusa:

<img width="647" height="260" alt="image" src="https://github.com/user-attachments/assets/4badb8f6-5184-48d1-93c0-954c4a679a63" />

Sans Medusa/Frida

<img width="196" height="396" alt="image" src="https://github.com/user-attachments/assets/f2ea2e50-1733-4c6d-a583-5f69cf5e4565" />

Root Detection Bypass using Frida

<img width="897" height="449" alt="image" src="https://github.com/user-attachments/assets/3cfd951c-05c3-497e-a56c-7b161685c81c" />

 En cas d'indisponibilité de Medusa, une tentative de contournement de la détection de root a été réalisée à l'aide de Frida et du script bypass_root.js.

Commande utilisée :

frida -U -f owasp.mstg.uncrackable1 -l bypass_root.js 

Une tentative d'exécution de Frida a été effectuée, mais la commande est restée active pendant une longue durée sans permettre de finaliser le test ni de valider le passage de l'application à l'état « Not rooted ».
