<properties 
    pageTitle="Αναλυτικές οδηγίες: Παρακολούθηση της Microsoft Dynamics CRM με ιδέες εφαρμογής" 
    description="Λάβετε τηλεμετρίας από το Microsoft Dynamics CRM Online με χρήση εφαρμογής ιδέες. Αναλυτικές οδηγίες εγκατάστασης, γρήγορα δεδομένα, απεικόνιση και εξαγωγή." 
    services="application-insights" 
    documentationCenter=""
    authors="mazharmicrosoft" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="11/17/2015" 
    ms.author="awills"/>
 
# <a name="walkthrough-enabling-telemetry-for-microsoft-dynamics-crm-online-using-application-insights"></a>Αναλυτικές οδηγίες: Ενεργοποίηση Τηλεμετρίας για το Microsoft Dynamics CRM Online με χρήση εφαρμογής ιδέες

Σε αυτό το άρθρο θα μάθετε πώς να λάβετε δεδομένα τηλεμετρίας από το [Microsoft Dynamics CRM Online](https://www.dynamics.com/) με χρήση του [Visual Studio εφαρμογή ιδέες](https://azure.microsoft.com/services/application-insights/). Θα σας καθοδηγήσουμε κατά τη διαδικασία ολοκλήρωσης της προσθήκης ιδέες εφαρμογή δέσμης ενεργειών για την εφαρμογή σας, η καταγραφή δεδομένων και απεικόνιση δεδομένων.

>[AZURE.NOTE] [Αναζητήστε τη λύση δείγμα](https://dynamicsandappinsights.codeplex.com/).

## <a name="add-application-insights-to-new-or-existing-crm-online-instance"></a>Προσθήκη εφαρμογής ιδέες σε νέα ή υπάρχουσα παρουσία CRM Online 

Για την παρακολούθηση της εφαρμογής σας, μπορείτε να προσθέσετε μια εφαρμογή του SDK ιδέες για την εφαρμογή σας. Το SDK στέλνει τηλεμετρίας στην [πύλη εφαρμογής ιδέες](https://portal.azure.com), όπου μπορείτε να χρησιμοποιήστε μας ισχυρές αναλύσεις και διαγνωστικά εργαλεία ή να εξαγάγετε τα δεδομένα στο χώρο αποθήκευσης.

### <a name="create-an-application-insights-resource-in-azure"></a>Δημιουργία ενός πόρου ιδέες εφαρμογή στο Azure

1. Αποκτήστε [ένα λογαριασμό στο Microsoft Azure](http://azure.com/pricing). 
2. Πραγματοποιήστε είσοδο στο [Azure πύλη](https://portal.azure.com) και να προσθέσετε ένα νέο πόρο εφαρμογής ιδέες. Αυτό είναι όπου θα υποβάλλονται σε επεξεργασία και θα εμφανιστούν τα δεδομένα σας.

    ![Κάντε κλικ στην επιλογή +, για προγραμματιστές των υπηρεσιών, εφαρμογή ιδέες.](./media/app-insights-sample-mscrm/01.png)

    Επιλέξτε ASP.NET ως ο τύπος της εφαρμογής.

3. Ανοίξτε την καρτέλα γρήγορης εκκίνησης και ανοίξτε τη δέσμη ενεργειών κώδικα.

    ![](./media/app-insights-sample-mscrm/03.png)

**Κρατήστε ανοιχτή τη σελίδα κώδικα** ενώ μπορείτε να κάνετε το επόμενο βήμα σε ένα άλλο παράθυρο προγράμματος περιήγησης. Θα χρειαστεί σύντομα τον κώδικα. 

### <a name="create-a-javascript-web-resource-in-microsoft-dynamics-crm"></a>Δημιουργία ενός πόρου web JavaScript στο Microsoft Dynamics CRM

1. Ανοίξτε το CRM Online παρουσία και συνδεθείτε με δικαιώματα διαχειριστή.
2. Άνοιγμα Microsoft Dynamics CRM ρυθμίσεις, προσαρμογές, η προσαρμογή του συστήματος

    ![](./media/app-insights-sample-mscrm/04.png)
    
    ![](./media/app-insights-sample-mscrm/05.png)


    ![](./media/app-insights-sample-mscrm/06.png)

3. Δημιουργήστε έναν πόρο JavaScript.

    ![](./media/app-insights-sample-mscrm/07.png)

    Δώσετε ένα όνομα, επιλέξτε **δέσμης ενεργειών (JScript)** και ανοίξτε το πρόγραμμα επεξεργασίας κειμένου.

    ![](./media/app-insights-sample-mscrm/08.png)
    
4. Αντιγράψτε τον κώδικα από εφαρμογή ιδέες. Κατά την αντιγραφή, βεβαιωθείτε ότι για να αγνοήσετε ετικέτες δέσμης ενεργειών. Αναφέρονται παρακάτω στιγμιότυπο οθόνης:

    ![](./media/app-insights-sample-mscrm/09.png)

    Ο κώδικας περιλαμβάνει το κλειδί οργάνων που προσδιορίζει τον πόρο ιδέες εφαρμογής.

5. Αποθήκευση και δημοσίευση.

    ![](./media/app-insights-sample-mscrm/10.png)

### <a name="instrument-forms"></a>Φόρμες Instrument

1. Στο Microsoft CRM Online, ανοίξτε τη φόρμα λογαριασμού

    ![](./media/app-insights-sample-mscrm/11.png)

2. Ανοίξτε τη φόρμα ιδιοτήτων

    ![](./media/app-insights-sample-mscrm/12.png)

3. Προσθήκη πόρου web JavaScript που δημιουργήσατε

    ![](./media/app-insights-sample-mscrm/13.png)

    ![](./media/app-insights-sample-mscrm/14.png)

4. Αποθηκεύστε και δημοσιεύστε τις προσαρμογές σας φόρμα.


## <a name="metrics-captured"></a>Μετρικά καταγράφονται

Τώρα έχετε ρυθμίσει καταγραφής τηλεμετρίας για τη φόρμα. Κάθε φορά που χρησιμοποιείται, θα σταλεί δεδομένων για τον πόρο εφαρμογής ιδέες.

Ακολουθούν παραδείγματα των δεδομένων που θα δείτε.

#### <a name="application-health"></a>Εφαρμογή εύρυθμης λειτουργίας

![](./media/app-insights-sample-mscrm/15.png)

![](./media/app-insights-sample-mscrm/16.png)

Πρόγραμμα περιήγησης εξαιρέσεις:

![](./media/app-insights-sample-mscrm/17.png)

Κάντε κλικ στο γράφημα για να λάβετε περισσότερες λεπτομέρειες:

![](./media/app-insights-sample-mscrm/18.png)

#### <a name="usage"></a>Χρήση

![](./media/app-insights-sample-mscrm/19.png)

![](./media/app-insights-sample-mscrm/20.png)

![](./media/app-insights-sample-mscrm/21.png)

#### <a name="browsers"></a>Προγράμματα περιήγησης

![](./media/app-insights-sample-mscrm/22.png)

![](./media/app-insights-sample-mscrm/23.png)

#### <a name="geolocation"></a>Γεωεντοπισμός

![](./media/app-insights-sample-mscrm/24.png)

![](./media/app-insights-sample-mscrm/25.png)

#### <a name="inside-page-view-request"></a>Αίτηση προβολή εσωτερικά σελίδας

![](./media/app-insights-sample-mscrm/26.png)

![](./media/app-insights-sample-mscrm/27.png)

![](./media/app-insights-sample-mscrm/28.png)

![](./media/app-insights-sample-mscrm/29.png)

![](./media/app-insights-sample-mscrm/30.png)

## <a name="sample-code"></a>Δείγμα κώδικα

[Αναζητήστε το δείγμα κώδικα](https://dynamicsandappinsights.codeplex.com/).

## <a name="power-bi"></a>Power BI

Μπορείτε να κάνετε ακόμα βαθύτερη ανάλυση Εάν κάνετε [Εξαγωγή των δεδομένων για το Microsoft Power BI](app-insights-export-power-bi.md).

## <a name="sample-microsoft-dynamics-crm-solution"></a>Δείγμα Microsoft Dynamics CRM λύσης

[Εδώ είναι η λύση δείγμα υλοποιηθεί σε Microsoft Dynamics CRM] (https://dynamicsandappinsights.codeplex.com/).

## <a name="learn-more"></a>Μάθε περισσότερα

* [Τι είναι η εφαρμογή ιδέες;](app-insights-overview.md)
* [Εφαρμογή ιδέες για τις ιστοσελίδες](app-insights-javascript.md)
* [Περισσότερα παραδείγματα και αναλυτικές παρουσιάσεις](app-insights-code-samples.md)

 
