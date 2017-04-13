<properties
   pageTitle="Η πύλη δεδομένων εσωτερικής εγκατάστασης | Microsoft Azure"
   description="Μια πύλη εσωτερικής εγκατάστασης είναι απαραίτητο, εάν σας σε διακομιστή υπηρεσιών ανάλυσης στο Azure θα συνδεθείτε σε προελεύσεις δεδομένων εσωτερικής εγκατάστασης."
   services="analysis-services"
   documentationCenter=""
   authors="minewiskan"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="analysis-services"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="owend"/>

# <a name="on-premises-data-gateway"></a>Η πύλη δεδομένων εσωτερικής εγκατάστασης

Η πύλη δεδομένων εσωτερικής εγκατάστασης λειτουργεί ως μια γέφυρα, παρέχοντας μεταφορά ασφαλούς δεδομένων μεταξύ των προελεύσεων δεδομένων εσωτερικής εγκατάστασης και του διακομιστή υπηρεσιών ανάλυσης Azure στο cloud.

Η πύλη είναι εγκατεστημένο στον υπολογιστή του δικτύου σας. Για κάθε διακομιστή υπηρεσιών ανάλυσης Azure στη συνδρομή σας στο Azure πρέπει να έχει εγκατασταθεί μια πύλη. Για παράδειγμα, εάν έχετε δύο διακομιστές στη συνδρομή σας στο Azure που συνδέονται με προελεύσεις δεδομένων εσωτερικής εγκατάστασης, πρέπει να έχει εγκατασταθεί μια πύλη σε δύο διαφορετικούς υπολογιστές στο δίκτυό σας.

## <a name="requirements"></a>Απαιτήσεις

**Ελάχιστες απαιτήσεις:**

- Διαίρεσης 4,5 .NET framework
- έκδοση 64 bit των Windows 7 / Windows Server 2008 R2 (ή νεότερη έκδοση)

**Προτεινόμενα:**

- 8 πυρήνα CPU
- 8 GB μνήμης
- έκδοση 64 bit των Windows 2012 R2 (ή νεότερη έκδοση)

**Σημαντικά θέματα:**

- Η πύλη δεν μπορεί να εγκατασταθεί σε έναν ελεγκτή τομέα.

- Μπορεί να εγκατασταθεί μόνο μία πύλη σε έναν υπολογιστή.

- Εγκατάσταση της πύλης σε έναν υπολογιστή που παραμένει και δεν τίθεται σε αναστολή λειτουργίας. Εάν ο υπολογιστής δεν είναι στο, σας σε διακομιστή υπηρεσιών ανάλυσης Azure δεν μπορεί να συνδεθεί με τις προελεύσεις δεδομένων εσωτερικής εγκατάστασης για την ανανέωση δεδομένων.

- Μην εγκαταστήσετε την πύλη σε έναν υπολογιστή ασύρματα συνδεδεμένοι στο δίκτυό σας. Μπορώ να υποβαθμιστεί επιδόσεων.

- Για να αλλάξετε το όνομα του διακομιστή για μια πύλη που έχει ήδη ρυθμιστεί, πρέπει να επαναλάβετε την εγκατάσταση και ρύθμιση παραμέτρων μια νέα πύλη.

- Σε ορισμένες περιπτώσεις, μοντέλα σε μορφή πίνακα τη σύνδεση προέλευσης δεδομένων με χρήση εγγενούς υπηρεσίες παροχής όπως SQL Server Native Client (SQLNCLI11) μπορεί να επιστρέψουν σφάλμα. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [συνδέσεις προέλευσης δεδομένων](analysis-services-datasource.md).

## <a name="supported-on-premises-data-sources"></a>Προελεύσεις δεδομένων υποστηρίζονται εσωτερικής εγκατάστασης
Για την προεπισκόπηση της πύλης υποστηρίζει συνδέσεις μεταξύ του διακομιστή υπηρεσιών ανάλυσης του Azure και τις ακόλουθες προελεύσεις δεδομένων εσωτερικής εγκατάστασης:

- SQL Server
- Αποθήκη δεδομένων του SQL
- ΣΗΜΕΊΑ ΠΡΌΣΒΑΣΗΣ
- Oracle
- Teradata


## <a name="download"></a>Λήψη
 [Κάντε λήψη της πύλης](https://aka.ms/azureasgateway)


## <a name="install-and-configure"></a>Εγκατάσταση και ρύθμιση παραμέτρων

1. Εκτέλεση του προγράμματος εγκατάστασης.

2. Επιλέξτε μια θέση εγκατάστασης και αποδεχτείτε τους όρους της άδειας χρήσης.

3. Πραγματοποιήστε είσοδο στο Azure.

4. Καθορίστε το όνομα του διακομιστή ανάλυσης Azure. Μπορείτε να καθορίσετε μόνο μία διακομιστή ανά πύλης. Κάντε κλικ στην επιλογή **Ρύθμιση παραμέτρων** και είστε έτοιμοι.

    ![Πραγματοποιήστε είσοδο στο azure](./media\analysis-services-gateway\aas-gateway-configure-server.png)


## <a name="how-it-works"></a>Πώς λειτουργεί
Η πύλη εκτελείται ως μια υπηρεσία των Windows, **η πύλη δεδομένων εσωτερικής εγκατάστασης**, σε έναν υπολογιστή στο δίκτυο της εταιρείας σας. Η πύλη εγκαταστήσετε για χρήση με τις υπηρεσίες ανάλυσης Azure βασίζεται στην ίδια πύλη χρησιμοποιείται για άλλες υπηρεσίες όπως το Power BI, αλλά με ορισμένες διαφορές πώς έχει ρυθμιστεί.

![Πώς λειτουργεί](./media/analysis-services-gateway/aas-gateway-how-it-works.png)

Ερωτημάτων και δεδομένων εργασία ροής ως εξής:

1.  Δημιουργείται ένα ερώτημα με την υπηρεσία cloud με το κρυπτογραφημένο διαπιστευτήρια για την προέλευση δεδομένων εσωτερικής εγκατάστασης. Στη συνέχεια, έχει σταλεί σε μια ουρά για την πύλη για την επεξεργασία.

2.  Η υπηρεσία cloud πύλης αναλύει το ερώτημα και προωθεί την αίτηση στο [Azure Service Bus](https://azure.microsoft.com/documentation/services/service-bus/).

3.  Η πύλη δεδομένων εσωτερικής εγκατάστασης σταθμοσκοπεί Azure Service Bus για αιτήσεων σε εκκρεμότητα.

4.  Η πύλη λαμβάνει το ερώτημα, αποκρυπτογραφεί τα διαπιστευτήρια και συνδέεται με τις προελεύσεις δεδομένων με αυτά τα διαπιστευτήρια.

5.  Η πύλη στέλνει το ερώτημα με την προέλευση δεδομένων για εκτέλεση.

6.  Τα αποτελέσματα αποστέλλονται από την προέλευση δεδομένων, πίσω για την πύλη και, στη συνέχεια σε την υπηρεσία cloud.

## <a name="windows-service-account"></a>Λογαριασμός υπηρεσίας Windows

Η πύλη δεδομένων εσωτερικής εγκατάστασης έχει ρυθμιστεί να χρησιμοποιεί *NT SERVICE\PBIEgwService* για τα διαπιστευτήρια σύνδεσης υπηρεσίας Windows. Από προεπιλογή, που έχει στα δεξιά της σύνδεσης ως υπηρεσία; στο περιβάλλον του υπολογιστή που θέλετε να εγκαταστήσετε την πύλη στην. Αυτά τα διαπιστευτήρια δεν είναι το ίδιο λογαριασμό που χρησιμοποιήσατε για να συνδεθείτε σε προελεύσεις δεδομένων εσωτερικής εγκατάστασης ή Azure το λογαριασμό σας.  

Εάν αντιμετωπίσετε προβλήματα με το διακομιστή μεσολάβησης λόγω ελέγχου ταυτότητας, ίσως θέλετε να αλλάξετε το λογαριασμό της υπηρεσίας Windows σε ένα χρήστη τομέα ή Διαχείριση λογαριασμού υπηρεσίας.

## <a name="ports"></a>Θύρες

Η πύλη δημιουργεί μια σύνδεση εξερχομένων σε Bus υπηρεσίας Azure. Επικοινωνεί σε εξερχομένων θύρες: TCP 443 (προεπιλογή), 5671, 5672, 9350 έως 9354.  Η πύλη δεν απαιτεί θύρες εισερχομένων.

Συνιστάται να whitelist οι διευθύνσεις IP για την περιοχή δεδομένων σας στο τείχος προστασίας σας. Μπορείτε να κάνετε λήψη τη [λίστα διευθύνσεων IP του Microsoft Azure κέντρου δεδομένων](https://www.microsoft.com/download/details.aspx?id=41653). Αυτή η λίστα ενημερώνεται κάθε εβδομάδα.

> [AZURE.NOTE]  Οι διευθύνσεις IP που αναφέρονται στη λίστα διευθύνσεων IP του κέντρου δεδομένων του Azure είναι στο CIDR σημειογραφία. Για παράδειγμα, 10.0.0.0/24 δεν σημαίνει 10.0.0.0 μέσω 10.0.0.24. Μάθετε περισσότερα σχετικά με τη [σημειογραφία CIDR](http://whatismyipaddress.com/cidr).

Οι παρακάτω είναι τα ονόματα πλήρως προσδιορισμένο τομέα που χρησιμοποιείται από την πύλη.

|Ονόματα τομέων|Θύρες εξερχομένων|Περιγραφή|
|---|---|---|
|*. powerbi.com|80|HTTP που χρησιμοποιείται για τη λήψη του προγράμματος εγκατάστασης.|
|*. powerbi.com|443|HTTPS|
|*. analysis.windows.net|443|HTTPS|
|*. login.windows.net|443|HTTPS|
|*. servicebus.windows.net|5671 5672|Για προχωρημένους μηνυμάτων Protocol (AMQP)|
|*. servicebus.windows.net|443, 9350 9354|Ακροατών στην υπηρεσία Bus μετάδοση μέσω TCP (απαιτεί 443 για διακριτικού απόκτηση ελέγχου πρόσβασης)|
|*. frontend.clouddatahub.net|443|HTTPS|
|*. core.windows.net|443|HTTPS|
|Login.microsoftonline.com|443|HTTPS|
|*. msftncsi.com|443|Χρησιμοποιείται για τη δοκιμή σύνδεσης στο internet, εάν η πύλη δεν είναι προσβάσιμη από την υπηρεσία Power BI.|
|*.microsoftonline p.com|443|Χρησιμοποιείται για τον έλεγχο ταυτότητας, ανάλογα με τη ρύθμιση των παραμέτρων.|


### <a name="forcing-https-communication-with-azure-service-bus"></a>Να επιβάλετε HTTPS επικοινωνίας με Bus υπηρεσίας Azure

Μπορείτε να επιβάλετε την πύλη στην επικοινωνία με το Azure Service Bus χρησιμοποιώντας το HTTPS αντί για άμεση TCP; Ωστόσο, αυτό μπορεί να μειώσει σημαντικά επιδόσεων. Πρέπει να τροποποιήσετε το αρχείο *Microsoft.PowerBI.DataMovement.Pipeline.GatewayCore.dll.config* . Αλλαγή της τιμής από `AutoDetect` να `Https`. Αυτό το αρχείο βρίσκεται, από προεπιλογή, στην *πύλη δεδομένων C:\Program Files\On εσωτερικής εγκατάστασης*.

```
<setting name="ServiceBusSystemConnectivityModeString" serializeAs="String">
    <value>Https</value>
</setting>
```


## <a name="troubleshooting"></a>Αντιμετώπιση προβλημάτων
Εμφάνιση σύνθετων ρυθμίσεων, η πύλη δεδομένων εσωτερικής εγκατάστασης που χρησιμοποιείται για τη σύνδεση υπηρεσιών ανάλυσης του Azure με τις προελεύσεις δεδομένων εσωτερικής εγκατάστασης είναι η ίδια πύλη χρησιμοποιούνται με το Power BI.

Εάν αντιμετωπίζετε προβλήματα κατά την εγκατάσταση και ρύθμιση παραμέτρων μιας πύλης, φροντίστε να ανατρέξτε στο θέμα [Αντιμετώπιση προβλημάτων της πύλης Power BI](https://powerbi.microsoft.com/documentation/powerbi-gateway-onprem-tshoot/). Εάν πιστεύετε ότι έχετε κάποιο πρόβλημα με το τείχος προστασίας, ανατρέξτε στις ενότητες τείχος προστασίας ή διακομιστή μεσολάβησης.

Εάν πιστεύετε ότι αντιμετωπίζει προβλήματα διακομιστή μεσολάβησης, με την πύλη, ανατρέξτε στο θέμα [Ρύθμιση παραμέτρων ρυθμίσεις διακομιστή μεσολάβησης για το Power BI πυλών](https://powerbi.microsoft.com/documentation/powerbi-gateway-proxy.md).

## <a name="next-steps"></a>Επόμενα βήματα
- [Διαχείριση των υπηρεσιών ανάλυσης](analysis-services-manage.md)
- [Λήψη δεδομένων από υπηρεσίες ανάλυσης του Azure](analysis-services-connect.md)