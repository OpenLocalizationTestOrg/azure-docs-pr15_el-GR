<properties
   pageTitle="Να ξεκινήσετε τη δημιουργία Internet αντικριστές εξισορρόπηση φόρτου κλασική ανάπτυξης μοντέλο χρησιμοποιώντας για τις υπηρεσίες cloud | Microsoft Azure"
   description="Μάθετε πώς να δημιουργείτε Internet αντικριστές εξισορρόπηση φόρτου στο μοντέλο κλασική ανάπτυξης για τις υπηρεσίες cloud"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/17/2016"
   ms.author="sewhee" />

# <a name="get-started-creating-an-internet-facing-load-balancer-for-cloud-services"></a>Να ξεκινήσετε τη δημιουργία Internet αντικριστές εξισορρόπηση φόρτου για τις υπηρεσίες cloud

[AZURE.INCLUDE [load-balancer-get-started-internet-classic-selectors-include.md](../../includes/load-balancer-get-started-internet-classic-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Σε αυτό το άρθρο καλύπτει το μοντέλο κλασική ανάπτυξης. Μπορείτε επίσης να [Μάθετε πώς να δημιουργείτε Internet αντικριστές εξισορρόπηση φόρτου χρησιμοποιώντας τη διαχείριση πόρων Azure](load-balancer-get-started-internet-arm-cli.md).

Υπηρεσίες cloud ρυθμίζονται αυτόματα με μια μονάδα εξισορρόπησης φόρτου και μπορούν να προσαρμοστούν μέσω του μοντέλου υπηρεσίας.

## <a name="create-a-load-balancer-using-the-service-definition-file"></a>Δημιουργήστε μια μονάδα εξισορρόπησης φόρτου χρησιμοποιώντας το αρχείο ορισμού υπηρεσίας

Μπορείτε να αξιοποιήσετε στο SDK Azure για το 2,5 .NET για να ενημερώσετε την υπηρεσία cloud. Ρυθμίσεις τελικό σημείο για τις υπηρεσίες cloud γίνονται στο αρχείο [ορισμού υπηρεσίας](https://msdn.microsoft.com/library/azure/gg557553.aspx).csdef.

Το παρακάτω παράδειγμα εμφανίζει τη ρύθμιση των παραμέτρων ενός αρχείου servicedefinition.csdef για μια ανάπτυξη του cloud:

Έλεγχος του snipet για το αρχείο .csdef που δημιουργούνται με μια ανάπτυξη του cloud, μπορείτε να δείτε το τελικό σημείο εξωτερικών που έχει ρυθμιστεί ώστε να χρησιμοποιούν θύρες HTTP στη θύρα 10000, 10001 και 10002.


    <ServiceDefinition name=“Tenant“>
    <WorkerRole name=“FERole” vmsize=“Small“>
    <Endpoints>
        <InputEndpoint name=“FE_External_Http” protocol=“http” port=“10000“ />
        <InputEndpoint name=“FE_External_Tcp“  protocol=“tcp“  port=“10001“ />
        <InputEndpoint name=“FE_External_Udp“  protocol=“udp“  port=“10002“ />

        <InputEndpointname=“HTTP_Probe” protocol=“http” port=“80” loadBalancerProbe=“MyProbe“ />

        <InstanceInputEndpoint name=“InstanceEP” protocol=“tcp” localPort=“80“>
           <AllocatePublicPortFrom>
              <FixedPortRange min=“10110” max=“10120“  />
           </AllocatePublicPortFrom>
        </InstanceInputEndpoint>
        <InternalEndpoint name=“FE_InternalEP_Tcp” protocol=“tcp“ />
    </Endpoints>
    </WorkerRole>
    </ServiceDefinition>




## <a name="check-load-balancer-health-status-for-cloud-services"></a>Ελέγξτε την κατάσταση εύρυθμης λειτουργίας της εξισορρόπησης φόρτου για τις υπηρεσίες cloud


Ακολουθεί ένα παράδειγμα ενός δοκιμή του εύρυθμης λειτουργίας:

        <LoadBalancerProbes>
        <LoadBalancerProbe name=“MyProbe” protocol=“http” path=“Probe.aspx” intervalInSeconds=“5” timeoutInSeconds=“100“ />
        </LoadBalancerProbes>

Η μονάδα εξισορρόπησης φόρτου συνδυάζει τις πληροφορίες από το τελικό σημείο και τις πληροφορίες για τη δοκιμή του για να δημιουργήσετε μια διεύθυνση URL με τη μορφή http://{DIP του VM}:80/Probe.aspx που μπορεί να χρησιμοποιηθεί για την εύρυθμη λειτουργία της υπηρεσίας του ερωτήματος.

Η υπηρεσία εντοπίζει περιοδικό καθετήρες από την ίδια διεύθυνση IP. Αυτή είναι η αίτηση δοκιμή του εύρυθμης λειτουργίας που προέρχονται από τον κεντρικό υπολογιστή του κόμβου όπου εκτελείται η εικονική μηχανή.
Η υπηρεσία πρέπει να απαντήσετε με κωδικό κατάστασης HTTP 200 για τη μονάδα εξισορρόπησης φόρτου για να λαμβάνεται ως δεδομένο ότι η υπηρεσία είναι σε καλή κατάσταση. Τυχόν άλλες κωδικό κατάστασης HTTP (για παράδειγμα 503) απευθείας λαμβάνει την εικονική μηχανή εκτός περιστροφής.

Τον ορισμό δοκιμή του ελέγχου επίσης τη συχνότητα τη διερεύνηση. Στην περίπτωσή μας παραπάνω, την εξισορρόπηση φόρτου είναι Διερεύνηση το τελικό σημείο κάθε 5 δευτερόλεπτα. Εάν δεν υπάρχει απάντηση θετικό λαμβάνεται για 10 δευτερόλεπτα (δύο διαστήματα δοκιμή του), λαμβάνεται η δοκιμή του προς τα κάτω και η εικονική μηχανή λαμβάνεται από περιστροφής. Ομοίως, εάν η υπηρεσία είναι εκτός των περιστροφής και ληφθεί ένα θετικό απάντηση, την υπηρεσία τοποθετείται ξανά αμέσως περιστροφή. Εάν η υπηρεσία διακυμάνσεις ανάμεσα σε καλή κατάσταση και κατεστραμμένες, την εξισορρόπηση φόρτου να αποφασίσετε να καθυστερήσετε την εκ νέου εισαγωγή της υπηρεσίας Επιστροφή στην περιστροφή μέχρι να είναι σε καλή κατάσταση για έναν αριθμό καθετήρες.

Επιλέξτε το σχήμα ορισμού υπηρεσίας για τη [Δοκιμή του εύρυθμης λειτουργίας](https://msdn.microsoft.com/library/azure/jj151530.aspx) για περισσότερες πληροφορίες.

## <a name="next-steps"></a>Επόμενα βήματα

[Γρήγορα αποτελέσματα με τη ρύθμιση των παραμέτρων μιας εσωτερικής εξισορρόπηση φόρτου](load-balancer-get-started-ilb-arm-ps.md)

[Ρύθμιση παραμέτρων μιας λειτουργίας διανομής εξισορρόπησης φόρτου](load-balancer-distribution-mode.md)

[Ρύθμιση παραμέτρων αδράνειας TCP χρονικού ορίου για την εξισορρόπηση φόρτου](load-balancer-tcp-idle-timeout.md)

