---
Sort: 7
---

Catégorie Générales:<br>
"Mail envoyé quand le seuil d'utilisation d'espace disque est dépassé"

## Stories [[18016]](https://redmine.condate.com/issues/18016)[[18397]](https://redmine.condate.com/issues/18397)

**En tant que** administrateur,<br>
**Je veux** être notifié par mail si mon espace de stockage atteint un certain seuil<br>
**Afin de** libérer l'espace disque inutilisé<br>

### Scénarios

**Soit** l'email envoyé <br>
**Quand** L'Admin ouvre son email<br>
**Alors** il est informé du dépassement<br>
**Et** il peut accéder à la page de purge depuis le mail<br>

**Soit** la notification activée<br>
**Et** un espace disque alloué de 50GB<br>
**Et** un seuil d'utilisation d'espace disque défini à '10%'<br>
**Et** 2 membres ayant le privilège 'Visibilité du menu Purge' activé<br>
**Et** une utilisation d'espace disque égale à 40GB<br>
**Quand** le batch s'exécute<br>
**Alors** les 2 personnes reçoivent le mail

### Technique

```java
// espace de noms
Syfadis.Supervision.Purge

// service
IPurgeNotificationService

```