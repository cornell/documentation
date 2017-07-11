---
Sort: 3
---

```
    "Vsix/Administrateur": Visibilité du menu de purge
```

Ce privilège permet:
* l'accès à la page de Purge ainsi qu'à toutes les actions associées à cette page.<br>
* de définir les personnes qui seront [notifiées lors du dépassement du quota d'espace disque](./notification.md).

## Technique

```java
//Find all validators from privilege
return Container.Get<IUserService>().FindByPrivilege("Admin.Purge.View");			
```