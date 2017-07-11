---
Sort: 10
---
# Recherche

## Stories 
[[18402]](https://redmine.condate.com/issues/18402)
[[18526]](https://redmine.condate.com/issues/18526)
[[18534]](https://redmine.condate.com/issues/18534)
[[18405]](https://redmine.condate.com/issues/18405)
[[18611]](https://redmine.condate.com/issues/18611)
[[18720]](https://redmine.condate.com/issues/18720)

**En tant que** administrateur,<br>
**Je veux** visualiser les packages les plus volumineux qui sont liés à des ressources studio non purgables<br>
**Afin** de pouvoir les archiver si ils ne sont plus utilisés<br>

### Scénarios


**Etant donné** un package qui est lié à une ressource non purgable<br>
**Quand** l'admin. accède à la page de purge<br>
**Alors** il visualise le nom du package<br>
**Et** sa taille<br>
**Et** le nom de la ressource<br>
**Et** un lien vers cette ressource<br>

**Etant donné** un package qui est lié à des ressources non purgables (non archivées)<br>
**Quand** l'admin. accède à la page de purge<br>
**Alors** il visualise le nom du package<br>
**Et** sa taille<br>
**Et** le nom des ressources qui y sont liées <br>
**Et** des liens vers les ressources qui y sont liées (seulement les non archivées)<br>

**Etant donné** plusieurs packages qui sont liés à des ressources non purgables<br>
**Quand** l'admin. accède à la page de purge<br>
**Alors** les packages sont affichés par taille décroissante<br>

**Etant donné** un package qui n'est lié qu'à des resources archivées<br>
**Quand** l'admin. accède à la page de purge<br>
**Alors** il ne visualise pas le package car ce package est purgeable.<br>

**Etant donné** un package qui est lié à une ressource inutilisée <br>
**Quand** l'admin. accède à la page de purge<br>
**Alors** il visualise le package ainsi que la ressource et a directement la possibilité de supprimer la ressource (archiver).<br>

**Etant donné** une liste de packages studio présents sur l'écran<br>
**Quand** le nombre est trop élevé<br>
**Alors** un paging est présent sur l'écran pour faciliter la visualisation des packages studio non purgeables.<br>

**Etant donné** une liste de packages studio présents sur l'écran<br>
**Alors** l'utilisateur a la possibilité de chercher des packages sur différents critères (nom, code et domaine des ressources, modules et formations liées)<br>


### Technique

```java
// espace de noms
Syfadis.WebSite.Features.Admin.Purge.StudioResources.Handler
//Pour les besoins de performance pour la possibilité de recherche dans cette fonctionnalité, l'utilisation d'une requête SQL a été choisie:
 Dao.FindBySQL<object[]>(...)

 
```