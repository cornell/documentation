---
Sort: 6
---
## Privilèges

"Vsix/Administrateur": Visibilité du menu de purge

Note: ce privilège permet l'accès à la page de Purge (ainsi qu'à toutes les actions disponibles sur celle-ci) mais définit aussi la liste de diffusion de la [notification](./notification.md) d'utilisation d'espace disque dépassé.
En effet, ce sont ceux qui ont l'accès à la fonctionnalité qui seront les destinataires de la notification.


```java
//Find all validators from privilege
return Container.Get<IUserService>().FindByPrivilege("Admin.Purge.View");			
```