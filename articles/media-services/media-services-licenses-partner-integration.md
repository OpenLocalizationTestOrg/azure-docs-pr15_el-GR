<properties 
    pageTitle="Χρήση συνεργάτες για την παράδοση Widevine άδειες χρήσης σε υπηρεσιών Azure Media Services | Microsoft Azure" 
    description="Σε αυτό το άρθρο περιγράφει πώς μπορείτε να χρησιμοποιήσετε Azure Media Services (αθροιστικής μέτρησης ενισχύσεων) για να παρέχουν μια ροή που είναι κρυπτογραφημένα δυναμικά από αθροιστικής μέτρησης ενισχύσεων με PlayReady και Widevine DRMs. Η άδεια χρήσης PlayReady προέρχεται από διακομιστή αδειών χρήσης PlayReady υπηρεσίες πολυμέσων και παραδίδεται Widevine άδειας χρήσης από castLabs άδεια χρήσης του διακομιστή." 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016"  
    ms.author="juliako"/>

#<a name="using-partners-to-deliver-widevine-licenses-to-azure-media-services"></a>Χρήση συνεργάτες για την παράδοση Widevine άδειες χρήσης σε υπηρεσιών Azure Media Services

##<a name="overview"></a>Επισκόπηση

Υπηρεσίες πολυμέσων Azure Microsoft σάς επιτρέπει να κάνετε MPEG-ΔΙΑΚΕΚΟΜΜΈΝΗ προστατεύεται με Widevine DRM, το οποίο είναι κρυπτογραφημένο ανά της προδιαγραφής κοινές κρυπτογράφησης (CENC).

Ξεκινώντας από το Media Services .NET SDK έκδοση 3.5.2, Media Services σάς επιτρέπει να ρυθμίσετε τις παραμέτρους Widevine άδεια χρήσης του προτύπου και λάβετε Widevine άδειες χρήσης. Μπορείτε επίσης να χρησιμοποιήσετε την παρακάτω συνεργάτες αθροιστικής μέτρησης ενισχύσεων θα σας βοηθήσουν να κάνουν Widevine άδειες χρήσης: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/), [castLabs](http://castlabs.com/company/partners/azure/).

##<a name="castlabs"></a>castLabs

Μπορείτε να χρησιμοποιήσετε [castLabs](http://castlabs.com/company/partners/azure/) για παράδοση Widevine άδειες χρήσης. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Χρήση castLabs για παράδοση DRM άδειες χρήσης των υπηρεσιών Azure Media Services](media-services-castlabs-integration.md)

##<a name="axinom"></a>Axinom

Μπορείτε να χρησιμοποιήσετε [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/) για παράδοση Widevine άδειες χρήσης. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Χρήση Axinom για παράδοση DRM άδειες χρήσης των υπηρεσιών Azure Media Services](media-services-axinom-integration.md)


##<a name="media-services-learning-paths"></a>Διαδικασίες εκμάθησης Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Παροχή σχολίων

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="see-also"></a>Δείτε επίσης

[Χρήση PlayReady ή/και Widevine δυναμικής κοινές κρυπτογράφησης](media-services-protect-with-drm.md)

[Ιστολόγιο του Mingfei](https://azure.microsoft.com/blog/azure-media-services-adds-google-widevine-packaging-for-delivering-multi-drm-stream/)

