# LAB 12 : Bypass de la Détection de Root Android avec Medusa

Objectif: réaliser, pas à pas (niveau débutant), un bypass de la détection de root Android en utilisant l’outil Medusa (ensemble d’outils/scripts d’instrumentation, généralement basés sur Frida), puis valider que l’application ne « voit » plus le root.

Prérequis simples

<img width="647" height="253" alt="image" src="https://github.com/user-attachments/assets/cae1cca2-0b6c-4459-8c96-135665f5921c" />

Cette capture montre la vérification de l'installation de Python et de pip sur la machine hôte. Les commandes `python --version` et `pip --version` permettent de confirmer que l'environnement Python est correctement configuré avant l'installation des outils nécessaires au laboratoire.

###  Préparer l’environnement Android et Frida

Installer Frida côté PC:

<img width="576" height="139" alt="image" src="https://github.com/user-attachments/assets/f28b95c0-4683-4624-996a-b2dc99b7f89b" />


Cette capture confirme que Frida est correctement installé sur le poste de travail. La commande `frida --version` affiche la version installée, tandis que le module Python Frida est vérifié à l'aide d'un script Python affichant la version de la bibliothèque.

Installer ADB et vérifier l’appareil:
<img width="2173" height="724" alt="image" src="https://github.com/user-attachments/assets/1038deeb-9df1-4d66-ae77-a1bad30cbbf8" />
<img width="2173" height="724" alt="image" src="https://github.com/user-attachments/assets/327ec34c-6870-42e7-a00d-e468e70ef0e3" />

Cette étape permet de vérifier la communication entre le poste de travail et l'émulateur Android. La commande `adb devices` affiche les appareils connectés et confirme que l'émulateur est détecté et prêt à être utilisé pour les tests.

### Démarrer Frida Server sur l'appareil

Télécharger depuis les releases Frida la version de **frida-server** correspondant à la version de Frida installée ainsi qu'à l'architecture de l'appareil Android. Après décompression de l'archive, transférer le binaire sur l'appareil :

```bash
adb push frida-server /data/local/tmp/
adb shell chmod 755 /data/local/tmp/frida-server
adb shell "/data/local/tmp/frida-server -l 0.0.0.0"
```

> Laisser le processus actif pendant toute la durée des tests. Il est également possible d'utiliser `nohup` pour l'exécuter en arrière-plan.

#### Redirection des ports (si nécessaire)

```bash
adb forward tcp:27042 tcp:27042
adb forward tcp:27043 tcp:27043
```

#### Vérification de la connexion Frida

```bash
frida-ps -Uai
```

Cette commande permet de vérifier que Frida communique correctement avec l'appareil Android et d'afficher la liste des applications installées.

### Installer Medusa (outil d’instrumentation)

<img width="638" height="419" alt="image" src="https://github.com/user-attachments/assets/37369689-22c3-43d4-94c7-948bb5dad1b6" />

Vérifiez l’aide/CLI de Medusa:

<img width="647" height="260" alt="image" src="https://github.com/user-attachments/assets/4badb8f6-5184-48d1-93c0-954c4a679a63" />

Sans Medusa/Frida

<img width="196" height="396" alt="image" src="https://github.com/user-attachments/assets/f2ea2e50-1733-4c6d-a583-5f69cf5e4565" />

### Root Detection Bypass using Frida

<img width="897" height="449" alt="image" src="https://github.com/user-attachments/assets/3cfd951c-05c3-497e-a56c-7b161685c81c" />

 En cas d'indisponibilité de Medusa, une tentative de contournement de la détection de root a été réalisée à l'aide de Frida et du script bypass_root.js.

Commande utilisée :

frida -U -f owasp.mstg.uncrackable1 -l bypass_root.js 

Une tentative d'exécution de Frida a été effectuée, mais la commande est restée active pendant une longue durée sans permettre de finaliser le test ni de valider le passage de l'application à l'état « Not rooted ».

Le comportement attendu de ce bypass est que l'application ne détecte plus l'appareil comme étant rooté et qu'elle poursuive son exécution normalement, comme si elle était exécutée sur un appareil non rooté.
