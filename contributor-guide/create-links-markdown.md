<properties
   pageTitle="Δημιουργία συνδέσεων σε άρθρα markdown" description="Εξηγεί τον τρόπο κώδικα crosslinks στο markdown." metaKeywords="" services="" solutions="" documentationCenter="" authors="tysonn" videoId="" scriptId="" manager="carolz" />

<tags ms.service="contributor-guide" ms.devlang="" ms.topic="article" ms.tgt_pltfrm="" ms.workload="" ms.date="02/03/2015" ms.author="tysonn" />

# <a name="linking-guidance-for-azure-technical-content"></a>Σύνδεση οδηγίες για Azure τεχνικό περιεχόμενο

### <a name="links-from-one-acom-article-to-another"></a>Συνδέσεις από ένα άρθρο ACOM σε ένα άλλο

Για να δημιουργήσετε μια σύνδεση στην ενσωματωμένη από ένα άρθρο τεχνική ACOM ένα άλλο άρθρο τεχνική ACOM, χρησιμοποιήστε την ακόλουθη σύνταξη σύνδεσης:  

- Ένα άρθρο σε μια υπηρεσία συνδέσεων καταλόγου σε ένα άλλο άρθρο στον ίδιο κατάλογο υπηρεσίας:

  `[link text](article-name.md)`

- Ένα άρθρο συνδέσεις από μια υπηρεσία δευτερεύοντα κατάλογο για ένα άρθρο στον ριζικό κατάλογο:

  `[link text](../article-name.md)`

- Ένα άρθρο στις συνδέσεις ριζικού καταλόγου για ένα άρθρο σε έναν δευτερεύοντα κατάλογο υπηρεσίας: 

  `[link text](./service-directory/article-name.md)`

- Ένα άρθρο σε μια υπηρεσία συνδέσεις δευτερεύοντα κατάλογο για ένα άρθρο σε άλλη υπηρεσία δευτερεύοντα κατάλογο:

  `[link text](../service-directory/article-name.md)`
 

## <a name="links-to-anchors"></a>Συνδέσεις σε οι αγκυρώσεις

Δεν χρειάζεται να δημιουργήσετε οι αγκυρώσεις - δημιουργούνται αυτόματα στο χρόνο για όλες τις επικεφαλίδες H2 δημοσίευσης. Το μόνο που έχετε να κάνετε είναι να δημιουργήσετε συνδέσεις στις ενότητες H2.

- Για να συνδέσετε μια επικεφαλίδα μέσα στο ίδιο άρθρο:

  `[link](#the-text-of-the-H2-section-separated-by-hyphens)`  
  `[Create cache](#create-cache)`

- Για να δημιουργήσετε σύνδεση προς ένα σημείο αγκύρωσης σε ένα άλλο άρθρο στο στον ίδιο δευτερεύοντα κατάλογο:

  `[link text](article-name.md#anchor-name)`
  `[Configure your profile](media-services-create-account.md#configure-your-profile)`

- Για να συνδέσετε σε ένα σημείο αγκύρωσης σε άλλη υπηρεσία δευτερεύοντα κατάλογο:

  `[link text](../service-directory/article-name.md#anchor-name)`
  `[Configure your profile](../service-directory/media-services-create-account.md#configure-your-profile)`

Ένας τρόπος για την αυτοματοποίηση η διαδικασία για τη δημιουργία συνδέσεων σε άρθρα σας στις συνδέσεις αυτόματης δημιουργίας αγκύρωσης είναι [MarkdownAnchorLinkGenerator - ένα εργαλείο για να δημιουργήσετε συνδέσεις αγκύρωσης για ACOM στη σωστή μορφή](https://github.com/Azure/Azure-CSI-Content-Tools/tree/master/Tools/ACOMMarkdownAnchorLinkGenerator).

## <a name="links-from-includes"></a>Περιλαμβάνει συνδέσεις από

Εφόσον περιλαμβάνει αρχεία που βρίσκονται σε άλλο κατάλογο, θα χρειαστεί να χρησιμοποιούν πλέον σχετικές διαδρομές, όπως φαίνεται παρακάτω. Για να συνδέσετε ένα άρθρο από ένα αρχείο συμπερίληψης, χρησιμοποιήστε αυτήν τη μορφή:

    [link text](../articles/service-folder/article-name.md)
    
Μάθετε περισσότερα σχετικά με το πώς μπορείτε να χρησιμοποιήσετε ένα αρχείο συμπερίληψης στις [προσαρμοσμένες markdown επεκτάσεις οδηγίες](custom-markdown-extensions.md#includes).

## <a name="links-in-selectors"></a>Συνδέσεις σε εργαλεία επιλογής

Εάν έχετε εργαλεία επιλογής που είναι ενσωματωμένο σε μια συμπερίληψη, πρέπει να χρησιμοποιήσετε αυτήν την ταξινόμηση της σύνδεσης: 

    > [AZURE. ΕΠΙΛΟΓΈΑΣ ΛΊΣΤΑ (Dropdown1 | Dropdown2)]     -  [(Κείμενο1 | Παράδειγμα1)](../articles/service-folder/article-name1.md)
    - [(Κείμενο1 | Example2)](../articles/service-folder/article-name2.md)
    - [(Κείμενο2 | Example3)](../articles/service-folder/article-name3.md)
    - [(Κείμενο2 | Example4)](../articles/service-folder/article-name4.md)


## <a name="reference-style-links"></a>Στυλ αναφοράς συνδέσεις

Μπορείτε να χρησιμοποιήσετε συνδέσεις στυλ αναφοράς για να κάνετε πιο ευανάγνωστο το περιεχόμενό σας προέλευσης. Οι συνδέσεις στυλ αναφοράς αντικαταστήστε την ενσωματωμένη σύνταξη σύνδεση με απλοποιημένη σύνταξη που σας επιτρέπει να μετακινείτε τις διευθύνσεις URL μεγάλη στο τέλος αυτού του άρθρου. Ακολουθεί παράδειγμα του τόλμης Fireball:

Κείμενο ενσωματωμένου:

    I get 10 times more traffic from [Google][1] than from [Yahoo][2] or [MSN][3].

Σύνδεση αναφορές στο τέλος αυτού του άρθρου:

    <!--Reference links in article-->
    [1]: http://google.com/
    [2]: http://search.yahoo.com/  
    [3]: http://search.msn.com/

Βεβαιωθείτε ότι μπορείτε να συμπεριλάβετε το διάστημα μετά το ερωτηματικό, πριν από τη σύνδεση. Όταν συνδέετε με άλλα τεχνικά άρθρα, αν ξεχάσετε να συμπεριλάβετε το διάστημα, η σύνδεση θα διαχωριστεί στο δημοσιευμένο άρθρο. 

## <a name="link-to-acom-pages-that-are-not-part-of-the-technical-documentation-set"></a>Σύνδεση με ACOM σελίδες που δεν αποτελούν μέρος ενός συνόλου τεχνική τεκμηρίωση

Για να συνδεθείτε σε μια σελίδα στην ACOM (όπως μια σελίδα τιμολόγησης, SLA σελίδας ή οτιδήποτε άλλο που δεν είναι ένα άρθρο τεκμηρίωση), χρησιμοποιήστε μια απόλυτη διεύθυνση URL, αλλά παραλείψετε τις τοπικές ρυθμίσεις. Ο στόχος εδώ είναι που λειτουργούν συνδέσεις στο GitHub και στην τοποθεσία αποδίδονται:

    [link text](http://azure.microsoft.com/pricing/details/virtual-machines/)


## <a name="link-to-msdn-or-technet"></a>Σύνδεση με MSDN ή στο TechNet

Όταν θέλετε να συνδέσετε MSDN ή στο TechNet, χρησιμοποιήστε την πλήρη σύνδεση στο θέμα και καταργήστε το en-μας τοπικές ρυθμίσεις γλώσσας από τη σύνδεση. 

### <a name="use-friendly-link-text-for-all-links"></a>Χρησιμοποιήστε κείμενο φιλικό σύνδεσης για όλες τις συνδέσεις

Οι λέξεις που περιλαμβάνουν μια σύνδεση πρέπει να είναι φιλική - με άλλα λόγια, θα πρέπει να κανονική λέξεων στα Αγγλικά ή τον τίτλο της σελίδας που πραγματοποιείτε σύνδεση με. Μην χρησιμοποιείτε "κάντε κλικ εδώ". Είναι εσφαλμένες για SEO και δεν περιγράφουν καταλλήλως προορισμού.

**Διόρθωση:**

- `For more information, see the [contributor guide index](https://github.com/Azure/azure-content/blob/master/contributor-guide/contributor-guide-index.md).`

- `For more details, see the [SET TRANSACTION ISOLATION LEVEL](https://msdn.microsoft.com/library/ms173763.aspx) reference.`

**Εσφαλμένη:**

- `For more details, see [https://msdn.microsoft.com/library/ms173763.aspx](https://msdn.microsoft.com/library/ms173763.aspx).`

- `For more information, click [here](https://github.com/Azure/azure-content/blob/master/contributor-guide/contributor-guide-index.md).`


## <a name="fwlinks"></a>FWLinks

Αποφύγετε FWLinks (το σύστημα ανακατεύθυνση) στο περιεχόμενο azure.microsoft.com. Αυτά θα πρέπει να χρησιμοποιηθεί μόνο ως τελευταία λύση όταν πρέπει να δημιουργήσετε μια σύνδεση για μια σελίδα της οποίας η διεύθυνση URL δεν γνωρίζετε ακόμα. Στην πραγματικότητα σχεδόν ποτέ δεν είναι απαραίτητα. Για ACOM, ορίζετε το όνομα του αρχείου, ώστε να μπορείτε να γνωρίζετε τι θα είναι εκ των προτέρων. Ένα θέμα της βιβλιοθήκης που δεν είναι ακόμη δημοσιευμένο, μπορείτε να δημιουργήσετε μια σύνδεση που χρησιμοποιεί το θέμα GUID έτσι ώστε να μην χρειάζεται να χρησιμοποιήσετε μια σύνδεση.

Εάν πρέπει να χρησιμοποιήσετε μια σύνδεση σε μια ιστοσελίδα, συμπεριλάβετε την παράμετρο P για να είναι μια μόνιμη ανακατεύθυνση:

    http://go.microsoft.com/fwlink/p/?LinkId=389595

Όταν κάνετε επικόλληση τη διεύθυνση URL προορισμού το εργαλείο σύνδεση, θυμηθείτε να καταργήσετε τις τοπικές ρυθμίσεις, εάν η σύνδεση προορισμού είναι η βιβλιοθήκη ACOM, ή το MSDN ή στο TechNet.

## <a name="remember-the-azure-library-chrome"></a>Να θυμάστε το chrome Azure βιβλιοθήκη!
Εάν θέλετε να συνδέσετε με ένα θέμα Azure βιβλιοθήκη που βρίσκεται κάτω από [αυτόν τον κόμβο](https://msdn.microsoft.com/library/azure), μην ξεχάσετε να καθορίσετε το Azure chrome στο πλαίσιο σύνδεση (/ azure /). Το Azure chrome μοιράζεται τις επιλογές περιήγησης ACOM και εμφανίζει μόνο το περιεχόμενο της Azure της βιβλιοθήκης του MSDN. Μια σύνδεση σωστά τις μοιάζει κάπως έτσι:

    http://msdn.microsoft.com/library/azure/dd163896.aspx

Διαφορετικά, η σελίδα θα αποδοθεί στην τυπική προβολή MSDN, με ολόκληρο το δέντρο MSDN εμφανίζεται.

### <a name="contributors-guide-links"></a>Οι συνεργάτες των συνδέσεων Οδηγός

- [Το άρθρο Επισκόπηση](./../README.md)
- [Ευρετήριο άρθρων με οδηγίες](./contributor-guide-index.md)

<!--image references-->
[1]: ./media/create-tables-markdown/table-markdown.png
[2]: ./media/create-tables-markdown/break-tables.png
