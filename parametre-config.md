---
Sort: 7
---
# Paramètres de configuration

## Menu "Administrateur/Purge et quotas"

* "Seuil de tolérance d'utilisation d'espace disque en % ou en GB. Ajoutez l'unité à la fin du paramètre. Ex: '150GB' ou '80%'."<br>
valeur par défaut: 80%

    ```java
    PurgeConfiguration.THRESHOLD = "Syfadis.Supervision.Parameters.DiskSpaceThreshold";
    ```

* "Espace disque alloué pour l'application en GB."<br>
valeur par défaut: aucune, c'est une clef de licence
    
    ```java
    PurgeConfiguration.ALLOCATED_SPACE_FROM_LICENCE = "Supervisor.AllocatedDiskSpace.Go";
    ```

* "Espace disque alloué pour l'application en GB. Le paramètre de licence du même nom est prioritaire à ce paramètre."<br>
valeur par défaut: 50
    
    ```java
    PurgeConfiguration.ALLOCATED_SPACE_FROM_CONFIG = "Syfadis.Supervision.Parameters.AllocatedDiskSpace.GB";
    ```

* "Activation de la tâche d'analyse de purge lors du traitement par lots"<br>
valeur par défaut: Vrai

    ```java
        PurgeConfiguration.TASK_ACTIVATION_CONFIG_KEY = "Syfadis.Supervision.Parameters.EnablePurgeAnalysis";
    ```
	
* "Définit si la purge déplacera les éléments purgés dans un répertoire corbeille au lieu de les supprimer."<br>
valeur par défaut: Vrai

    ```java
        private const string MOVE_TO_TRASH_CONFIG = "Syfadis.Supervision.Parameters.PurgeToTrash";
    ```