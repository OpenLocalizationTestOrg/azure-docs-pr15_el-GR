<properties
   pageTitle="Διαχείριση πολλά περιβάλλοντα στην υπηρεσία ύφασμα | Microsoft Azure"
   description="Εφαρμογές υπηρεσίας ύφασμα μπορούν να εκτελεστούν σε συμπλεγμάτων που κυμαίνονται μέγεθος από έναν υπολογιστή σε χιλιάδες μηχανές. Σε ορισμένες περιπτώσεις, θα θέλετε να ρυθμίσετε τις παραμέτρους της εφαρμογής διαφορετικά για αυτά τα διάφορα περιβάλλοντα. Σε αυτό το άρθρο περιγράφει τον τρόπο για να ορίσετε μια διαφορετική εφαρμογή παράμετροι ανά περιβάλλον."
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="07/19/2016"
   ms.author="seanmck"/>

# <a name="manage-application-parameters-for-multiple-environments"></a>Διαχείριση οι παράμετροι της εφαρμογής για πολλά περιβάλλοντα

Μπορείτε να δημιουργήσετε συμπλεγμάτων Azure Service υφάσματος με τη χρήση οπουδήποτε μέσα από ένα έως πολλών χιλιάδων μηχανές. Ενώ δυαδικά αρχεία εφαρμογών είναι δυνατό να εκτελεστεί χωρίς να τροποποιηθεί σε αυτό ευρύ φάσμα περιβάλλοντα, συχνά θέλετε να ρυθμίσετε τις παραμέτρους της εφαρμογής διαφορετικά, ανάλογα με τον αριθμό των μηχανές την ανάπτυξη σε.

Ως ένα απλό παράδειγμα, εξετάστε το ενδεχόμενο να `InstanceCount` για μια υπηρεσία χωρίς κατάσταση. Όταν εκτελείτε εφαρμογές στο Azure, θα είναι γενικά θέλετε να ορίσετε αυτήν την παράμετρο στην ειδική τιμή "-1". Αυτό εξασφαλίζει ότι η υπηρεσία σας εκτελείται σε κάθε κόμβο του συμπλέγματος. Ωστόσο, αυτή η ρύθμιση παραμέτρων δεν είναι κατάλληλη για ένα σύμπλεγμα ενός υπολογιστή, επειδή δεν μπορείτε να έχετε πολλές διεργασίες ακρόαση στη το ίδιο τελικό σημείο σε έναν υπολογιστή. Αντί για αυτό, συνήθως θα ορίσετε `InstanceCount` σε "1".

## <a name="specifying-environment-specific-parameters"></a>Καθορισμός περιβάλλοντος ειδικές παραμέτρους

Η λύση για αυτό το πρόβλημα ρύθμισης παραμέτρων είναι ένα σύνολο προεπιλεγμένων με παραμέτρους υπηρεσιών και αρχεία παραμέτρων εφαρμογής που συμπληρώνουν αυτές τις τιμές παραμέτρων για ένα δεδομένο περιβάλλον. Προεπιλεγμένες υπηρεσίες και οι παράμετροι της εφαρμογής έχουν ρυθμιστεί οι παράμετροι του δηλώσεων εφαρμογών και υπηρεσιών. Ο ορισμός σχήματος για τα αρχεία ServiceManifest.xml και ApplicationManifest.xml έχει εγκατασταθεί με την υπηρεσία ύφασμα SDK και εργαλεία για να *C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*.

### <a name="default-services"></a>Προεπιλεγμένες υπηρεσίες

Εφαρμογές υπηρεσίας ύφασμα αποτελούνται από μια συλλογή των παρουσιών της υπηρεσίας. Ενώ είναι δυνατό να δημιουργήσετε μια κενή εφαρμογή και, στη συνέχεια, να δημιουργήσετε δυναμικά όλες τις εμφανίσεις της υπηρεσίας, περισσότερες εφαρμογές έχουν ένα σύνολο των βασικών υπηρεσιών που θα πρέπει να δημιουργείται πάντα όταν δημιουργηθεί η εφαρμογή του. Αυτές αναφέρονται ως "προεπιλεγμένες υπηρεσίες". Καθορίζονται στο δηλωτικό εφαρμογής, με σύμβολα κράτησης θέσης για τη ρύθμιση παραμέτρων ανά περιβάλλον που περιλαμβάνονται σε αγκύλες:

    <DefaultServices>
        <Service Name="Stateful1">
            <StatefulService
                ServiceTypeName="Stateful1Type"
                TargetReplicaSetSize="[Stateful1_TargetReplicaSetSize]"
                MinReplicaSetSize="[Stateful1_MinReplicaSetSize]">

                <UniformInt64Partition
                    PartitionCount="[Stateful1_PartitionCount]"
                    LowKey="-9223372036854775808"
                    HighKey="9223372036854775807"
                />
        </StatefulService>
    </Service>
  </DefaultServices>

Κάθε μία από τις ονομαστικές παραμέτρους πρέπει να καθοριστούν μέσα στο στοιχείο παράμετροι το δηλωτικό εφαρμογής:

    <Parameters>
        <Parameter Name="Stateful1_MinReplicaSetSize" DefaultValue="2" />
        <Parameter Name="Stateful1_PartitionCount" DefaultValue="1" />
        <Parameter Name="Stateful1_TargetReplicaSetSize" DefaultValue="3" />
    </Parameters>

Το χαρακτηριστικό DefaultValue Καθορίζει την τιμή που θα χρησιμοποιηθεί σε απουσίας μιας παραμέτρου περισσότερα ειδικά για ένα δεδομένο περιβάλλον.

>[AZURE.NOTE] Όχι σε όλες τις παραμέτρους παρουσία υπηρεσίας είναι κατάλληλες για τη ρύθμιση παραμέτρων ανά περιβάλλον. Στο παραπάνω παράδειγμα, οι τιμές LowKey και HighKey για την υπηρεσία συνδυασμού διαμερισμού ρητά ορίζονται για όλες τις εμφανίσεις της υπηρεσίας από την περιοχή διαμερίσματα είναι μια συνάρτηση του τομέα δεδομένα, όχι το περιβάλλον.


### <a name="per-environment-service-configuration-settings"></a>Ρυθμίσεις παραμέτρων υπηρεσίας ανά περιβάλλον

Το [μοντέλο εφαρμογών υπηρεσίας ύφασμα](service-fabric-application-model.md) ενεργοποιεί τις υπηρεσίες για να συμπεριλάβετε πακέτων ρύθμισης παραμέτρων που περιέχουν προσαρμοσμένες ζεύγη κλειδιού-τιμής που είναι αναγνώσιμο κατά το χρόνο εκτέλεσης. Τις τιμές από αυτές τις ρυθμίσεις μπορούν να επίσης διαφέρει από το περιβάλλον, καθορίζοντας μια `ConfigOverride` στο δηλωτικό εφαρμογής.

Ας υποθέσουμε ότι έχετε την ακόλουθη ρύθμιση στο αρχείο Config\Settings.xml για το `Stateful1` υπηρεσίας:


    <Section Name="MyConfigSection">
      <Parameter Name="MaxQueueSize" Value="25" />
    </Section>

Για να παρακάμψετε αυτή την τιμή για μια συγκεκριμένη εφαρμογή/περιβάλλον ζεύγος, δημιουργήστε μια `ConfigOverride` κατά την εισαγωγή της δήλωσης υπηρεσίας στο δηλωτικό εφαρμογής.

    <ConfigOverrides>
     <ConfigOverride Name="Config">
        <Settings>
           <Section Name="MyConfigSection">
              <Parameter Name="MaxQueueSize" Value="[Stateful1_MaxQueueSize]" />
           </Section>
        </Settings>
     </ConfigOverride>
  </ConfigOverrides>

Αυτή η παράμετρος, στη συνέχεια, μπορούν να ρυθμιστούν από το περιβάλλον, όπως φαίνεται παραπάνω. Μπορείτε να το κάνετε ανακοινώσετε ότι στην ενότητα τις παραμέτρους της εφαρμογής δηλωτικό και καθορίζοντας συγκεκριμένο περιβάλλον τιμές σε αρχεία παραμέτρων της εφαρμογής.

>[AZURE.NOTE] Στην περίπτωση ρυθμίσεις παραμέτρων υπηρεσίας, υπάρχουν τρία σημεία όπου μπορεί να οριστεί η τιμή ενός κλειδιού: το πακέτο ρύθμισης παραμέτρων υπηρεσίας, η δήλωση εφαρμογής και το αρχείο παραμέτρων της εφαρμογής. Υπηρεσία ύφασμα θα κάνετε πάντα την επιλογή από το αρχείο παραμέτρων εφαρμογής πρώτη (Εάν καθοριστεί), στη συνέχεια, τη δήλωση της εφαρμογής και, τέλος του πακέτου ρύθμισης παραμέτρων.


### <a name="application-parameter-files"></a>Αρχεία παραμέτρων εφαρμογής

Το έργο εφαρμογής υπηρεσίας ύφασμα να συμπεριλάβετε ένα ή περισσότερα αρχεία παραμέτρων εφαρμογής. Κάθε μία από αυτές ορίζει τις συγκεκριμένες τιμές για τις παραμέτρους που ορίζονται στο δηλωτικό εφαρμογής:

    <!-- ApplicationParameters\Local.xml -->

    <Application Name="fabric:/Application1" xmlns="http://schemas.microsoft.com/2011/01/fabric">
        <Parameters>
            <Parameter Name ="Stateful1_MinReplicaSetSize" Value="2" />
            <Parameter Name="Stateful1_PartitionCount" Value="1" />
            <Parameter Name="Stateful1_TargetReplicaSetSize" Value="3" />
        </Parameters>
    </Application>

Από προεπιλογή, μια νέα εφαρμογή περιλαμβάνει δύο αρχεία παραμέτρων εφαρμογής, με το όνομα Local.xml και Cloud.xml:

![Εφαρμογή παραμέτρου αρχεία στην Εξερεύνηση λύσεων][app-parameters-solution-explorer]

Για να δημιουργήσετε ένα νέο αρχείο παραμέτρων, απλώς αντιγράψτε και επικολλήστε ένα υπάρχον και δώσετε ένα νέο όνομα.

## <a name="identifying-environment-specific-parameters-during-deployment"></a>Εντοπισμός περιβάλλον ειδικές παραμέτρους κατά τη διάρκεια της ανάπτυξης

Κατά το χρόνο ανάπτυξης, πρέπει να επιλέξετε το αρχείο κατάλληλο παραμέτρου για να εφαρμόσετε με την εφαρμογή σας. Μπορείτε να το κάνετε μέσω του παραθύρου διαλόγου δημοσίευση στο Visual Studio ή μέσω του PowerShell.

### <a name="deploy-from-visual-studio"></a>Ανάπτυξη από το Visual Studio

Μπορείτε να επιλέξετε από τη λίστα των αρχείων διαθέσιμη παραμέτρου όταν δημοσιεύετε την εφαρμογή στο Visual Studio.

![Επιλέξτε ένα αρχείο παραμέτρων στο παράθυρο διαλόγου "Δημοσίευση"][publishdialog]

### <a name="deploy-from-powershell"></a>Ανάπτυξη από PowerShell

Το `Deploy-FabricApplication.ps1` δέσμη ενεργειών του PowerShell που περιλαμβάνονται στο πρότυπο έργου της εφαρμογής αποδέχεται προφίλ δημοσίευση ως παράμετρο και το PublishProfile περιέχει μια αναφορά σε αρχείο παραμέτρων της εφαρμογής.

  ```PowerShell
    ./Deploy-FabricApplication -ApplicationPackagePath <app_package_path> -PublishProfileFile <publishprofile_path>
  ```

## <a name="next-steps"></a>Επόμενα βήματα

Για να μάθετε περισσότερα σχετικά με ορισμένες από τις βασικές έννοιες που περιγράφονται σε αυτό το θέμα, ανατρέξτε στο θέμα η [υπηρεσία ύφασμα Τεχνική επισκόπηση](service-fabric-technical-overview.md). Για πληροφορίες σχετικά με άλλες δυνατότητες διαχείρισης εφαρμογών που είναι διαθέσιμες στο Visual Studio, ανατρέξτε στο θέμα [Διαχείριση εφαρμογών σας ύφασμα υπηρεσίας στο Visual Studio](service-fabric-manage-application-in-visual-studio.md).

<!-- Image references -->

[publishdialog]: ./media/service-fabric-manage-multiple-environment-app-configuration/publish-dialog-choose-app-config.png
[app-parameters-solution-explorer]:./media/service-fabric-manage-multiple-environment-app-configuration/app-parameters-in-solution-explorer.png
