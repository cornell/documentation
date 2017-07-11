---
Sort: 2
---
## Introduction

Lors de l'utilisation de la plateforme, nous permettons aux clients d'ajouter des fichiers (image, vidéo, pdf, excel, etc...).

Avec le temps, ses fichiers prennent une place importante (plusieurs Go). La fonctionnalité de purge a pour but de libérer cet espace disque.

Pour cela, nous allons identifier les éléments rattachés à des fichiers qui ne sont plus utilisés.

NOTE:<br>
*Tous ses fichiers sont stockés dans [des répertoires spécifiques](./stockage-des-fichiers.md).*

## Les éléments à purger

La purge s'effectue sur les éléments suivants:
* les ressources
* les questions

### Les ressources

Les ressources concernées sont les types suivants:
* note          (Note)
* cas pratique  (PracticalCase)
* vidéo         (Video)
* scorm         (Content)
* contenu       (CourseDocument)
* quiz          (Quiz)

Les ressources suivantes ne nécessitent pas de purge car elles ne stockent pas de fichier:
* module                    (Module)
* ressource externe         (ExternalResource)
* ressource communautaire   (CommunityResource)

les ressources suivantes ne sont pas prise en compte pour l'instant:
* bibliothèque multimédia   (MediaLibrary)
* aicc                      (AICCResource)
* xapi                      (XApiResource)
* quiz epsi                 (QuizEpsiResource)

### Les questions

La resource "quiz" contient des questions que l'on purge aussi.

Les questions concernées sont les suivantes:
* oui/non               (MultipleChoiceQuestion)
* à choix multiples     (MultipleChoiceQuestion)
* à réponse unique      (MultipleChoiceQuestion)
* ouverte               (OpenQuestion)
* à trous               (FillInTheGapsQuestion)
* glisser, déplacer     (DragAndDropQuestion)
* appariement           (MatchQuestion)
* ordonnancement        (OrderQuestion)
* satisfaction           (SatisfactionQuestion)
* ipsative              (IpsativeQuestion)

TECH:<br>
Ce sont les questions qui implémentent l'interface *IQuestion*.

## User story [#18015](https://redmine.condate.com/issues/18015)

*En tant qu'*administrateur<br>
*Je souhaite* visualiser mon espace de stockage<br>
*Afin de* connaître la place restante<br>

**Soit**  une licence ne contenant pas la clé qui définit l'espace disque alloué à l'application<br>
**Quand** l'admin accède à la page de purge<br>
**Alors** l'espace disque total affiché est de 50Gb<br>

**Soit**  une licence ne contenant pas la clé qui définit l'espace disque alloué à l'application<br>
**Quand** l'admin modifie l'espace disque alloué à l'application (paramètre de configuration)
**Et**    que l'admin accède à la page de purge<br>
**Alors** l'espace disque total affiché est égal au paramètre de configuration<br>


**Quand** l'application a le paramètre de licence d'espace disque alloué<br>
**Alors** la valeur maximale du visuel de l'utilisation de l'espace disque est égale à ce paramètre de licence<br>
**Sinon** la valeur maximale est égale au paramètre de configuration équivoque

## User story 1

*En tant qu'*administrateur<br>
*Je souhaite* supprimer les packages des *ressources autonomes archivées*<br>
*Afin de* libérer de l'espace disque<br>

### Scénarios

#### Le package est supprimable

**Soit**  un package référencé par une *ressource autonome archivée*<br>
**Quand** j'identifie son package<br>
**Alors** la ressource est purgée<br>
**Et**    le package est supprimé<br>

**Soit**  un package supprimable référencé par une *ressource autonome archivée* à l'instant T<br>
**Quand** je souhaite supprimer ce package à T+1<br>
**Et**    que la ressource est toujours archivée<br>
**Alors** la ressource est supprimée<br>
**Et**    le package est supprimé<br>

#### Le package n'est pas supprimable

**Soit**  un package supprimable référencé par une *ressource autonome archivée* à l'instant T<br>
**Quand** je souhaite supprimer ce package à T+1<br>
**Et**    que la ressource n'est plus archivée<br>
**Alors** la ressource n'est pas supprimée<br>
**Et**    le package n'est pas supprimé<br>

## User story 2

*En tant qu'*administrateur<br>
*Je souhaite* supprimer les packages des *ressources archivées*<br>
*Afin de* libérer de l'espace disque<br>

### Scénarios


#### Le package est supprimable

**Soit**  un package référencé par deux *ressources archivées*<br>
**Quand** j'identifie leur package<br>
**Alors** les deux ressources sont purgées<br>
**Et**    le package est supprimé<br>

**Soit**  un package supprimable référencé par deux *ressource archivées* à l'instant T<br>
**Quand** je souhaite supprimer ce package à T+1<br>
**Et**    que les deux ressoruce sont toujours archivées<br>
**Alors** les ressources ne sont pas supprimées<br>
**Et**    le package n'est pas supprimé<br>

#### Le package n'est pas supprimable

**Soit**  un package référencé par 2 ressources<br>
**Quand** une ressource est archivée et que l'autre ressource est utilisée<br>
**Alors** ce package n'est pas supprimé<br>

**Soit**  un package référencé par 2 ressources<br>
**Quand** une ressource est archivée et que l'autre ressource est inutilisée<br>
**Alors** ce package n'est pas supprimé<br>

**Soit**  un package supprimable référencé par deux *ressource archivées* à l'instant T<br>
**Quand** je souhaite supprimer ce package à T+1<br>
**Et**    qu'au moins une des ressources n'est plus archivée<br>
**Alors** les ressources ne sont pas supprimées<br>
**Et**    le package n'est pas supprimé<br>

## User story 3

*En tant qu'*administrateur<br>
*Je souhaite* être notifié par mail si mon espace de stockage atteint un certain seuil<br>
*Afin d*'être alerté à temps<br>

### Scénarios

#### L'utilisation de mon espace de stockage dépasse le seuil configuré

**Soit**  que des personnes ont le privilège "Vsix/Administration/Visibilité du menu de purge"<br>
**Quand** l'espace de stockage dépasse ce seuil<br>
**Alors** ces personnes reçoivent un email lors du traitement par lot<br>

**Soit**  que l'email a été envoyé <br>
**Quand** l'admin ouvre son email<br>
**Alors** il est informé du dépassement<br>
**Et**    il peut accéder à la page de purge<br>

## User story 5 [#18017](https://redmine.condate.com/issues/18017)[#18341](https://redmine.condate.com/issues/18341)

*En tant qu'*administrateur<br>
*Je souhaite* visualiser l'occupation de l'espace disque par types de répertoires<br>
*Afin de* identifier les éléments les plus volumineux<br>

### Scénarios

**Soit**  une formation ou une session de formation<br>
**Quand** j'y ajoute un document<br>
**Alors** l'espace disque occupé par les documents augmentent<br>

**Soit**  une formation ou une session de formation<br>
**Quand** j'y ajoute un élément téléchargé<br>
**Alors** l'espace disque occupé par les documents augmentent<br>

**Soit**  plusieurs répertoires visualisés<br>
**Quand** un des répertoires n'occupe plus d'espace disque<br>
**Alors** il n'est plus visible<br>

## User story 6

**En tant qu'** administrateur<br>
**Je souhaite** visualiser les packages les plus volumineux qui sont liés à des ressources studio non purgables<br>
**Afin de** pouvoir les archiver si ils ne sont plus utilisés<br>

### Scénarios

**Soit**  un package qui est lié à une ressource non purgable<br>
**Quand** l'admin. accède à la page de purge<br>
**Alors** il visualise le nom du package<br>
**Et**    sa taille<br>
**Et**    le nom de la ressource<br>
**Et**    un lien vers cette ressource<br>

**Soit**  un package qui est lié à des ressources non purgables (non archivées)<br>
**Quand** l'admin. accède à la page de purge<br>
**Alors** il visualise le nom du package<br>
**Et**    sa taille<br>
**Et**    le nom des ressources qui y sont liées <br>
**Et**    des liens vers les ressources qui y sont liées (seulement les non archivées)<br>

**Soit**  plusieurs packages qui sont liés à des ressources non purgables<br>
**Quand** l'admin. accède à la page de purge<br>
**Alors** les packages sont affichés par taille décroissante<br>

**Soit**  un package qui n'est lié qu'à des resources archivées<br>
**Quand** l'admin. accède à la page de purge<br>
**Alors** il ne visualise pas le package car ce package est purgeable.<br>

## Eléments techniques

```sql
-- (1) identifie des resources (hors module) archivées qui sont liées à un répertoire
SELECT	ResourceId,Reso_Subclass,Reso_Version,Reso_DeletedOn,Reso_CreatedOn,Reso_ModifiedOn,Reso_Name, Reso_IdStudioEntity, Reso_VersionOnline, Reso_LastVersionOnline, Prev_IdStudioEntity, Prev_IdUserEntity, Reso_Guid, Reso_CourseId 
FROM	Stu_Resource RES
WHERE	RES.Reso_Subclass<>'Module' AND RES.Reso_Subclass<>'Quiz' 
		--AND RES.Reso_ApplicationId = 65537 
		and RES.Reso_DeletedOn is not null    -- archivé
        and RES.Reso_Guid is not null         -- lié à un répertoire
		and RES.Reso_IdStudioEntity is null   -- not online
		and RES.Prev_IdUserEntity is null     -- not preview
ORDER	BY RES.Reso_Name asc
```

```bash
# contenu pour tester les ressources qui partagent un même répertoire.
D:\Travail\Trunk\Dev\Tests\Player\Utils\Documents\Scorm\SimplifiedGolf.zip
```