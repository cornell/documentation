---
Sort: 1
---
Lors de l'utilisation de la plateforme, nous permettons aux clients d'ajouter des fichiers (image, vidéo, pdf, excel, etc...).

Avec le temps, ses fichiers prennent une place importante (plusieurs Go). La fonctionnalité de purge a pour but de libérer cet espace disque.

Pour cela, nous allons identifier les éléments rattachés à des fichiers qui ne sont plus utilisés.

NOTE:<br>
*Tous ses fichiers sont stockés dans [des répertoires spécifiques](./stockage-des-fichiers.md).*

## Les éléments à purger

La purge s'effectue sur les packages associées à des ressources.

### Les ressources

Les ressources concernées sont les types suivants:
* note          (Note)
* cas pratique  (PracticalCase)
* vidéo         (Video)
* scorm         (Content)
* contenu       (CourseDocument)
* quiz          (Quiz)

NOTE:
La resource "quiz" contient des questions qui ne sont pas purgées pour l'instant.

Les ressources suivantes ne nécessitent pas de purge car elles ne stockent pas de fichier:
* module                    (Module)
* ressource externe         (ExternalResource)
* ressource communautaire   (CommunityResource)

les ressources suivantes ne sont pas prise en compte pour l'instant:
* bibliothèque multimédia   (MediaLibrary)
* aicc                      (AICCResource)
* xapi                      (XApiResource)
* quiz epsi                 (QuizEpsiResource)
