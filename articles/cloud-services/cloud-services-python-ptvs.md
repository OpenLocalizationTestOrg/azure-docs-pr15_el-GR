<properties
    pageTitle="Ρόλοι Python web και εργασίας με το Visual Studio | Microsoft Azure"
    description="Επισκόπηση της χρήσης εργαλείων Python για το Visual Studio για τη δημιουργία υπηρεσιών Azure cloud όπως web ρόλους και εργασίας."
    services="cloud-services"
    documentationCenter="python"
    authors="thraka"
    manager="timlt"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="hero-article"
    ms.date="08/03/2016"
    ms.author="adegeo"/>


# <a name="python-web-and-worker-roles-with-python-tools-for-visual-studio"></a>Ρόλοι Python web και εργασίας με τα εργαλεία Python για το Visual Studio

Σε αυτό το άρθρο παρέχει μια επισκόπηση της χρήσης Python web εργαζόμενου ρόλους και χρήση των [Εργαλείων Python για το Visual Studio][]. Θα μάθετε πώς να χρησιμοποιείτε το Visual Studio για να δημιουργήσετε και να αναπτύξετε μια βασική υπηρεσία Cloud, η οποία χρησιμοποιεί Python.

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

 - Visual Studio 2013 ή του 2015
 - [Εργαλεία Python για το Visual Studio][] (PTVS)
 - [Εργαλεία Azure SDK για το 2013 και στο][] ή [Εργαλεία Azure SDK για ΣΎΓΚΡΙΣΗ 2015][]
 - [Python 2.7 32 bit][] ή [Python 3.5 32 bit][]

[AZURE.INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

## <a name="what-are-python-web-and-worker-roles"></a>Τι είναι οι ρόλοι Python web και εργασίας;

Azure παρέχει τρία τον υπολογισμό μοντέλα για την εκτέλεση εφαρμογών: [η δυνατότητα Web Apps στο Azure εφαρμογής υπηρεσίας][execution model-web sites], [εικονικές μηχανές Windows Azure][execution model-vms], και [Τις υπηρεσίες Cloud Azure][execution model-cloud services]. Όλα τα τρία μοντέλα υποστηρίζει Python. Υπηρεσίες cloud, οι οποίες περιλαμβάνουν web και εργαζόμενου ρόλους, παρέχουν *πλατφόρμα ως υπηρεσία (PaaS)*. Μέσα σε μια υπηρεσία cloud, ένα ρόλο web παρέχει ένα αποκλειστικό διακομιστή web Internet Information Services (IIS) σε εφαρμογές web προσκηνίου του κεντρικού υπολογιστή, ενώ ένα ρόλο εργαζόμενου να εκτελέσετε εργασίες ασύγχρονης, μεγάλη διάρκεια εκτέλεσης ή διαρκή ανεξάρτητα από την παρέμβαση του χρήστη ή εισαγωγή δεδομένων.

Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Τι είναι μια υπηρεσία στο Cloud;].

> [AZURE.NOTE]*Looking για να δημιουργήσετε μια απλή τοποθεσία Web;*
Εάν το σενάριό σας περιλαμβάνει μόνο μια απλή τοποθεσία Web προσκηνίου, μπορείτε να χρησιμοποιήσετε τη δυνατότητα ελαφριά Web Apps στο Azure εφαρμογής υπηρεσίας. Μπορείτε εύκολα να αναβαθμίσετε σε μια υπηρεσία Cloud καθώς εξελίσσεται η τοποθεσία Web σας και να αλλάξετε τις απαιτήσεις σας. Ανατρέξτε στο <a href="/develop/python/">Κέντρο για προγραμματιστές Python</a> για άρθρα που καλύπτουν ανάπτυξη της δυνατότητας Web Apps στο Azure εφαρμογής υπηρεσίας.
<br />


## <a name="project-creation"></a>Δημιουργία έργου

Στο Visual Studio, μπορείτε να επιλέξετε **Μια υπηρεσία Cloud Azure** στο παράθυρο διαλόγου **Νέο έργο** , στην περιοχή **Python**.

![Παράθυρο διαλόγου νέου έργου](./media/cloud-services-python-ptvs/new-project-cloud-service.png)

Στον Οδηγό Azure υπηρεσία Cloud, μπορείτε να δημιουργήσετε νέους ρόλους web και εργασίας.

![Παράθυρο διαλόγου υπηρεσία Azure Cloud](./media/cloud-services-python-ptvs/new-service-wizard.png)

Το πρότυπο ρόλο εργαζόμενου διατίθεται με κώδικα στερεότυπο κείμενο για να συνδεθείτε με ένα λογαριασμό Azure χώρου αποθήκευσης ή Bus υπηρεσίας Azure.

![Λύση υπηρεσίας cloud](./media/cloud-services-python-ptvs/worker.png)

Μπορείτε να προσθέσετε τους ρόλους web ή εργασίας σε μια υπάρχουσα υπηρεσία cloud οποιαδήποτε στιγμή.  Μπορείτε να επιλέξετε να προσθέσετε υπάρχοντα έργα στη λύση σας ή να δημιουργήσετε νέα.

![Προσθήκη εντολής ρόλων](./media/cloud-services-python-ptvs/add-new-or-existing-role.png)

Υπηρεσία cloud μπορούν να περιέχουν ρόλους υλοποιηθεί σε διαφορετικές γλώσσες.  Για παράδειγμα, μπορείτε να έχετε ένα ρόλο web Python υλοποιηθεί με τη χρήση Django, με Python ή με τους ρόλους εργαζόμενου C#.  Μπορείτε εύκολα να επικοινωνήσετε μεταξύ ρόλους σας με τη χρήση υπηρεσιών Bus ουρές ή ουρές χώρου αποθήκευσης.

## <a name="install-python-on-the-cloud-service"></a>Εγκατάσταση Python στην υπηρεσία cloud

>[AZURE.WARNING] Οι δέσμες ενεργειών εγκατάστασης που έχουν εγκατασταθεί με το Visual Studio (τη στιγμή σε αυτό το άρθρο έγινε η τελευταία ενημέρωση) δεν λειτουργεί. Αυτή η ενότητα περιγράφει μια λύση.

Το κύριο ζήτημα με τις δέσμες ενεργειών εγκατάστασης είναι ότι δεν εγκαθιστούν python. Πρώτα, μπορείτε να ορίσετε δύο [εργασίες εκκίνησης](cloud-services-startup-tasks.md) στο αρχείο [ServiceDefinition.csdef](cloud-services-model-and-package.md#servicedefinitioncsdef) . Η πρώτη εργασία (**PrepPython.ps1**) πραγματοποιεί λήψη και εγκαθιστά το περιβάλλον εκτέλεσης Python. Η δεύτερη εργασία (**PipInstaller.ps1**) εκτελείται pip για να εγκαταστήσετε τυχόν εξαρτήσεις ενδέχεται να έχετε.

Οι παρακάτω δέσμες ενεργειών έχουν εγγραφεί στόχευσης Python 3.5. Εάν θέλετε να χρησιμοποιήσετε την έκδοση 2.x του python, ορίστε το αρχείο μεταβλητής **PYTHON2** σε **στην** για τις δύο εργασίες εκκίνησης και την εργασία χρόνου εκτέλεσης: `<Variable name="PYTHON2" value="<mark>on</mark>" />`.


```xml
<Startup>

  <Task executionContext="elevated" taskType="simple" commandLine="bin\ps.cmd PrepPython.ps1">
    <Environment>
      <Variable name="EMULATED">
        <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
      </Variable>
      <Variable name="PYTHON2" value="off" />
    </Environment>
  </Task>

  <Task executionContext="elevated" taskType="simple" commandLine="bin\ps.cmd PipInstaller.ps1">
    <Environment>
      <Variable name="EMULATED">
        <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
      </Variable>
      <Variable name="PYTHON2" value="off" />
    </Environment>
    
  </Task>

</Startup>
```

Πρέπει να προστεθεί στην εργασία εκκίνησης εργαζόμενου τις μεταβλητές **PYTHON2** και **PYPATH** . Η μεταβλητή **PYPATH** χρησιμοποιείται μόνο εάν η μεταβλητή **PYTHON2** έχει οριστεί **σε**.

```xml
<Runtime>
  <Environment>
    <Variable name="EMULATED">
      <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
    </Variable>
    <Variable name="PYTHON2" value="off" />
    <Variable name="PYPATH" value="%SystemDrive%\Python27" />
  </Environment>
  <EntryPoint>
    <ProgramEntryPoint commandLine="bin\ps.cmd LaunchWorker.ps1" setReadyOnProcessStart="true" />
  </EntryPoint>
</Runtime>
```

#### <a name="sample-servicedefinitioncsdef"></a>Δείγμα ServiceDefinition.csdef

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceDefinition name="AzureCloudServicePython" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition" schemaVersion="2015-04.2.6">
  <WorkerRole name="WorkerRole1" vmsize="Small">
    <ConfigurationSettings>
      <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" />
      <Setting name="Python2" />
    </ConfigurationSettings>
    <Startup>
      <Task executionContext="elevated" taskType="simple" commandLine="bin\ps.cmd PrepPython.ps1">
        <Environment>
          <Variable name="EMULATED">
            <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
          </Variable>
          <Variable name="PYTHON2" value="off" />
        </Environment>
      </Task>
      <Task executionContext="elevated" taskType="simple" commandLine="bin\ps.cmd PipInstaller.ps1">
        <Environment>
          <Variable name="EMULATED">
            <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
          </Variable>
          <Variable name="PYTHON2" value="off" />
        </Environment>
      </Task>
    </Startup>
    <Runtime>
      <Environment>
        <Variable name="EMULATED">
          <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
        </Variable>
        <Variable name="PYTHON2" value="off" />
        <Variable name="PYPATH" value="%SystemDrive%\Python27" />
      </Environment>
      <EntryPoint>
        <ProgramEntryPoint commandLine="bin\ps.cmd LaunchWorker.ps1" setReadyOnProcessStart="true" />
      </EntryPoint>
    </Runtime>
    <Imports>
      <Import moduleName="RemoteAccess" />
      <Import moduleName="RemoteForwarder" />
    </Imports>
  </WorkerRole>
</ServiceDefinition>
```



Στη συνέχεια, δημιουργήστε τα αρχεία **PrepPython.ps1** και **PipInstaller.ps1** το **. / κλάσης** φάκελο του ρόλου σας.

#### <a name="preppythonps1"></a>PrepPython.ps1

Αυτή η δέσμη ενεργειών εγκαθιστά python. Εάν έχει οριστεί η μεταβλητή enviornment **PYTHON2** **στην** κατόπιν Python 2.7 θα εγκατασταθεί, διαφορετικά θα εγκατασταθεί Python 3.5.

```powershell
$is_emulated = $env:EMULATED -eq "true"
$is_python2 = $env:PYTHON2 -eq "on"
$nl = [Environment]::NewLine

if (-not $is_emulated){
    Write-Output "Checking if python is installed...$nl"
    if ($is_python2) {
        & "${env:SystemDrive}\Python27\python.exe"  -V | Out-Null
    }
    else {
        py -V | Out-Null
    }

    if (-not $?) {

        $url = "https://www.python.org/ftp/python/3.5.2/python-3.5.2-amd64.exe"
        $outFile = "${env:TEMP}\python-3.5.2-amd64.exe"

        if ($is_python2) {
            $url = "https://www.python.org/ftp/python/2.7.12/python-2.7.12.amd64.msi"
            $outFile = "${env:TEMP}\python-2.7.12.amd64.msi"
        }
        
        Write-Output "Not found, downloading $url to $outFile$nl"
        Invoke-WebRequest $url -OutFile $outFile
        Write-Output "Installing$nl"

        if ($is_python2) {
            Start-Process msiexec.exe -ArgumentList "/q", "/i", "$outFile", "ALLUSERS=1" -Wait
        }
        else {
            Start-Process "$outFile" -ArgumentList "/quiet", "InstallAllUsers=1" -Wait
        }

        Write-Output "Done$nl"
    }
    else {
        Write-Output "Already installed"
    }
}
```

#### <a name="pipinstallerps1"></a>PipInstaller.ps1

Αυτή η δέσμη ενεργειών καλεί τον pip και εγκαθιστά όλες τις εξαρτήσεις στο αρχείο **requirements.txt** . Εάν έχει οριστεί η μεταβλητή enviornment **PYTHON2** **στην** κατόπιν Python 2.7 θα χρησιμοποιηθεί, διαφορετικά θα χρησιμοποιηθεί Python 3.5.

```powershell
$is_emulated = $env:EMULATED -eq "true"
$is_python2 = $env:PYTHON2 -eq "on"
$nl = [Environment]::NewLine

if (-not $is_emulated){
    Write-Output "Checking if requirements.txt exists$nl"
    if (Test-Path ..\requirements.txt) {
        Write-Output "Found. Processing pip$nl"

        if ($is_python2) {
            & "${env:SystemDrive}\Python27\python.exe" -m pip install -r ..\requirements.txt
        }
        else {
            py -m pip install -r ..\requirements.txt
        }

        Write-Output "Done$nl"
    }
    else {
        Write-Output "Not found$nl"
    }
}
```

#### <a name="modify-launchworkerps1"></a>Τροποποίηση LaunchWorker.ps1

>[AZURE.NOTE] Στην περίπτωση **ρόλο εργασίας** έργου, το αρχείο **LauncherWorker.ps1** απαιτείται για την εκτέλεση του αρχείου εκκίνησης. Σε ένα έργο **ρόλου web** , το αρχείο εκκίνησης αντί για αυτό έχει οριστεί στις ιδιότητες του έργου.

Το **bin\LaunchWorker.ps1** δημιουργήθηκε αρχικά για να το κάνετε πολλές εργασίες προετοιμασίας, αλλά δεν λειτουργεί πραγματικά. Αντικαταστήστε τα περιεχόμενα σε αυτό το αρχείο με την ακόλουθη δέσμη ενεργειών.

Αυτή η δέσμη ενεργειών καλεί το αρχείο **worker.py** από το έργο σας python. Εάν έχει οριστεί η μεταβλητή enviornment **PYTHON2** **στην** κατόπιν Python 2.7 θα χρησιμοποιηθεί, διαφορετικά θα χρησιμοποιηθεί Python 3.5.

```powershell
$is_emulated = $env:EMULATED -eq "true"
$is_python2 = $env:PYTHON2 -eq "on"
$nl = [Environment]::NewLine

if (-not $is_emulated)
{
    Write-Output "Running worker.py$nl"

    if ($is_python2) {
        cd..
        iex "$env:PYPATH\python.exe worker.py"
    }
    else {
        cd..
        iex "py worker.py"
    }
}
else
{
    Write-Output "Running (EMULATED) worker.py$nl"

    # Customize to your local dev environment

    if ($is_python2) {
        cd..
        iex "$env:PYPATH\python.exe worker.py"
    }
    else {
        cd..
        iex "py worker.py"
    }
}
```

#### <a name="pscmd"></a>PS.cmd

Τα πρότυπα του Visual Studio θα πρέπει να έχετε δημιουργήσει ένα αρχείο **ps.cmd** στο το **. / κλάσης** φακέλου. Αυτή η δέσμη ενεργειών κελύφους κλήσεις εκτός του περιτυλίγματος δεσμών ενεργειών του PowerShell παραπάνω και παρέχει καταγραφή με βάση το όνομα του το περιτυλίγματος PowerShell που ονομάζεται. Εάν αυτό το αρχείο δεν δημιουργήθηκε, θα δείτε τι πρέπει να είναι σε αυτό. 

```bat
@echo off

cd /D %~dp0

if not exist "%DiagnosticStore%\LogFiles" mkdir "%DiagnosticStore%\LogFiles"
%SystemRoot%\System32\WindowsPowerShell\v1.0\powershell.exe -ExecutionPolicy Unrestricted -File %* >> "%DiagnosticStore%\LogFiles\%~n1.txt" 2>> "%DiagnosticStore%\LogFiles\%~n1.err.txt"
```



## <a name="run-locally"></a>Εκτέλεση τοπικά

Εάν ορίσετε το έργο σας υπηρεσία cloud ως το έργο εκκίνησης και πατήστε το πλήκτρο F5, την υπηρεσία cloud θα εκτελείται σε το τοπικό Azure προσομοίωσης.

Παρόλο που υποστηρίζει PTVS εάν πραγματοποιηθεί εκκίνηση στο το προσομοίωσης, τον εντοπισμό σφαλμάτων (για παράδειγμα, σημεία διακοπής) δεν θα λειτουργούν.

Για τον εντοπισμό σφαλμάτων ρόλους σας web και εργασίας, μπορείτε να ορίσετε το έργο ρόλων με το έργο της εκκίνησης και να εντοπισμός σφαλμάτων που αντί για αυτό.  Μπορείτε επίσης να ορίσετε πολλά έργα εκκίνησης.  Κάντε δεξί κλικ τη λύση και, στη συνέχεια, επιλέξτε το στοιχείο **Ορισμός εκκίνησης έργα**.

![Ιδιότητες έργου εκκίνησης λύσης](./media/cloud-services-python-ptvs/startup.png)

## <a name="publish-to-azure"></a>Δημοσίευση σε Azure

Για να δημοσιεύσετε, κάντε δεξί κλικ στο έργο υπηρεσία cloud της λύσης και, στη συνέχεια, επιλέξτε **Δημοσίευση**.

![Microsoft Azure δημοσίευση είσοδος](./media/cloud-services-python-ptvs/publish-sign-in.png)

Ακολουθήστε τα βήματα του οδηγού. Εάν χρειάζεται, ενεργοποίηση απομακρυσμένης επιφάνειας εργασίας. Σύνδεση απομακρυσμένης επιφάνειας εργασίας είναι χρήσιμη όταν θέλετε να εντοπίσετε σφάλματα κάτι.

Όταν ολοκληρώσετε τη ρύθμιση των παραμέτρων ρυθμίσεων, κάντε κλικ στο κουμπί **Δημοσίευση**.

Ορισμένες προόδου θα εμφανίζονται στο παράθυρο εξόδου και, στη συνέχεια, θα δείτε το παράθυρο αρχείο καταγραφής του Microsoft Azure δραστηριοτήτων.

![Παράθυρο αρχείου καταγραφής δραστηριότητας Microsoft Azure](./media/cloud-services-python-ptvs/publish-activity-log.png)

Ανάπτυξη θα διαρκέσει αρκετά λεπτά για να ολοκληρωθεί και, στη συνέχεια, θα να εκτελεί ρόλους σας web ή/και εργαζόμενου σε Azure!

### <a name="investigate-logs"></a>Διερεύνηση αρχείων καταγραφής

Μετά την υπηρεσία cloud εικονική μηχανή εκκίνηση και εγκαθιστά Python, μπορείτε να δείτε τα αρχεία καταγραφής για να βρείτε όλα τα μηνύματα αποτυχία. Αυτά τα αρχεία καταγραφής βρίσκονται στο το **C:\Resources\Directory\\\LogFiles {ρόλο}** φακέλου. **PrepPython.err.txt** θα έχει τουλάχιστον ένα σφάλμα σε αυτό από όταν η δέσμη ενεργειών προσπαθεί να εντοπίσει εάν είναι εγκατεστημένο το Python και **PipInstaller.err.txt** μπορεί να υποβάλλει καταγγελία σχετικά με μια παλιά έκδοση του pip.

## <a name="next-steps"></a>Επόμενα βήματα

Για πιο λεπτομερείς πληροφορίες σχετικά με την εργασία με το web και εργαζόμενου ρόλους σε εργαλεία Python για το Visual Studio, ανατρέξτε στην τεκμηρίωση PTVS:

- [Έργα υπηρεσίας cloud][]

Για περισσότερες λεπτομέρειες σχετικά με τη χρήση των υπηρεσιών Azure από το web και εργαζόμενου ρόλους, όπως χρησιμοποιώντας το χώρο αποθήκευσης Azure ή Bus υπηρεσίας, ανατρέξτε στα ακόλουθα άρθρα.

- [Υπηρεσία BLOB][]
- [Υπηρεσία πίνακα][]
- [Υπηρεσία Ουράς][]
- [Υπηρεσία Bus ουρές][]
- [Θέματα Bus υπηρεσίας][]


<!--Link references-->

[Τι είναι μια υπηρεσία στο Cloud;]: cloud-services-choose-me.md
[execution model-web sites]: ../app-service-web/app-service-web-overview.md
[execution model-vms]: ../virtual-machines/virtual-machines-windows-about.md
[execution model-cloud services]: cloud-services-choose-me.md
[Python Developer Center]: /develop/python/

[Υπηρεσία BLOB]: ../storage/storage-python-how-to-use-blob-storage.md
[Υπηρεσία Ουράς]: ../storage/storage-python-how-to-use-queue-storage.md
[Υπηρεσία πίνακα]: ../storage/storage-python-how-to-use-table-storage.md
[Υπηρεσία Bus ουρές]: ../service-bus-messaging/service-bus-python-how-to-use-queues.md
[Θέματα Bus υπηρεσίας]: ../service-bus-messaging/service-bus-python-how-to-use-topics-subscriptions.md


<!--External Link references-->

[Εργαλεία Python για το Visual Studio]: http://aka.ms/ptvs
[Python Tools for Visual Studio Documentation]: http://aka.ms/ptvsdocs
[Έργα υπηρεσίας cloud]: http://go.microsoft.com/fwlink/?LinkId=624028
[Εργαλεία Azure SDK για το 2013 και στο]: http://go.microsoft.com/fwlink/?LinkId=323510
[Εργαλεία Azure SDK για ΣΎΓΚΡΙΣΗ 2015]: http://go.microsoft.com/fwlink/?LinkId=518003
[Python 2.7 32-bit]: https://www.python.org/downloads/
[Python 3.5 32-bit]: https://www.python.org/downloads/
