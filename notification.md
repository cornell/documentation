---
Sort: 6
---
## Stories 
[[18016]](https://redmine.condate.com/issues/18016)
[[18397]](https://redmine.condate.com/issues/18397)

**En tant que** administrateur,<br>
**Je veux** être notifié par mail si mon espace de stockage atteint un certain seuil<br>
**Afin de** libérer l'espace disque inutilisé<br>


**Etant donné que** le paramètre de configuration du seuil d'alerte a été saisi<br>
**Quand** l'espace de stockage dépasse ce seuil<br>
**Alors** l'Admin reçoit un email<br>

**Etant donné que** l'email a été envoyé <br>
**Quand** L'Admin ouvre son email<br>
**Alors** il est informé du dépassement<br>
**Et** il peut accéder à la page de purge<br>

### Scénarios

**Soit** un seuil d'utilisation d'espace disque définit à '10%' <br>
**Et** une utilisation de 50% <br>
**Quand** traitement par lot de données tourne <br>
**Alors** les utilisateurs qui ont le privilège de Purge recoivent un mail<br>

**Soit** un seuil d'utilisation d'espace disque définit à '80%' <br>
**Et** une utilisation de 20GB sur 50GB <br>
**Quand** traitement par lot de données tourne <br>
**Alors** les utilisateurs qui ont le privilège de Purge ne recoivent pas de mail<br>


### Technique

```java
// espace de noms
Syfadis.Supervision.Purge.Notifications

// service
PurgeNotificationServiceImpl.WakeUp()

```