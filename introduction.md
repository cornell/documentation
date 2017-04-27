---
Title: Introduction
Sort: 1
Modified: 
---
## Introduction

Lors de l'utilisation de la plateforme, nous permettons aux clients d'ajouter des fichiers (image, vidéo, pdf, excel, etc...).

Avec le temps, ses fichiers prennent une place importante (plusieurs Go). La fonctionnalité de purge a pour but de libérer cet espace disque.

Pour cela, nous allons identifier les éléments rattachés à des fichiers qui ne sont plus utilisés.

NOTE:<br>
*Tous ses fichiers sont stockés dans [des répertoires spécifiques](./01-stockage-des-fichiers.md).*

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

## User story 1

*En tant qu'*administrateur<br>
*Je souhaite* supprimer les packages des *ressources autonomes archivées*<br>
*Afin de* libérer de l'espace disque<br>

### Scénarios

#### Le package est supprimable

**Etant** donné un package référencé par une *ressource autonome archivée*<br>
**Quand** j'identifie son package<br>
**Alors** la ressource est purgée<br>
**Et** le package est supprimé<br>

**Etant** donné un package supprimable référencé par une *ressource autonome archivée* à l'instant T<br>
**Quand** je souhaite supprimer ce package à T+1<br>
**Et** que la ressource est toujours archivée<br>
**Alors** la ressource est supprimée<br>
**Et** le package est supprimé<br>

#### Le package n'est pas supprimable

**Etant** donné un package supprimable référencé par une *ressource autonome archivée* à l'instant T<br>
**Quand** je souhaite supprimer ce package à T+1<br>
**Et** que la ressource n'est plus archivée<br>
**Alors** la ressource n'est pas supprimée<br>
**Et** le package n'est pas supprimé<br>

## User story 2

*En tant qu'*administrateur<br>
*Je souhaite* supprimer les packages des *ressources archivées*<br>
*Afin de* libérer de l'espace disque<br>

### Scénarios

#### Le package est supprimable

**Etant** donné un package référencé par deux *ressources archivées*<br>
**Quand** j'identifie leur package<br>
**Alors** les deux ressources sont purgées<br>
**Et** le package est supprimé<br>

**Etant** donné un package supprimable référencé par deux *ressource archivées* à l'instant T<br>
**Quand** je souhaite supprimer ce package à T+1<br>
**Et** que les deux ressoruce sont toujours archivées<br>
**Alors** les ressources ne sont pas supprimées<br>
**Et** le package n'est pas supprimé<br>

#### Le package n'est pas supprimable

**Etant** donné un package référencé par 2 ressources<br>
**Quand** une ressource est archivée et que l'autre ressource est utilisée<br>
**Alors** ce package n'est pas supprimé<br>

**Etant** donné un package référencé par 2 ressources<br>
**Quand** une ressource est archivée et que l'autre ressource est inutilisée<br>
**Alors** ce package n'est pas supprimé<br>

**Etant** donné un package supprimable référencé par deux *ressource archivées* à l'instant T<br>
**Quand** je souhaite supprimer ce package à T+1<br>
**Et** qu'au moins une des ressources n'est plus archivée<br>
**Alors** les ressources ne sont pas supprimées<br>
**Et** le package n'est pas supprimé<br>

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

## Privilèges

Privilège pour voir le menu Purge
    Vsix/Manager: Visibilité des utilisateurs dans Mon équipe 

## Paramètres de configuration

Paramètres: 
    "Seuil de tolérance d'utilisation d'espace disque en % ou en GB. Ajoutez l'unité à la fin du paramètre. Ex: '150GB' ou '80%'. "
    valeur par défaut: 80%

    Espace disque alloué pour l'application en GB. Le paramètre de licence du même nom est prioritaire à ce paramètre.
    valeur par défaut: 50

# Glossaire

**ressource utilisée**<br>
ressource associée à un ou plusieurs modules.

**ressource inutilisée**<br>
ressource qui n'est associéee à aucun module.

**ressource archivée**<br>
ressource qui n'est associée à aucun module et qui est présente dans la corbeille.

**ressource archivée autonome**<br>
ressource qui n'est associée à aucun module, qui est présente dans la corbeille et dont le package ne dépend d'aucune autre ressource.

**ressource archivée dépendante**<br>
ressource qui n'est associée à aucun module, qui est présente dans la corbeille et dont le package dépend d'autres ressources.