<properties
   pageTitle="Αλληλεπιδράστε με αναφορές με χρήση του API της JavaScript | Microsoft Azure"
   description="Power BI ενσωματωμένο, να αλληλεπιδράσετε με αναφορές με χρήση του API της JavaScript"
   services="power-bi-embedded"
   documentationCenter=""
   authors="guyinacube"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="power-bi-embedded"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="powerbi"
   ms.date="10/04/2016"
   ms.author="asaxton"/>

# <a name="interact-with-power-bi-reports-using-the-javascript-api"></a>Αλληλεπίδραση με χρήση του API της JavaScript αναφορές του Power BI

Το Power BI JavaScript API σάς επιτρέπει να ενσωματώσετε εύκολα αναφορές του Power BI στις εφαρμογές σας. Με το API, τις εφαρμογές σας μέσω προγραμματισμού να αλληλεπιδράσετε με διαφορετική εκτύπωση στοιχεία, όπως σελίδες και τα φίλτρα. Αυτό αλληλεπίδρασης κάνει αναφορές του Power BI μια πιο ολοκληρωμένη τμήμα της εφαρμογής σας.

Μπορείτε να ενσωματώσετε μια αναφορά του Power BI στην εφαρμογή σας, χρησιμοποιώντας ένα iframe που φιλοξενείται ως μέρος της εφαρμογής. Το iframe λειτουργεί ως ένα όριο μεταξύ της εφαρμογής σας και την αναφορά, όπως μπορείτε να δείτε στην παρακάτω εικόνα. 

![Power BI ενσωματωμένο iframe χωρίς API της Javascript](media\powerbi-embedded-interact-with-reports\powerbi-embedded-interact-report-1.png)

Το iframe διευκολύνει τη διαδικασία ενσωμάτωσης συχνά, αλλά χωρίς το API της JavaScript στην έκθεση και την εφαρμογή σας δεν είναι δυνατό να αλληλεπιδρούν μεταξύ τους. Αυτή η έλλειψη επικοινωνίας μπορούν να αισθάνεστε ότι η έκθεση δεν είναι πραγματικά τμήμα της εφαρμογής. Η αναφορά και η εφαρμογή πραγματικά πρέπει να επικοινωνούν και πίσω, όπως η παρακάτω εικόνα.

![Power BI ενσωματωμένα iframe με το API της Javascript](media\powerbi-embedded-interact-with-reports\powerbi-embedded-interact-report-2.png)

Το Power BI JavaScript API σάς επιτρέπει να σύνταξη κώδικα που μπορεί να περάσει με ασφάλεια έως το όριο iframe. Αυτή η δυνατότητα επιτρέπει την εφαρμογή σας να μέσω προγραμματισμού εκτελεί μια ενέργεια σε μια αναφορά και να ακούσετε για συμβάντα από ενέργειες που κάνουν οι χρήστες μέσα στην αναφορά.

## <a name="what-can-you-do-with-the-power-bi-javascript-api"></a>Τι μπορείτε να κάνετε με το API JavaScript του Power BI;
Με το API της JavaScript μπορείτε να διαχειριστείτε τις αναφορές, περιήγηση σε σελίδες σε μια αναφορά, φιλτράρετε μια αναφορά, και χειρισμού ενσωμάτωση συμβάντα. Το παρακάτω διάγραμμα παρουσιάζει τη δομή του API.

![Power BI JavaScript API διαγράμματος](media\powerbi-embedded-interact-with-reports\powerbi-embedded-interact-report-3.png)


### <a name="manage-reports"></a>Διαχείριση αναφορών
Το API της Javascript σάς επιτρέπει να διαχειριστείτε συμπεριφορά στο επίπεδο της αναφοράς και σελίδας:

- Ενσωμάτωση μια συγκεκριμένη αναφορά του Power BI με ασφάλεια στην εφαρμογή - προσπαθήστε να την [ενσωματώσετε επίδειξη εφαρμογής](http://azure-samples.github.io/powerbi-angular-client/#/scenario1)
  - Ορισμός διακριτικό πρόσβασης
- Ρύθμιση παραμέτρων της αναφοράς
  - Ενεργοποίηση και απενεργοποίηση του παραθύρου φίλτρων και παράθυρο περιήγησης σελίδας - δοκιμάστε την [Ενημέρωση ρυθμίσεων εφαρμογής επίδειξης](http://azure-samples.github.io/powerbi-angular-client/#/scenario6)
  - Ορισμός προεπιλογών για σελίδες και φίλτρα - δοκιμάστε το [Ορισμός προεπιλογών επίδειξης](http://azure-samples.github.io/powerbi-angular-client/#/scenario5)
- Πληκτρολογήστε και έξοδος από τη λειτουργία πλήρους οθόνης

[Μάθετε περισσότερα σχετικά με την ενσωμάτωση μιας αναφοράς](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Embedding-Basics)


### <a name="navigate-to-pages-in-a-report"></a>Περιήγηση σε σελίδες σε μια αναφορά
Το enbales API της JavaScript για να ανακαλύψετε όλες τις σελίδες σε μια αναφορά και να ορίσετε την τρέχουσα σελίδα. Δοκιμάστε την [εφαρμογή επίδειξη περιήγησης](http://azure-samples.github.io/powerbi-angular-client/#/scenario3).

[Μάθετε περισσότερα σχετικά με την περιήγηση σελίδας](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Page-Navigation)

### <a name="filter-a-report"></a>Φιλτράρισμα μιας αναφοράς
Το API της JavaScript παρέχει βασικές και πρόσθετες δυνατότητες για ενσωματωμένο αναφορές και τις σελίδες αναφοράς φιλτραρίσματος. Δοκιμάστε το [Φιλτράρισμα επίδειξη εφαρμογή](http://azure-samples.github.io/powerbi-angular-client/#/scenario4)και εξετάστε ορισμένες εισαγωγικό κωδικό εδώ.  


#### <a name="basic-filters"></a>Βασικά φίλτρα
Ένα βασικό φίλτρο τοποθετείται σε ένα επίπεδο στήλης ή της ιεραρχίας και περιέχει μια λίστα τιμών για να συμπεριλάβετε ή να εξαιρέσετε.

```
const basicFilter: pbi.models.IBasicFilter = {
  $schema: "http://powerbi.com/product/schema#basic",
  target: {
    table: "Store",
    column: "Count"
  },
  operator: "In",
  values: [1,2,3,4]
}
```


#### <a name="advanced-filters"></a>Σύνθετα φίλτρα
Σύνθετα φίλτρα, χρησιμοποιήστε τον τελεστή λογική και ή OR και αποδεχτείτε μία ή δύο συνθήκες, κάθε μία με τις δικές τους τελεστή και την τιμή. Υποστηρίζονται οι συνθήκες είναι:

- Κανένας
- LessThan
- LessThanOrEqual
- GreaterThan
- GreaterThanOrEqual
- Περιέχει
- DoesNotContain
- StartsWith
- DoesNotStartWith
- Είναι
- IsNot
- IsBlank
- IsNotBlank

```
const advancedFilter: pbi.models.IAdvancedFilter = {
  $schema: "http://powerbi.com/product/schema#advanced",
  target: {
    table: "Store",
    column: "Name"
  },
  logicalOperator: "Or",
  conditions: [
    {
      operator: "Contains",
      value: "Wash"
    },
    {
      operator: "Contains",
      value: "Park"
    }
  ]
}
```
[Μάθετε περισσότερα σχετικά με το φιλτράρισμα](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Filters)


### <a name="handling-events"></a>Χειρισμού συμβάντων
Εκτός από την αποστολή πληροφοριών σε iframe, την εφαρμογή σας μπορεί επίσης να λάβετε πληροφορίες σχετικά με τα παρακάτω συμβάντα που προέρχονται από το iframe:

- Ενσωμάτωση
  - φόρτωση
  - Σφάλμα
- Αναφορές
  - pageChanged
  - dataSelected (σύντομα διαθέσιμο)

[Μάθετε περισσότερα σχετικά με χειρισμού συμβάντων](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Handling-Events)


## <a name="next-steps"></a>Επόμενα βήματα
Για περισσότερες πληροφορίες σχετικά με το API Power BI JavaScript, ελέγξτε τις παρακάτω συνδέσεις:

- [API Wiki της JavaScript](https://github.com/Microsoft/PowerBI-JavaScript/wiki)
- [Αναφορά μοντέλου αντικειμένου](https://microsoft.github.io/powerbi-models/modules/_models_.html)
- Δείγματα
  - [Γωνιωδών](http://azure-samples.github.io/powerbi-angular-client)
  - [Ember](https://github.com/Microsoft/powerbi-ember)
- [Επίδειξη Live](https://microsoft.github.io/PowerBI-JavaScript/demo/)
