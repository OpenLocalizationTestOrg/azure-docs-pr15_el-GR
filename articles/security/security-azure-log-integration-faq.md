<properties
   pageTitle="Ενοποίηση του Azure καταγραφής συνήθεις Ερωτήσεις | Microsoft Azure"
   description="Αυτές οι συνήθεις Ερωτήσεις παρέχει απαντήσεις σε ερωτήσεις σχετικά με την ενοποίηση του Azure καταγραφής."
   services="security"
   documentationCenter="na"
   authors="TomShinder"
   manager="MBaldwin"
   editor="TerryLanfear"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/23/2016"
   ms.author="TomSh"/>

# <a name="azure-log-integration-frequently-asked-questions-faq"></a>Ενοποίηση του Azure καταγραφής συνήθεις ερωτήσεις

Αυτές οι συνήθεις Ερωτήσεις παρέχει απαντήσεις σε ερωτήσεις σχετικά με την ενοποίηση Azure καταγραφής, μια υπηρεσία που σας επιτρέπει να ενσωματώσετε ανεπεξέργαστα αρχεία καταγραφής από τους πόρους σας Azure σε συστήματα πληροφοριών ασφαλείας και διαχείρισης συμβάντων (SIEM) σας εσωτερικής εγκατάστασης. Αυτή η ενσωμάτωση παρέχει ενοποιημένες πίνακα εργαλείων για όλους τους πόρους σας, εσωτερικής εγκατάστασης ή στο cloud, ώστε να μπορείτε να συγκεντρώσετε, συσχετισμός, την ανάλυση και ειδοποίηση για συμβάντα ασφαλείας που σχετίζονται με τις εφαρμογές σας.

## <a name="how-can-i-see-the-storage-accounts-from-which-azure-log-integration-is-pulling-azure-vm-logs-from"></a>Πώς μπορώ να δω τους λογαριασμούς χώρου αποθήκευσης από την οποία ενοποίησης Azure καταγραφής έλξη Εικονική Azure αρχεία καταγραφής από;

Εκτελέστε την εντολή **azlog λίστα προέλευσης**.

## <a name="how-can-i-update-the-proxy-configuration"></a>Πώς μπορώ να ενημερώσω τη ρύθμιση παραμέτρων διακομιστή μεσολάβησης;

Εάν η ρύθμιση του διακομιστή μεσολάβησης δεν επιτρέπει απευθείας πρόσβαση Azure χώρου αποθήκευσης, ανοίξτε το **AZLOG. EXE. Ρύθμιση ΠΑΡΑΜΈΤΡΩΝ** αρχείου σε **c:\Program Files\Microsoft Azure καταγραφής ενοποίησης**. Ενημέρωση του αρχείου για να συμπεριλάβετε στην ενότητα **defaultProxy** με τη διεύθυνση του διακομιστή μεσολάβησης της εταιρείας σας. Μόλις ολοκληρωθεί η ενημερωμένη έκδοση, διακόψτε και ξεκινήστε την υπηρεσία, χρησιμοποιώντας εντολές **καθαρή διακοπή azlog** και **azlog καθαρή έναρξης**.

    <?xml version="1.0" encoding="utf-8"?>
    <configuration>
      <system.net>
        <connectionManagement>
          <add address="*" maxconnection="400" />
        </connectionManagement>
        <defaultProxy>
          <proxy usesystemdefault="true"
          proxyaddress=http://127.0.0.1:8888
          bypassonlocal="true" />
        </defaultProxy>
      </system.net>
      <system.diagnostics>
        <performanceCounters filemappingsize="20971520" />
      </system.diagnostics>   

## <a name="how-can-i-see-the-subscription-information-in-windows-events"></a>Πώς μπορώ να δω πληροφορίες για τη συνδρομή στο συμβάντων των Windows;

Προσαρτήστε το **subscriptionid** για να το φιλικό όνομα κατά την προσθήκη της προέλευσης.

    Azlog source add <sourcefriendlyname>.<subscription id> <StorageName> <StorageKey>  

Το συμβάν XML έχει τα μετα-δεδομένα, όπως φαίνεται παρακάτω, συμπεριλαμβάνοντας το αναγνωριστικό συνδρομής.

![Συμβάν XML][1]

## <a name="error-messages"></a>Μηνύματα σφάλματος

### <a name="when-running-command-azlog-createazureid-why-do-i-get-the-following-error"></a>Κατά την εκτέλεση εντολής **azlog createazureid**, γιατί λαμβάνω το ακόλουθο σφάλμα;

Σφάλμα:

  *Απέτυχε η δημιουργία εφαρμογής AAD - μισθωτή 72f988bf-86f1-41af-91ab-2d7cd011db37-λόγο = 'Απαγορεύεται' - μήνυμα = 'Επαρκή δικαιώματα για την ολοκλήρωση της λειτουργίας.'*

**Azlog createazureid** προσπαθεί να δημιουργήσετε αρχής υπηρεσίας σε όλα τα μισθωτές Azure AD για τις συνδρομές στην οποία Azure login έχει πρόσβαση. Εάν το Azure τη σύνδεσή σας είναι μόνο ένας χρήστης επισκέπτη σε αυτόν το μισθωτή Azure AD, στη συνέχεια, η εντολή αποτυγχάνει χωρίς 'Επαρκή δικαιώματα για την ολοκλήρωση της λειτουργίας.' Αίτηση διαχειριστή μισθωτή για να προσθέσετε το λογαριασμό σας ως χρήστη στο μισθωτή.

### <a name="when-running-command-azlog-authorize-why-do-i-get-the-following-error"></a>Κατά την εκτέλεση εντολής **azlog εγκρίνετε**, γιατί λαμβάνω το ακόλουθο σφάλμα;

Σφάλμα:

  *Προειδοποίηση τη δημιουργία εκχώρηση ρόλων - AuthorizationFailed: Ο υπολογιστής-πελάτης janedo@microsoft.com' με αντικείμενο αναγνωριστικό 'fe9e03e4-4dad-4328-910f-fd24a9660bd2' δεν διαθέτει εξουσιοδότηση για να εκτελέσετε την ενέργεια 'Microsoft.Authorization/roleAssignments/write' πάνω από το εύρος '/ συνδρομές/70 d 95299-d689-4c 97-b971-0d8ff0000000'.*

Εντολή **Azlog εγκρίνετε** αντιστοιχίζει ο ρόλος του αναγνώστη στην υπηρεσία Azure AD κεφαλαίου (που δημιουργήθηκε με **Azlog createazureid**) για τις συνδρομές που παρέχονται. Εάν το Azure σύνδεσης δεν είναι από κοινού ένα διαχειριστή ή κάτοχο της συνδρομής, αποτυγχάνει με το μήνυμα σφάλματος "Εξουσιοδότηση απέτυχε". Στοιχείο ελέγχου Azure πρόσβασης βάσει ρόλων (RBAC) από το διαχειριστή ή κάτοχο είναι απαραίτητη για να ολοκληρώσετε αυτήν την ενέργεια.

## <a name="where-can-i-find-the-definition-of-the-properties-in-audit-log"></a>Πού μπορώ να βρω τον ορισμό των ιδιοτήτων στο αρχείο καταγραφής ελέγχου;

Ανατρέξτε στα θέματα:

- [Λειτουργίες ελέγχου με τη διαχείριση πόρων](../resource-group-audit.md)
- [Λίστα με τα συμβάντα διαχείρισης σε μια συνδρομή στο Azure οθόνη REST API](https://msdn.microsoft.com/library/azure/dn931934.aspx)

## <a name="where-can-i-find-details-on-azure-security-center-alerts"></a>Πού μπορώ να βρω λεπτομέρειες στο Κέντρο ασφάλειας Azure ειδοποιήσεις;

Ανατρέξτε στο θέμα [Διαχείριση και απάντηση σε ειδοποιήσεις ασφαλείας στο Κέντρο ασφάλειας Azure](../security-center/security-center-managing-and-responding-alerts.md).

## <a name="how-can-i-modify-what-is-collected-with-vm-diagnostics"></a>Πώς μπορώ να αλλάξω τι συλλέγονται με Εικονική Διαγνωστικά;

Ανατρέξτε στο θέμα [Χρήση του PowerShell για να ενεργοποιήσετε τα Διαγνωστικά σε έναν εικονικό υπολογιστή που εκτελεί Windows Azure](../virtual-machines/virtual-machines-windows-ps-extensions-diagnostics.md) για λεπτομέρειες σχετικά με τη λήψη, τροποποίηση, και ορίστε τα Διαγνωστικά του Azure στα Windows *(WAD)* ρύθμισης παραμέτρων. Ακολουθεί ένα δείγμα:

### <a name="get-the-wad-config"></a>Λήψη του config WAD

    -AzureRmVMDiagnosticsExtension -ResourceGroupName AzLog-Integration -VMName AzlogClient
    $publicsettings = (Get-AzureRmVMDiagnosticsExtension -ResourceGroupName AzLog-Integration -VMName AzlogClient).PublicSettings
    $encodedconfig = (ConvertFrom-Json -InputObject $publicsettings).xmlCfg
    $xmlconfig = [System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($encodedconfig))
    Write-Host $xmlconfig

    $xmlconfig | Out-File -Encoding utf8 -FilePath "d:\WADConfig.xml"

### <a name="modify-the-wad-config"></a>Τροποποίηση του Config WAD

Το παρακάτω παράδειγμα είναι μια ρύθμιση παραμέτρων όπου μόνο EventID 4624 και EventId 4625 συλλέγονται από το αρχείο καταγραφής συμβάντων ασφαλείας. Λογισμικό κακόβουλης λειτουργίας Microsoft συμβάντα συλλέγονται από το αρχείο καταγραφής συμβάντων του συστήματος. Ανατρέξτε στο θέμα [κατανάλωση συμβάντα] (https://msdn.microsoft.com/library/windows/desktop/dd996910 (v=vs.85) για λεπτομέρειες σχετικά με τη χρήση παραστάσεων XPath.

    <WindowsEventLog scheduledTransferPeriod="PT1M">
        <DataSource name="Security!*[System[(EventID=4624 or EventID=4625)]]" />
        <DataSource name="System!*[System[Provider[@Name='Microsoft Antimalware']]]"/>
    </WindowsEventLog>

### <a name="set-the-wad-configuration"></a>Ρύθμιση των παραμέτρων WAD

    $diagnosticsconfig_path = "d:\WADConfig.xml"
    Set-AzureRmVMDiagnosticsExtension -ResourceGroupName AzLog-Integration -VMName AzlogClient -DiagnosticsConfigurationPath $diagnosticsconfig_path -StorageAccountName log3121 -StorageAccountKey <storage key>

Αφού κάνετε τις αλλαγές, επιλέξτε το λογαριασμό χώρου αποθήκευσης για να βεβαιωθείτε ότι η σωστή συμβάντα συλλέγονται.

Εάν έχετε ερωτήσεις σχετικά με την ενοποίηση του αρχείου καταγραφής Azure, στείλτε ένα μήνυμα ηλεκτρονικού ταχυδρομείου για να [AzSIEMteam@microsoft.com](mailto:AzSIEMteam@microsoft.com)

<!--Image references-->
[1]: ./media/security-azure-log-integration-faq/event-xml.png
