<properties
   pageTitle="Δείγματα ρύθμισης παραμέτρων δρομολογητή πελατών ExpressRoute | Microsoft Azure"
   description="Αυτή η σελίδα παρέχει δείγματα ρύθμισης παραμέτρων δρομολογητή για δρομολογητές Cisco και Juniper."
   documentationCenter="na"
   services="expressroute"
   authors="cherylmc"
   manager="carmonm"
   editor="" />
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/10/2016"
   ms.author="cherylmc"/>

# <a name="router-configuration-samples-to-setup-and-manage-nat"></a>Δείγματα ρύθμισης παραμέτρων δρομολογητή εγκατάσταση και διαχείριση NAT

Αυτή η σελίδα παρέχει δείγματα ρύθμισης παραμέτρων NAT για δρομολογητές σειρά Cisco ASA και Juniper SRX. Αυτές που προορίζονται να δείγματα για οδηγίες μόνο και δεν πρέπει να χρησιμοποιούνται ως έχει. Μπορείτε να εργαστείτε με τον προμηθευτή επιστρέφοντας κατάλληλες ρυθμίσεις παραμέτρων για το δίκτυό σας. 

>[AZURE.IMPORTANT] Δείγματα σε αυτήν τη σελίδα προορίζονται αποκλειστικά για οδηγίες. Πρέπει να εργάζεστε με τον προμηθευτή πωλήσεων / technical ομάδας και η ομάδα σας κοινωνικής δικτύωσης επιστρέφοντας κατάλληλες ρυθμίσεις παραμέτρων ώστε να ανταποκρίνεται στις ανάγκες σας. Η Microsoft δεν θα υποστηρίζει θέματα που σχετίζονται με ρυθμίσεις παραμέτρων που αναφέρονται σε αυτήν τη σελίδα. Πρέπει να επικοινωνήσετε με τον προμηθευτή της συσκευής για θέματα υποστήριξης.

Παρακάτω παραδείγματα ρύθμισης παραμέτρων δρομολογητή εφαρμόζονται σε δημόσια Azure και Microsoft peerings. Δεν πρέπει να ρυθμίσετε NAT για Azure διεισδύουν ιδιωτικό. Αναθεώρηση [απαιτήσεις ExpressRoute NAT](expressroute-nat.md) και [ExpressRoute peerings](expressroute-circuit-peerings.md) για περισσότερες λεπτομέρειες.

**Σημείωση:** ΠΡΈΠΕΙ να χρησιμοποιήσετε ξεχωριστό χώρους συγκέντρωσης NAT IP για τη σύνδεση στο internet και ExpressRoute. Χρησιμοποιώντας το ίδιο χώρο συγκέντρωσης NAT IP μέσω του internet και ExpressRoute θα έχει ως αποτέλεσμα ασύμμετρη δρομολόγηση και απώλεια σύνδεσης.

## <a name="cisco-asa-firewalls"></a>Τείχη προστασίας ASA Cisco

### <a name="pat-configuration-for-traffic-from-customer-network-to-microsoft"></a>Ρύθμιση παραμέτρων ΜΑΡΊΑ για την κίνηση από το δίκτυο πελατών της Microsoft

    object network MSFT-PAT
      range <SNAT-START-IP> <SNAT-END-IP>
    
    
    object-group network MSFT-Range
      network-object <IP> <Subnet_Mask>
    
    object-group network on-prem-range-1
      network-object <IP> <Subnet-Mask>
    
    object-group network on-prem-range-2
      network-object <IP> <Subnet-Mask>
    
    object-group network on-prem
      network-object object on-prem-range-1
      network-object object on-prem-range-2
    
    nat (outside,inside) source dynamic on-prem pat-pool MSFT-PAT destination static MSFT-Range MSFT-Range

### <a name="pat-configuration-for-traffic-from-microsoft-to-customer-network"></a>Ρύθμιση παραμέτρων ΜΑΡΊΑ για την κίνηση δικτύου πελατών από τη Microsoft

#### <a name="interfaces-and-direction"></a>Διασυνδέσεων και κατεύθυνση:
    Source Interface (where the traffic enters the ASA): inside
    Destination Interface (where the traffic exits the ASA): outside

#### <a name="configuration"></a>Ρύθμιση παραμέτρων:
Σύνολο NAT:

    object network outbound-PAT
        host <NAT-IP>

Διακομιστής προορισμού:

    object network Customer-Network
        network-object <IP> <Subnet-Mask>

Ομάδα αντικειμένου για διευθύνσεις IP πελάτη

    object-group network MSFT-Network-1
        network-object <MSFT-IP> <Subnet-Mask>
    
    object-group network MSFT-PAT-Networks
        network-object object MSFT-Network-1

Εντολές NAT:

    nat (inside,outside) source dynamic MSFT-PAT-Networks pat-pool outbound-PAT destination static Customer-Network Customer-Network


## <a name="juniper-srx-series-routers"></a>Juniper SRX σειρά δρομολογητές 

### <a name="1-create-redundant-ethernet-interfaces-for-the-cluster"></a>1. Δημιουργία πλεονάζοντα διασυνδέσεων Ethernet για το σύμπλεγμα

    interfaces {
        reth0 {
            description "To Internal Network";
            vlan-tagging;
            redundant-ether-options {
                redundancy-group 1;
            }
            unit 100 {
                vlan-id 100;
                family inet {
                    address <IP-Address/Subnet-mask>;
                }
            }
        }
        reth1 {
            description "To Microsoft via Edge Router";
            vlan-tagging;
            redundant-ether-options {
                redundancy-group 2;
            }
            unit 100 {
                description "To Microsoft via Edge Router";
                vlan-id 100;
                family inet {
                    address <IP-Address/Subnet-mask>;
                }
            }
        }
    }


### <a name="2-create-two-security-zones"></a>2. Δημιουργήστε δύο ζώνες ασφαλείας

 - Ζώνη αξιοπιστίας για εσωτερικό σας δίκτυο και Untrust ζώνης για εξωτερικό δίκτυο αντικριστές δρομολογητές άκρου
 - Εκχώρηση κατάλληλες διασυνδέσεις για τις ζώνες
 - Δυνατότητα υπηρεσιών στις διασυνδέσεις


    ασφάλεια {ζώνες {αξιοπιστίας ζώνη ασφαλείας {host εισερχομένων-κυκλοφορίας {-υπηρεσίες συστήματος {ping;                  } πρωτόκολλα {πρωτόκολλο bgp;                  διασυνδέσεων}} {reth0.100;              }} Untrust ζώνη ασφαλείας {host εισερχομένων-κυκλοφορίας {-υπηρεσίες συστήματος {ping;                  } πρωτόκολλα {πρωτόκολλο bgp;                  διασυνδέσεων}} {reth1.100;              }          }      }  }


### <a name="3-create-security-policies-between-zones"></a>3. Δημιουργία πολιτικών ασφάλειας μεταξύ των ζωνών
 
    security {
        policies {
            from-zone Trust to-zone Untrust {
                policy allow-any {
                    match {
                        source-address any;
                        destination-address any;
                        application any;
                    }
                    then {
                        permit;
                    }
                }
            }
            from-zone Untrust to-zone Trust {
                policy allow-any {
                    match {
                        source-address any;
                        destination-address any;
                        application any;
                    }
                    then {
                        permit;
                    }
                }
            }
        }
    }


### <a name="4-configure-nat-policies"></a>4. ρύθμιση παραμέτρων πολιτικών NAT
 - Δημιουργήστε δύο σύνολα NAT. Μία θα χρησιμοποιηθεί για την κυκλοφορία NAT εξερχομένων στη Microsoft και άλλες από τη Microsoft για τον πελάτη.
 - Δημιουργία κανόνων για NAT το αντίστοιχο κίνηση

        security {
            nat {
                source {
                    pool SNAT-To-ExpressRoute {
                        routing-instance {
                            External-ExpressRoute;
                        }
                        address {
                            <NAT-IP-address/Subnet-mask>;
                        }
                    }
                    pool SNAT-From-ExpressRoute {
                        routing-instance {
                            Internal;
                        }
                        address {
                            <NAT-IP-address/Subnet-mask>;
                        }
                    }
                    rule-set Outbound_NAT {
                        from routing-instance Internal;
                        to routing-instance External-ExpressRoute;
                        rule SNAT-Out {
                            match {
                                source-address 0.0.0.0/0;
                            }
                            then {
                                source-nat {
                                    pool {
                                        SNAT-To-ExpressRoute;
                                    }
                                }
                            }
                        }
                    }
                    rule-set Inbound-NAT {
                        from routing-instance External-ExpressRoute;
                        to routing-instance Internal;
                        rule SNAT-In {
                            match {
                                source-address 0.0.0.0/0;
                            }
                            then {
                                source-nat {
                                    pool {
                                        SNAT-From-ExpressRoute;
                                    }
                                }
                            }
                        }
                    }
                }
            }
        }


### <a name="5-configure-bgp-to-advertise-selective-prefixes-in-each-direction"></a>5. ρύθμιση παραμέτρων το πρωτόκολλο BGP για να κοινοποιήσετε επιλεκτική προθέματα σε κάθε κατεύθυνση

Ανατρέξτε στο δείγματα στη σελίδα [δείγματα ρύθμισης παραμέτρων δρομολόγησης](expressroute-config-samples-routing.md) .

### <a name="6-create-policies"></a>6. Δημιουργία πολιτικών

    routing-options {
                  autonomous-system <Customer-ASN>;
    }
    policy-options {
        prefix-list Microsoft-Prefixes {
            <IP-Address/Subnet-Mask;
            <IP-Address/Subnet-Mask;
        }
        prefix-list private-ranges {
            10.0.0.0/8;
            172.16.0.0/12;
            192.168.0.0/16;
            100.64.0.0/10;
        }
        policy-statement Advertise-NAT-Pools {
            from {
                protocol static;
                route-filter <NAT-Pool-Address/Subnet-mask> prefix-length-range /32-/32;
            }
            then accept;
        }
        policy-statement Accept-from-Microsoft {
            term 1 {
                from {
                    instance External-ExpressRoute;
                    prefix-list-filter Microsoft-Prefixes orlonger;
                }
                then accept;
            }
            term deny {
                then reject;
            }
        }
        policy-statement Accept-from-Internal {
            term no-private {
                from {
                    instance Internal;
                    prefix-list-filter private-ranges orlonger;
                }
                then reject;
            }
            term bgp {
                from {
                    instance Internal;
                    protocol bgp;
                }
                then accept;
            }
            term deny {
                then reject;
            }
        }
    }
    routing-instances {
        Internal {
            instance-type virtual-router;
            interface reth0.100;
            routing-options {
                static {
                    route <NAT-Pool-IP-Address/Subnet-mask> discard;
                }
                instance-import Accept-from-Microsoft;
            }
            protocols {
                bgp {
                    group customer {
                        export <Advertise-NAT-Pools>;
                        peer-as <Customer-ASN-1>;
                        neighbor <BGP-Neighbor-IP-Address>;
                    }
                }
            }
        }
        External-ExpressRoute {
            instance-type virtual-router;
            interface reth1.100;
            routing-options {
                static {
                    route <NAT-Pool-IP-Address/Subnet-mask> discard;
                }
                instance-import Accept-from-Internal;
            }
            protocols {
                bgp {
                    group edge-router {
                        export <Advertise-NAT-Pools>;
                        peer-as <Customer-Public-ASN>;
                        neighbor <BGP-Neighbor-IP-Address>;
                    }
                }
            }
        }
    }

## <a name="next-steps"></a>Επόμενα βήματα

Ανατρέξτε στο θέμα [Συνήθεις Ερωτήσεις ExpressRoute](expressroute-faqs.md) για περισσότερες λεπτομέρειες.
