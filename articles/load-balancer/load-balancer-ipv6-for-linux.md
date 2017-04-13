<properties
    pageTitle="Ρύθμιση παραμέτρων DHCPv6 για ΣΠΣ Linux | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να ρυθμίσετε τις παραμέτρους DHCPv6 για ΣΠΣ Linux."
    services="load-balancer"
    documentationCenter="na"
    authors="sdwheeler"
    manager="carmonm"
    editor=""
    keywords="το IPv6, azure εξισορρόπηση φόρτου, διπλή στοίβα, δημόσια ip, εγγενούς ipv6, mobile, iot"
/>
<tags
    ms.service="load-balancer"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="09/14/2016"
    ms.author="sewhee"
/>

# <a name="configuring-dhcpv6-for-linux-vms"></a>Ρύθμιση παραμέτρων DHCPv6 για ΣΠΣ Linux

Ορισμένες από τις εικόνες εικονική μηχανή Linux από το Azure Marketplace δεν έχουν DHCPv6 έχει ρυθμιστεί από προεπιλογή. Για την υποστήριξη IPv6, DHCPv6 πρέπει να ρυθμιστεί στο εντός της κατανομής Linux λειτουργικό σύστημα που χρησιμοποιείτε. Διαφορετικά κατανομές Linux έχουν διαφορετικούς τρόπους ρύθμισης παραμέτρων DHCPv6 επειδή χρησιμοποιούν διαφορετικά πακέτα.

>[AZURE.NOTE] Πρόσφατες SUSE Linux και CoreOS εικόνες από το Azure Marketplace έχουν προκαθορισμένες ρυθμίσεις παραμέτρων με DHCPv6. Χωρίς πρόσθετες αλλαγές απαιτούνται όταν χρησιμοποιείτε αυτές τις εικόνες.

Αυτό το έγγραφο περιγράφει πώς μπορείτε να ενεργοποιήσετε DHCPv6, έτσι ώστε το Linux εικονική μηχανή λαμβάνει μια διεύθυνση IPv6.

>[AZURE.WARNING] Εσφαλμένη επεξεργασία αρχείων ρύθμισης παραμέτρων δικτύου μπορεί να προκαλέσει την απώλεια πρόσβαση στο δίκτυο για να σας Εικονική. Συνιστάται να ελέγξετε τις αλλαγές ρύθμισης παραμέτρων σε συστήματα μη παραγωγής. Τις οδηγίες σε αυτό το άρθρο έχει ελεγχθεί σε τις πιο πρόσφατες εκδόσεις των εικόνων Linux από το Azure Marketplace. Ανατρέξτε στην τεκμηρίωση για τη συγκεκριμένη έκδοση του Linux για πιο λεπτομερείς οδηγίες.

## <a name="ubuntu"></a>Ubuntu

1. Επεξεργαστείτε το αρχείο `/etc/dhcp/dhclient6.conf` και προσθέστε την ακόλουθη γραμμή:

        timeout 10;

2. Επεξεργαστείτε τη ρύθμιση παραμέτρων δικτύου για το περιβάλλον εργασίας eth0 με τις ακόλουθες ρυθμίσεις:

    * Στον **Ubuntu 12.04 και 14.04**, επεξεργαστείτε το αρχείο`/etc/network/interfaces.d/eth0.cfg`
    * Στον **Ubuntu 16.04**, επεξεργαστείτε το αρχείο`/etc/network/interfaces.d/50-cloud-init.cfg`

    ```bash
    iface eth0 inet6 auto
        up sleep 5
        up dhclient -1 -6 -cf /etc/dhcp/dhclient6.conf -lf /var/lib/dhcp/dhclient6.eth0.leases -v eth0 || true
    ```

3. Ανανέωση διεύθυνση IPv6:

    ```bash
    # sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="debian"></a>Debian

1. Επεξεργαστείτε το αρχείο `/etc/dhcp/dhclient6.conf` και προσθέστε την ακόλουθη γραμμή:

        timeout 10;

2. Επεξεργαστείτε το αρχείο `/etc/network/interfaces` και προσθέστε τις ακόλουθες ρυθμίσεις παραμέτρων:

        iface eth0 inet6 auto
            up sleep 5
            up dhclient -1 -6 -cf /etc/dhcp/dhclient6.conf -lf /var/lib/dhcp/dhclient6.eth0.leases -v eth0 || true

3. Ανανέωση διεύθυνση IPv6:

    ```bash
    # sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="rhel--centos--oracle-linux"></a>RHEL / CentOS / Oracle Linux

1. Επεξεργαστείτε το αρχείο `/etc/sysconfig/network` και προσθέστε την ακόλουθη παράμετρο:

        NETWORKING_IPV6=yes

2. Επεξεργαστείτε το αρχείο `/etc/sysconfig/network-scripts/ifcfg-eth0` και προσθέστε τις ακόλουθες δύο παραμέτρους:

        IPV6INIT=yes
        DHCPV6C=yes

3. Ανανέωση διεύθυνση IPv6:

    ```bash
    # sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="sles-11--opensuse-13"></a>SLES 11 & openSUSE 13

Πρόσφατες SLES και openSUSE εικόνων στο Azure έχουν προκαθορισμένες ρυθμίσεις παραμέτρων με DHCPv6. Χωρίς πρόσθετες αλλαγές απαιτούνται όταν χρησιμοποιείτε αυτές τις εικόνες. Εάν έχετε μια Εικονική που βασίζεται σε μια εικόνα SUSE παλαιότερων ή προσαρμοσμένο, χρησιμοποιήστε τα ακόλουθα βήματα:

1. Εγκαταστήστε το `dhcp-client` συσκευασία, εάν χρειάζεται:

    ```bash
    # sudo zypper install dhcp-client
    ```

2. Επεξεργαστείτε το αρχείο `/etc/sysconfig/network/ifcfg-eth0` και προσθέστε την ακόλουθη παράμετρο:

        DHCLIENT6_MODE='managed'

3. Ανανέωση της διεύθυνσης IPv6:

    ```bash
    # sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="sles-12-and-opensuse-leap"></a>SLES 12 και openSUSE δίσεκτο

Πρόσφατες SLES και openSUSE εικόνων στο Azure έχουν προκαθορισμένες ρυθμίσεις παραμέτρων με DHCPv6. Χωρίς πρόσθετες αλλαγές απαιτούνται όταν χρησιμοποιείτε αυτές τις εικόνες. Εάν έχετε μια Εικονική που βασίζεται σε μια εικόνα SUSE παλαιότερων ή προσαρμοσμένο, χρησιμοποιήστε τα ακόλουθα βήματα:

1. Επεξεργαστείτε το αρχείο `/etc/sysconfig/network/ifcfg-eth0` και να αντικαταστήσετε αυτή η παράμετρος

        #BOOTPROTO='dhcp4'

    με την ακόλουθη τιμή:

        BOOTPROTO='dhcp'

2. Προσθέστε την ακόλουθη παράμετρο να `/etc/sysconfig/network/ifcfg-eth0`:

        DHCLIENT6_MODE='managed'

3. Ανανέωση της διεύθυνσης IPv6:

    ```bash
    # sudo ifdown eth0 && sudo ifup eth0
    ```

## <a name="coreos"></a>CoreOS

Πρόσφατες CoreOS εικόνες στο Azure έχουν προκαθορισμένες ρυθμίσεις παραμέτρων με DHCPv6. Χωρίς πρόσθετες αλλαγές απαιτούνται όταν χρησιμοποιείτε αυτές τις εικόνες. Εάν έχετε μια Εικονική που βασίζεται σε μια εικόνα CoreOS παλαιότερων ή προσαρμοσμένο, χρησιμοποιήστε τα ακόλουθα βήματα:

1. Επεξεργαστείτε το αρχείο`/etc/systemd/network/10_dhcp.network`

        [Match]
        eth0

        [Network]
        DHCP=ipv6

2. Ανανέωση της διεύθυνσης IPv6:

    ```bash
    # sudo systemctl restart systemd-networkd
    ```
