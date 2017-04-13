
<properties
   pageTitle="Azure καθοδήγηση | μοτίβα & πρακτικές | Microsoft Azure"
   description="Αρχιτεκτονικές Azure αναφοράς"
   services=""
   documentationCenter="na"
   authors="bennage"
   manager="marksou"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="christb"/>

# <a name="azure-reference-architectures"></a>Αρχιτεκτονικές Azure αναφοράς

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

_Αυτό το περιεχόμενο είναι ενεργή ανάπτυξη. Είναι χρήσιμο σήμερα, ώστε να πραγματοποιούμε το διαθέσιμο για προεπισκόπηση. Εκτιμούμε τα σχόλιά σας._

Μας αρχιτεκτονικών αναφοράς έχουν τακτοποιηθεί με το σενάριο, με πολλούς σχετικά αρχιτεκτονικές ομαδοποιημένα.
Κάθε μεμονωμένα αρχιτεκτονική προσφέρει προτεινόμενες πρακτικές και καθιερωμένα βήματα, καθώς και ένα στοιχείο εκτελέσιμο που ενσωματώνει τις συστάσεις.
Πολλά από τα αρχιτεκτονικές είναι προοδευτική; Δημιουργία επάνω από το προηγούμενο αρχιτεκτονικές που έχουν λιγότερες απαιτήσεις.

## <a name="designing-your-infrastructure-for-resiliency"></a>Σχεδίαση υποδομής σας για ανοχή

Αυτήν τη σειρά αρχίζει με προτεινόμενες πρακτικές για βέλτιστη ρύθμιση Εικονική και culminates σε μια ανάπτυξη πολλών περιοχών με ανακατεύθυνσης.

- [Εκτέλεση Windows Εικονική σε Azure](guidance-compute-single-vm.md)
- [Εκτελείται μια Εικονική Linux σε Azure](guidance-compute-single-vm-linux.md)
- [Εκτέλεση πολλών ΣΠΣ για κλιμάκωση και διαθεσιμότητα](guidance-compute-multi-vm.md)
- [Εκτελεί Windows ΣΠΣ για μια αρχιτεκτονική N-επιπέδων](guidance-compute-n-tier-vm.md)
- [Εκτέλεση ΣΠΣ Linux για μια αρχιτεκτονική N-επιπέδων](guidance-compute-n-tier-vm-linux.md)
- [Εκτέλεση ΣΠΣ των Windows σε πολλές περιοχές για υψηλή διαθεσιμότητα](guidance-compute-multiple-datacenters.md)
- [Εκτέλεση ΣΠΣ Linux σε πολλές περιοχές για υψηλή διαθεσιμότητα](guidance-compute-multiple-datacenters-linux.md)

## <a name="connecting-your-on-premises-network-to-azure"></a>Σύνδεση το δίκτυο εσωτερικής εγκατάστασης με Azure

Από την επίδειξη τους τρόπους για τη σύνδεση του υπάρχοντος δικτύου με Azure ξεκινά αυτήν τη σειρά. Στη συνέχεια, επεκτείνεται για να καλύψετε απαιτήσεις για τη διαθεσιμότητα και την ασφάλεια.

- [Εφαρμογή μιας υβριδικής αρχιτεκτονική δικτύου με Azure και εσωτερικής VPN](guidance-hybrid-network-vpn.md)
- [Εφαρμογή ενός υβριδικού αρχιτεκτονική δικτύου με Azure ExpressRoute](guidance-hybrid-network-expressroute.md)
- [Εφαρμογή μια αρχιτεκτονική δικτύου ιδιαίτερα διαθέσιμο υβριδικό](guidance-hybrid-network-expressroute-vpn-failover.md)

## <a name="securing-your-hybrid-network"></a>Ασφάλιση του υβριδικού δικτύου

Αυτήν τη σειρά καλύπτει αποδεδειγμένη πρακτικές σχετικά με τη δημιουργία DMZ στο Azure ασφαλείς συνδέσεις που προέρχονται από το κέντρο δεδομένων εσωτερικής εγκατάστασης και στο Internet.

- [Εφαρμογή ενός DMZ μεταξύ Azure και το κέντρο δεδομένων εσωτερικής εγκατάστασης](guidance-iaas-ra-secure-vnet-hybrid.md)
- [Εφαρμογή ενός DMZ μεταξύ Azure και στο Internet](guidance-iaas-ra-secure-vnet-dmz.md)

## <a name="providing-identity-services"></a>Παροχή ταυτότητας υπηρεσιών

Αυτήν τη σειρά που ξεκινά με το οποίο παρουσιάζει τον τρόπο για να χρησιμοποιήσετε Azure Active Directory για να δώσετε τον έλεγχο ταυτότητας χρήστη στο Azure. Στη συνέχεια, επεκτείνεται για να καλύψετε σύνθετα σενάρια επέκταση υποδομής σας ΠΡΟΣΘΈΤΕΙ σε Azure και χρήση ADFS για ανάθεση.

- [Εφαρμογή καταλόγου Azure Active Directory](./guidance-identity-aad.md)
- [Επέκταση υπηρεσίες καταλόγου Active Directory (ΠΡΟΣΘΈΤΕΙ) για να Azure](./guidance-identity-adds-extend-domain.md)
- [Δημιουργία μιας δομών πόρων υπηρεσιών καταλόγου Active Directory (ΠΡΟΣΘΈΤΕΙ) στο Azure](./guidance-identity-adds-resource-forest.md)
- [Εφαρμογή υπηρεσίας καταλόγου Active Directory Federation Services (ADFS) στο Azure](./guidance-identity-adfs.md)

## <a name="architecting-scalable-web-application-using-azure-paas"></a>Εξειδικευμένος εφαρμογής web με χρήση Azure PaaS

Αυτήν τη σειρά καλύπτει συστάσεις για τη δημιουργία εφαρμογών web μεταβλητού μεγέθους και ιδιαίτερα διαθέσιμο. 

- [Εφαρμογή του βασικού web](guidance-web-apps-basic.md)
- [Βελτίωση της κλιμάκωση σε μια εφαρμογή web](guidance-web-apps-scalability.md)
- [Εφαρμογή Web με υψηλή διαθεσιμότητα](guidance-web-apps-multi-region.md)
