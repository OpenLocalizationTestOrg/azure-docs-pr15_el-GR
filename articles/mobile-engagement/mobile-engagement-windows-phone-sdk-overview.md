<properties 
    pageTitle="Windows Phone Επισκόπηση Silverlight SDK" 
    description="Επισκόπηση του Windows Phone SDK του Silverlight για Azure δέσμευση κινητές συσκευές"                     
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede"
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows-phone" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="windows-phone-silverlight-sdk-overview-for-azure-mobile-engagement"></a>Windows Phone Επισκόπηση SDK του Silverlight για Azure δέσμευση κινητές συσκευές

Ξεκινήστε εδώ για λεπτομέρειες σχετικά με τον τρόπο για να ενσωματώσετε Azure δέσμευση Mobile σε μια εφαρμογή του Silverlight για Windows Phone. Εάν θέλετε να του δώσετε μια, δοκιμάστε πρώτα, βεβαιωθείτε ότι μπορείτε να ολοκληρώσετε το [πρόγραμμα εκμάθησης 15 λεπτά](mobile-engagement-windows-phone-get-started.md).

Κάντε κλικ στο κουμπί για να δείτε το [Περιεχόμενο SDK](mobile-engagement-windows-phone-sdk-content.md)

##<a name="integration-procedures"></a>Διαδικασίες ολοκλήρωσης

1. Ξεκινήστε εδώ: [πώς μπορείτε να ενοποιήσετε δέσμευση Mobile στην εφαρμογή Silverlight Windows Phone](mobile-engagement-windows-phone-integrate-engagement.md)

2. Για τις ειδοποιήσεις: [πώς μπορείτε να ενοποιήσετε Reach (ειδοποιήσεις) στην εφαρμογή Silverlight Windows Phone](mobile-engagement-windows-phone-integrate-engagement-reach.md)

3. Προσθήκη ετικετών σε εφαρμογή πρόγραμμα: [πώς μπορείτε να χρησιμοποιήσετε το σύνθετο δέσμευση Mobile προσθήκης ετικετών API στην εφαρμογή Silverlight Windows Phone](mobile-engagement-windows-phone-use-engagement-api.md)

##<a name="release-notes"></a>Σημειώσεις έκδοσης

###<a name="330-04192016"></a>3.3.0 (19/04/2016)
Τμήμα του nuget *MicrosoftAzure.MobileEngagement* συσκευασία **v3.4.0**

-   Προστέθηκε "TestLogLevel" API για ενεργοποίηση/απενεργοποίηση φίλτρου κονσόλα αρχεία καταγραφής που εκπέμπει το SDK.

Για την προηγούμενη έκδοση, ανατρέξτε στις [Σημειώσεις έκδοσης ολοκλήρωσης](mobile-engagement-windows-phone-release-notes.md)

##<a name="upgrade-procedures"></a>Διαδικασίες αναβάθμισης

Εάν έχετε ήδη έχετε συνδέσει μια παλαιότερη έκδοση του SDK μας στην εφαρμογή σας, πρέπει να λάβετε υπόψη σας τα ακόλουθα σημεία κατά την αναβάθμιση του SDK.

Ίσως χρειαστεί να ακολουθήσετε διάφορες διαδικασίες, εάν έχετε παραβλέψει πολλές εκδόσεις του SDK. Δείτε την πλήρη [Αναβάθμιση διαδικασίες](mobile-engagement-windows-phone-upgrade-procedure.md). Για παράδειγμα, εάν κάνετε μετεγκατάσταση από 0.10.1 σε 0.11.0 πρέπει να πρώτα να ακολουθήστε τη διαδικασία "από 0.9.0 να 0.10.1", στη συνέχεια, τη διαδικασία "από 0.10.1 να 0.11.0".

###<a name="from-200-to-330"></a>Από το 2.0.0 να 3.3.0

####<a name="test-logs"></a>Αρχεία καταγραφής ελέγχου

Αρχεία καταγραφής κονσόλας που δημιουργήθηκαν με το SDK τώρα μπορεί να είναι ενεργοποιημένες/απενεργοποίησης/φιλτραρισμένη. Για να προσαρμόσετε έτσι, ενημερώστε την ιδιότητα `EngagementAgent.Instance.TestLogEnabled` σε μία από την τιμή που είναι διαθέσιμα από το `EngagementTestLogLevel` απαρίθμηση, για παράδειγμα:

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();

### <a name="upgrade-from-older-versions"></a>Αναβάθμιση από παλαιότερες εκδόσεις

Ανατρέξτε στο θέμα [διαδικασίες αναβάθμισης](mobile-engagement-windows-phone-upgrade-procedure.md)
 
