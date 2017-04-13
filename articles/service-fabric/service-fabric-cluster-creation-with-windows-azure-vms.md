<properties
   pageTitle="Δημιουργήστε ένα σύμπλεγμα μεμονωμένη με ΣΠΣ Azure με Windows | Microsoft Azure"
   description="Μάθετε πώς μπορείτε να δημιουργήσετε και να διαχειριστείτε ένα σύμπλεγμα ύφασμα Azure Service στην Azure εικονικές μηχανές εκτελούν τον Windows Server."
   services="service-fabric"
   documentationCenter=".net"
   authors="dsk-2015"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/05/2016"
   ms.author="dkshir;chackdan"/>



# <a name="create-a-three-node-standalone-service-fabric-cluster-with-azure-virtual-machines-running-windows-server"></a>Δημιουργήστε ένα σύμπλεγμα τρεις κόμβου μεμονωμένη υπηρεσία υφάσματος με Azure εικονικές μηχανές εκτελούν τον Windows Server

Αυτό το άρθρο περιγράφει πώς μπορείτε να δημιουργήσετε ένα σύμπλεγμα σε βασίζεται σε Windows Azure εικονικές μηχανές (ΣΠΣ), χρησιμοποιώντας το πρόγραμμα εγκατάστασης του μεμονωμένου ύφασμα υπηρεσίας για τον Windows Server. Αυτή είναι μια ειδική περίπτωση των [Δημιουργία και διαχείριση ένα σύμπλεγμα εκτελείται σε Windows Server](service-fabric-cluster-creation-for-windows-server.md) του ΣΠΣ βρίσκονται [ΣΠΣ Azure που εκτελούν τον Windows Server](../virtual-machines/virtual-machines-windows-hero-tutorial.md), αλλά δεν θέλετε να δημιουργήσετε [ένα Azure σύμπλεγμα ύφασμα υπηρεσίας βασίζεται στο cloud](service-fabric-cluster-creation-via-portal.md). Η διαφορά είναι ότι το σύμπλεγμα υπηρεσίας ύφασμα μεμονωμένη δημιουργήθηκε από τα παρακάτω βήματα εντελώς γίνεται από εσάς, ενώ το Azure συμπλεγμάτων ύφασμα υπηρεσία που βασίζεται στο cloud γίνεται και αναβάθμιση από την υπηρεσία παροχής υπηρεσίας ύφασμα πόρων.


## <a name="steps-to-create-the-standalone-cluster"></a>Βήματα για τη δημιουργία του μεμονωμένου συμπλέγματος

1. Πραγματοποιήστε είσοδο στην πύλη του Azure και δημιουργήστε μια νέα Windows Server 2012 R2 κέντρο δεδομένων εικονική Μηχανή σε μια ομάδα πόρων. Διαβάστε το άρθρο [Δημιουργία μια Εικονική Windows στην πύλη του Azure](../virtual-machines/virtual-machines-windows-hero-tutorial.md) για περισσότερες λεπτομέρειες.
2. Προσθέστε μερικά περισσότερες Windows Server 2012 R2 κέντρο δεδομένων ΣΠΣ στην ίδια ομάδα πόρων. Βεβαιωθείτε ότι κάθε ένα από τα ΣΠΣ έχει το ίδιο όνομα χρήστη του διαχειριστή και τον κωδικό πρόσβασης κατά τη δημιουργία. Μόλις δημιουργήσατε θα πρέπει να βλέπετε όλες τις τρεις ΣΠΣ στο ίδιο δίκτυο εικονικού.
3. Σύνδεση σε κάθε ένα από τα ΣΠΣ και απενεργοποίηση του τείχους προστασίας των Windows χρησιμοποιώντας τη [Διαχείριση διακομιστών, πίνακα εργαλείων του τοπικού διακομιστή](https://technet.microsoft.com/library/jj134147.aspx). Αυτό εξασφαλίζει ότι η κυκλοφορία του δικτύου μπορούν να επικοινωνούν μεταξύ τους υπολογιστές. Ενώ είστε συνδεδεμένοι σε κάθε υπολογιστή, λάβετε τη διεύθυνση IP ανοίγοντας μια γραμμή εντολών και πληκτρολογώντας `ipconfig`. Εναλλακτικά, μπορείτε να δείτε τη διεύθυνση IP του κάθε υπολογιστή, επιλέγοντας τον πόρο εικονικού δικτύου για την ομάδα πόρων στην πύλη του Azure.
4. Σύνδεση σε ένα από τα ΣΠΣ και να ελέγξετε ότι μπορείτε να κάνετε ping του άλλων δύο ΣΠΣ με επιτυχία.
5. Σύνδεση με μία από τις ΣΠΣ και [κάντε λήψη του πακέτου μεμονωμένη ύφασμα Service για τον Windows Server](http://go.microsoft.com/fwlink/?LinkId=730690) σε έναν νέο φάκελο στον υπολογιστή και εξαγωγή του πακέτου.
6. Ανοίξτε το αρχείο *ClusterConfig.Unsecure.MultiMachine.json* στο Σημειωματάριο και επεξεργαστείτε κάθε κόμβου με τις τρεις διευθύνσεις IP από τους υπολογιστές. Αλλάξτε το όνομα του συμπλέγματος στο επάνω μέρος και αποθηκεύστε το αρχείο.  Ένα παράδειγμα μερική το δηλωτικό σύμπλεγμα εμφανίζεται παρακάτω.

    ```
    {
        "name": "TestCluster",
        "clusterManifestVersion": "1.0.0",
        "apiVersion": "2015-01-01-alpha",
        "nodes": [
        {
            "nodeName": "vm0",
            "metadata": "Replace the localhost with valid IP address or FQDN below",
            "iPAddress": "10.7.0.5",
            "nodeTypeRef": "NodeType0",
            "faultDomain": "fd:/dc1/r0",
            "upgradeDomain": "UD0"
        },
        {
            "nodeName": "vm1",
            "metadata": "Replace the localhost with valid IP address or FQDN below",
            "iPAddress": "10.7.0.4",
            "nodeTypeRef": "NodeType0",
            "faultDomain": "fd:/dc2/r0",
            "upgradeDomain": "UD1"
        },
        {
            "nodeName": "vm2",
            "metadata": "Replace the localhost with valid IP address or FQDN below",
            "iPAddress": "10.7.0.6",
            "nodeTypeRef": "NodeType0",
            "faultDomain": "fd:/dc3/r0",
            "upgradeDomain": "UD2"
        }
    ],
    ```

7. Ανοίξτε ένα [παράθυρο του PowerShell ISE](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/introducing-the-windows-powershell-ise). Μεταβείτε στο φάκελο όπου που έχουν εξαχθεί του πακέτου προγράμματος εγκατάστασης ληφθέντων μεμονωμένη και αποθηκεύσατε το αρχείο δήλωσης σύμπλεγμα. Εκτελέστε την ακόλουθη εντολή PowerShell.

    ```
    .\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.MultiMachine.json -MicrosoftServiceFabricCabFilePath .\MicrosoftAzureServiceFabric.cab
    ```

8. Θα πρέπει να βλέπετε το PowerShell, εκτελέστε, σύνδεση σε κάθε υπολογιστή και δημιουργήστε ένα σύμπλεγμα. Αφού περίπου ένα λεπτό, μπορείτε να ελέγξετε εάν το σύμπλεγμα είναι λειτουργικές κατά τη σύνδεση με την Εξερεύνηση ύφασμα υπηρεσίας μία της διεύθυνσης IP του υπολογιστή π.χ., χρησιμοποιώντας `http://10.7.0.5:19080/Explorer/index.html`. Επειδή πρόκειται για ένα σύμπλεγμα μεμονωμένη χρησιμοποιώντας ΣΠΣ Azure, ώστε να είναι ασφαλής, θα έχετε για την [ανάπτυξη των πιστοποιητικών του ΣΠΣ Azure](service-fabric-windows-cluster-x509-security.md) ή ορίστε μία από τις μηχανές ως [ελεγκτή Windows Server Active Directory (AD) για τον έλεγχο ταυτότητας των Windows](service-fabric-windows-cluster-windows-security.md), όπως θα κάνατε εσωτερικής εγκατάστασης.


## <a name="next-steps"></a>Επόμενα βήματα
- [Δημιουργία μεμονωμένου ύφασμα υπηρεσία συμπλεγμάτων στα Windows Server ή Linux](service-fabric-deploy-anywhere.md)
- [Προσθήκη ή κατάργηση κόμβους σε ένα σύμπλεγμα υπηρεσίας ύφασμα μεμονωμένη](service-fabric-cluster-windows-server-add-remove-nodes.md)
- [Ρύθμιση παραμέτρων για μεμονωμένη Windows συμπλέγματος](service-fabric-cluster-manifest.md)
- [Ασφαλούς ένα σύμπλεγμα μεμονωμένη στα Windows χρησιμοποιώντας ασφαλείας των Windows](service-fabric-windows-cluster-windows-security.md)
- [Ασφαλούς ένα σύμπλεγμα μεμονωμένη στα Windows χρησιμοποιώντας X509 πιστοποιητικών](service-fabric-windows-cluster-x509-security.md)
