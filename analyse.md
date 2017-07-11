---
Sort: 12
---
## Stories <br>
[[18018]](https://redmine.condate.com/issues/18018)<br>

### Scénarios<br>

**En tant qu**'administrateur,<br>
**Je veux** supprimer les _*ressources archivées*_<br>
**Afin de** libérer de l'espace disque<br>

### Critères d'acceptation<br>

**Etant donné** une ressource archivée (présente dans la corbeille)<br>
**Quand** l'admin. clique sur le bouton _*Nettoyer*_<br>
**Alors** de l'espace disque est récupéré<br>
**Et** la ressource n'est plus visible dans la corbeille<br>


[[18401]](https://redmine.condate.com/issues/18401)<br>

### Scénarios<br>

**En tant qu**'administrateur,<br>
**Je veux** supprimer les _*ressources archivées*_<br>
**Afin de** libérer de l'espace disque<br>

### Critères d'acceptation<br>

**Etant donné** une ressource archivée (présente dans la corbeille)<br>
**Quand** l'admin. clique sur le bouton _*Nettoyer*_<br>
**Alors** de l'espace disque est récupéré<br>
**Et** la ressource n'est plus visible dans la corbeille<br>

Les ressources prises en compte sont les ressource de type:<br>
* note<br>
* cas pratique<br>
* séance présentielle<br>
* classe virtuelle<br>
* vidéo<br>


[[18404]](https://redmine.condate.com/issues/18404)<br>

### Scénarios<br>

**En tant qu**'administrateur,<br>
**Je veux** Visualiser les modules qui sont liés à des ressources studio non purgeables*_<br>


[[18019]](https://redmine.condate.com/issues/18019)<br>

### Scénarios<br>

**En tant qu**'administrateur,<br>
**Je veux** supprimer les _*ressources en production inutilisées*_<br>
**Afin de** libérer de l'espace disque<br>

### Spécifications<br>

Identifier ce qu'est une ressource en production inutilisée:<br>
Dans un 1er temps nous ne gérerons pas les ressources de type 'Quiz'<br>

### Critères d'acceptation<br>

**Etant donné** une ressource en _**production inutilisée**_<br>
**Quand** l'admin. clique sur le bouton _**Nettoyer**_<br>
**Alors** de l'espace disque est récupéré<br>

**Etant donné** une ressource en _**production inutilisée**_ appartenant à un seul module nommé "module A"<br>
**Quand** l'admin. clique sur le bouton _*Nettoyer*_<br>
**Alors** de l'espace disque est récupéré<br>
**Et** le module nommé "module A" n'est plus visible dans l'onglet des _modules en production_<br>
[[18203]](https://redmine.condate.com/issues/18203)<br>

### Scénarios<br>

**En tant qu**'administrateur,<br>
**Je veux** accéder à la liste des ressources à supprimer<br>
**Afin de** m'assurer de ce que l'on supprime ou de ce que l'on a supprimé<br>

### Spécifications<br>

La liste des ressources à supprimer doit être accessible avant et après le nettoyage.<br>

### Critères d'acceptation<br>

**Etant donné** une _*ressource archivée*_ (présente dans la corbeille)<br>
**Quand** j'accède à la liste des ressources à supprimer<br>
**Alors** je retrouve cette ressource<br>

**Etant donné** une _*ressource en production inutilisée*_<br>
**Quand** j'accède à la liste des ressources à supprimer<br>
**Alors** je retrouve cette ressource<br>

[[18562]](https://redmine.condate.com/issues/18562) <br>
**refactoring**<br>
[[18563]](https://redmine.condate.com/issues/18563)<br>
**Story Technique**<br>

Sauvegarder en base les données d'analyse des tailles des répertoires pour permettre un accès instantané à la fonctionnalité<br>


[[18653]](https://redmine.condate.com/issues/18653)<br>

### Scénarios<br>

**En tant qu'administrateur,<br>
**Je veux visualiser la quantité de données qui vont être supprimées <br>
**Afin de connaitre l'impact de la purge<br>

### Spécifications<br>
La quantité des ressources à supprimer doit être accessible avant la purge.<br>

### Critères d'acceptation<br>
**Etant donné** un package avec uniquement des ressources archivées<br>
**Quand** j'accède à la page de la purge<br>
**Alors** je visualise une quantité de données prennant en compte la taille du package archivé<br>

**Etant donné** des packages archivés et en ligne inutilisés<br>
**Quand** j'accède à la fonctionnalité de purge<br>
**Alors** je peux définir ce que je veux purger (archivé et/ou online inutilisé) et la prévisualisation de la taille se met à jour si je modifie mon choix.<br>


[[18757]](https://redmine.condate.com/issues/18757)<br>

### Scénarios<br>

**En tant qu**'Admin,<br>
**je souhaite** récupérer les packages en production inutilisées purgées qui n'auraient pas dû l'être<br>
**Afin de** les restaurer<br>

L'action de restaurer un package se fera manuellement.<br>

Soit un package avec le guid '4D59CA76-1CB0-4433-A77C-2ED04C71FAC4' purgé le jour J-1 qui n'aurait pas dû être purgé<br>
Quand j'accède au répertoire '/Files/Logs'<br>
Alors je retrouve le package dans le répertoire 'Files/Logs/Trashed/{date purge}/Applications/<applicationId>/4D59CA76-1CB0-4433-A77C-2ED04C71FAC4<br>



### Technique

```java
// espace de noms


// service

// objets
```