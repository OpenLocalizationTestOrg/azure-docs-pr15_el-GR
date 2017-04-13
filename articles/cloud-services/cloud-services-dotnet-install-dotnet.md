<properties
   pageTitle="Εγκατάσταση του .NET σε ένα ρόλο υπηρεσία Cloud | Microsoft Azure"
   description="Σε αυτό το άρθρο περιγράφει πώς μπορείτε να εγκαταστήσετε με μη αυτόματο τρόπο το .NET framework στο Cloud υπηρεσίας Web και τους ρόλους εργαζόμενου"
   services="cloud-services"
   documentationCenter=".net"
   authors="thraka"
   manager="timlt"
   editor=""/>

<tags
   ms.service="cloud-services"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/10/2016"
   ms.author="adegeo"/>

# <a name="install-net-on-a-cloud-service-role"></a>Εγκατάσταση του .NET σε ένα ρόλο υπηρεσία Cloud 

Σε αυτό το άρθρο περιγράφει πώς μπορείτε να εγκαταστήσετε το .NET framework στο Cloud υπηρεσίας Web και τους ρόλους εργασίας. Μπορείτε να χρησιμοποιήσετε αυτά τα βήματα για να εγκαταστήσετε το .NET 4.6.1 στην το Azure επισκέπτη OS οικογένεια 4. Για τις πιο πρόσφατες πληροφορίες στο λειτουργικό σύστημα επισκέπτη εκδόσεις δείτε [Azure επισκέπτη λειτουργικό σύστημα για ενημερωμένες εκδόσεις και SDK συμβατότητας μήτρα](cloud-services-guestos-update-matrix.md).

Η διαδικασία για την εγκατάσταση του .NET σε ρόλους σας web και εργαζόμενου περιλαμβάνει συμπεριλαμβανομένου του πακέτου προγράμματος εγκατάστασης .NET ως μέρος του έργου σας Cloud και την εκκίνηση του προγράμματος εγκατάστασης ως μέρος των εργασιών εκκίνησης του ρόλου.  

## <a name="add-the-net-installer-to-your-project"></a>Προσθέστε το πρόγραμμα εγκατάστασης του .NET στο έργο σας
- Κάντε λήψη του το πρόγραμμα εγκατάστασης του web για το .NET framework που θέλετε να εγκαταστήσετε
    - [.NET 4.6.1 web Installer](http://go.microsoft.com/fwlink/?LinkId=671729)
- Για ένα ρόλο Web
  1. Στην **Εξερεύνηση λύσεων**, στην περιοχή με **τους ρόλους** στο έργο υπηρεσία cloud κάντε δεξί κλικ σε ρόλο και επιλέξτε **Προσθήκη > Νέος φάκελος**. Δημιουργήστε ένα φάκελο που ονομάζεται *Ανακύκλωσης*
  2. Κάντε δεξιό κλικ στο φάκελο **bin** και επιλέξτε **Προσθήκη > υπάρχον στοιχείο**. Επιλέξτε το πρόγραμμα εγκατάστασης του .NET και προσθέστε το στο φάκελο bin.
- Για ένα ρόλο εργαζόμενου
  1. Κάντε δεξί κλικ σε ρόλο και επιλέξτε **Προσθήκη > υπάρχον στοιχείο**. Επιλέξτε το πρόγραμμα εγκατάστασης του .NET και προσθέστε το με το ρόλο. 

Τα αρχεία προστίθενται με αυτόν τον τρόπο στο φάκελο περιεχομένου ρόλο θα αυτόματα προστεθεί στο πακέτο υπηρεσία cloud και αναπτυχθεί σε σταθερή θέση στον υπολογιστή εικονική. Επαναλάβετε τη διαδικασία για όλους τους ρόλους web και εργαζόμενου στην υπηρεσία Cloud, ώστε όλοι οι ρόλοι έχουν ένα αντίγραφο του προγράμματος εγκατάστασης.

> [AZURE.NOTE] Θα πρέπει να εγκαταστήσετε .NET 4.6.1 σχετικά με το ρόλο υπηρεσία Cloud, ακόμα και εάν η εφαρμογή σας ως προορισμό .NET 4.6. Το λειτουργικό σύστημα επισκέπτη Azure περιλαμβάνει ενημερώσεις [3098779](https://support.microsoft.com/kb/3098779) και [3097997](https://support.microsoft.com/kb/3097997). Κατά την εγκατάσταση του .NET 4.6 επάνω από τις ακόλουθες ενημερώσεις μπορεί να προκαλέσει ζητήματα κατά την εκτέλεση του εφαρμογών .NET, έτσι θα πρέπει να εγκαταστήσετε απευθείας .NET 4.6.1 αντί για το .NET 4.6. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [KB 3118750](https://support.microsoft.com/kb/3118750).

![Περιεχόμενα ρόλων με τα αρχεία προγράμματος εγκατάστασης][1]

## <a name="define-startup-tasks-for-your-roles"></a>Ορισμός εκκίνησης εργασίες για ρόλους σας
Εργασίες εκκίνησης σάς επιτρέπουν να εκτελέσετε λειτουργίες πριν από την εκκίνηση ένα ρόλο. Κατά την εγκατάσταση του .NET Framework ως μέρος της εργασίας εκκίνησης θα βεβαιωθείτε ότι το πλαίσιο έχει εγκατασταθεί πριν από την εκτέλεση οποιαδήποτε από κώδικα της εφαρμογής σας. Για περισσότερες πληροφορίες σχετικά με την εκκίνηση ανατρέξτε στο θέμα εργασίες: [Εκτέλεση εργασιών εκκίνησης στο Azure](cloud-services-startup-tasks.md). 

1. Προσθέστε τα ακόλουθα στο αρχείο *ServiceDefinition.csdef* κάτω από τον κόμβο **WebRole** ή **WorkerRole** για όλους τους ρόλους:
    
    ```xml
    <LocalResources>
      <LocalStorage name="NETFXInstall" sizeInMB="1024" cleanOnRoleRecycle="false" />
    </LocalResources>    
    <Startup>
      <Task commandLine="install.cmd" executionContext="elevated" taskType="simple">
        <Environment>
          <Variable name="PathToNETFXInstall">
            <RoleInstanceValue xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='NETFXInstall']/@path" />
          </Variable>
          <Variable name="ComputeEmulatorRunning">
            <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
          </Variable>
        </Environment>
      </Task>
    </Startup>
    ```

    Τις παραπάνω παραμέτρους θα εκτελείται η εντολή κονσόλας *install.cmd* με δικαιώματα διαχειριστή, ώστε να εγκαταστήσει το .NET framework. Η ρύθμιση παραμέτρων δημιουργεί επίσης ένα LocalStorage με το όνομα *NETFXInstall*. Δέσμη ενεργειών εκκίνησης θα ορίσετε το φάκελο temp για να χρησιμοποιήσετε αυτόν τον πόρο τοπικού χώρου αποθήκευσης, έτσι ώστε το πρόγραμμα εγκατάστασης του .NET framework θα λήψη και εγκατάσταση από αυτόν τον πόρο. Είναι σημαντικό για να ορίσετε το μέγεθος του αυτόν τον πόρο σε τουλάχιστον 1024MB για να βεβαιωθείτε ότι το πλαίσιο θα εγκατασταθεί σωστά. Για περισσότερες πληροφορίες σχετικά με την εκκίνηση εργασίες, δείτε: [εργασίες εκκίνησης κοινές υπηρεσία στο Cloud](cloud-services-startup-tasks-common.md) 

2. Δημιουργήστε ένα αρχείο **install.cmd** και να προσθέσετε σε όλους τους ρόλους με δεξί κλικ στην το ρόλο και επιλέγοντας **Προσθήκη > υπάρχον στοιχείο...**. Επομένως, όλοι οι ρόλοι τώρα θα πρέπει να έχετε το αρχείο εγκατάστασης .NET, καθώς και το αρχείο install.cmd.
    
    ![Περιεχόμενα ρόλων με όλα τα αρχεία][2]

    > [AZURE.NOTE] Χρησιμοποιήστε ένα πρόγραμμα επεξεργασίας απλού κειμένου, όπως το Σημειωματάριο για να δημιουργήσετε αυτό το αρχείο. Εάν χρησιμοποιείτε το Visual Studio για να δημιουργήσετε ένα αρχείο κειμένου και, στη συνέχεια, μετονομάστε την για να. 'cmd.' του αρχείου εξακολουθεί να μπορεί να περιέχει ένα σημάδι UTF-8 Byte σειρά και εκτελείται η πρώτη γραμμή της δέσμης ενεργειών θα έχει ως αποτέλεσμα σφάλμα. Εάν θέλετε να χρησιμοποιήσετε το Visual Studio για να δημιουργήσετε το αρχείο αποχώρηση προσθέσετε ένα REM (παρατηρήσεις) στην πρώτη γραμμή του αρχείου, ώστε να παραβλέπεται κατά την εκτέλεση. 

3. Προσθέστε την ακόλουθη δέσμη ενεργειών στο αρχείο **install.cmd** :

    ```
    REM Set the value of netfx to install appropriate .NET Framework. 
    REM ***** To install .NET 4.5.2 set the variable netfx to "NDP452" *****
    REM ***** To install .NET 4.6 set the variable netfx to "NDP46" *****
    REM ***** To install .NET 4.6.1 set the variable netfx to "NDP461" *****
    REM ***** To install .NET 4.6.2 set the variable netfx to "NDP462" *****
    set netfx="NDP461"
    
    REM ***** Set script start timestamp *****
    set timehour=%time:~0,2%
    set timestamp=%date:~-4,4%%date:~-10,2%%date:~-7,2%-%timehour: =0%%time:~3,2%
    set "log=install.cmd started %timestamp%."
    
    REM ***** Exit script if running in Emulator *****
    if %ComputeEmulatorRunning%=="true" goto exit
    
    REM ***** Needed to correctly install .NET 4.6.1, otherwise you may see an out of disk space error *****
    set TMP=%PathToNETFXInstall%
    set TEMP=%PathToNETFXInstall%
    
    REM ***** Setup .NET filenames and registry keys *****
    if %netfx%=="NDP462" goto NDP462
    if %netfx%=="NDP461" goto NDP461
    if %netfx%=="NDP46" goto NDP46
        set "netfxinstallfile=NDP452-KB2901954-Web.exe"
        set netfxregkey="0x5cbf5"
        goto logtimestamp
    
    :NDP46
    set "netfxinstallfile=NDP46-KB3045560-Web.exe"
    set netfxregkey="0x6004f"
    goto logtimestamp
    
    :NDP461
    set "netfxinstallfile=NDP461-KB3102438-Web.exe"
    set netfxregkey="0x6040e"
    goto logtimestamp
    
    :NDP462
    set "netfxinstallfile=NDP462-KB3151802-Web.exe"
    set netfxregkey="0x60632"
    
    :logtimestamp
    REM ***** Setup LogFile with timestamp *****
    md "%PathToNETFXInstall%\log"
    set startuptasklog="%PathToNETFXInstall%log\startuptasklog-%timestamp%.txt"
    set netfxinstallerlog="%PathToNETFXInstall%log\NetFXInstallerLog-%timestamp%"
    echo %log% >> %startuptasklog%
    echo Logfile generated at: %startuptasklog% >> %startuptasklog%
    echo TMP set to: %TMP% >> %startuptasklog%
    echo TEMP set to: %TEMP% >> %startuptasklog%
    
    REM ***** Check if .NET is installed *****
    echo Checking if .NET (%netfx%) is installed >> %startuptasklog%
    set /A netfxregkeydecimal=%netfxregkey%
    set foundkey=0
    FOR /F "usebackq skip=2 tokens=1,2*" %%A in (`reg query "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\NET Framework Setup\NDP\v4\Full" /v Release 2^>nul`) do @set /A foundkey=%%C
    echo Minimum required key: %netfxregkeydecimal% -- found key: %foundkey% >> %startuptasklog%
    if %foundkey% GEQ %netfxregkeydecimal% goto installed
    
    REM ***** Installing .NET *****
    echo Installing .NET with commandline: start /wait %~dp0%netfxinstallfile% /q /serialdownload /log %netfxinstallerlog%  /chainingpackage "CloudService Startup Task" >> %startuptasklog%
    start /wait %~dp0%netfxinstallfile% /q /serialdownload /log %netfxinstallerlog% /chainingpackage "CloudService Startup Task" >> %startuptasklog% 2>>&1
    if %ERRORLEVEL%== 0 goto installed
        echo .NET installer exited with code %ERRORLEVEL% >> %startuptasklog%   
        if %ERRORLEVEL%== 3010 goto restart
        if %ERRORLEVEL%== 1641 goto restart
        echo .NET (%netfx%) install failed with Error Code %ERRORLEVEL%. Further logs can be found in %netfxinstallerlog% >> %startuptasklog%
    
    :restart
    echo Restarting to complete .NET (%netfx%) installation >> %startuptasklog%
    EXIT /B %ERRORLEVEL%
    
    :installed
    echo .NET (%netfx%) is installed >> %startuptasklog%
    
    :end
    echo install.cmd completed: %date:~-4,4%%date:~-10,2%%date:~-7,2%-%timehour: =0%%time:~3,2% >> %startuptasklog%
    
    :exit
    EXIT /B 0
    ```
        
    Η δέσμη ενεργειών εγκατάσταση ελέγχει εάν η καθορισμένη έκδοση .NET framework είναι ήδη εγκατεστημένο στον υπολογιστή κατά την υποβολή ερωτημάτων του μητρώου. Εάν δεν έχει εγκατασταθεί η έκδοση .NET, στη συνέχεια, το .net Web Installer είναι ξεκινά. Για την αντιμετώπιση τυχόν προβλημάτων με το τη δέσμη ενεργειών θα συνδεθείτε όλων των δραστηριοτήτων σε ένα αρχείο με το όνομα *startuptasklog-(currentdatetime) .txt* που είναι αποθηκευμένα σε *InstallLogs* τοπικού χώρου αποθήκευσης.

    > [AZURE.NOTE] Η δέσμη ενεργειών εξακολουθεί να εμφανίζει μπορείτε πώς να εγκαταστήσετε το .NET 4.5.2 ή .NET 4.6 συνέχειας. Δεν χρειάζεται να εγκαταστήσετε με μη αυτόματο τρόπο .NET 4.5.2 ως ήδη είναι διαθέσιμη στο λειτουργικό σύστημα Azure επισκέπτη. Αντί για την εγκατάσταση του .NET 4.6 θα πρέπει να εγκαταστήσετε απευθείας .NET 4.6.1 λόγω [KB 3118750](https://support.microsoft.com/kb/3118750).
      

## <a name="configure-diagnostics-to-transfer-the-startup-task-logs-to-blob-storage"></a>Ρύθμιση παραμέτρων διαγνωστικών για να μεταφέρετε τα αρχεία καταγραφής εργασίας εκκίνησης στο χώρο αποθήκευσης blob 
Για να απλοποιήσετε αντιμετώπιση τυχόν προβλημάτων εγκατάσταση, μπορείτε να ρυθμίσετε Διαγνωστικά Azure για να μεταφέρετε όλα τα αρχεία καταγραφής που δημιουργούνται από τη δέσμη ενεργειών εκκίνησης ή το πρόγραμμα εγκατάστασης του .NET με το χώρο αποθήκευσης αντικειμένων blob. Με αυτήν την προσέγγιση, μπορείτε να προβάλετε τα αρχεία καταγραφής, απλώς τη λήψη των αρχείων καταγραφής από το χώρο αποθήκευσης αντικειμένων blob αντί να χρειάζεται να απομακρυσμένης επιφάνειας εργασίας σε το ρόλο.

Για να ρυθμίσετε τις παραμέτρους Άνοιγμα Διαγνωστικά του *diagnostics.wadcfgx* και προσθέστε το εξής κάτω από τον κόμβο **σε καταλόγους** : 

```xml 
<DataSources>
 <DirectoryConfiguration containerName="netfx-install">
  <LocalResource name="NETFXInstall" relativePath="log"/>
 </DirectoryConfiguration>
</DataSources>
```

Αυτό θα ρυθμίσετε τις παραμέτρους του azure Διαγνωστικά για να μεταφέρετε όλα τα αρχεία στον κατάλογο *καταγραφής* στην περιοχή ο πόρος *NETFXInstall* με το λογαριασμό χώρου αποθήκευσης Διαγνωστικά στο κοντέινερ αντικειμένων blob *netfx της εγκατάστασης* .

## <a name="deploying-your-service"></a>Για την ανάπτυξη της υπηρεσίας 
Κατά την ανάπτυξη της υπηρεσίας τις εργασίες εκκίνησης θα εκτελέσετε και να εγκαταστήσετε το .NET framework, εάν δεν είναι ήδη εγκατεστημένο. Ρόλοι σας θα είναι σε κατάσταση διαθεσιμότητας ενώ το πλαίσιο είναι κατά την εγκατάσταση και μπορεί να επανεκκινήσετε ακόμα και εάν απαιτεί την εγκατάσταση του πλαισίου. 

## <a name="additional-resources"></a>Πρόσθετοι πόροι

- [Κατά την εγκατάσταση του .NET Framework][]
- [Πώς μπορείτε να: καθορίζουν ποιες εκδόσεις .NET Framework είναι εγκατεστημένα][]
- [Αντιμετώπιση προβλημάτων του .NET Framework εγκαταστάσεις][]

[Πώς μπορείτε να: καθορίζουν ποιες εκδόσεις .NET Framework είναι εγκατεστημένα]: https://msdn.microsoft.com/library/hh925568.aspx
[Κατά την εγκατάσταση του .NET Framework]: https://msdn.microsoft.com/library/5a4x27ek.aspx
[Αντιμετώπιση προβλημάτων του .NET Framework εγκαταστάσεις]: https://msdn.microsoft.com/library/hh925569.aspx

<!--Image references-->
[1]: ./media/cloud-services-dotnet-install-dotnet/rolecontentwithinstallerfiles.png
[2]: ./media/cloud-services-dotnet-install-dotnet/rolecontentwithallfiles.png

 
