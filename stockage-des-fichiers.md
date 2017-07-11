---
Title: Stockage des fichiers
Sort: 11
Modified: 
---
```bash
# Arborescence du répertoire "Files"
Files
 |-Applications
   |-65537 # id de l'application
     |-Documents
     |-EditorResources
     |-FtpPackages
     |-Licence
     |-Packages
     |-Pictures
     |-Traces
 |-LocalTmp
 |-Logs
   |-Audit
   |-MySyfadis
   |-Upgrades
   |-WebLrs
 |-Scripts
 |-Themes
 |-Tmp
```

L'utilisation de la plateforme génère deux catégories de fichiers stockées:
* Les logs
* les fichiers clients

#### Les logs

la 1ère catégorie, les logs, est utilisée à des fins d'analyses.

Pour cela on génère des fichiers qui vont nous permettre de comprendre (ou pas) les raisons d'un comportement anormal.

Par exemple: *Pourquoi, un apprenant qui a réussi son quiz ne voit-il pas son état de succès sur la ressource ?*

Pour être en mesure d'analyser ce problème, nous nous servons des fichiers qui sont stockés dans les répertoires **Files/Logs** et **Files/&lt;ApplicationId>/Traces**.

NB: *Ce sont des fichiers au format texte.*

#### Les fichiers clients

La 2nde catégorie concerne les fichiers stockés par le client. 
Il peut s'agir, par exemple:
* d'images présentes dans un quiz ou dans une note.
* des ressources de type vidéo ou des vidéos insérées dans un quiz.
* des documents pdf, excel, word, etc...

Ses fichiers sont stockées dans les répertoires **Documents**, **EditorResources**, **FtpPackages**, **Packages**, **Pictures** et **LocalTmp**.

NB:*Les formats de fichiers acceptées sont définis par le client grâce à un paramètre de configuration*

## Les logs

```bash
# Arborescence du répertoire des logs
Files
 |-Applications
   |-65537 # id de l'application
     |-Traces
 |-Logs
    |-Audit
    |-MySyfadis
    |-Upgrades
    |-WebLrs 
```

L'utilisation de la plateforme génère des informations stockées dans des fichiers textes conservés sur l'espace de stockage vendu au client.

L'espace utilisée par ses logs étant relativement important pour certains clients (de 10% à 40% de l'espace total), nous allons remonter cette information au client.

Les logs sont de différentes natures:
* les logs applicatifs
* les logs scorms (appelés traces scorm)
* les logs IIS

### Les logs applicatifs

```bash
# Arborescence du répertoire des logs
Files
 |-Logs
    |- 2016.09.06.zip #(1 zip par fichier)
    |- 2016.10.25.txt
    |-Audit
       |- 2016-09-05.zip #(1 zip par fichier)
       |- 2017-01-25.syfaudit
    |-MySyfadis
        |- 2016.09.07.zip #(1 zip par fichier)
        |- 2016.09.07.zip
        |- 2016.09.13.txt
    |-Upgrades
      |- 20170124@100000_16885.log
    |-WebLrs 
       |- 2017.01.31.txt # ! répertoire non pris en compte dans les zips !
```

Les logs applicatifs sont de différentes natures:
* les logs initiaux ( Logs )
  * contiennent des informations sur l'utilisation de l'application, ainsi que la trace des erreurs non gérées.
* les logs d'audit ( Audit ) 
  * changements sur les entités, pas ou peu utilisé (activable par la clé "audit.logging.enabled" du fichier de configuration "Web.Config.AppSettings")
* les logs mySyfadis ( MySyfadis )
  * communications de l'application 'MySyfadis' avec les webservices
* les logs de mise à jour ( Upgrades )
  * mise à jour de la *BDD* (schémas et données)
* les logs du LRS ( WebLrs )
  * communications entre les contenus et le LMS avec le LRS (Learning Record Store), il concerne les ressources xAPI.

NB: *les logs applicatifs sont communs pour toutes les applications*

#### Les logs initiaux

NB: *Un script se charge de nettoyer ce répertoire après une durée définie par un paramètre de configuration*

```c#
// Supprime les fichiers zips du répertoire (prend en compte les webfarms)
TempFilesCleaner.DispatchCleanTempFilesAndZipLogs()
```

### Les logs scorm (traces)

```bash
# Arborescence du répertoire des traces
Files
 |-Applications
   |-65537 # id de l'application
      |-Traces
        |-24772608 # userId
          |-8126464 # trainingId
            |- 29655044_29327952.trace.txt # <traineeId>_<studioEntityId>.trace.txt 
            |- 29655044_29327950.trace.txt          
```

Ils contiennent les messages au format xml qui transitent entre les contenus scorm et le LMS. Ils sont localisés par *applicationId*.

NB: *pour l'instant, les traces sont archivées au bout de ... mais jamais supprimées (info baptiste)*

### Les logs IIS

Ce sont les logs du serveur d'application, son emplacement est variable suivant les clients.

NB: *Prévoir un paramètre de configuration afin de saisir l'emplacement des logs IIS.*

NB: *D'un point de vue légal, nous devons garder les logs du serveur d'application pendant 1 an (info baptiste)*.

## Les fichiers clients

La destination des fichiers téléchargés par les clients est représentée ci-dessous.

```bash
# Arborescence du répertoire "Files"
Files
 |-Applications
   |-65537 # id de l'application
     |-Documents
     |-EditorResources
     |-FtpPackages
     |-Licence
     |-Packages
     |-Pictures
 |-LocalTmp
```

### Le répertoire "Documents"

Le composant qui permet de télécharger des fichiers vers la plateforme, qui revêt plusieurs aspects visuels, enregistrent les fichiers dans le répertoire *Documents*.

```bash
# Arborescence du répertoire des logs
Files
 |-Applications
   |-65537 # id de l'application
      |-Documents
        |-411303944
          |- le golf.pptx
        |-464453632
      |-Licence
```

Voici, quelques exemples de pages qui utilise ce composant:

*Composant présent dans l'onglet formations*
<p style="text-align:left;display:inline-block">![formation-onglet-document-upload](%image_url%/formation-onglet-document-upload.png)</p>

*Composant présent dans le cas pratique coté apprenant et formateur pour la correction du cas pratique*
<p style="text-align:left;display:inline-block">![cas-pratique-fil-de-discussion](%image_url%/cas-pratique-fil-de-discussion.png)</p>

*Composant présent lors de la correction d'un cas pratique*
<p style="text-align:left;display:inline-block">![cas-pratique-tutorat](%image_url%/cas-pratique-tutorat.png)</p>

### Le répertoire EditorResources

Stocke tous les éléments téléchargés avec les gestionnaires de contenus de l'éditeur de Html qui **ne sont pas des ressources**.

*Voici, par exemple, l'éditeur html présent dans une formation*
<p style="text-align:left;display:inline-block">![gestionnaire-contenu-editeur-html](%image_url%/gestionnaire-contenu-editeur-html.png)</p>

### Le répertoire FtpPackages

***non traité pour l'instant***

### Le répertoire "Licence"

Stocke le répertoire de la licence.

### Le répertoire Packages

Stocke tous éléments liés à une ressource.

### Le répertoire Pictures

Stocke toutes les images qui utilisent le composant ci-dessous.

*Composant présent dans la page d'un utilisateur afin d'ajouter une image pour son profil*
<p style="text-align:left;display:inline-block">![image-utilisateur](%image_url%/image-utilisateur.png)</p>

On retrouve aussi ce composant dans la page formation dans le but d'illustrer la formation dans le catalogue.

### Le répertoire LocalTmp

Stocke tous les éléments téléchargés qui sont en lien avec une ressource. Bien que le fichier soit copié dans son répertoire de destination ("Packages"), il reste toujours présent dans ce répertoire temporaire.

NB: *Un script se charge de nettoyer ce répertoire après une durée définie par un paramètre de configuration*

```c#
// Supprime les données du fichier LocalTmp (prend en compte les webfarms)
TempFilesCleaner.DispatchCleanTempFilesAndZipLogs()
```