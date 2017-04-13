<properties
   pageTitle="Δημιουργήστε μια εσωτερική εξισορρόπηση φόρτου για τις υπηρεσίες cloud στο μοντέλο κλασική ανάπτυξης | Microsoft Azure"
   description="Μάθετε πώς να δημιουργείτε μια εσωτερική εξισορρόπηση φόρτου με χρήση του PowerShell στο μοντέλο κλασική ανάπτυξης"
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
   ms.date="02/09/2016"
   ms.author="sewhee" />

# <a name="get-started-creating-an-internal-load-balancer-classic-for-cloud-services"></a>Γρήγορα αποτελέσματα με τη δημιουργία μιας εσωτερικής εξισορρόπηση φόρτου (κλασική) για τις υπηρεσίες cloud

[AZURE.INCLUDE [load-balancer-get-started-ilb-classic-selectors-include.md](../../includes/load-balancer-get-started-ilb-classic-selectors-include.md)]

<BR>

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]Μάθετε πώς μπορείτε να [εκτελέσετε αυτά τα βήματα χρησιμοποιώντας το μοντέλο από διαχειριστή πόρων](load-balancer-get-started-ilb-arm-ps.md).


## <a name="configure-internal-load-balancer-for-cloud-services"></a>Ρύθμιση παραμέτρων του εσωτερικού εξισορρόπηση φόρτου για τις υπηρεσίες cloud

Εσωτερική εξισορρόπηση φόρτου υποστηρίζεται για εικονικές μηχανές και τις υπηρεσίες cloud. Ένα τελικό σημείο εξισορρόπησης εσωτερικό φόρτωσης δημιουργήθηκε σε μια υπηρεσία cloud που βρίσκεται έξω από ένα τοπικές εικονικό δίκτυο θα είναι προσβάσιμα μόνο μέσα την υπηρεσία cloud.

Η ρύθμιση παραμέτρων εξισορρόπησης εσωτερικό φόρτωσης έχει θα οριστεί κατά τη δημιουργία της πρώτης ανάπτυξης στην υπηρεσία cloud, όπως φαίνεται στο παρακάτω δείγμα.

>[AZURE.IMPORTANT] Για ένα εικονικό δίκτυο που έχετε ήδη δημιουργήσει για την ανάπτυξη cloud είναι προϋπόθεση για να εκτελέσετε τα παρακάτω βήματα. Θα χρειαστεί το όνομα εικονικού δικτύου και υποδικτύου για να δημιουργήσετε το εσωτερικό εξισορρόπηση φόρτου.

### <a name="step-1"></a>Βήμα 1

Ανοίξτε το αρχείο ρύθμισης παραμέτρων υπηρεσίας (.cscfg) για την ανάπτυξη cloud στο Visual Studio και προσθέστε την παρακάτω ενότητα για να δημιουργήσετε το εσωτερικό εξισορρόπηση φόρτου κάτω από την τελευταία "`</Role>`" στοιχείου για τη ρύθμιση παραμέτρων δικτύου.




    <NetworkConfiguration>
      <LoadBalancers>
        <LoadBalancer name="name of the load balancer">
          <FrontendIPConfiguration type="private" subnet="subnet-name" staticVirtualNetworkIPAddress="static-IP-address"/>
        </LoadBalancer>
      </LoadBalancers>
    </NetworkConfiguration>


Ας προσθέσουμε τις τιμές για το αρχείο ρύθμισης παραμέτρων δικτύου για να δείτε πώς θα εμφανίζονται. Στο παράδειγμα, ας υποθέσουμε ότι έχετε δημιουργήσει ένα υποδίκτυο που ονομάζεται "test_vnet" με ένα 10.0.0.0/24 υποδικτύου που ονομάζεται test_subnet και στατική IP 10.0.0.4. Η μονάδα εξισορρόπησης φόρτου θα ονομαστεί testLB.

    <NetworkConfiguration>
      <LoadBalancers>
        <LoadBalancer name="testLB">
          <FrontendIPConfiguration type="private" subnet="test_subnet" staticVirtualNetworkIPAddress="10.0.0.4"/>
        </LoadBalancer>
      </LoadBalancers>
    </NetworkConfiguration>

Για περισσότερες πληροφορίες σχετικά με το σχήμα εξισορρόπησης φόρτου, ανατρέξτε στο θέμα [Προσθήκη μονάδα εξισορρόπησης φόρτου](https://msdn.microsoft.com/library/azure/dn722411.aspx).

### <a name="step-2"></a>Βήμα 2


Αλλάξτε το αρχείο ορισμού (.csdef) υπηρεσίας για να προσθέσετε τα τελικά σημεία του εσωτερικού εξισορρόπηση φόρτου. Τη στιγμή που δημιουργείται μια παρουσία ρόλο, το αρχείο ορισμού υπηρεσίας θα προσθέσει τις παρουσίες ρόλο του εσωτερικού εξισορρόπηση φόρτου.


    <WorkerRole name="worker-role-name" vmsize="worker-role-size" enableNativeCodeExecution="[true|false]">
      <Endpoints>
        <InputEndpoint name="input-endpoint-name" protocol="[http|https|tcp|udp]" localPort="local-port-number" port="port-number" certificate="certificate-name" loadBalancerProbe="load-balancer-probe-name" loadBalancer="load-balancer-name" />
      </Endpoints>
    </WorkerRole>

Παρακολουθείτε τις ίδιες τιμές από το παραπάνω παράδειγμα, ας προσθέσουμε τις τιμές για το αρχείο ορισμού υπηρεσίας.

    <WorkerRole name=WorkerRole1" vmsize="A7" enableNativeCodeExecution="[true|false]">
      <Endpoints>
        <InputEndpoint name="endpoint1" protocol="http" localPort="80" port="80" loadBalancer="testLB" />
      </Endpoints>
    </WorkerRole>

Η κυκλοφορία του δικτύου θα εξισορρόπηση με τη χρήση της θύρας 80 για εισερχόμενες αιτήσεις, Αποστολή σε παρουσίες των ρόλων εργαζόμενου επίσης στη θύρα 80 testLB μονάδα εξισορρόπησης φόρτου.


## <a name="next-steps"></a>Επόμενα βήματα

[Ρύθμιση παραμέτρων μια λειτουργία διανομής εξισορρόπησης φόρτου χρησιμοποιώντας συσχέτισης IP προέλευσης](load-balancer-distribution-mode.md)

[Ρύθμιση παραμέτρων αδράνειας TCP χρονικού ορίου για την εξισορρόπηση φόρτου](load-balancer-tcp-idle-timeout.md)