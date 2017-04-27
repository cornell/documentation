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

La purge s'effectue sur plusieurs éléments différents:
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
*Je souhaite* visualiser les **ressources archivées autonomes** liées à des fichiers<br>
*Afin de* les supprimer pour libérer de l'espace disque<br>

NB: *Une ressource archivée est une ressource qui n'est associée à aucun module.*

NB: *Une ressource archivée autonome est une ressource qui n'est associée à aucune autre ressource. Dans le cas d'une ressource multi-sco, un même répertoire de stockage des fichiers est partagé par plusieurs ressources.*

### Scénarios

#### Vérifier que la ressource est toujours archivée avant la suppression

*Etant* donné une ressource identifiée comme archivée<br>
*Quand* je souhaite la supprimer<br>
*Et* que la ressource est toujours dans la corbeille<br>
*Alors* le répertoire associé à la ressource est supprimée<br>
*Et* la ressource n'est plus visible dans la corbeille<br>

*Etant* donné une ressource identifiée comme archivée<br>
*Quand* je souhaite la supprimer<br>
*Et* que la ressource n'est plus dans la corbeille<br>
*Alors* on m'informe que je ne peux plus la supprimer<br>

#### Vérifier les cas d'apparitions d'un élément multi-sco dans la liste des ressources archivées

*Etant* donné une ressource identifiée comme archivée et multi-sco<br>
*Et* que cette ressource partage ses fichiers avec d'autres ressources archivées ou orphelines<br>
*Quand* je visualise la liste des ressources archivées<br>
*Alors* cette ressource est visible dans la liste<br>

*Etant* donné une ressource identifiée comme archivée et multi-sco<br>
*Et* que cette ressource partage ses fichiers avec d'autres ressources en cours d'utilisations<br>
*Quand* je visualise la liste des ressources archivées<br>
*Alors* cette ressource n'est pas visible dans la liste<br>

### Eléments techniques

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

## User story 2

*En tant qu'*administrateur<br>
*Je souhaite* visualiser les **ressources orphelines autonome** liées à des fichiers<br>
*Afin de* les supprimer pour libérer de l'espace disque<br>

NB: *Une ressource orpheline est une ressource qui est visible dans la bibliothèque des ressources mais qui n'est associée à aucun module.*

### Scénarios

#### Vérifier que la ressource est toujours orpheline avant la suppression

*Etant* donné une ressource identifiée comme orpheline<br>
*Quand* je souhaite la supprimer<br>
*Et* que la ressource est toujours inutilisée<br>
*Alors* le répertoire associé à la ressource est supprimée<br>
*Et* la ressource n'est plus visible dans la bibliothèque des ressources<br>

*Etant* donné une ressource identifiée comme orpheline<br>
*Quand* je souhaite la supprimer<br>
*Et* que la ressource est maintenant utilisé<br>
*Alors* on m'informe que je ne peux plus la supprimer<br>

#### Vérifier les cas d'apparitions d'un élément multi-sco dans la liste des ressources orphelines

*Etant* donné une ressource identifiée comme orpheline et multi-sco<br>
*Et* que cette ressource partage ses fichiers avec d'autres ressources archivées ou orphelines<br>
*Quand* je visualise la liste des ressources orphelines<br>
*Alors* cette ressource est visible dans la liste<br>

*Etant* donné une ressource identifiée comme orpheline et multi-sco<br>
*Et* que cette ressource partage ses fichiers avec d'autres ressources en cours d'utilisations<br>
*Quand* je visualise la liste des ressources orphelines<br>
*Alors* cette ressource n'est pas visible dans la liste<br>

#### Eléments techniques

```sql
-- éléments associés à une ressource hors module (onglets éléments associés)
SELECT *
FROM   stu_resource RES,
       stu_coursecomponent CC
WHERE  RES.reso_subclass = 'Module'
       AND CC.cour_subclass IN ('ItemResource', 'ItemQuestion', 'ItemPresential', 'ItemRegistration','ItemVirtualClassroom', 'ItemUserAuthentication')
       AND CC.cour_resourceid = 786436
       AND ( CC.cour_deletedon IS NULL )
       AND RES.reso_courseid = CC.cour_courseid
       AND ( RES.reso_deletedon IS NULL )  
```

## User story 3

En tant qu'administrateur<br>
Je souhaite visualiser les **modules archivés** dont toutes les ressources sont archivées ou orphelines<br>
Afin de les supprimer pour libérer de l'espace disque<br>

NB: *Un module archivé est un module qui n'est associée à aucune formation et qui est présent dans la corbeille.*

### Scénarios

#### Vérifier que le module est toujours archivé et que ses ressources sont archivées ou orphelines avant la suppression

*Etant* donné une module identifié comme archivé<br>
*Quand* je souhaite le supprimer<br>
*Et* que le module est toujours archivé<br>
*Et* que les ressources associées sont toujours archivées ou orphelines<br>
*Alors* les répertoires associés aux ressources sont supprimées<br>
*Et* le module n'est plus présent dans la corbeille<br>
*Et* les resource associées ne sont plus visibles dans la bibliothèque des ressources<br>

*Etant* donné une module identifié comme archivé<br>
*Quand* je souhaite le supprimer<br>
*Et* que le module n'est plus archivé<br>
*Alors* on m'informe que je ne peux plus supprimer le module<br>

*Etant* donné une module identifié comme archivé<br>
*Quand* je souhaite le supprimer<br>
*Et* que le module est toujours archivé<br>
*Et* que les ressources associées ne sont plus toutes archivées ou orphelines<br>
*Alors* on m'informe que je ne peux plus supprimer le module<br>

## User story 4

En tant qu'administrateur<br>
Je souhaite visualiser les **modules orphelins** dont toutes les ressources sont archivées ou orphelines<br>
Afin de les supprimer pour libérer de l'espace disque<br>

NB: *Un module orphelin est un module qui n'est associée à aucune formation et qui est présent dans la bibliothèque des modules.*

### Scénarios

#### Vérifier que le module est toujours orphelin et que ses ressources sont archivées ou orphelines avant la suppression

*Etant* donné une module identifié comme orphelin<br>
*Quand* je souhaite le supprimer<br>
*Et* que le module est toujours orphelin<br>
*Et* que les ressources associées sont toujours archivées ou orphelines<br>
*Alors* les répertoires associés aux ressources sont supprimées<br>
*Et* le module n'est plus visible dans la bibliothèque des modules<br>
*Et* les resource associées ne sont plus visibles dans la bibliothèque des ressources<br>

*Etant* donné une module identifié comme orphelin<br>
*Quand* je souhaite le supprimer<br>
*Et* que le module n'est plus orphelin<br>
*Alors* on m'informe que je ne peux plus supprimer le module<br>

*Etant* donné une module identifié comme orphelin<br>
*Quand* je souhaite le supprimer<br>
*Et* que le module est toujours orphelin<br>
*Et* que les ressources associées ne sont plus toutes archivées ou orphelines<br>
*Alors* on m'informe que je ne peux plus supprimer le module<br>

## User story 5

En tant qu'administrateur<br>
Je souhaite visualiser les **sessions archivées** dont toutes les ressources sont archivées ou orphelines<br>
Afin de les supprimer pour libérer de l'espace disque<br>

NB: une ressource en production est partagée entre sessions quelque soit la formation

#### Vérifier que les ressources rattachées à la session sont archivées ou orphelines avant la suppression

*Etant* donné une session archivée* dont toutes les ressources sont identifiées comme archivées ou orphelines<br>
*Quand* je souhaite la supprimer<br>
*Et* que la session est toujours archivée<br>
*Et* que les ressources associées sont toujours archivées ou orphelines<br>
*Alors* les répertoires associés aux ressources sont supprimées<br>
*Et* la session n'est plus visible dans la corbeille<br>

*Etant* donné une session archivée dont toutes les ressources sont identifiées comme archivées ou orphelines<br>
*Quand* je souhaite la supprimer<br>
*Et* que la session n'est plus archivée<br>
*Alors* on m'informe que je ne peux plus supprimer la session<br>

*Etant* donné une session archivée dont toutes les ressources sont identifiées comme archivées ou orphelines<br>
*Quand* je souhaite la supprimer<br>
*Et* que la session est toujours archivée<br>
*Et* que les ressources associées ne sont plus toutes archivées ou orphelines<br>
*Alors* on m'informe que je ne peux plus supprimer la session<br>

## User story 6

En tant qu'administrateur<br>
Je souhaite visualiser les **sessions cloturées**<br>
Afin de les supprimer pour libérer de l'espace disque<br>

NB: UNe session clôturée ne peut pas être archivée.

#### Vérifier que les ressources rattachées à la session sont archivées ou orphelines avant la suppression

*Etant* donné une session clôturée dont toutes les ressources sont identifiées comme archivées ou orphelines<br>
*Quand* je souhaite la supprimer<br>
*Et* que la session est toujours clôturée<br>
*Et* que les ressources associées sont toujours archivées ou orphelines<br>
*Alors* les répertoires associés aux ressources sont supprimées<br>
*Et* la session n'est plus visible dans la liste des sessions<br>

*Etant* donné une session clôturée dont toutes les ressources sont identifiées comme archivées ou orphelines<br>
*Quand* je souhaite la supprimer<br>
*Et* que la session n'est plus clôturée<br>
*Alors* on m'informe que je ne peux plus supprimer la session<br>

*Etant* donné une session clôturée dont toutes les ressources sont identifiées comme archivées ou orphelines<br>
*Quand* je souhaite la supprimer<br>
*Et* que la session est toujours clôturée<br>
*Et* que les ressources associées ne sont plus toutes archivées ou orphelines<br>
*Alors* on m'informe que je ne peux plus supprimer la session<br>

## User story 7

En tant qu'administrateur<br>
Je souhaite visualiser les **sessions réalisées**<br>
Afin de les clôturer et de les supprimer pour libérer de l'espace disque<br>

NB: UNe session réalisée est une session dont la date de fin est dépassée.

#### Vérifier que les ressources rattachées à la session sont archivées ou orphelines avant la clôture et la suppression

*Etant* donné une session réalisée dont toutes les ressources sont identifiées comme archivées ou orphelines<br>
*Quand* je souhaite la supprimer<br>
*Et* que la session est toujours réalisée<br>
*Et* que les ressources associées sont toujours archivées ou orphelines<br>
*Alors* les répertoires associés aux ressources sont supprimées<br>
*Et* la session n'est plus visible dans la liste des sessions<br>

*Etant* donné une session réalisée dont toutes les ressources sont identifiées comme archivées ou orphelines<br>
*Quand* je souhaite la supprimer<br>
*Et* que la session est repassé en cours de réalisation<br>
*Alors* on m'informe que je ne peux plus supprimer la session<br>

*Etant* donné une session réalisée dont toutes les ressources sont identifiées comme archivées ou orphelines<br>
*Quand* je souhaite la supprimer<br>
*Et* que la session est toujours réalisée<br>
*Et* que les ressources associées ne sont plus toutes archivées ou orphelines<br>
*Alors* on m'informe que je ne peux plus supprimer la session<br>

## User story 8

NB: Une session supprimable est une session dont les ressources sont archivées, orphelines ou dont les session

En tant qu'administrateur
Je souhaite visualiser les **formations archivées**
Afin de les supprimer pour libérer de l'espace disque

## User story 9

En tant qu'administrateur
Je souhaite visualiser les formations dont les sessions ne sont plus en cours de réalisation
Afin de les supprimer pour libérer de l'espace disque

## User story 10

En tant que système
Je souhaite supprimer les versions des ressources qui ne sont pas utilisées
Afin de libérer de l'espace disque


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

**ressource archivées**<br>
ressource qui n'est associée à aucun module et qui est présente dans la corbeille.