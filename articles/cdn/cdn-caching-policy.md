<properties
    pageTitle="CDN σε cache πολιτικής στην επέκταση υπηρεσίες πολυμέσων"
    description="Αυτό το θέμα παρέχει μια επισκόπηση του ένα CDN σε cache πολιτικής στην επέκταση υπηρεσίες πολυμέσων."
    services="media-services,cdn"
    documentationCenter=".NET"
    authors="juliako"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="juliako"/>
 
#<a name="cdn-caching-policy-in-media-services-extension"></a>CDN σε cache πολιτικής στην επέκταση υπηρεσίες πολυμέσων

Azure Media Services παρέχει HTTP με βάση προσαρμόσιμη ροής και προοδευτική λήψη. HTTP βάσει ροής είναι ιδιαίτερα μεταβλητού μεγέθους με πλεονεκτήματα της εγγραφής στο cache του διακομιστή μεσολάβησης και επίπεδα CDN καθώς και σε cache πλευρά του προγράμματος-πελάτη. Η ροή τελικά σημεία παρέχει γενικές δυνατότητες ροής και επίσης τη ρύθμιση παραμέτρων για τις κεφαλίδες cache HTTP. Η ροή τελικά σημεία σύνολα στοιχείων ελέγχου Cache HTTP: max-ηλικία και λήξη κεφαλίδες. Μπορείτε να λάβετε περισσότερες πληροφορίες για τις κεφαλίδες cache HTTP από [W3.org](http://www.w3.org/Protocols/rfc2616/rfc2616-sec13.html).

##<a name="default-caching-headers"></a>Προεπιλεγμένη προσωρινή αποθήκευση κεφαλίδες

Από προεπιλογή η ροή τελικά σημεία εφαρμογή κεφαλίδες cache 3 ημερών για ροή δεδομένων σε ζήτηση (πραγματική πολυμέσων τμήματα/μπλοκ) και manifest(playlist). Για ζωντανή ροή ροής τελικά σημεία εφαρμόστε κεφαλίδες cache 3 ημερών για τα δεδομένα (πραγματική πολυμέσων τμήματα/μπλοκ) και 2 δευτερόλεπτα cache κεφαλίδας για αιτήσεις manifest(playlist). Όταν ζωντανή πρόγραμμα μεταβαίνει στη ζήτηση (ζωντανή αρχειοθήκη) στη συνέχεια, εφαρμόστε σε ζήτηση ροής κεφαλίδες cache.

##<a name="azure-cdn-integration"></a>Ενοποίηση του Azure CDN

Azure Media Services παρέχει [ενσωματωμένη CDN](https://azure.microsoft.com/updates/azure-media-services-now-fully-integrated-with-azure-cdn/) για ροή τελικά σημεία. Κεφαλίδες cache-control εφαρμόζεται με τον ίδιο τρόπο όπως ροής τελικών σημείων CDN με δυνατότητα ροής τελικά σημεία. Azure CDN χρησιμοποιεί ροή τελικού σημείου ρυθμιστεί cache τιμές για να ορίσετε τη διάρκεια ζωής των αντικειμένων στο cache εσωτερικά και χρησιμοποιεί επίσης αυτήν την τιμή για να ορίσετε την παράδοση κεφαλίδες cache. Όταν χρησιμοποιείτε CDN ενεργοποιημένη ροής τελικά σημεία δεν συνιστάται για τον ορισμό τιμών μικρές cache. Ρύθμιση μικρές τιμές θα μειωθεί η απόδοση και να μειώσετε το όφελος των CDN. Δεν επιτρέπεται για να ορίσετε κεφαλίδες cache μικρότερες του 600 δευτερόλεπτα για CDN με δυνατότητα ροής τελικά σημεία.

>[AZURE.IMPORTANT] Azure Media Services ενοποίηση με το Azure CDN έχει υλοποιηθεί σε **CDN Azure από Verizon**.  Εάν θέλετε να χρησιμοποιήσετε **CDN Azure από Akamai** για υπηρεσιών Azure Media Services, πρέπει να [ρυθμίσετε με μη αυτόματο τρόπο το τελικό σημείο](cdn-create-new-endpoint.md).  Για περισσότερες πληροφορίες σχετικά με τις δυνατότητες Azure CDN, ανατρέξτε στο θέμα η [Επισκόπηση CDN](cdn-overview.md).

##<a name="configuring-cache-headers-with-azure-media-services"></a>Ρύθμιση παραμέτρων cache κεφαλίδες με υπηρεσιών Azure Media Services

Μπορείτε να χρησιμοποιήσετε την πύλη διαχείρισης Azure ή API υπηρεσίες πολυμέσων Azure για ρύθμιση παραμέτρων cache κεφαλίδα τιμές.

1. Για τη ρύθμιση παραμέτρων cache κεφαλίδες χρησιμοποιώντας τη Διαχείριση πύλη, ανατρέξτε στην ενότητα [πώς μπορείτε να διαχειριστείτε ροής τελικά σημεία](../media-services/media-services-portal-manage-streaming-endpoints.md) ρύθμιση παραμέτρων το τελικό σημείο ροής.
2. Υπηρεσίες πολυμέσων Azure REST API, [StreamingEndpoint](https://msdn.microsoft.com/library/azure/dn783468.aspx#StreamingEndpointCacheControl).
3. Υπηρεσίες πολυμέσων Azure .NET SDK, [StreamingEndpointCacheControl ιδιότητες](http://go.microsoft.com/fwlink/?LinkId=615302).

##<a name="cache-configuration-precedence-order"></a>Σειρά προτεραιότητας ρύθμιση παραμέτρων cache

1. Τιμή cache Azure Media Services έχει ρυθμιστεί παρακάμπτει προεπιλεγμένη τιμή.
2. Εάν δεν υπάρχει καμία μη αυτόματη ρύθμιση παραμέτρων, ισχύει προεπιλεγμένες τιμές.
3. Από προεπιλογή 2 δευτερόλεπτα cache κεφαλίδες ισχύει για live ροή manifest(playlist) ανεξάρτητα από το Azure Media ή χώρο αποθήκευσης Azure ρύθμισης παραμέτρων και δεν είναι διαθέσιμη η παράκαμψη αυτής της τιμής.
