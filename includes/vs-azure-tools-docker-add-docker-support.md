1. Στο Visual Studio **Εξερεύνηση λύσεων**, κάντε δεξί κλικ στο έργο και επιλέξτε **Προσθήκη > υποστήριξη Docker** από το μενού περιβάλλοντος.

    ![Προσθήκη μενού περιβάλλοντος Docker υποστήριξης](media/vs-azure-tools-docker-add-docker-support/docker-support-context-menu.png)

1. Προσθήκη Docker υποστήριξης για ένα βασικό ASP.NET web project ως αποτέλεσμα την προσθήκη διάφορα αρχεία Docker που σχετίζονται με την προσθήκη του έργου, συμπεριλαμβανομένων σύνθεση Docker αρχεία, δέσμες ενεργειών ανάπτυξης του Windows PowerShell και Docker ιδιότητα αρχεία. 

    ![Αρχεία docker που θα προστεθούν στο project](media/vs-azure-tools-docker-add-docker-support/docker-files-added.png)
    
> [AZURE.NOTE]Εάν χρησιμοποιείτε το [Docker για την έκδοση Beta του Windows](https://beta.docker.com), ανοίξτε Properties\Docker.props, καταργήστε την προεπιλεγμένη τιμή και επανεκκινήστε το Visual Studio για την τιμή για να τεθούν σε ισχύ.
> 
> ```
> <DockerMachineName Condition="'$(DockerMachineName)'=='' "></DockerMachineName>
> ```
