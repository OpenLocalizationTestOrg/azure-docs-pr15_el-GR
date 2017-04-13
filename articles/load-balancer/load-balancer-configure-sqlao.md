<properties
   pageTitle="Ρύθμιση παραμέτρων εξισορρόπηση φόρτου για SQL πάντα στο | Microsoft Azure"
   description="Ρύθμιση παραμέτρων εξισορρόπηση φόρτου για εργασία με την SQL πάντα σε και πώς μπορείτε να αξιοποιήσετε powershell για τη δημιουργία εξισορρόπηση φόρτου για την εφαρμογή SQL"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />

# <a name="configure-load-balancer-for-sql-always-on"></a>Ρύθμιση παραμέτρων εξισορρόπηση φόρτου για SQL πάντα στο

Ομάδες διαθεσιμότητας AlwaysOn SQL Server τώρα μπορεί να εκτελεστεί με ILB. Διαθεσιμότητα ομάδας είναι λύση flagship του SQL Server για υψηλή διαθεσιμότητα και την καταστροφή αποκατάστασης. Η διαθεσιμότητα ακρόαση ομάδα επιτρέπει στις εφαρμογές απρόσκοπτα σύνδεσης στην κύρια ρεπλίκα, ανεξάρτητα από τον αριθμό των τα αντίγραφα στη ρύθμιση παραμέτρων προγράμματος-πελάτη.

Το όνομα ακρόασης (DNS) που έχει αντιστοιχιστεί σε μια διεύθυνση IP εξισορρόπηση φόρτου και του Azure εξισορρόπηση φόρτου κατευθύνει τα εισερχόμενα δεδομένα μόνο στον πρωτεύοντα διακομιστή του συνόλου αντιγράφου.

Μπορείτε να χρησιμοποιήσετε ILB υποστήριξης για τα τελικά σημεία AlwaysOn SQL Server (ακρόασης). Τώρα έχετε ελέγχετε περισσότερο τη δυνατότητα πρόσβασης από το ακροατήριο και να επιλέξετε τη διεύθυνση IP εξισορρόπηση φόρτου από ένα συγκεκριμένο υποδίκτυο στο δίκτυο εικονικού (VNet).

Με τη χρήση ILB στη λειτουργία ακρόασης, το τελικό σημείο του SQL server (π.χ. διακομιστή = tcp:ListenerName, 1433; βάσης δεδομένων = όνομα βάσης δεδομένων) είναι δυνατή η πρόσβαση μόνο από:

- Υπηρεσίες και ΣΠΣ στο ίδιο δίκτυο εικονικές
- Υπηρεσίες και ΣΠΣ από δίκτυο συνδεδεμένο εσωτερικής εγκατάστασης
- Υπηρεσίες και ΣΠΣ από διασυνδεδεμένων VNets

![ILB_SQLAO_NewPic](./media/load-balancer-configure-sqlao/sqlao1.png)

Σχήμα 1 - AlwaysOn SQL που έχουν ρυθμιστεί με μονάδα εξισορρόπησης φόρτου μέσω Internet

## <a name="add-internal-load-balancer-to-the-service"></a>Προσθήκη εσωτερικής εξισορρόπηση φόρτου στην υπηρεσία

1. Στο παρακάτω παράδειγμα, θα σας θα ρυθμίσει μια εικονική δικτύου που περιέχει ένα δευτερεύον που ονομάζεται "Υποδικτύου-1":

        Add-AzureInternalLoadBalancer -InternalLoadBalancerName ILB_SQL_AO -SubnetName Subnet-1 -ServiceName SqlSvc

2. Προσθέστε τα τελικά σημεία εξισορρόπησης φόρτου για ILB σε κάθε εικονική Μηχανή

        Get-AzureVM -ServiceName SqlSvc -Name sqlsvc1 | Add-AzureEndpoint -Name "LisEUep" -LBSetName "ILBSet1" -Protocol tcp -LocalPort 1433 -PublicPort 1433 -ProbePort 59999 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 –
        DirectServerReturn $true -InternalLoadBalancerName ILB_SQL_AO | Update-AzureVM

        Get-AzureVM -ServiceName SqlSvc -Name sqlsvc2 | Add-AzureEndpoint -Name "LisEUep" -LBSetName "ILBSet1" -Protocol tcp -LocalPort 1433 -PublicPort 1433 -ProbePort 59999 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 –DirectServerReturn $true -InternalLoadBalancerName ILB_SQL_AO | Update-AzureVM

    Στο παραπάνω παράδειγμα, έχετε 2 Εικονική που ονομάζεται "sqlsvc1" και "sqlsvc2" εκτελείται στο cloud υπηρεσίας "Sqlsvc". Αφού δημιουργήσετε το ILB με το διακόπτη "DirectServerReturn", μπορείτε να προσθέσετε τα τελικά σημεία εξισορρόπησης φόρτου για να το ILB για να επιτρέψετε σε SQL για να ρυθμίσετε τις παραμέτρους του ακροατών για τις ομάδες διαθεσιμότητα.

Για περισσότερες πληροφορίες σχετικά με την SQL AlwaysOn, ανατρέξτε στο θέμα [Ρύθμιση παραμέτρων μιας εσωτερικής εξισορρόπηση φόρτου για μια ομάδα διαθεσιμότητας AlwaysOn στο Azure](../virtual-machines/virtual-machines-windows-portal-sql-alwayson-int-listener.md).

## <a name="see-also"></a>Δείτε επίσης

[Γρήγορα αποτελέσματα με τη ρύθμιση των παραμέτρων Internet αντικριστές εξισορρόπηση φόρτου](load-balancer-get-started-internet-arm-ps.md)

[Γρήγορα αποτελέσματα με τη ρύθμιση των παραμέτρων μιας εσωτερικής εξισορρόπηση φόρτου](load-balancer-get-started-ilb-arm-ps.md)

[Ρύθμιση παραμέτρων μιας λειτουργίας διανομής εξισορρόπησης φόρτου](load-balancer-distribution-mode.md)

[Ρύθμιση παραμέτρων αδράνειας TCP χρονικού ορίου για την εξισορρόπηση φόρτου](load-balancer-tcp-idle-timeout.md)
