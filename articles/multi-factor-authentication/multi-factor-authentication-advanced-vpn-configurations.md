<properties
    pageTitle="Σύνθετα σενάρια με Azure έλεγχο ταυτότητας πολλών παραγόντων και 3η πάρτι VPN"
    description="Αυτή η σελίδα παρέχει πληροφορίες για το βήμα προς βήμα ρύθμισης παραμέτρων για Azure MFA με prodcuts τρίτου κατασκευαστή."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban" 
    manager="femila"
    editor="curtland"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="advanced-scenarios-with-azure-multi-factor-authentication-and-3rd-party-vpn"></a>Σύνθετα σενάρια με Azure έλεγχο ταυτότητας πολλών παραγόντων και 3η πάρτι VPN
Azure έλεγχο ταυτότητας πολλών παραγόντων μπορεί να χρησιμοποιηθεί για την απρόσκοπτη σύνδεση με μια ποικιλία λύσεις VPN άλλων κατασκευαστών.  Αυτό περιλαμβάνει συσκευής Cisco® ASA VPN, συσκευής Citrix NetScaler SSL VPN και συσκευής Juniper δίκτυα ασφαλούς πρόσβασης/Pulse ασφαλούς σύνδεσης ασφαλούς SSL VPN.

## <a name="cisco-asa-vpn-appliance-and-azure-multi-factor-authentication"></a>Συσκευή ASA VPN Cisco και έλεγχος ταυτότητας πολλών παραγόντων Azure
Azure έλεγχο ταυτότητας πολλών παραγόντων ενσωματώνεται απρόσκοπτα με σας συσκευή Cisco® ASA VPN για πρόσθετη ασφάλεια για συνδέσεις VPN Cisco AnyConnect® και πρόσβασης πύλης.  Αυτό μπορεί να γίνει χρησιμοποιώντας το LDAP ή ΑΚΤΊΝΑ πρωτόκολλο.  Επιλέξτε ένα από τα εξής για να κάνετε λήψη των οδηγών λεπτομερή ρύθμισης παραμέτρων βήμα προς βήμα.

Οδηγός ρύθμισης παραμέτρων  | Περιγραφή
------------- | ------------- |
[Cisco ASA με Anyconnect VPN και Azure MFA τη ρύθμιση παραμέτρων για LDAP](http://download.microsoft.com/download/A/2/0/A201567C-C3DE-4227-AF89-4567A470899E/Cisco_ASA_Azure_MFA_LDAP.docx) | Απρόσκοπτη ενοποίηση σας συσκευής Cisco ASA VPN με MFA Azure χρησιμοποιώντας LDAP|
[Cisco ASA με Anyconnect VPN και Azure MFA τη ρύθμιση παραμέτρων για ΑΚΤΊΝΑ](http://download.microsoft.com/download/4/5/7/4579C1CF-35B0-4FBE-8A1A-B49CB2CC0382/Cisco_ASA_Azure_MFA_RADIUS.docx) | Απρόσκοπτη ενοποίηση σας συσκευής Cisco ASA VPN με χρήση ΑΚΤΊΝΑ MFA Azure

## <a name="citrix-netscaler-ssl-vpn-and-azure-multi-factor-authentication"></a>Έλεγχος ταυτότητας πολλών παραγόντων Citrix NetScaler SSL VPN και Azure
Azure έλεγχο ταυτότητας πολλών παραγόντων ενσωματώνεται απρόσκοπτα με σας συσκευής Citrix NetScaler SSL VPN για πρόσθετη ασφάλεια για συνδέσεις Citrix NetScaler SSL VPN και πρόσβασης πύλης.  Αυτό μπορεί να γίνει χρησιμοποιώντας το LDAP ή ΑΚΤΊΝΑ πρωτόκολλο.  Επιλέξτε ένα από τα εξής για να κάνετε λήψη των οδηγών λεπτομερή ρύθμισης παραμέτρων βήμα προς βήμα.

Οδηγός ρύθμισης παραμέτρων  | Περιγραφή
------------- | ------------- |
[Citrix NetScaler SSL VPN και Azure ρύθμισης παραμέτρων MFA για LDAP](http://download.microsoft.com/download/2/4/E/24E1E722-72DF-471F-A88A-D1338DB1AF83/Citrix_NS_Azure_MFA_LDAP.docx) | Απρόσκοπτη ενοποίηση σας VPN Citrix NetScaler SSL με συσκευή Azure MFA χρησιμοποιώντας LDAP|
[Citrix NetScaler SSL VPN και Azure ρύθμισης παραμέτρων MFA για ΑΚΤΊΝΑ](http://download.microsoft.com/download/1/A/4/1A482764-4A63-45C2-A5EC-2B673ACCDD12/Citrix_NS_Azure_MFA_RADIUS.docx) | Απρόσκοπτη ενοποίηση σας συσκευής Citrix NetScaler SSL VPN με χρήση ΑΚΤΊΝΑ MFA Azure

##<a name="juniperpulse-secure-ssl-vpn-appliance-and-azure-multi-factor-authentication"></a>Juniper/Pulse ασφαλούς SSL VPN συσκευής και έλεγχος ταυτότητας πολλών παραγόντων Azure
Azure έλεγχο ταυτότητας πολλών παραγόντων ενσωματώνεται απρόσκοπτα με σας συσκευής VPN SSL ασφαλούς Juniper/Pulse για πρόσθετη ασφάλεια για συνδέσεις VPN SSL ασφαλούς Juniper/Pulse και πρόσβασης πύλης.  Αυτό μπορεί να γίνει χρησιμοποιώντας το LDAP ή ΑΚΤΊΝΑ πρωτόκολλο.  Επιλέξτε ένα από τα εξής για να κάνετε λήψη των οδηγών λεπτομερή ρύθμισης παραμέτρων βήμα προς βήμα.

Οδηγός ρύθμισης παραμέτρων  | Περιγραφή
------------- | ------------- |
[Juniper/Pulse ασφαλούς SSL VPN και Azure ρύθμισης παραμέτρων MFA για LDAP](http://download.microsoft.com/download/6/5/8/6587B418-75B1-4FCB-84D4-984BC479309E/JuniperPulse_Azure_MFA_LDAP.docx)| Απρόσκοπτη ενοποίηση σας VPN SSL ασφαλούς Juniper/Pulse με συσκευή Azure MFA χρησιμοποιώντας LDAP|
[Juniper/Pulse ασφαλούς SSL VPN και Azure ρύθμισης παραμέτρων MFA για ΑΚΤΊΝΑ](http://download.microsoft.com/download/7/9/A/79AB3DAD-4799-4379-B1DA-B95ABDF231DC/JuniperPulse_Azure_MFA_RADIUS.docx) | Απρόσκοπτη ενοποίηση σας συσκευής VPN SSL ασφαλούς Juniper/Pulse με MFA Azure χρησιμοποιώντας ΑΚΤΊΝΑ
