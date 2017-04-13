<properties
   pageTitle="Ρύθμιση παραμέτρων σε μια υπηρεσία παροχής φιλοξενίας Docker με VirtualBox | Microsoft Azure"
   description="Οδηγίες βήμα προς βήμα για να ρυθμίσετε τις παραμέτρους μιας προεπιλεγμένης παρουσίας Docker με χρήση του υπολογιστή Docker και VirtualBox"
   services="azure-container-service"
   documentationCenter="na"
   authors="mlearned"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="06/08/2016"
   ms.author="mlearned" />

# <a name="configure-a-docker-host-with-virtualbox"></a>Ρύθμιση παραμέτρων σε μια υπηρεσία παροχής φιλοξενίας Docker με VirtualBox

## <a name="overview"></a>Επισκόπηση
Σε αυτό το άρθρο σάς καθοδηγεί τη ρύθμιση των παραμέτρων μια προεπιλεγμένη παρουσία Docker με χρήση του υπολογιστή Docker και VirtualBox. Εάν χρησιμοποιείτε την έκδοση [beta Docker για Windows](http://beta.docker.com/), αυτή η ρύθμιση παραμέτρων δεν είναι απαραίτητο.

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία
Τα ακόλουθα εργαλεία πρέπει να εγκατασταθούν.

- [Εργαλειοθήκη docker](https://www.docker.com/products/overview#/docker_toolbox)

## <a name="configuring-the-docker-client-with-windows-powershell"></a>Ρύθμιση παραμέτρων του προγράμματος-πελάτη Docker με το Windows PowerShell

Για να ρυθμίσετε ένα πρόγραμμα-πελάτη Docker, απλώς ανοίξτε του Windows PowerShell και ακολουθήστε τα παρακάτω βήματα:

1. Δημιουργία μιας παρουσίας προεπιλεγμένη docker κεντρικού υπολογιστή.

    ```PowerShell
    docker-machine create --driver virtualbox default
    ```
 
1. Επαληθεύστε την προεπιλεγμένη εμφάνιση είναι έχει ρυθμιστεί και λειτουργεί. (Θα πρέπει να βλέπετε μια παρουσία με το όνομα "Προεπιλογή" εκτελείται.

    ```PowerShell
    docker-machine ls 
    ```
        
    ![docker-μηχανικής ls εξόδου][0]
 
1. Ορισμός προεπιλεγμένης ως τον κεντρικό υπολογιστή του τρέχοντος και ρυθμίσετε τις παραμέτρους του κελύφους.

    ```PowerShell
    docker-machine env default | Invoke-Expression
    ```

1. Εμφάνιση του ενεργού κοντέινερ Docker. Η λίστα θα πρέπει να είναι κενή.

    ```PowerShell
    docker ps
    ```

    ![docker ps εξόδου][1]
 
> [AZURE.NOTE]Κάθε φορά που κάνετε επανεκκίνηση του υπολογιστή ανάπτυξης, θα πρέπει να επανεκκινήσετε τον τοπικό docker κεντρικού υπολογιστή.
> Για να το κάνετε αυτό, εισαγάγετε την ακόλουθη εντολή στη γραμμή εντολών: `docker-machine start default`.

[0]: ./media/vs-azure-tools-docker-setup/docker-machine-ls.png
[1]: ./media/vs-azure-tools-docker-setup/docker-ps.png
