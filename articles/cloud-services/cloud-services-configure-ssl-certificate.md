<properties 
    pageTitle="Ρύθμιση παραμέτρων SSL για μια υπηρεσία cloud (κλασικό) | Microsoft Azure" 
    description="Μάθετε πώς μπορείτε να καθορίσετε ένα τελικό σημείο HTTPS για ένα ρόλο web και πώς να αποστείλετε ένα πιστοποιητικό SSL για την ασφάλιση της εφαρμογής σας." 
    services="cloud-services" 
    documentationCenter=".net" 
    authors="Thraka" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/04/2016"
    ms.author="adegeo"/>




# <a name="configuring-ssl-for-an-application-in-azure"></a>Ρύθμιση παραμέτρων SSL για μια εφαρμογή στο Azure

> [AZURE.SELECTOR]
- [Πύλη του Azure](cloud-services-configure-ssl-certificate-portal.md)
- [Azure κλασική πύλη](cloud-services-configure-ssl-certificate.md)

Secure Socket Layer (SSL) με κρυπτογράφησης είναι η πιο συχνά χρησιμοποιούμενες μέθοδος ασφάλεια των δεδομένων που αποστέλλονται μέσω του internet. Αυτή η εργασία κοινές περιγράφει τον τρόπο για να καθορίσετε ένα τελικό σημείο HTTPS για ένα ρόλο web και να αποστείλετε ένα πιστοποιητικό SSL για την ασφάλιση της εφαρμογής σας.

> [AZURE.NOTE] Εφαρμόστε τις διαδικασίες σε αυτήν την εργασία με τις υπηρεσίες Cloud Azure; για τις υπηρεσίες εφαρμογών, ανατρέξτε [σε αυτό](../app-service-web/web-sites-configure-ssl-certificate.md) το άρθρο.

Αυτή η εργασία χρησιμοποιεί μια ανάπτυξη παραγωγής. Πληροφορίες σχετικά με τη χρήση μιας ενδιάμεσου σταδίου ανάπτυξης παρέχονται στο τέλος αυτού του θέματος.

Διαβάστε [αυτό](cloud-services-how-to-create-deploy.md) το άρθρο πρώτα, εάν δεν έχετε ακόμη δημιουργήσει μια υπηρεσία στο cloud.

[AZURE.INCLUDE [websites-cloud-services-css-guided-walkthrough](../../includes/websites-cloud-services-css-guided-walkthrough.md)]


## <a name="step-1-get-an-ssl-certificate"></a>Βήμα 1: Λήψη πιστοποιητικού SSL

Για να ρυθμίσετε τις παραμέτρους SSL για μια εφαρμογή, πρέπει πρώτα να λάβετε ένα πιστοποιητικό SSL που έχει υπογραφεί από μια αρχή έκδοσης πιστοποιητικών (CA), αξιόπιστων τρίτους εκδίδει πιστοποιητικά για αυτόν το σκοπό. Εάν δεν έχετε ήδη ένα, πρέπει να αποκτήσετε ένα από μια εταιρεία που πωλεί πιστοποιητικά SSL.

Το πιστοποιητικό πρέπει να ικανοποιεί τις ακόλουθες απαιτήσεις για πιστοποιητικά SSL στο Azure:

-   Το πιστοποιητικό πρέπει να περιέχει ένα ιδιωτικό κλειδί.
-   Το πιστοποιητικό πρέπει να δημιουργηθούν για ανταλλαγή κλειδιών, δυνατότητα εξαγωγής σε ένα αρχείο ανταλλαγής προσωπικών πληροφοριών (.pfx).
-   Όνομα θέματος το πιστοποιητικό πρέπει να συμφωνεί με τον τομέα που χρησιμοποιείται για πρόσβαση στην υπηρεσία cloud. Δεν μπορείτε να αποκτήσετε ένα πιστοποιητικό SSL από μια αρχή έκδοσης πιστοποιητικών (CA) για τον τομέα cloudapp.net. Θα πρέπει να αποκτήσετε ένα προσαρμοσμένο όνομα τομέα να τα χρησιμοποιείτε όταν πρόσβαση στην υπηρεσία. Όταν κάνετε αίτηση για ένα πιστοποιητικό από μια αρχή έκδοσης Πιστοποιητικών, όνομα θέματος το πιστοποιητικό πρέπει να συμφωνεί με το προσαρμοσμένο όνομα τομέα χρησιμοποιείται για πρόσβαση σε εφαρμογή σας. Για παράδειγμα, εάν το προσαρμοσμένο όνομα τομέα είναι **contoso.com** μπορείτε να ζητήσετε ένα πιστοποιητικό από την αρχή έκδοσης Πιστοποιητικών για * **. contoso.com** ή * *www.contoso.com**.
-   Το πιστοποιητικό πρέπει να χρησιμοποιήσετε τουλάχιστον κρυπτογράφηση 2048 bit.

Για σκοπούς δοκιμής, μπορείτε να [δημιουργήσετε](cloud-services-certs-create.md) και να χρησιμοποιήσετε ένα αυτο-υπογεγραμμένο πιστοποιητικό. Ένα αυτο-υπογεγραμμένο πιστοποιητικό δεν έχει γίνει έλεγχος ταυτότητας από μια αρχή έκδοσης Πιστοποιητικών και να χρησιμοποιήσετε τον τομέα cloudapp.net ως τη διεύθυνση URL της τοποθεσίας Web. Για παράδειγμα, η ακόλουθη εργασία χρησιμοποιεί ένα πιστοποιητικό αυτόματης υπογραφής με την οποία το κοινό όνομα (CN) που χρησιμοποιούνται στο πιστοποιητικό είναι **sslexample.cloudapp.net**.

Στη συνέχεια, πρέπει να συμπεριλάβετε πληροφορίες σχετικά με το πιστοποιητικό στο ορισμού υπηρεσίας και αρχεία ρύθμισης παραμέτρων της υπηρεσίας.

## <a name="step-2-modify-the-service-definition-and-configuration-files"></a>Βήμα 2: Τροποποίηση τα αρχεία ορισμού και τη ρύθμιση παραμέτρων της υπηρεσίας

Η εφαρμογή σας πρέπει να ρυθμιστεί για να χρησιμοποιήσετε το πιστοποιητικό και πρέπει να προστεθεί ένα τελικό σημείο HTTPS. Ως αποτέλεσμα, το ορισμό υπηρεσίας και αρχεία ρύθμισης παραμέτρων υπηρεσίας πρέπει να ενημερωθούν.

1.  Στο περιβάλλον ανάπτυξής σας, ανοίξτε το αρχείο ορισμού υπηρεσίας (CSDEF), προσθέστε μια ενότητα **πιστοποιητικά** μέσα στην ενότητα **WebRole** και περιλαμβάνει τις ακόλουθες πληροφορίες σχετικά με το πιστοποιητικό (και ενδιάμεσα πιστοποιητικά):

        <WebRole name="CertificateTesting" vmsize="Small">
        ...
            <Certificates>
                <Certificate name="SampleCertificate" 
                             storeLocation="LocalMachine" 
                             storeName="My"
                             permissionLevel="limitedOrElevated" />
                <!-- IMPORTANT! Unless your certificate is either
                self-signed or signed directly by the CA root, you
                must include all the intermediate certificates
                here. You must list them here, even if they are
                not bound to any endpoints. Failing to list any of
                the intermediate certificates may cause hard-to-reproduce
                interoperability problems on some clients.-->
                <Certificate name="CAForSampleCertificate"
                             storeLocation="LocalMachine"
                             storeName="CA"
                             permissionLevel="limitedOrElevated" />
            </Certificates>
        ...
        </WebRole>

    Στην ενότητα **πιστοποιητικά** Καθορίζει το όνομα του πιστοποιητικού μας, τη θέση και το όνομα του χώρου αποθήκευσης όπου βρίσκεται.
    
    Δικαιώματα (`permisionLevel` χαρακτηριστικό) μπορεί να οριστεί σε μία από τις παρακάτω τιμές:

  	| Τιμή δικαιωμάτων  | Περιγραφή |
  	| ----------------  | ----------- |
  	| limitedOrElevated | **(Προεπιλογή)** Όλες οι διεργασίες ρόλο να αποκτήσετε πρόσβαση στο ιδιωτικό κλειδί. |
  	| αναβαθμισμένα δικαιώματα          | Μόνο αναβαθμισμένα δικαιώματα διαδικασίες να αποκτήσετε πρόσβαση στο ιδιωτικό κλειδί.|

2.  Στο αρχείο ορισμού υπηρεσίας, προσθέστε ένα στοιχείο **InputEndpoint** μέσα στην ενότητα **τελικά σημεία** για να ενεργοποιήσετε την HTTPS:

        <WebRole name="CertificateTesting" vmsize="Small">
        ...
            <Endpoints>
                <InputEndpoint name="HttpsIn" protocol="https" port="443" 
                    certificate="SampleCertificate" />
            </Endpoints>
        ...
        </WebRole>

3.  Στο αρχείο ορισμού υπηρεσίας, προσθέστε ένα στοιχείο **σύνδεση** μέσα στην ενότητα **τοποθεσίες** . Αυτή η ενότητα προσθέτει μια σύνδεση HTTPS για να αντιστοιχίσετε το τελικό σημείο στην τοποθεσία σας:

        <WebRole name="CertificateTesting" vmsize="Small">
        ...
            <Sites>
                <Site name="Web">
                    <Bindings>
                        <Binding name="HttpsIn" endpointName="HttpsIn" />
                    </Bindings>
                </Site>
            </Sites>
        ...
        </WebRole>

    Έχουν ολοκληρωθεί όλες τις απαραίτητες αλλαγές για το αρχείο ορισμού υπηρεσίας, αλλά εξακολουθείτε να χρειάζεστε για να προσθέσετε τις πληροφορίες του πιστοποιητικού για το αρχείο ρύθμισης παραμέτρων υπηρεσίας.

4.  Στο το αρχείο ρύθμισης παραμέτρων υπηρεσίας (CSCFG), ServiceConfiguration.Cloud.cscfg, προσθέστε μια ενότητα **πιστοποιητικά** μέσα σε ενότητα **ρόλο** , αντικαταστήστε την τιμή αποτύπωση δείγμα που φαίνεται παρακάτω με αυτήν του πιστοποιητικού:

        <Role name="Deployment">
        ...
            <Certificates>
                <Certificate name="SampleCertificate" 
                    thumbprint="9427befa18ec6865a9ebdc79d4c38de50e6316ff" 
                    thumbprintAlgorithm="sha1" />
                <Certificate name="CAForSampleCertificate"
                    thumbprint="79d4c38de50e6316ff9427befa18ec6865a9ebdc" 
                    thumbprintAlgorithm="sha1" />
            </Certificates>
        ...
        </Role>

(Το προηγούμενο παράδειγμα χρησιμοποιεί **sha1** για την αποτύπωση αλγόριθμο. Καθορίστε την κατάλληλη τιμή για Αλγόριθμος αποτύπωσης πιστοποιητικού).

Τώρα που η ορισμού υπηρεσίας και αρχεία ρύθμισης παραμέτρων της υπηρεσίας έχουν ενημερωθεί, πακέτου για αποστολή σε Azure σας ανάπτυξη. Εάν χρησιμοποιείτε **cspack**, μην χρησιμοποιήσετε τη σημαία **/generateConfigurationFile** , όπως που αντικαθιστά τις πληροφορίες πιστοποιητικού που έχετε εισαγάγει.

## <a name="step-3-upload-a-certificate"></a>Βήμα 3: Αποστολή ενός πιστοποιητικού

Το πακέτο ανάπτυξης έχει ενημερωθεί για να χρησιμοποιήσετε το πιστοποιητικό και έχει προστεθεί ένα τελικό σημείο HTTPS. Τώρα μπορείτε να αποστείλετε το πακέτο και το πιστοποιητικό στο Azure με την πύλη του Azure κλασική.

1. Συνδεθείτε στο [Azure κλασική πύλη][]. 
2. Κάντε κλικ στην επιλογή **Υπηρεσίες Cloud** στο παράθυρο περιήγησης αριστερή πλευρά.
3. Κάντε κλικ στην υπηρεσία cloud που θέλετε.
4. Κάντε κλικ στην καρτέλα **πιστοποιητικά** .

    ![Κάντε κλικ στην καρτέλα πιστοποιητικών](./media/cloud-services-configure-ssl-certificate/click-cert.png)

5. Κάντε κλικ στο κουμπί **Αποστολή** .

    ![Αποστολή](./media/cloud-services-configure-ssl-certificate/upload-button.png)
    
6. Δώστε στο **αρχείο**, **τον κωδικό πρόσβασης**, και, στη συνέχεια, κάντε κλικ στην επιλογή **Ολοκλήρωση** (το σημάδι ελέγχου).

## <a name="step-4-connect-to-the-role-instance-by-using-https"></a>Βήμα 4: Σύνδεση με την παρουσία ρόλων με χρήση HTTPS

Τώρα που ανάπτυξή σας βρίσκεται σε λειτουργία στο Azure, μπορείτε να συνδεθείτε σε αυτήν χρησιμοποιώντας το HTTPS.

1.  Στην κλασική Azure πύλη, επιλέξτε την ανάπτυξή σας και, στη συνέχεια, κάντε κλικ στη σύνδεση στην περιοχή **Διεύθυνση URL της τοποθεσίας**.

    ![Καθορισμός διεύθυνσης URL της τοποθεσίας][2]

2.  Στο πρόγραμμα περιήγησης web, τροποποιήστε τη σύνδεση για να χρησιμοποιήσετε **https** αντί για **http**και, στη συνέχεια, επισκεφθείτε τη σελίδα.

    >[AZURE.NOTE] Εάν χρησιμοποιείτε ένα πιστοποιητικό αυτόματης υπογραφής, όταν κάνετε περιήγηση σε ένα τελικό σημείο HTTPS που είναι συσχετισμένη με το πιστοποιητικό αυτόματης υπογραφής που ενδέχεται να εμφανιστεί το μήνυμα σφάλματος πιστοποιητικού στο πρόγραμμα περιήγησης. Χρησιμοποιώντας ένα πιστοποιητικό υπογεγραμμένο από μια αξιόπιστη αρχή έκδοσης πιστοποιητικών λύνει αυτό το ζήτημα; στο μεταξύ, μπορείτε να αγνοήσετε το σφάλμα. (Μια άλλη επιλογή είναι να προσθέσετε το αυτο-υπογεγραμμένο πιστοποιητικό στο χώρο αποθήκευσης πιστοποιητικών αρχή αξιόπιστο πιστοποιητικό του χρήστη.)

    ![Παράδειγμα τοποθεσίας web SSL][3]

Εάν θέλετε να χρησιμοποιήσετε το SSL για μια ανάπτυξη ενδιάμεσου σταδίου αντί για μια ανάπτυξη παραγωγής, πρέπει πρώτα να καθορίσετε τη διεύθυνση URL που χρησιμοποιούνται για την ανάπτυξη ενδιάμεσου σταδίου. Αναπτύξτε την υπηρεσία cloud για το περιβάλλον δοκιμής χωρίς να συμπεριλάβετε ένα πιστοποιητικό ή πληροφορίες πιστοποιητικού. Όταν αναπτυχθεί, μπορείτε να προσδιορίσετε το URL βάσει GUID, το οποίο εμφανίζεται στο πεδίο **Διεύθυνση URL της τοποθεσίας** του Azure κλασική της πύλης. Δημιουργήστε ένα πιστοποιητικό με το κοινό όνομα (CN) ίσο με βάση GUID διεύθυνσης URL (για παράδειγμα, **32818777-6e77-4ced-a8fc-57609d404462.cloudapp.net**). Χρησιμοποιήστε το Azure κλασική πύλη για να προσθέσετε το πιστοποιητικό για την υπηρεσία cloud σταδιακή. Στη συνέχεια, προσθέστε τις πληροφορίες του πιστοποιητικού στα αρχεία σας CSDEF και CSCFG, συσκευάσετε ξανά την εφαρμογή και ενημέρωση σταδιακή ανάπτυξή σας για να χρησιμοποιήσετε το νέο πακέτο.

## <a name="next-steps"></a>Επόμενα βήματα

* [Γενικές ρύθμιση παραμέτρων για την υπηρεσία cloud](cloud-services-how-to-configure.md).
* Μάθετε πώς να [αναπτύξετε μια υπηρεσία στο cloud](cloud-services-how-to-create-deploy.md).
* Ρύθμιση παραμέτρων ένα [προσαρμοσμένο όνομα τομέα](cloud-services-custom-domain-name.md).
* [Διαχείριση της υπηρεσίας cloud](cloud-services-how-to-manage.md).


  [Azure κλασική πύλη]: http://manage.windowsazure.com
  [0]: ./media/cloud-services-configure-ssl-certificate/CreateCloudService.png
  [1]: ./media/cloud-services-configure-ssl-certificate/AddCertificate.png
  [2]: ./media/cloud-services-configure-ssl-certificate/CopyURL.png
  [3]: ./media/cloud-services-configure-ssl-certificate/SSLCloudService.png
  [4]: ./media/cloud-services-configure-ssl-certificate/AddCertificateComplete.png  
