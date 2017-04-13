<properties
    pageTitle="Προετοιμασία VHD των Windows για να αποστείλετε στο Azure | Microsoft Azure"
    description="Προτεινόμενες πρακτικές για την προετοιμασία Windows VHD πριν από την αποστολή σε Azure"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="genlin"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/18/2016"
    ms.author="glimoli;genli"/>

# <a name="prepare-a-windows-vhd-to-upload-to-azure"></a>Προετοιμασία VHD των Windows για να αποστείλετε στο Azure
Για να αποστείλετε μια Εικονική των Windows από εσωτερικής εγκατάστασης για Azure, πρέπει να προετοιμάσετε σωστά το εικονικό σκληρό δίσκο (VHD). Υπάρχουν αρκετές συνιστώμενα βήματα για να ολοκληρώσετε πριν από την αποστολή ενός VHD Azure. Αυτό το άρθρο παρουσιάζει τον τρόπο προετοιμασίας ενός VHD Windows για αποστολή στο Microsoft Azure και εξηγεί επίσης [πότε και πώς μπορείτε να χρησιμοποιήσετε το Sysprep](#step23).

## <a name="prepare-the-virtual-disk"></a>Προετοιμασία του εικονικού δίσκου

>[AZURE.NOTE] 
> Azure υποστηρίζει μόνο [γενιάς 1 εικονικές μηχανές](http://blogs.technet.com/b/ausoemteam/archive/2015/04/21/deciding-when-to-use-generation-1-or-generation-2-virtual-machines-with-hyper-v.aspx) που είναι σε μορφή αρχείου VHD. Στη νεότερη μορφή VHDX δεν υποστηρίζεται στο Azure. 
>
> VHD πρέπει να είναι σταθερό μέγεθος, δεν δυναμικές. Εάν είναι απαραίτητο, με λεπτομέρειες τις παρακάτω οδηγίες για τη μετατροπή από VHDX ή δυναμικής δίσκων. Το μέγιστο μέγεθος που επιτρέπεται για τον VHD είναι 1,023 GB.


Βεβαιωθείτε ότι το Windows VHD λειτουργεί σωστά στον τοπικό διακομιστή. Επίλυση τυχόν σφάλματα σε η Εικονική ίδια πριν να επιχειρήσετε να μετατρέψετε ή να αποστείλετε Azure.

Εάν χρειάζεστε για να μετατρέψετε το εικονικό δίσκο την απαιτούμενη μορφή Azure, χρησιμοποιήστε μία από τις μεθόδους σημειωθεί στις ενότητες που ακολουθούν. Δημιουργία αντιγράφου ασφαλείας του Εικονική πριν από την εκτέλεση οποιαδήποτε διαδικασία μετατροπής εικονικού δίσκου ή Sysprep.

### <a name="convert-using-hyper-v-manager"></a>Μετατροπή χρησιμοποιώντας τη Διαχείριση Hyper-V
- Άνοιγμα της διαχείρισης Hyper-V και επιλέξτε τοπικό υπολογιστή σας στην αριστερή πλευρά. Στο μενού επάνω από αυτό, κάντε κλικ στην **ενέργεια** > **Επεξεργασία δίσκου**.
    - Στην οθόνη **Εντοπίστε εικονικού σκληρού δίσκου** , αναζητήστε και επιλέξτε το εικονικό δίσκο.
    - Επιλέξτε " **Μετατροπή** " στην επόμενη οθόνη
        - Εάν θέλετε να μετατρέψετε από VHDX, επιλέξτε **VHD** και κάντε κλικ στο κουμπί **Επόμενο**
        - Εάν θέλετε να μετατρέψετε από δυναμικό δίσκο, επιλέξτε **σταθερό μέγεθος** και κάντε κλικ στο κουμπί **Επόμενο**

    - Αναζητήστε και επιλέξτε **διαδρομή για το νέο αρχείο VHD**.
    - Κάντε κλικ στο κουμπί **Τέλος** για να κλείσετε.

### <a name="convert-using-powershell"></a>Μετατροπή με χρήση του PowerShell
Μπορείτε να μετατρέψετε έναν εικονικό δίσκο χρησιμοποιώντας το [cmdlet του PowerShell VHD μετατροπή](http://technet.microsoft.com/library/hh848454.aspx). Στο παρακάτω παράδειγμα, θα σας τη μετατροπή από ένα VHDX VHD και τη μετατροπή από μια δυναμική σε σταθερή τύπο:

```powershell
Convert-VHD –Path c:\test\MY-VM.vhdx –DestinationPath c:\test\MY-NEW-VM.vhd -VHDType Fixed
```

### <a name="convert-from-vmware-vmdk-disk-format"></a>Μετατροπή από μορφή δίσκου VMware VMDK
Εάν έχετε μια εικόνα Εικονική Windows στη [μορφή αρχείου VMDK](https://en.wikipedia.org/wiki/VMDK), μετατρέψτε τον VHD με χρήση του [Microsoft εικονική μηχανή μετατροπέα](https://www.microsoft.com/download/details.aspx?id=42497). Διαβάστε το ιστολόγιο [πώς μπορείτε να μετατρέψετε ένα VMDK VMware να VHD Hyper-V](http://blogs.msdn.com/b/timomta/archive/2015/06/11/how-to-convert-a-vmware-vmdk-to-hyper-v-vhd.aspx) , για περισσότερες πληροφορίες.

## <a name="prepare-windows-configuration-for-upload"></a>Προετοιμασία ρύθμισης παραμέτρων των Windows για αποστολή

> [AZURE.NOTE] Εκτελέστε τις ακόλουθες εντολές με [δικαιώματα διαχειριστή](https://technet.microsoft.com/library/cc947813.aspx).

1. Καταργήστε τυχόν στατική μόνιμη δρομολόγηση στον πίνακα δρομολόγησης:

    - Για να προβάλετε τον πίνακα δρομολόγησης, εκτελέστε `route print`.
    - Ελέγξτε τις ενότητες **Δρομολογεί διατήρησης** . Εάν υπάρχει μια μόνιμη διαδρομή, χρησιμοποιήστε [δρομολόγηση διαγραφή](https://technet.microsoft.com/library/cc739598.apx) για να την καταργήσετε.

2. Κατάργηση του διακομιστή μεσολάβησης WinHTTP:

    ```
    netsh winhttp reset proxy
    ```

3. Ρύθμιση παραμέτρων του δίσκου πολιτικής ΣΑΝ να [Onlineall](https://technet.microsoft.com/library/gg252636.aspx):

    ```
    diskpart san policy=onlineall
    ```

4. Χρήση του χρόνου Συντονισμένη παγκόσμια ώρα (UTC) για Windows και να ορίσετε τον τύπο εκκίνησης της υπηρεσίας Windows Time (w32time) να **αυτόματα**:

    ```
    REG ADD HKLM\SYSTEM\CurrentControlSet\Control\TimeZoneInformation /v RealTimeIsUniversal /t REG_DWORD /d 1
    sc config w32time start= auto
    ```


## <a name="configure-windows-services"></a>Ρύθμιση παραμέτρων των υπηρεσιών Windows
5. Βεβαιωθείτε ότι κάθε μία από τις ακόλουθες υπηρεσίες Windows έχει οριστεί στις **προεπιλεγμένες τιμές των Windows**. Να έχουν ρυθμιστεί με τις ρυθμίσεις εκκίνησης που αναφέρονται στην παρακάτω λίστα. Μπορείτε να εκτελέσετε αυτές τις εντολές για να επαναφέρετε τις ρυθμίσεις εκκίνησης:

    ```
    sc config bfe start= auto

    sc config dcomlaunch start= auto

    sc config dhcp start= auto

    sc config dnscache start= auto

    sc config IKEEXT start= auto

    sc config iphlpsvc start= auto

    sc config PolicyAgent start= demand

    sc config LSM start= auto

    sc config netlogon start= demand

    sc config netman start= demand

    sc config NcaSvc start= demand

    sc config netprofm start= demand

    sc config NlaSvc start= auto

    sc config nsi start= auto

    sc config RpcSs start= auto

    sc config RpcEptMapper start= auto

    sc config termService start= demand

    sc config MpsSvc start= auto

    sc config WinHttpAutoProxySvc start= demand

    sc config LanmanWorkstation start= auto

    sc config RemoteRegistry start= auto
    ```


## <a name="configure-remote-desktop-configuration"></a>Ρύθμιση παραμέτρων ρύθμισης παραμέτρων απομακρυσμένης επιφάνειας εργασίας
6. Εάν υπάρχουν οποιαδήποτε συνδεδεμένη με το πρωτόκολλο απομακρυσμένης επιφάνειας εργασίας (RDP) ακρόασης πιστοποιητικά αυτόματης υπογραφής, καταργήστε τις:

    ```
    REG DELETE "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp\SSLCertificateSHA1Hash”
    ```

    Για περισσότερες πληροφορίες σχετικά με τη ρύθμιση των παραμέτρων πιστοποιητικά RDP ακροατήριο, ανατρέξτε στην ενότητα [Ρυθμίσεις παραμέτρων πιστοποιητικού ακρόασης σε Windows Server](https://blogs.technet.microsoft.com/askperf/2014/05/28/listener-certificate-configurations-in-windows-server-2012-2012-r2/)

7. Ρυθμίστε τις τιμές [διατήρησης](https://technet.microsoft.com/library/cc957549.aspx) για την υπηρεσία RDP:

    ```
    REG ADD "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v KeepAliveEnable /t REG_DWORD  /d 1 /f

    REG ADD "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v KeepAliveInterval /t REG_DWORD  /d 1 /f

    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp" /v KeepAliveTimeout /t REG_DWORD /d 1 /f
    ```

8. Ρύθμιση παραμέτρων τη λειτουργία ελέγχου ταυτότητας για την υπηρεσία RDP:

    ```
    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v UserAuthentication /t REG_DWORD  /d 1 /f

    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v SecurityLayer /t REG_DWORD  /d 1 /f

    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v fAllowSecProtocolNegotiation /t REG_DWORD  /d 1 /f
    ```

9. Ενεργοποίηση της υπηρεσίας RDP, προσθέτοντας τα ακόλουθα δευτερεύοντα κλειδιά μητρώου:

    ```
    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v fDenyTSConnections /t REG_DWORD  /d 0 /f
    ```


## <a name="configure-windows-firewall-rules"></a>Ρύθμιση παραμέτρων κανόνες τείχους προστασίας των Windows
10. Να επιτρέπεται WinRM μέσω των προφίλ τρεις τείχος προστασίας (τομέα, ιδιωτικό και δημόσια) και να ενεργοποιήσετε την υπηρεσία απομακρυσμένου PowerShell:

    ```
    Enable-PSRemoting -force
    ```

11. Βεβαιωθείτε ότι οι παρακάτω κανόνες τείχους προστασίας λειτουργικό σύστημα επισκέπτη είναι στη θέση:

    - Εισερχομένων

    ```
    netsh advfirewall firewall set rule dir=in name="File and Printer Sharing (Echo Request - ICMPv4-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Network Discovery (LLMNR-UDP-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Network Discovery (NB-Datagram-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Network Discovery (NB-Name-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Network Discovery (Pub-WSD-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Network Discovery (SSDP-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Network Discovery (UPnP-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Network Discovery (WSD EventsSecure-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Windows Remote Management (HTTP-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Windows Remote Management (HTTP-In)" new enable=yes
    ```

    - Εισερχόμενων και εξερχόμενων

    ```
    netsh advfirewall firewall set rule group="Remote Desktop" new enable=yes

    netsh advfirewall firewall set rule group="Core Networking" new enable=yes
    ```

    - Εξερχομένων

    ```
    netsh advfirewall firewall set rule dir=out name="Network Discovery (LLMNR-UDP-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (NB-Datagram-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (NB-Name-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (Pub-WSD-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (SSDP-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (UPnPHost-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (UPnP-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (WSD Events-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (WSD EventsSecure-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (WSD-Out)" new enable=yes
    ```


## <a name="additional-windows-configuration-steps"></a>Επιπλέον βήματα ρύθμισης παραμέτρων των Windows
12. Εκτέλεση `winmgmt /verifyrepository` για να επιβεβαιώσετε ότι το αρχείο φύλαξης οργάνων διαχείρισης των Windows (WMI) είναι συνεπείς. Εάν το αποθετήριο δεδομένων είναι κατεστραμμένη, ανατρέξτε στο θέμα [αυτήν τη δημοσίευση ιστολογίου](https://blogs.technet.microsoft.com/askperf/2014/08/08/wmi-repository-corruption-or-not).

13. Βεβαιωθείτε ότι οι ρυθμίσεις δεδομένων παραμέτρων εκκίνησης (ΚΆΣΑ) συμφωνούν με τα εξής:

    ```
    bcdedit /set {bootmgr} integrityservices enable

    bcdedit /set {default} device partition=C:

    bcdedit /set {default} integrityservices enable

    bcdedit /set {default} recoveryenabled Off

    bcdedit /set {default} osdevice partition=C:

    bcdedit /set {default} bootstatuspolicy IgnoreAllFailures
    ```

14. Καταργήστε τυχόν επιπλέον φίλτρα διασύνδεση προγράμματος οδήγησης μεταφοράς, όπως το λογισμικό που αναλύει τα πακέτα TCP.
15. Για να βεβαιωθείτε ότι ο δίσκος είναι σε καλή κατάσταση και συνεπή, εκτελέστε το `CHKDSK /f` εντολή.
16. Καταργήστε την εγκατάσταση λογισμικού τρίτων κατασκευαστών και το πρόγραμμα οδήγησης που σχετίζονται με φυσικά στοιχεία ή οποιαδήποτε άλλη τεχνολογία virtualization.
17. Βεβαιωθείτε ότι δεν χρησιμοποιεί μια εφαρμογή άλλου κατασκευαστή θύρα 3389. Αυτήν τη θύρα χρησιμοποιείται για την υπηρεσία RDP στο Azure. Μπορείτε να χρησιμοποιήσετε το `netstat -anob` εντολή για να ελέγξετε τις θύρες που χρησιμοποιούν από τις εφαρμογές.
18. Εάν το VHD των Windows που θέλετε να αποστείλετε είναι ελεγκτή τομέα, ακολουθήστε [αυτά τα επιπλέον βήματα](https://support.microsoft.com/kb/2904015) για να προετοιμάσετε το δίσκο.
19. Επανεκκινήστε τον υπολογιστή η Εικονική για να βεβαιωθείτε ότι τα Windows είναι καλή εξακολουθούν να μπορούν να σας καλούν χρησιμοποιώντας τη σύνδεση RDP.
20. Επαναφέρετε τον τρέχοντα κωδικό πρόσβασης του τοπικού διαχειριστή και βεβαιωθείτε ότι μπορείτε να χρησιμοποιήσετε αυτόν το λογαριασμό για να πραγματοποιήσετε είσοδο με το Windows μέσω της σύνδεσης RDP.  Αυτό το δικαίωμα πρόσβασης ελέγχονται από το αντικείμενο πολιτικής "Να επιτρέπεται η σύνδεση στην στις υπηρεσίες απομακρυσμένης επιφάνειας εργασίας". Αυτό το αντικείμενο βρίσκεται στην περιοχή "Υπολογιστή ρυθμίσεις των Windows\Ρυθμίσεις ασφαλείας ασφαλείας\Τοπικές Policies\User εκχώρηση δικαιωμάτων."


## <a name="install-windows-updates"></a>Εγκατάσταση ενημερώσεων των Windows
22. Εγκαταστήστε τις πιο πρόσφατες ενημερώσεις για Windows. Εάν που δεν είναι δυνατό, βεβαιωθείτε ότι έχουν εγκατασταθεί οι παρακάτω ενημερώσεις:

    - [KB3137061](https://support.microsoft.com/kb/3137061) Microsoft Azure ΣΠΣ δεν ανάκτηση από μια μη διαθεσιμότητα δικτύου και προκύψουν ζητήματα καταστροφή δεδομένων

    - [KB3115224](https://support.microsoft.com/kb/3115224) Βελτιώσεις αξιοπιστία για ΣΠΣ που εκτελούνται σε ένα κεντρικό υπολογιστή Windows Server 2012 R2 ή Windows Server 2012

    - [KB3140410](https://support.microsoft.com/kb/3140410) MS16-031: Ενημερωμένη έκδοση ασφαλείας για τα Microsoft Windows, για να διεύθυνση προβιβασμό δικαιώματος: 8 Μαρτίου 2016

    - [KB3063075](https://support.microsoft.com/kb/3063075) Πολλά συμβάντα 129 Αναγνωριστικό καταγράφονται όταν εκτελείτε μια εικονική μηχανή Windows Server 2012 R2 στο Microsoft Azure

    - [KB3137061](https://support.microsoft.com/kb/3137061) Microsoft Azure ΣΠΣ δεν ανάκτηση από μια μη διαθεσιμότητα δικτύου και προκύψουν ζητήματα καταστροφή δεδομένων

    - [KB3114025](https://support.microsoft.com/kb/3114025) Αργές επιδόσεις κατά την πρόσβαση σε αρχεία του Azure χώρου αποθήκευσης από το Windows 8.1 ή Server 2012 R2

    - [KB3033930](https://support.microsoft.com/kb/3033930) Επείγουσα επιδιόρθωση αυξάνουν το όριο 64K σε buffer ΡΊΟ ανά διαδικασία για την υπηρεσία του Windows Azure

    - [KB3004545](https://support.microsoft.com/kb/3004545) Δεν μπορείτε να αποκτήσετε πρόσβαση σε εικονικές μηχανές που φιλοξενούνται σε Azure υπηρεσίες φιλοξενίας μέσα από μια σύνδεση VPN στα Windows

    - [KB3082343](https://support.microsoft.com/kb/3082343) Συνδεσιμότητα VPN σταυρό εσωτερικής εγκατάστασης χάνονται όταν Azure σήραγγες VPN--τοποθεσίας χρήση του Windows Server 2012 R2 RRAS

    - [KB3140410](https://support.microsoft.com/kb/3140410) MS16-031: Ενημερωμένη έκδοση ασφαλείας για τα Microsoft Windows, για να διεύθυνση προβιβασμό δικαιώματος: 8 Μαρτίου 2016

    - [KB3146723](https://support.microsoft.com/kb/3146723) MS16-048: Περιγραφή για την ενημερωμένη έκδοση ασφαλείας για CSRSS: 12 Απριλίου 2016
    - [KB2904100](https://support.microsoft.com/kb/2904100) Σύστημα παγώνει κατά τη διάρκεια δίσκου εισόδου/εξόδου στα Windows<a id="step23"></a>
23. Εάν θέλετε να δημιουργήσετε μια εικόνα σε πολλούς υπολογιστές από την ανάπτυξη, πρέπει να γενίκευση της εικόνας, εκτελώντας `sysprep` πριν από την αποστολή του VHD Azure. Δεν χρειάζεται να εκτελέσετε `sysprep` για τη χρήση ενός εξειδικευμένες VHD. Για περισσότερες πληροφορίες σχετικά με τον τρόπο για να δημιουργήσετε μια γενική εικόνα, ανατρέξτε στα ακόλουθα άρθρα:

    - [Δημιουργήστε μια Εικονική εικόνα από μια υπάρχουσα Εικονική Azure χρησιμοποιώντας το μοντέλο ανάπτυξης για τη διαχείριση πόρων](virtual-machines-windows-create-vm-generalized.md)
    - [Δημιουργήστε μια Εικονική εικόνα από μια υπάρχουσα Εικονική Azure χρησιμοποιώντας το μόντεμ ανάπτυξης κλασική](virtual-machines-windows-classic-capture-image.md)
    - [Υποστήριξη Sysprep για τους ρόλους διακομιστή](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)



## <a name="suggested-extra-configurations"></a>Προτεινόμενη επιπλέον ρυθμίσεις παραμέτρων

Οι ακόλουθες ρυθμίσεις δεν επηρεάζουν VHD αποστολή. Ωστόσο, συνιστάμε ιδιαιτέρως ότι έχετε τις έχει ρυθμιστεί.

- Εγκαταστήστε τον [παράγοντα Azure εικονικές μηχανές](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Αφού εγκαταστήσετε τον παράγοντα, μπορείτε να ενεργοποιήσετε Εικονική επεκτάσεις. Οι επεκτάσεις Εικονική υλοποιήστε περισσότερες τις κρίσιμες λειτουργίες που θέλετε να χρησιμοποιήσετε με το ΣΠΣ όπως επαναφορά κωδικών πρόσβασης, τη ρύθμιση των παραμέτρων RDP και πολλά άλλα άτομα.

- Το αρχείο καταγραφής της ένδειξης μπορεί να είναι χρήσιμες κατά την αντιμετώπιση προβλημάτων σε θέματα διακοπή λειτουργίας των Windows. Ενεργοποίηση της συλλογής καταγραφής ένδειξης:

    ```
    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 2 /f`

    REG ADD "HKLM\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps" /v DumpFolder /t REG_EXPAND_SZ /d "c:\CrashDumps" /f

    REG ADD "HKLM\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps" /v DumpCount /t REG_DWORD /d 10 /f

    REG ADD "HKLM\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps" /v DumpType /t REG_DWORD /d 2 /f

    sc config wer start= auto
    ```

- Μετά την εικονική Μηχανή δημιουργείται στο Azure, ρυθμίστε τις παραμέτρους του που ορίζονται από το μέγεθος του αρχείου σελιδοποίησης στη μονάδα δίσκου δ:

    ```
    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\Session Manager\Memory Management" /t REG_MULTI_SZ /v PagingFiles /d "D:\pagefile.sys 0 0" /f
    ```


## <a name="next-steps"></a>Επόμενα βήματα

- [Αποστολή μιας εικόνας Εικονική Windows Azure για αναπτύξεις από διαχειριστή πόρων](virtual-machines-windows-upload-image.md)
