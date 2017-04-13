
<properties
    pageTitle="Γρήγορα συστάσεις σε δέσμες: υπολογιστή εκμάθησης API συστάσεις | Microsoft Azure"
    description="Azure μηχανικής εκμάθησης συστάσεις--γρήγορα συστάσεις σε δέσμες"
    services="cognitive-services"
    documentationCenter=""
    authors="luiscabrer"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="cognitive-services"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/17/2016"
    ms.author="luisca"/>

# <a name="get-recommendations-in-batches"></a>Λάβετε συστάσεις σε δέσμες

>[AZURE.NOTE] Γρήγορα συστάσεις σε δέσμες είναι πιο σύνθετη από γρήγορα συστάσεις μία κάθε φορά. Ελέγξτε τα API για πληροφορίες σχετικά με τον τρόπο για να δείτε προτάσεις για μία μόνο αίτηση:

> [Συστάσεις στοιχείο σε στοιχείο](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f30d77eda5650db055a3d4)<br>
> [Συστάσεις χρήστη σε στοιχείο](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f30d77eda5650db055a3dd)
>
> Μαζική βαθμολογίας λειτουργεί μόνο για τις εκδόσεις που έχουν δημιουργηθεί μετά τις 21 Ιουλίου 2016.


Υπάρχουν περιπτώσεις στις οποίες πρέπει να λάβετε συστάσεις για περισσότερα από ένα στοιχείο κάθε φορά. Για παράδειγμα, ίσως σας ενδιαφέρει να δημιουργήσετε cache συστάσεις ή ακόμα και την ανάλυση των τύπων του συστάσεις που λαμβάνετε.

Βαθμολογίας λειτουργίες μαζικής επεξεργασίας, όπως αυτά, ονομάζουμε είναι ασύγχρονων λειτουργιών. Πρέπει να υποβάλετε την αίτηση, περιμένετε να ολοκληρωθεί η λειτουργία και, στη συνέχεια, να συγκεντρώσετε τα αποτελέσματα.  

Για να είναι πιο ακριβείς, αυτά είναι τα βήματα για να παρακολουθήσετε:

1.  Δημιουργήστε ένα κοντέινερ χώρου αποθήκευσης Azure, εάν δεν έχετε ήδη.
2.  Αποστολή αρχείου εισαγωγής που περιγράφει κάθε μία από τις αιτήσεις σύσταση με το χώρο αποθήκευσης αντικειμένων Blob του Azure.
3.  Kick-Start βαθμολογίας μαζική εργασία.
4.  Περιμένετε να ολοκληρωθεί η λειτουργία ασύγχρονης.
5.  Όταν ολοκληρωθεί η λειτουργία, συλλογή τα αποτελέσματα από το χώρο αποθήκευσης αντικειμένων Blob.

Ας δούμε κάθε ένα από τα παρακάτω βήματα.

## <a name="create-a-storage-container-if-you-dont-have-one-already"></a>Δημιουργήστε ένα κοντέινερ χώρου αποθήκευσης, εάν δεν έχετε ήδη

Μεταβείτε στην [πύλη του Azure](https://portal.azure.com) και δημιουργήστε έναν νέο λογαριασμό του χώρου αποθήκευσης, εάν δεν έχετε ήδη. Για να το κάνετε αυτό, μεταβείτε στη **νέα** > **δεδομένων** + **χώρου αποθήκευσης** > **Λογαριασμού χώρου αποθήκευσης**.

Αφού έχετε ένα λογαριασμό του χώρου αποθήκευσης, πρέπει να δημιουργήσετε το κοντέινερ αντικειμένων blob πού θα αποθηκεύσετε το εισόδου και εξόδου της εκτέλεσης δέσμης.

Αποστολή αρχείου εισαγωγής που περιγράφει κάθε μία από τις σύσταση ζητά από χώρο αποθήκευσης αντικειμένων Blob--ας κλήση εδώ input.json το αρχείο.
Αφού έχετε ένα κοντέινερ, πρέπει να αποστείλετε ένα αρχείο που περιγράφει κάθε μία από τις αιτήσεις που χρειάζεστε για να εκτελέσετε από την υπηρεσία συστάσεις.

Μια δέσμη να εκτελέσετε έναν μόνο τύπο αίτησης από μια συγκεκριμένη build. Θα σας θα εξηγούν τον τρόπο για να ορίσετε αυτές τις πληροφορίες στην επόμενη ενότητα. Τώρα, ας υποθέσουμε ότι πραγματοποιούμε συστάσεις στοιχείου από μια συγκεκριμένη Δόμηση. Το αρχείο εισαγωγής, στη συνέχεια, περιέχει τις πληροφορίες εισόδου (σε αυτήν την περίπτωση, τα στοιχεία σπόρων) για κάθε μία από τις αιτήσεις.

Αυτό είναι ένα παράδειγμα του τι το αρχείο input.json είναι:

    {
      "requests": [
      { "SeedItems": [ "C9F-00163", "FKF-00689" ] },
      { "SeedItems": [ "F34-03453" ] },
      { "SeedItems": [ "D16-3244" ] },
      { "SeedItems": [ "C9F-00163", "FKF-00689" ] },
      { "SeedItems": [ "F43-01467" ] },
      { "SeedItems": [ "BD5-06013" ] },
      { "SeedItems": [ "P45-00163", "FKF-00689" ] },
      { "SeedItems": [ "C9A-69320" ] }
      ]
    }

Όπως μπορείτε να δείτε, το αρχείο είναι ένα αρχείο JSON, όπου κάθε μία από τις αιτήσεις έχει τις πληροφορίες που είναι απαραίτητο να στείλετε μια αίτηση συστάσεις. Δημιουργία αρχείου παρόμοια JSON για τις αιτήσεις που χρειάζεστε για την κάλυψη και αντιγράψτε το στο κοντέινερ που μόλις δημιουργήσατε στο χώρο αποθήκευσης αντικειμένων Blob.

## <a name="kick-start-the-batch-job"></a>Kick-Start τη μαζική εργασία

Το επόμενο βήμα είναι να υποβάλετε μια νέα εργασία δέσμης. Για περισσότερες πληροφορίες, επιλέξτε την [αναφορά API](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/).

Σώμα της αίτησης του API πρέπει να ορίσετε τις θέσεις όπου το εισαγωγής εξόδου και αρχεία σφάλματος πρέπει να είναι αποθηκευμένο. Πρέπει επίσης να ορίσετε τα διαπιστευτήρια που είναι απαραίτητα για να αποκτήσετε πρόσβαση σε αυτές τις θέσεις. Επίσης, πρέπει να καθορίσετε ορισμένες παράμετροι που ισχύουν για τη δέσμη ολόκληρη (ο τύπος του συστάσεις για να ζητήσετε, μοντέλο/Δημιουργία για να χρησιμοποιήσετε, τον αριθμό των αποτελεσμάτων ανά κλήση, κ.ο.κ.)

Αυτό είναι ένα παράδειγμα του τι πρέπει να μοιάζει σώμα της αίτησης:

    {
      "input": {
        "authenticationType": "PublicOrSas",
        "baseLocation": "https://mystorage1.blob.core.windows.net/",
        "relativeLocation": "container1/batchInput.json",
        "sasBlobToken": "?sv=2015-07_restofToken_…4Z&sp=rw"
      },
      "output": {
        "authenticationType": "PublicOrSas",
        "baseLocation": "https://mystorage1.blob.core.windows.net/",
        "relativeLocation": "container1/batchOutput.json ",
        "sasBlobToken": "?sv=2015-07_restofToken_…4Z&sp=rw"
      },
      "error": {
        "authenticationType": "PublicOrSas",
        "baseLocation": "https://mystorage1.blob.core.windows.net/",
        "relativeLocation": "container1/errors.txt",
        "sasBlobToken": "?sv=2015-07_restofToken_…4Z&sp=rw"
      },
      "job": {
        "apiName": "ItemRecommend",
        "modelId": "9ac12a0a-1add-4bdc-bf42-c6517942b3a6",
        "buildId": 1015703,
        "numberOfResults": 10,
        "includeMetadata": true,
        "minimalScore": 0.0
      }
    }

Δείτε εδώ μερικά σημαντικά πράγματα που πρέπει να λάβετε υπόψη:

-   Προς το παρόν, **authenticationType** πρέπει πάντα να οριστεί ως **PublicOrSas**.

-   Πρέπει να λάβετε ένα διακριτικό θέσει σε κοινή χρήση Access υπογραφή (συσχετισμών Ασφαλείας) για να επιτρέψετε την API συστάσεις για ανάγνωση και εγγραφή από/προς το λογαριασμό χώρου αποθήκευσης αντικειμένων Blob. Μπορείτε να βρείτε περισσότερες πληροφορίες σχετικά με τον τρόπο δημιουργίας διακριτικά συσχετισμών Ασφαλείας στη [σελίδα API συστάσεις](../storage/storage-dotnet-shared-access-signature-part-1.md).

-   Το μόνο **apiName** που υποστηρίζεται αυτήν τη στιγμή είναι **ItemRecommend**, που χρησιμοποιείται για το στοιχείο σε στοιχείο συστάσεις. Δέσμης για αυτήν τη στιγμή δεν υποστηρίζει συστάσεις χρήστη στο στοιχείο.

## <a name="wait-for-the-asynchronous-operation-to-finish"></a>Περιμένετε να ολοκληρωθεί η λειτουργία ασύγχρονης

Κατά την εκκίνηση της λειτουργίας δέσμης, η απόκριση επιστρέφει την κεφαλίδα λειτουργία θέση που παρέχει τις πληροφορίες που είναι απαραίτητες για την παρακολούθηση της λειτουργίας.
Μπορείτε να παρακολουθείτε τη λειτουργία με τη χρήση του [API ανάκτηση κατάστασης λειτουργίας]( https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f30d77eda5650db055a3da), ακριβώς όπως θα κάνατε για την παρακολούθηση της λειτουργίας μιας λειτουργίας δόμησης.

## <a name="get-the-results"></a>Λάβετε τα αποτελέσματα

Αφού ολοκληρωθεί η λειτουργία, με την προϋπόθεση ότι δεν υπάρχουν σφάλματα, μπορείτε να συγκεντρώσετε τα αποτελέσματα από την έξοδο αντικειμένων Blob χώρου αποθήκευσης.

Το παρακάτω παράδειγμα εμφανίζει πώς μπορεί να φαίνεται το αποτέλεσμα. Σε αυτό το παράδειγμα, θα σας δείξουμε αποτελέσματα για μια δέσμη με δύο μόνο αιτήματα (για συντομία).

    {
      "results":
      [   
        {
          "request": { "seedItems": [ "DAF-00500", "P3T-00003" ] },
          "recommendations": [
            {
              "items": [
                {
                  "itemId": "F5U-00011",
                  "name": "L2 Ethernet Adapter-Win8Pro SC EN/XD/XX Hdwr",
                  "metadata": ""
                }
              ],
              "rating": 0.722,
              "reasoning": [ "People who like the selected items also like 'L2 Ethernet Adapter-Win8Pro SC EN/XD/XX Hdwr'" ]
            },
            {
              "items": [
                {
                  "itemId": "G5Y-00001",
                  "name": "Docking Station for Srf Pro/Pro2 SC EN/XD/ES Hdwr",
                  "metadata": ""
                }
              ],
              "rating": 0.718,
              "reasoning": [ "People who like the selected items also like 'Docking Station for Srf Pro/Pro2 SC EN/XD/ES Hdwr'" ]
            }
          ]
        },
        {
          "request": { "seedItems": [ "C9F-00163" ] },
          "recommendations": [
            {
              "items": [
                {
                  "itemId": "C9F-00172",
                  "name": "Nokia 2K Shell for Nokia Lumia 630/635 - Green",
                  "metadata": ""
                }
              ],
              "rating": 0.649,
              "reasoning": [ "People who like 'MOZO Flip Cover for Nokia Lumia 635 - White' also like 'Nokia 2K Shell for Nokia Lumia 630/635 - Green'" ]
            },
            {
              "items": [
                {
                  "itemId": "C9F-00171",
                  "name": "Nokia 2K Shell for Nokia Lumia 630/635 - Orange",
                  "metadata": ""
                }
              ],
              "rating": 0.647,
              "reasoning": [ "People who like 'MOZO Flip Cover for Nokia Lumia 635 - White' also like 'Nokia 2K Shell for Nokia Lumia 630/635 - Orange'" ]
            },
            {
              "items": [
                {
                  "itemId": "C9F-00170",
                  "name": "Nokia 2K Shell for Nokia Lumia 630/635 - Yellow",
                  "metadata": ""
                }
              ],
              "rating": 0.646,
              "reasoning": [ "People who like 'MOZO Flip Cover for Nokia Lumia 635 - White' also like 'Nokia 2K Shell for Nokia Lumia 630/635 - Yellow'" ]
            }       
          ]
        }
    ]}


## <a name="learn-about-the-limitations"></a>Μάθετε περισσότερα σχετικά με τους περιορισμούς

-   Μπορεί να είναι η οποία ονομάζεται μαζική μόνο μία εργασία ανά συνδρομή κάθε φορά.
-   Ένα αρχείο εισαγωγής δέσμης εργασίας δεν μπορεί να είναι περισσότερες από 2 MB.
