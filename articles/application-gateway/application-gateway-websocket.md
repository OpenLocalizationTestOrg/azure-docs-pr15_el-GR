<properties
   pageTitle="Υποστήριξη εφαρμογών πύλης WebSocket | Microsoft Azure"
   description="Αυτή η σελίδα παρέχει μια επισκόπηση της υπηρεσίας υποστήριξης της εφαρμογής πύλης WebSocket."
   documentationCenter="na"
   services="application-gateway"
   authors="amsriva"
   manager="rossort"
   editor="amsriva"/>
<tags
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/16/2016"
   ms.author="amsriva"/>

# <a name="application-gateway-websocket-support"></a>Υποστήριξη εφαρμογών WebSocket πύλης

Πύλη εφαρμογής παρέχει υποστήριξη για WebSocket σε όλα τα μεγέθη πύλης. Δεν υπάρχει καμία ρύθμιση ρυθμίζονται από το χρήστη να ενεργοποιήσετε ή να απενεργοποιήσετε την υποστήριξη WebSocket επιλεκτική. Μπορείτε να συνεχίσετε να χρησιμοποιείτε μια τυπική HTTPListener στη θύρα 80/443 να λαμβάνει WebSocket κυκλοφορία. Κίνηση WebSocket, στη συνέχεια, κατευθύνεται στο διακομιστή με δυνατότητα υποστήριξης WebSocket χρησιμοποιώντας το χώρο συγκέντρωσης κατάλληλο παρασκηνίου σύμφωνα με την εφαρμογή κανόνων πύλης. Πρωτόκολλο WebSocket τυποποιημένες στο [RFC6455](https://tools.ietf.org/html/rfc6455) ενεργοποιεί ένα πλήρως αμφίδρομη επικοινωνία μεταξύ διακομιστή και προγράμματος-πελάτη μέσω μιας μεγάλης διάρκειας σύνδεσης TCP. Αυτή η δυνατότητα επιτρέπει για μια πιο αλληλεπιδραστική επικοινωνία μεταξύ του πελάτη, η οποία μπορεί να είναι διπλής κατεύθυνσης χωρίς να χρειάζεται σταθμοσκόπησης ως απαιτούμενη στο υλοποιήσεις βάσει HTTP και διακομιστή web.  WebSocket μικρό έχουν επιβάρυνσης σε αντίθεση με HTTP και να τη χρησιμοποιήσετε ξανά την ίδια σύνδεση TCP για πολλές αίτηση/απαντήσεις, με αποτέλεσμα μια πιο αποτελεσματική χρήση των πόρων. WebSocket πρωτόκολλα έχουν σχεδιαστεί για να εργαστείτε σε παραδοσιακά HTTP θύρες 80 και 443.

Το διακομιστή υποστήριξης πρέπει να ανταποκρίνεται στις καθετήρες πύλης εφαρμογής, που περιγράφονται στην ενότητα [εύρυθμη λειτουργία επισκόπησης δοκιμή του](application-gateway-probe-overview.md) . Εφαρμογή πύλης εύρυθμης λειτουργίας καθετήρες είναι μόνο HTTP/HTTPS, αυτό σημαίνει ότι κάθε διακομιστή παρασκηνίου πρέπει να ανταποκρίνεται στις καθετήρες HTTP για πύλη εφαρμογής για να δρομολογείτε την κίνηση WebSocket στο διακομιστή.

## <a name="listener-configuration-element"></a>Στοιχείο παραμέτρων ακρόασης

Υπάρχουσα HTTPListener μπορεί να χρησιμοποιηθεί για την υποστήριξη WebSocket. Ακολουθεί ένα απόκομμα HttpListeners στοιχείου από ένα δείγμα αρχείου προτύπου. Θα χρειαστείτε HTTP και HTTPS ακροατών υποστηρίζουν WebSocket και εξασφάλισης της WebSocket κίνηση. Ομοίως μπορείτε να χρησιμοποιήσετε την [πύλη](application-gateway-create-gateway-portal.md) ή το [PowerShell](application-gateway-create-gateway-arm.md) για να δημιουργήσετε μια πύλη εφαρμογής με ακροατών στη θύρα 80/443 για την υποστήριξη WebSocket κίνηση.


    "httpListeners": [
                {
                    "name": "appGatewayHttpsListener",
                    "properties": {
                        "FrontendIPConfiguration": {
                            "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/frontendIPConfigurations/DefaultFrontendPublicIP"
                        },
                        "FrontendPort": {
                            "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/frontendPorts/appGatewayFrontendPort443'"
                        },
                        "Protocol": "Https",
                        "SslCertificate": {
                            "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/sslCertificates/appGatewaySslCert1'"
                        },
                    }
                },
                {
                    "name": "appGatewayHttpListener",
                    "properties": {
                        "FrontendIPConfiguration": {
                            "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/frontendIPConfigurations/appGatewayFrontendIP'"
                        },
                        "FrontendPort": {
                            "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/frontendPorts/appGatewayFrontendPort80'"
                        },
                        "Protocol": "Http",
                    }
                }
            ],

## <a name="backendaddresspool-backendhttpsetting-and-routing-rule-configuration"></a>Ρύθμιση παραμέτρων κανόνα BackendAddressPool, BackendHttpSetting και δρομολόγησης

BackendAddressPool πρέπει να χρησιμοποιείται για τον ορισμό ενός χώρου συγκέντρωσης παρασκηνίου με τους διακομιστές WebSocket με δυνατότητα. BackendHttpSetting θα πρέπει να οριστούν με παρασκηνίου θύρα 80/443 μόνο. Ιδιότητες για βασίζεται σε cookie συσχέτισης και requestTimeouts δεν είναι σχετικά με την κυκλοφορία WebSocket. Δεν υπάρχει καμία αλλαγή απαιτείται σε κανόνα δρομολόγησης. Δρομολόγηση κανόνας 'Βασική' πρέπει να συνεχίσετε να χρησιμοποιηθεί για να συνδέσετε στο κατάλληλο πρόγραμμα ακρόασης στο αντίστοιχο χώρο συγκέντρωσης διεύθυνση παρασκηνίου. 

    "requestRoutingRules": [{
        "name": "<ruleName1>",
        "properties": {
            "RuleType": "Basic",
            "httpListener": {
                "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/httpListeners/appGatewayHttpsListener')]"
            },
            "backendAddressPool": {
                "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendAddressPools/ContosoServerPool')]"
            },
            "backendHttpSettings": {
                "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendHttpSettingsCollection/appGatewayBackendHttpSettings')]"
            }
        }

    }, {
        "name": "<ruleName2>",
        "properties": {
            "RuleType": "Basic",
            "httpListener": {
                "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/httpListeners/appGatewayHttpListener')]"
            },
            "backendAddressPool": {
                "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendAddressPools/ContosoServerPool')]"
            },
            "backendHttpSettings": {
                "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendHttpSettingsCollection/appGatewayBackendHttpSettings')]"
            }

        }
    }]

## <a name="websocket-enabled-backend"></a>WebSocket με δυνατότητα υποστήριξης

Παρασκηνίου σας πρέπει να έχετε ένα διακομιστή web HTTP/HTTPS λειτουργεί με τη ρύθμιση παραμέτρων θύρα (συνήθως 80/443) για WebSocket για να εργαστείτε. Αυτή η απαίτηση είναι επειδή το πρωτόκολλο WebSocket απαιτεί ο αρχικός συγχρονισμός για να HTTP με αναβάθμιση πρωτοκόλλου WebSocket ως πεδίο κεφαλίδας.

    GET /chat HTTP/1.1
    Host: server.example.com
    Upgrade: websocket
    Connection: Upgrade
    Sec-WebSocket-Key: dGhlIHNhbXBsZSBub25jZQ==
    Origin: http://example.com
    Sec-WebSocket-Protocol: chat, superchat
    Sec-WebSocket-Version: 13

Ένας άλλος λόγος για αυτό είναι που δοκιμή του εύρυθμης λειτουργίας παρασκηνίου εφαρμογής πύλης υποστηρίζει μόνο πρωτόκολλα HTTP/HTTPS. Εάν το διακομιστή παρασκηνίου δεν ανταποκρίνεται σε HTTP/HTTPS καθετήρες, θα λαμβάνονται από το χώρο συγκέντρωσης παρασκηνίου και δεν υπάρχει αιτήσεις συμπεριλαμβανομένων των αιτήσεων WebSocket, να φτάσει στο αυτό παρασκηνίου.

## <a name="next-steps"></a>Επόμενα βήματα

Μετά την εκμάθηση σχετικά με την υποστήριξη WebSocket, μεταβείτε για να [δημιουργήσετε μια πύλη εφαρμογής](application-gateway-create-gateway.md) για να ξεκινήσετε με μια εφαρμογή web WebSocket με δυνατότητα.
