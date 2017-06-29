# Quotas

## Story 01 [[18015]](https://redmine.condate.com/issues/18015)

*En tant qu*'administrateur,<br>
*Je veux* visualiser l'espace de stockage<br>
*Afin de* connaître la place restante<br>



## Story 02 [[18017]](https://redmine.condate.com/issues/18017) [[18341]](https://redmine.condate.com/issues/18341)

*En tant qu*'administrateur,<br>
*Je veux* visualiser l'occupation de l'espace disque par types de répertoires<br>
*Afin d*'identifier les éléments les plus volumineux<br>

### Scénarios


### Technique

```java
// espace de noms
Syfadis.Supervision.Purge.Quotas

// service
ApplicationDiskUsage PurgeService.GetDiskSpaceUsageByApplication();

// objets
DiskSpaceUsage -> PurgeConfiguration
```

## Story 03 [[18208]](https://redmine.condate.com/issues/18208)

*En tant qu*'administrateur,<br>
*Je veux* spécifier le quota d'espace disque vendu grâce à la licence<br>
*Afin de* contractualiser l'space disque vendu<br>

## Scénarios

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

