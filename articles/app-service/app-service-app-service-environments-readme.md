<properties 
    pageTitle="Περιβάλλον εφαρμογής υπηρεσίας | Microsoft Azure" 
    description="Τι είναι ένα περιβάλλον Azure εφαρμογής υπηρεσίας; Μια εισαγωγή σχετικά με την εφαρμογή υπηρεσίας περιβάλλον." 
    keywords="Azure εφαρμογής υπηρεσίας περιβάλλον, εικονικού δικτύου, ασφαλούς δικτύωση"
    services="app-service" 
    documentationCenter="" 
    authors="stefsch" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/04/2016" 
    ms.author="stefsch"/>

# <a name="app-service-environment-documentation"></a>Τεκμηρίωση περιβάλλον εφαρμογής υπηρεσίας

Περιβάλλον εφαρμογής υπηρεσίας είναι [Premium] [ PremiumTier] υπηρεσίας πρόγραμμα επιλογή Azure εφαρμογής υπηρεσίας που παρέχει μια πλήρως απομόνωσης και αποκλειστικό περιβάλλον για την ασφαλή εκτέλεση Azure εφαρμογής υπηρεσίας εφαρμογών σε υψηλή κλίμακα, συμπεριλαμβανομένων των [Εφαρμογών Web][WebApps], [Οι εφαρμογές του Mobile][MobileApps], και [Εφαρμογές API][APIApps].  

Περιβάλλοντα εφαρμογής υπηρεσίας είναι ιδανική για φόρτους εργασίας εφαρμογών που απαιτεί:

- Πολύ υψηλή κλίμακας
- Απομόνωση και ασφαλή πρόσβαση στο δίκτυο

Οι πελάτες να δημιουργήσετε πολλά περιβάλλοντα υπηρεσία εφαρμογής μέσα σε μία μόνο περιοχή Azure, καθώς και σε πολλές περιοχές Azure.  Με αυτόν τον τρόπο εφαρμογής υπηρεσίας περιβάλλοντα ιδανικό για την οριζόντια κλιμάκωση βαθμίδες νομό λιγότερο εφαρμογής που υποστηρίζουν υψηλή RPS φόρτους εργασίας.

Περιβάλλοντα εφαρμογής υπηρεσίας είναι απομονωμένες στην εκτέλεση μόνο ενός πελάτη μόνο εφαρμογών και αναπτύσσονται πάντα σε ένα εικονικό δίκτυο.  Οι πελάτες έχουν λεπτομερή έλεγχο και τα δύο κίνηση του δικτύου εισερχόμενων και εξερχόμενων εφαρμογής χρησιμοποιώντας [ομάδες ασφαλείας δικτύου][NetworkSecurityGroups].  Εφαρμογές επίσης να δημιουργήσετε συνδέσεις υψηλής ταχύτητας ασφαλούς μέσω εικονικών δικτύων για εταιρικούς πόρους εσωτερικής εγκατάστασης.

Εφαρμογές συχνά πρέπει να αποκτήσετε πρόσβαση σε εταιρικούς πόρους, όπως εσωτερικές βάσεις δεδομένων και υπηρεσίες web.  Εφαρμογές που εκτελείται σε εφαρμογή υπηρεσίας περιβάλλοντα να αποκτήσετε πρόσβαση σε πόρους προσβάσιμος μέσω [Τοποθεσίας σε τοποθεσία] [ SiteToSite] VPN και [Azure ExpressRoute] [ ExpressRoute] συνδέσεις.

* [Τι είναι το περιβάλλον εφαρμογής υπηρεσίας;](../app-service-web/app-service-app-service-environment-intro.md)
* [Δημιουργία μιας εφαρμογής υπηρεσίας περιβάλλοντος](../app-service-web/app-service-web-how-to-create-an-app-service-environment.md)
* [Δημιουργία εφαρμογών σε ένα περιβάλλον εφαρμογής υπηρεσίας](../app-service-web/app-service-web-how-to-create-a-web-app-in-an-ase.md)
* [Δημιουργία και χρήση μιας εσωτερικής εξισορρόπηση φόρτου με περιβάλλοντα εφαρμογής υπηρεσίας](../app-service-web/app-service-environment-with-internal-load-balancer.md)
* [Ρύθμιση των παραμέτρων μιας εφαρμογής υπηρεσίας περιβάλλον](../app-service-web/app-service-web-configure-an-app-service-environment.md) 
* [Κλιμάκωση εφαρμογές σε περιβάλλον εφαρμογής υπηρεσίας](../app-service-web/app-service-web-scale-a-web-app-in-an-app-service-environment.md)
* [Ασφάλεια δικτύου και την αρχιτεκτονική](../app-service-web/app-service-app-service-environment-network-architecture-overview.md)

## <a name="how-tos"></a>Πώς μπορείτε να του

[AZURE.INCLUDE [app-service-blueprint-app-service-environment](../../includes/app-service-blueprint-app-service-environment.md)]


## <a name="videos"></a>Βίντεο
[AZURE.VIDEO azurecon-2015-deploying-highly-scalable-and-secure-web-and-mobile-apps]

[AZURE.VIDEO microsoft-ignite-2015-running-enterprise-web-and-mobile-apps-on-azure-app-service]


<!-- LINKS -->
[PremiumTier]: http://azure.microsoft.com/pricing/details/app-service/
[WebApps]: http://azure.microsoft.com/documentation/articles/app-service-web-overview/
[MobileApps]: http://azure.microsoft.com/documentation/articles/app-service-mobile-value-prop-preview/
[APIApps]: http://azure.microsoft.com/documentation/articles/app-service-api-apps-why-best-platform/
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[SiteToSite]: https://azure.microsoft.com/documentation/articles/vpn-gateway-site-to-site-create/
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
