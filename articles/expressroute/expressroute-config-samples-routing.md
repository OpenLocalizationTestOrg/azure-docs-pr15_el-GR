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

# <a name="router-configuration-samples-to-setup-and-manage-routing"></a>Δείγματα ρύθμισης παραμέτρων δρομολογητή εγκατάσταση και Διαχείριση δρομολόγησης

Αυτή η σελίδα παρέχει περιβάλλοντος εργασίας και δρομολόγησης δείγματα ρύθμισης παραμέτρων για Cisco IOS-XE και Juniper MX δρομολογητές σειρά. Αυτές που προορίζονται να δείγματα για οδηγίες μόνο και δεν πρέπει να χρησιμοποιούνται ως έχει. Μπορείτε να εργαστείτε με τον προμηθευτή επιστρέφοντας κατάλληλες ρυθμίσεις παραμέτρων για το δίκτυό σας. 

>[AZURE.IMPORTANT] Δείγματα σε αυτήν τη σελίδα προορίζονται αποκλειστικά για οδηγίες. Πρέπει να εργάζεστε με τον προμηθευτή πωλήσεων / technical ομάδας και η ομάδα σας κοινωνικής δικτύωσης επιστρέφοντας κατάλληλες ρυθμίσεις παραμέτρων ώστε να ανταποκρίνεται στις ανάγκες σας. Η Microsoft δεν θα υποστηρίζει θέματα που σχετίζονται με ρυθμίσεις παραμέτρων που αναφέρονται σε αυτήν τη σελίδα. Πρέπει να επικοινωνήσετε με τον προμηθευτή της συσκευής για θέματα υποστήριξης.

Δείγματα ρύθμισης παραμέτρων δρομολογητή παρακάτω εφαρμογή σε όλα peerings. Αναθεώρηση [ExpressRoute δρομολόγησης απαιτήσεις](expressroute-routing.md) και [ExpressRoute peerings](expressroute-circuit-peerings.md) για περισσότερες λεπτομέρειες σχετικά με τη δρομολόγηση.

## <a name="cisco-ios-xe-based-routers"></a>Πεδίο XE IOS Cisco με βάση δρομολογητές

Τα δείγματα σε αυτήν την ενότητα ισχύουν για οποιαδήποτε δρομολογητή εκτελούνται οι οικογένειες OS IOS XE.

### <a name="1-configuring-interfaces-and-sub-interfaces"></a>1. ρύθμιση παραμέτρων διασυνδέσεων και δευτερεύουσες διασυνδέσεις

Θα χρειαστείτε ένα περιβάλλον εργασίας sub ανά διεισδύουν σε κάθε δρομολογητή μπορείτε να συνδεθείτε στη Microsoft. Περιβάλλον εργασίας sub μπορούν να προσδιοριστούν με ένα Αναγνωριστικό εικονικού ΔΙΚΤΎΟΥ ή ένα ζεύγος σωρευμένων αναγνωριστικά VLAN και μια διεύθυνση IP.

#### <a name="dot1q-interface-definition"></a>Ορισμός περιβάλλοντος Dot1Q

Αυτό το δείγμα παρέχει τον ορισμό δευτερεύουσες περιβάλλοντος για μια δευτερεύουσα διασύνδεση με ένα μεμονωμένο αναγνωριστικό VLAN. Το Αναγνωριστικό εικονικού ΔΙΚΤΎΟΥ είναι μοναδικό ανά διεισδύουν. Η τελευταία οκτάδα της διεύθυνσης του IPv4 θα είναι πάντα μονό αριθμό.

    interface GigabitEthernet<Interface_Number>.<Number>
     encapsulation dot1Q <VLAN_ID>
     ip address <IPv4_Address><Subnet_Mask>

#### <a name="qinq-interface-definition"></a>Ορισμός περιβάλλοντος QinQ

Αυτό το δείγμα παρέχει τον ορισμό δευτερεύουσες περιβάλλοντος για μια δευτερεύουσα διασύνδεση με μια δύο αναγνωριστικά VLAN. Το εξωτερικό Αναγνωριστικό εικονικού ΔΙΚΤΎΟΥ (s-ετικέτα), αν χρησιμοποιηθεί παραμένει το ίδιο σε όλα τα peerings. Το εσωτερικό Αναγνωριστικό εικονικού ΔΙΚΤΎΟΥ (c-ετικέτα) είναι μοναδικό ανά διεισδύουν. Η τελευταία οκτάδα της διεύθυνσης του IPv4 θα είναι πάντα μονό αριθμό.

    interface GigabitEthernet<Interface_Number>.<Number>
     encapsulation dot1Q <s-tag> seconddot1Q <c-tag>
     ip address <IPv4_Address><Subnet_Mask>
    
### <a name="2-setting-up-ebgp-sessions"></a>2. ρύθμιση eBGP περίοδοι λειτουργίας

Πρέπει να μπορείτε να ρυθμίσετε μια περίοδο λειτουργίας πρωτόκολλο BGP με τη Microsoft για κάθε διεισδύουν. Το παρακάτω δείγμα σάς επιτρέπει να ρυθμίσετε μια περίοδο λειτουργίας το πρωτόκολλο BGP με τη Microsoft. Εάν η διεύθυνση IPv4 που χρησιμοποιείται για το περιβάλλον εργασίας sub ήταν a.b.c.d, τη διεύθυνση IP του γειτονικού στοιχείου το πρωτόκολλο BGP (Microsoft) θα είναι a.b.c.d+1. Η τελευταία οκτάδα διεύθυνσης IPv4 του γειτονικού στοιχείου πρωτόκολλο BGP θα είναι πάντα άρτιος αριθμός.

    router bgp <Customer_ASN>
     bgp log-neighbor-changes
     neighbor <IP#2_used_by_Azure> remote-as 12076
     !        
     address-family ipv4
     neighbor <IP#2_used_by_Azure> activate
     exit-address-family
    !

### <a name="3-setting-up-prefixes-to-be-advertised-over-the-bgp-session"></a>3. ρύθμιση προθέματα θα κοινοποιηθεί πάνω από την περίοδο λειτουργίας πρωτόκολλο BGP

Μπορείτε να ρυθμίσετε το δρομολογητή σας για να κοινοποιήσετε επιλέξτε προθέματα στη Microsoft. Μπορείτε να το κάνετε χρησιμοποιώντας το παρακάτω δείγμα.

    router bgp <Customer_ASN>
     bgp log-neighbor-changes
     neighbor <IP#2_used_by_Azure> remote-as 12076
     !        
     address-family ipv4
      network <Prefix_to_be_advertised> mask <Subnet_mask>
      neighbor <IP#2_used_by_Azure> activate
     exit-address-family
    !

### <a name="4-route-maps"></a>4. χάρτες δρομολόγηση

Μπορείτε να χρησιμοποιήσετε χάρτες δρομολόγηση και λίστες προθέματος για προθέματα φίλτρο που έχουν μεταδοθεί στο δίκτυό σας. Μπορείτε να χρησιμοποιήσετε το παρακάτω δείγμα για την ολοκλήρωση της εργασίας. Βεβαιωθείτε ότι έχετε εγκαταστήσει το κατάλληλο πρόθεμα λίστες εγκατάστασης.

    router bgp <Customer_ASN>
     bgp log-neighbor-changes
     neighbor <IP#2_used_by_Azure> remote-as 12076
     !        
     address-family ipv4
      network <Prefix_to_be_advertised> mask <Subnet_mask>
      neighbor <IP#2_used_by_Azure> activate
      neighbor <IP#2_used_by_Azure> route-map <MS_Prefixes_Inbound> in
     exit-address-family
    !
    route-map <MS_Prefixes_Inbound> permit 10
     match ip address prefix-list <MS_Prefixes>
    !


## <a name="juniper-mx-series-routers"></a>Δρομολογητές σειρά juniper MX 

Τα δείγματα σε αυτήν την ενότητα ισχύουν για οποιαδήποτε δρομολογητές σειρά Juniper MX.

### <a name="1-configuring-interfaces-and-sub-interfaces"></a>1. ρύθμιση παραμέτρων διασυνδέσεων και δευτερεύουσες διασυνδέσεις

#### <a name="dot1q-interface-definition"></a>Ορισμός περιβάλλοντος Dot1Q

Αυτό το δείγμα παρέχει τον ορισμό δευτερεύουσες περιβάλλοντος για μια δευτερεύουσα διασύνδεση με ένα μεμονωμένο αναγνωριστικό VLAN. Το Αναγνωριστικό εικονικού ΔΙΚΤΎΟΥ είναι μοναδικό ανά διεισδύουν. Η τελευταία οκτάδα της διεύθυνσης του IPv4 θα είναι πάντα μονό αριθμό.

    interfaces {
        vlan-tagging;
        <Interface_Number> {
            unit <Number> {
                vlan-id <VLAN_ID>;
                family inet {
                    address <IPv4_Address/Subnet_Mask>;
                }
            }
        }
    }


#### <a name="qinq-interface-definition"></a>Ορισμός περιβάλλοντος QinQ

Αυτό το δείγμα παρέχει τον ορισμό δευτερεύουσες περιβάλλοντος για μια δευτερεύουσα διασύνδεση με μια δύο αναγνωριστικά VLAN. Το εξωτερικό Αναγνωριστικό εικονικού ΔΙΚΤΎΟΥ (s-ετικέτα), αν χρησιμοποιηθεί παραμένει το ίδιο σε όλα τα peerings. Το εσωτερικό Αναγνωριστικό εικονικού ΔΙΚΤΎΟΥ (c-ετικέτα) είναι μοναδικό ανά διεισδύουν. Η τελευταία οκτάδα της διεύθυνσης του IPv4 θα είναι πάντα μονό αριθμό.

    interfaces {
        <Interface_Number> {
            flexible-vlan-tagging;
            unit <Number> {
                vlan-tags outer <S-tag> inner <C-tag>;
                family inet {
                    address <IPv4_Address/Subnet_Mask>;
                }                           
            }                               
        }                                   
    }                           

### <a name="2-setting-up-ebgp-sessions"></a>2. ρύθμιση eBGP περίοδοι λειτουργίας

Πρέπει να μπορείτε να ρυθμίσετε μια περίοδο λειτουργίας το πρωτόκολλο BGP με τη Microsoft για κάθε διεισδύουν. Το παρακάτω δείγμα σάς επιτρέπει να ρυθμίσετε μια περίοδο λειτουργίας το πρωτόκολλο BGP με τη Microsoft. Εάν η διεύθυνση IPv4 που χρησιμοποιείται για το περιβάλλον εργασίας sub ήταν a.b.c.d, τη διεύθυνση IP του γειτονικού στοιχείου το πρωτόκολλο BGP (Microsoft) θα είναι a.b.c.d+1. Η τελευταία οκτάδα διεύθυνσης IPv4 του γειτονικού στοιχείου το πρωτόκολλο BGP θα είναι πάντα άρτιος αριθμός.

    routing-options {
        autonomous-system <Customer_ASN>;
    }
    }
    protocols {
        bgp { 
            group <Group_Name> { 
                peer-as 12076;              
                neighbor <IP#2_used_by_Azure>;
            }                               
        }                                   
    }

### <a name="3-setting-up-prefixes-to-be-advertised-over-the-bgp-session"></a>3. ρύθμιση προθέματα θα κοινοποιηθεί πάνω από την περίοδο λειτουργίας το πρωτόκολλο BGP

Μπορείτε να ρυθμίσετε το δρομολογητή σας για να κοινοποιήσετε επιλέξτε προθέματα στη Microsoft. Μπορείτε να το κάνετε χρησιμοποιώντας το παρακάτω δείγμα.

    policy-options {
        policy-statement <Policy_Name> {
            term 1 {
                from protocol OSPF;
        route-filter <Prefix_to_be_advertised/Subnet_Mask> exact;
                then {
                    accept;
                }
            }
        }
    }
    protocols {
        bgp { 
            group <Group_Name> { 
                export <Policy_Name>
                peer-as 12076;              
                neighbor <IP#2_used_by_Azure>;
            }                               
        }                                   
    }


### <a name="4-route-maps"></a>4. χάρτες δρομολόγηση

Μπορείτε να χρησιμοποιήσετε χάρτες δρομολόγηση και λίστες προθέματος για προθέματα φίλτρο που έχουν μεταδοθεί στο δίκτυό σας. Μπορείτε να χρησιμοποιήσετε το παρακάτω δείγμα για την ολοκλήρωση της εργασίας. Βεβαιωθείτε ότι έχετε εγκαταστήσει το κατάλληλο πρόθεμα λίστες εγκατάστασης.

    policy-options {
        prefix-list MS_Prefixes {
            <IP_Prefix_1/Subnet_Mask>;
            <IP_Prefix_2/Subnet_Mask>;
        }
        policy-statement <MS_Prefixes_Inbound> {
            term 1 {
                from {
        prefix-list MS_Prefixes;
                }
                then {
                    accept;
                }
            }
        }
    }
    protocols {
        bgp { 
            group <Group_Name> { 
                export <Policy_Name>
                import <MS_Prefixes_Inbound>
                peer-as 12076;              
                neighbor <IP#2_used_by_Azure>;
            }                               
        }                                   
    }

## <a name="next-steps"></a>Επόμενα βήματα

Ανατρέξτε στο θέμα [Συνήθεις Ερωτήσεις ExpressRoute](expressroute-faqs.md) για περισσότερες λεπτομέρειες.
