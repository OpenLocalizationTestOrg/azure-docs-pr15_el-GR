<properties
   pageTitle="Ανάπτυξη ενός κοντέινερ ASP.NET πυρήνα Linux Docker προς έναν απομακρυσμένο υπολογιστή Docker | Microsoft Azure"
   description="Μάθετε πώς μπορείτε να χρησιμοποιήσετε εργαλεία του Visual Studio για Docker για να αναπτύξετε μια εφαρμογή web ASP.NET πυρήνα σε κοντέινερ Docker εκτελείται σε μια Εικονική Linux Azure Docker κεντρικού υπολογιστή"   
   services="azure-container-service"
   documentationCenter=".net"
   authors="mlearned"
   manager="douge"
   editor=""/>

<tags
   ms.service="azure-container-service"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="06/08/2016"
   ms.author="mlearned"/>

# <a name="deploy-an-aspnet-container-to-a-remote-docker-host"></a>Ανάπτυξη ενός κοντέινερ ASP.NET προς έναν απομακρυσμένο υπολογιστή Docker

## <a name="overview"></a>Επισκόπηση
Docker είναι ένας μηχανισμός ελαφριά κοντέινερ, παρόμοια στο μερικοί τρόποι για να μια εικονική μηχανή, το οποίο μπορείτε να χρησιμοποιήσετε για να host εφαρμογών και υπηρεσιών.
Αυτό το πρόγραμμα εκμάθησης σάς καθοδηγεί σε χρησιμοποιώντας την επέκταση [2015 εργαλεία του Visual Studio για Docker](http://aka.ms/DockerToolsForVS) για να αναπτύξετε μια εφαρμογή ASP.NET πυρήνα σε μια υπηρεσία παροχής φιλοξενίας Docker στη χρήση του PowerShell Azure.

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία
Τα παρακάτω είναι απαραίτητη για να ολοκληρώσετε αυτό το πρόγραμμα εκμάθησης:

- Δημιουργήστε μια Εικονική Azure Docker κεντρικού υπολογιστή, όπως περιγράφεται στην [πώς μπορείτε να χρησιμοποιήσετε docker-μηχανής με Azure](./virtual-machines/virtual-machines-linux-docker-machine.md)
- Εγκαταστήστε την [Ενημέρωση 2015 Visual Studio 3](https://go.microsoft.com/fwlink/?LinkId=691129)
- [SDK του Microsoft ASP.NET πυρήνα 1.0](https://go.microsoft.com/fwlink/?LinkID=809122)
- Εγκατάσταση του [2015 εργαλεία του Visual Studio για Docker - Preview](http://aka.ms/DockerToolsForVS)

## <a name="1-create-an-aspnet-core-web-app"></a>1. Δημιουργία μιας εφαρμογής web ASP.NET πυρήνα
Ακολουθήστε τα παρακάτω βήματα θα σας καθοδηγήσει δημιουργώντας μια βασική εφαρμογή ASP.NET πυρήνα που θα χρησιμοποιηθεί σε αυτό το πρόγραμμα εκμάθησης.

[AZURE.INCLUDE [create-aspnet5-app](../includes/create-aspnet5-app.md)]

## <a name="2-add-docker-support"></a>2. Προσθέστε Docker υποστήριξης

[AZURE.INCLUDE [create-aspnet5-app](../includes/vs-azure-tools-docker-add-docker-support.md)]

## <a name="3-use-the-dockertaskps1-powershell-script"></a>3. Χρησιμοποιήστε τη δέσμη ενεργειών PowerShell DockerTask.ps1 

1.  Ανοίξτε μια γραμμή εντολών του PowerShell στον ριζικό κατάλογο του έργου σας. 

    ```
    PS C:\Src\WebApplication1>
    ```

1.  Επικυρώστε ο απομακρυσμένος κεντρικός υπολογιστής λειτουργεί. Θα πρέπει να δείτε την κατάσταση = εκτελείται 

    ```
    docker-machine ls
    NAME         ACTIVE   DRIVER   STATE     URL                        SWARM   DOCKER    ERRORS
    MyDockerHost -        azure    Running   tcp://xxx.xxx.xxx.xxx:2376         v1.10.3
    ```

    > [AZURE.NOTE] Εάν χρησιμοποιείτε την έκδοση Beta του Docker, ο κεντρικός υπολογιστής σας δεν θα εμφανίζεται εδώ.

1.  Δημιουργήστε την εφαρμογή χρησιμοποιώντας - Δημιουργία παραμέτρου

    ```
    PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Build -Environment Release -Machine mydockerhost
    ```  

    > [AZURE.NOTE] Εάν χρησιμοποιείτε την έκδοση Beta του Docker, παραλείψτε - μηχανικής όρισμα
    > 
    > ```
    > PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Build -Environment Release 
    > ```  


1.  Εκτελέστε την εφαρμογή, χρησιμοποιώντας την εκτέλεση παράμετρο -

    ```
    PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Run -Environment Release -Machine mydockerhost
    ```

    > [AZURE.NOTE] Εάν χρησιμοποιείτε την έκδοση Beta του Docker, παραλείψτε - μηχανικής όρισμα
    > 
    > ```
    > PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Run -Environment Release 
    > ```

    Μόλις ολοκληρωθεί η docker, θα πρέπει να βλέπετε αποτελέσματα παρόμοια με τα εξής:

    ![Προβολή της εφαρμογής σας][3]

[0]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/docker-props-in-solution-explorer.png
[1]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/change-docker-machine-name.png
[2]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/launch-application.png
[3]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/view-application.png
