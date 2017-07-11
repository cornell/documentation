---
Sort: 6
---
## Story 01 [[18015]](https://redmine.condate.com/issues/18015)

*En tant qu*'administrateur,<br>
*Je veux* visualiser l'espace de stockage<br>
*Afin de* connaître la place restante<br>

### Scénarios

*Soit* un espace disque alloué de 100GB
*Et* un espace disque utilisé de 60GB
*Alors* l'espace disque restant est de 40GB

*Soit* un espace disque alloué de 50GB
*Et* un seuil égal à 80%
*Alors* la valeur du seuil d'alerte est de 40GB

*Soit* un espace disque alloué de 50GB
*Et* un seuil égal à 40GB
*Alors* la valeur du seuil d'alerte est de 40GB

*Soit* l'espace disque du paramètre de configuration qui est vide
*Quand* l'espace disque utilisé est de 10GB
*Alors* l'espace disque alloué est égal à 13GB (130% de l'espace disque utilisé)

*Soit* un espace disque alloué de 50GB
*Et* un seuil égal à 0.3GB
*Alors* la valeur du seuil d'alerte est de 0.3GB


## Story 02 [[18017]](https://redmine.condate.com/issues/18017) [[18341]](https://redmine.condate.com/issues/18341)

*En tant qu*'administrateur,<br>
*Je veux* visualiser l'occupation de l'espace disque par types de répertoires<br>
*Afin d*'identifier les éléments les plus volumineux<br>

### Scénarios

L'élément "Packages" contient la somme de tous les sous répertoires de "Packages"
L'élément "Documents" contient la somme des répertoires "Documents", "Pictures" et "EditorResources".
L'élément "Logs" contient la somme des répertoires "Logs" et "Traces".


## Story 03 [[18208]](https://redmine.condate.com/issues/18208)

*En tant qu*'administrateur,<br>
*Je veux* spécifier le quota d'espace disque vendu grâce à la licence<br>
*Afin de* contractualiser l'espace disque vendu<br>

### Scénarios

*Soit* une application sans licence<br>
*Quand* je récupère l'espace disque alloué<br>
*Alors* cet espace alloué est de 50GB par défaut<br>

*Soit* une application sans licence<br>
*Quand* je récupère les paramètres de configuration<br>
*Alors* le seuil est défini par défaut à 80%<br>

*Soit* une application avec une licence à 60GB<br>
*Et* un paramètre d'espace disque alloué défini à 50GB
*Quand* je récupère les paramètres de configuration
*Alors* l'espace disque alloué est de 60GB

## Technique

NOTE:
Si les droits d'accès sur certains répertoires sont refusés alors la taille du répertoire concerné renverra 0.

```java
// espace de noms
Syfadis.Supervision.Purge.Quotas

// service
ApplicationDiskUsage PurgeService.GetDiskSpaceUsageByApplication();

// objets
DiskSpaceUsage -> PurgeConfiguration
```