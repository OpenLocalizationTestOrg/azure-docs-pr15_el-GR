<properties
   pageTitle="Αντιμετώπιση σφαλμάτων προγράμματος-πελάτη Docker στα Windows με χρήση του Visual Studio | Microsoft Azure"
   description="Αντιμετώπιση προβλημάτων που αντιμετωπίζετε κατά τη χρήση του Visual Studio για τη δημιουργία και ανάπτυξη εφαρμογών web με χρήση του Visual Studio Docker στα Windows."
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
   ms.author="allclark" />

# <a name="troubleshooting-visual-studio-docker-development"></a>Αντιμετώπιση προβλημάτων του Visual Studio Docker ανάπτυξης

Όταν εργάζεστε με εργαλεία του Visual Studio για Docker Preview, που μπορεί να αντιμετωπίσετε κάποια προβλήματα λόγω της φύσης preview.
Ακολουθούν ορισμένα κοινά προβλήματα και λύσεις.


## <a name="unable-to-validate-volume-mapping"></a>Δεν είναι δυνατή η επικύρωση αντιστοίχισης έντασης ήχου
Αντιστοίχιση ένταση απαιτείται για την κοινή χρήση του πηγαίου κώδικα και δυαδικά δεδομένα της εφαρμογής σας με το φάκελο εφαρμογής στο κοντέινερ.  Αντιστοιχίσεις συγκεκριμένου τόμου περιέχονται τα αρχεία docker compose.dev.debug.yml και docker compose.dev.release.yml. Καθώς αλλάζουν τα αρχεία στον υπολογιστή σας κεντρικού υπολογιστή, το κοντέινερ αντανακλούν αυτών των αλλαγών σε μια παρόμοια δομή φακέλων.

Για να ενεργοποιήσετε την ένταση αντιστοίχισης, ανοίξτε τις **ρυθμίσεις...** από το εικονίδιο "moby" Docker για Windows και, στη συνέχεια, επιλέξτε την καρτέλα **Κατευθύνει την κοινή χρήση** .  Βεβαιωθείτε ότι το γράμμα που φιλοξενεί το έργο σας, καθώς και το γράμμα της μονάδας δίσκου όπου βρίσκεται το % USERPROFILE % χρησιμοποιούνται από κοινού από τον έλεγχο τους και, στη συνέχεια, κάνοντας κλικ στην επιλογή **εφαρμογή**.

Για να ελέγξετε εάν λειτουργεί αντιστοίχισης ένταση, αφού έχουν τεθεί σε κοινή χρήση τις μονάδες δίσκων, αναδόμηση και F5 από μέσα από το Visual Studio ή δοκιμάστε τα εξής από μια εντολή ερώτηση:

*Σε μια γραμμή εντολών των Windows*

*[Σημείωση: αυτή προϋποθέτει φάκελο χρήστες βρίσκεται στη μονάδα δίσκου "C" και ότι το έχει τεθεί σε κοινή χρήση.  Ενημέρωση ανάλογα με τις ανάγκες, εάν έχετε κάνει κοινή χρήση μια διαφορετική μονάδα δίσκου]*
```
docker run -it -v /c/Users/Public:/wormhole busybox
```

*Στο κοντέινερ Linux*

```
/ # ls
```

Θα πρέπει να δείτε μια λίστα από το φάκελο χρήστες/δημόσια του καταλόγου.
Εάν δεν εμφανίζονται αρχεία και ο φάκελος /c/Users/Public δεν είναι κενό, ένταση αντιστοίχισης δεν έχει ρυθμιστεί σωστά. 

```
bin       etc       proc      sys       usr       wormhole
dev       home      root      tmp       var
```

Αλλαγή στον κατάλογο σκουληκότρυπα για να δείτε τα περιεχόμενα του `/c/Users/Public` καταλόγου:

```
/ # cd wormhole/
/wormhole # ls
AccountPictures  Downloads        Music            Videos
Desktop          Host             NuGet.Config     desktop.ini
Documents        Libraries        Pictures
/wormhole #
```

**Σημείωση:** *Όταν εργάζεστε με Linux ΣΠΣ, το σύστημα αρχείων κοντέινερ είναι διάκριση πεζών-κεφαλαίων.*

##<a name="build--prepareforbuild-task-failed-unexpectedly"></a>Δόμηση: "PrepareForBuild" αποτυχία της εργασίας.

Microsoft.DotNet.Docker.CommandLine.ClientException: Παρουσιάστηκε σφάλμα όταν προσπαθείτε να συνδεθείτε:

Επιβεβαιώστε τον προεπιλεγμένο κεντρικό υπολογιστή docker εκτελείται. Ανοίξτε μια γραμμή εντολών και εκτέλεση:

```
docker info
```

Εάν αυτή η συνάρτηση επιστρέφει ένα σφάλμα επιχειρήσει, στη συνέχεια, για να ξεκινήσετε την εφαρμογή υπολογιστή **Docker για Windows** .  Εάν η εφαρμογή υπολογιστή εκτελείται, στη συνέχεια, το εικονίδιο **moby** στη εργασιών θα πρέπει να είναι ορατό. Κάντε δεξί κλικ στο εικονίδιο εργασιών και ανοίξτε τις **Ρυθμίσεις**.  Κάντε κλικ στην καρτέλα **Επαναφορά** και, στη συνέχεια, **επανεκκίνηση Docker..**.

##<a name="manually-upgrading-from-version-031-to-040"></a>Με μη αυτόματο τρόπο την αναβάθμιση από την έκδοση 0.31 να 0.40


1. Δημιουργία αντιγράφων ασφαλείας του έργου
1. Διαγράψτε τα ακόλουθα αρχεία στο έργο:

    ```
      Dockerfile
      Dockerfile.debug
      DockerTask.ps1
      docker-compose-yml
      docker-compose.debug.yml
      .dockerignore
      Properties\Docker.props
      Properties\Docker.targets
    ```

1. Κλείστε τη λύση και καταργήστε τις ακόλουθες γραμμές από το αρχείο .xproj:

    ```
      <DockerToolsMinVersion>0.xx</DockerToolsMinVersion>
      <Import Project="Properties\Docker.props" />
      <Import Project="Properties\Docker.targets" />
    ```

1. Ανοίξτε ξανά τη λύση
1. Καταργήστε τις ακόλουθες γραμμές από το αρχείο Properties\launchSettings.json:

    ```
      "Docker": {
        "executablePath": "%WINDIR%\\System32\\WindowsPowerShell\\v1.0\\powershell.exe",
        "commandLineArgs": "-ExecutionPolicy RemoteSigned .\\DockerTask.ps1 -Run -Environment $(Configuration) -Machine '$(DockerMachineName)'"
      }
    ```

1. Καταργήστε τα ακόλουθα αρχεία που σχετίζονται με Docker από project.json στο το publishOptions:

    ```
    "publishOptions": {
      "include": [
        ...
        "docker-compose.yml",
        "docker-compose.debug.yml",
        "Dockerfile.debug",
        "Dockerfile",
        ".dockerignore"
      ]
    },
    ```

1. Κατάργηση της εγκατάστασης της προηγούμενης έκδοσης και εγκαταστήστε εργαλεία Docker 0.40 και, στη συνέχεια, **Προσθήκη -> υποστήριξη Docker** ξανά από το μενού περιβάλλοντος για το Web ASP.Net πυρήνα ή εφαρμογής κονσόλας. Αυτό θα προσθέσει το νέο απαιτείται αντικείμενα Docker πίσω στο έργο σας. 

## <a name="an-error-dialog-occurs-when-attempting-to-add-docker-support-or-debug-f5-an-aspnet-core-application-in-a-container"></a>Ένα παράθυρο διαλόγου σφάλματος προκύπτει κατά την προσπάθεια για **Προσθήκη -> υποστήριξη Docker** ή σφαλμάτων (F5) σε μια εφαρμογή του βασικού ASP.NET σε ένα κοντέινερ

Θα σας έχουν κάποιες φορές εμφανίζεται μετά την κατάργηση της εγκατάστασης και την εγκατάσταση επεκτάσεις, cache του Visual Studio MEF (διαχειριζόμενων υποδομή επεκτασιμότητα του) μπορεί να έχει καταστραφεί. Όταν συμβεί αυτό, μπορεί να προκαλέσει διάφορες σφάλματος παράθυρα διαλόγου κατά την προσθήκη Docker υποστήριξης ή/και επιχειρήσετε να εκτελέσετε ή να σφαλμάτων (F5) την εφαρμογή του βασικού ASP.NET. Ως λύση προσωρινά, εκτελέστε τα ακόλουθα βήματα για να διαγράψετε και να αναδημιουργήσετε το cache MEF.

1. Κλείστε όλες τις εμφανίσεις του Visual Studio
1. Άνοιγμα %USERPROFILE%\AppData\Local\Microsoft\VisualStudio\14.0\
1. Διαγράψτε τους παρακάτω φακέλους
     ```
       ComponentModelCache
       Extensions
       MEFCacheBackup
    ```
1. Άνοιγμα Visual Studio
1. Επανάληψη προσπάθειας το σενάριο 
