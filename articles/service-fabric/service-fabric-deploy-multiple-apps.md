<properties
   pageTitle="Ανάπτυξη μιας εφαρμογής Node.js που χρησιμοποιεί MongoDB | Microsoft Azure"
   description="Αναλυτικές οδηγίες σχετικά με τον τρόπο για να συσκευάσετε πολλές εκτελέσιμα επισκέπτη για να αναπτύξετε σε ένα σύμπλεγμα ύφασμα υπηρεσίας Azure"
   services="service-fabric"
   documentationCenter=".net"
   authors="msfussell"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/22/2016"
   ms.author="msfussell;mikhegn"/>


# <a name="deploy-multiple-guest-executables"></a>Ανάπτυξη πολλών εκτελέσιμα επισκέπτη

Σε αυτό το άρθρο δείχνει πώς μπορείτε να συσκευάσετε και να αναπτύξετε πολλές εκτελέσιμα επισκέπτη σε ύφασμα υπηρεσίας Azure. Για τη δημιουργία και την ανάπτυξη ενός μεμονωμένου πακέτου ύφασμα υπηρεσίας Διαβάστε πώς μπορείτε να [αναπτύξετε το εκτελέσιμο σε ύφασμα υπηρεσίας επισκέπτης](service-fabric-deploy-existing-app.md).

Ενώ αυτόν τον Οδηγό δείχνει πώς μπορείτε να αναπτύξετε μια εφαρμογή με ένα Node.js προσκήνιο που χρησιμοποιεί MongoDB ως ο χώρος αποθήκευσης δεδομένων, μπορείτε να εφαρμόσετε τα βήματα που περιγράφονται σε οποιαδήποτε εφαρμογή που έχει εξαρτήσεις από άλλη εφαρμογή.   

Μπορείτε να χρησιμοποιήσετε το Visual Studio για να προκύψει το πακέτο εφαρμογών που περιέχει πολλές εκτελέσιμα επισκέπτη. Ανατρέξτε στο θέμα [Χρήση του Visual Studio για να συσκευάσετε μια υπάρχουσα εφαρμογή](service-fabric-deploy-existing-app.md#using-visual-studio-to-package-an-existing-executable). Αφού προσθέσετε το πρώτο εκτελέσιμο αρχείο επισκέπτη, κάντε δεξί κλικ στην εφαρμογή του έργου και επιλέξτε **Προσθήκη -> νέα υπηρεσία ύφασμα υπηρεσίας** για να προσθέσετε το δεύτερο έργο εκτελέσιμο επισκέπτη στη λύση. Σημείωση: Εάν επιλέξετε να συνδέσετε την προέλευση του έργου Visual Studio, δημιουργείτε τη λύση Visual Studio, θα βεβαιωθείτε ότι το πακέτο εφαρμογών είναι ενημερωμένα με τις αλλαγές στο αρχείο προέλευσης. 

## <a name="manually-package-the-multiple-guest-executable-application"></a>Συσκευασία με μη αυτόματο τρόπο το πολλών εκτελέσιμη εφαρμογή επισκέπτη
Εναλλακτικά μπορείτε να συσκευάσετε με μη αυτόματο τρόπο το εκτελέσιμο επισκέπτη. Για τη μη αυτόματη συσκευασία, σε αυτό το άρθρο χρησιμοποιεί το εργαλείο συσκευασία ύφασμα υπηρεσίας, το οποίο είναι διαθέσιμο στο [http://aka.ms/servicefabricpacktool](http://aka.ms/servicefabricpacktool).

### <a name="packaging-the-nodejs-application"></a>Συσκευασία της εφαρμογής Node.js
Σε αυτό το άρθρο προϋποθέτει ότι Node.js δεν έχει εγκατασταθεί σε τους κόμβους του συμπλέγματος ύφασμα υπηρεσίας. Κατά συνέπεια, πρέπει να προσθέσετε Node.exe στον ριζικό κατάλογο της εφαρμογής σας κόμβου πριν συσκευασία. Η δομή καταλόγου της εφαρμογής Node.js (χρησιμοποιώντας πλαίσιο web Express και ο μηχανισμός Jade πρότυπο) θα πρέπει να είναι παρόμοια με την παρακάτω:

```
|-- NodeApplication
  	|-- bin
        |-- www
  	|-- node_modules
        |-- .bin
        |-- express
        |-- jade
        |-- etc.
  	|-- public
        |-- images
        |-- etc.
  	|-- routes
        |-- index.js
        |-- users.js
  	|-- views
        |-- index.jade
        |-- etc.
  	|-- app.js
  	|-- package.json
  	|-- node.exe
```

Ως επόμενο βήμα, μπορείτε να δημιουργήσετε ένα πακέτο εφαρμογών για την εφαρμογή Node.js. Ο παρακάτω κώδικας δημιουργεί ένα πακέτο εφαρμογών υπηρεσίας ύφασμα που περιέχει την εφαρμογή Node.js.

```
.\ServiceFabricAppPackageUtil.exe /source:'[yourdirectory]\MyNodeApplication' /target:'[yourtargetdirectory] /appname:NodeService /exe:'node.exe' /ma:'bin/www' /AppType:NodeAppType
```

Ακολουθεί μια περιγραφή των παραμέτρων που χρησιμοποιούνται:

- **/Source** σημείων στον κατάλογο της εφαρμογής που θα πρέπει να συσκευαστούν.
- **/ Target** καθορίζει τον κατάλογο στην οποία θα πρέπει να δημιουργηθεί το πακέτο. Αυτός ο κατάλογος πρέπει να είναι διαφορετικός από τον κατάλογο προέλευσης.
- **/ appname** Καθορίζει το όνομα της εφαρμογής της υπάρχουσας εφαρμογής. Είναι σημαντικό να κατανοήσετε αυτό μεταφράζεται για το όνομα της υπηρεσίας στο δηλωτικό, και όχι για το όνομα της εφαρμογής υπηρεσίας ύφασμα.
- **/exe** ορίζει το εκτελέσιμο αρχείο που είναι πρέπει ύφασμα υπηρεσίας για να ξεκινήσετε, σε αυτήν την περίπτωση `node.exe`.
- **/Ma** Καθορίζει το όρισμα που χρησιμοποιείται για να εκκινήσετε το εκτελέσιμο αρχείο. Καθώς Node.js δεν είναι εγκατεστημένο, ύφασμα υπηρεσίας πρέπει να εκκινήσετε το διακομιστή web Node.js με την εκτέλεση `node.exe bin/www`.  `/ma:'bin/www'`σας ενημερώνει για το εργαλείο συσκευασία για να χρησιμοποιήσετε `bin/ma` ως το όρισμα για node.exe.
- **/AppType** Καθορίζει το όνομα του τύπου εφαρμογής υπηρεσίας ύφασμα.

Εάν πραγματοποιήσετε αναζήτηση στον κατάλογο που έχει καθοριστεί από την παράμετρο/Target, μπορείτε να δείτε ότι το εργαλείο έχει δημιουργήσει ένα πλήρως λειτουργικό πακέτο ύφασμα υπηρεσία, όπως φαίνεται παρακάτω:

```
|--[yourtargetdirectory]
  	|-- NodeApplication
        |-- C
              |-- bin
              |-- data
              |-- node_modules
              |-- public
              |-- routes
              |-- views
              |-- app.js
              |-- package.json
              |-- node.exe
        |-- config
              |--Settings.xml
        |-- ServiceManifest.xml
  	|-- ApplicationManifest.xml
```
Το που δημιουργήθηκε ServiceManifest.xml τώρα διαθέτει μια ενότητα που περιγράφει πώς θα πρέπει να ξεκινήσει το διακομιστή web Node.js, όπως φαίνεται στην παρακάτω τμήμα κώδικα:

```xml
<CodePackage Name="C" Version="1.0">
    <EntryPoint>
        <ExeHost>
            <Program>node.exe</Program>
            <Arguments>'bin/www'</Arguments>
            <WorkingFolder>CodePackage</WorkingFolder>
        </ExeHost>
    </EntryPoint>
</CodePackage>
```
Σε αυτό το δείγμα, ο διακομιστής web Node.js αναμένει στη θύρα 3000, ώστε να πρέπει να ενημερώσετε τις πληροφορίες τελικό σημείο στο αρχείο ServiceManifest.xml όπως φαίνεται παρακάτω.   

```xml
<Resources>
      <Endpoints>
        <Endpoint Name="NodeServiceEndpoint" Protocol="http" Port="3000" Type="Input" />
      </Endpoints>
</Resources>
```
### <a name="packaging-the-mongodb-application"></a>Συσκευασία της εφαρμογής MongoDB
Τώρα που συσκευάσετε την εφαρμογή Node.js, μπορείτε να προχωρήσετε και να συσκευάσετε MongoDB. Όπως αναφέρθηκε πριν, τα βήματα που θα ακολουθήσετε τώρα δεν αφορούν ειδικά Node.js και MongoDB. Στην πραγματικότητα, ισχύουν για όλες τις εφαρμογές που προορίζονται για συσκευαστούν μαζί ως μία εφαρμογή υπηρεσίας ύφασμα.  

Για να συσκευάσετε MongoDB, που θέλετε να βεβαιωθείτε ότι το πακέτο Mongod.exe και Mongo.exe. Και οι δύο δυαδικά δεδομένα βρίσκονται στο το `bin` καταλόγου του καταλόγου εγκατάστασης MongoDB. Η δομή καταλόγου φαίνεται παρόμοια με την παρακάτω.

```
|-- MongoDB
  	|-- bin
        |-- mongod.exe
        |-- mongo.exe
        |-- anybinary.exe
```
Ύφασμα υπηρεσίας πρέπει να ξεκινήσετε MongoDB με μια εντολή παρόμοια με αυτό που φαίνεται παρακάτω, ώστε να πρέπει να χρησιμοποιήσετε το `/ma` παράμετρο, όταν συσκευασία MongoDB.

```
mongod.exe --dbpath [path to data]
```
> [AZURE.NOTE] Τα δεδομένα δεν γίνεται διατηρείται στην περίπτωση αποτυχίας κόμβο Εάν τοποθετήσετε τον κατάλογο δεδομένων MongoDB στον τοπικό κατάλογο του κόμβου. Που θα πρέπει να χρησιμοποιήσετε διαρκή χώρο αποθήκευσης ή να υλοποιήσετε ρεπλίκα MongoDB ρύθμιση για να αποφευχθεί η απώλεια δεδομένων.  

Στο PowerShell ή το κέλυφος εντολών, μπορούμε να εκτελέσουμε το εργαλείο συσκευασία με τις ακόλουθες παραμέτρους:

```
.\ServiceFabricAppPackageUtil.exe /source: [yourdirectory]\MongoDB' /target:'[yourtargetdirectory]' /appname:MongoDB /exe:'bin\mongod.exe' /ma:'--dbpath [path to data]' /AppType:NodeAppType
```

Για να προσθέσετε το πακέτο εφαρμογών υπηρεσίας ύφασμα MongoDB, πρέπει να βεβαιωθείτε ότι τα σημεία παράμετρο/Target στον ίδιο κατάλογο που περιέχει ήδη την εφαρμογή δήλωσης μαζί με την εφαρμογή Node.js. Πρέπει επίσης να βεβαιωθείτε ότι χρησιμοποιείτε το ίδιο όνομα ApplicationType.

Ας αναζήτηση στον κατάλογο και εξετάστε το εργαλείο που έχει δημιουργήσει.

```
|--[yourtargetdirectory]
  	|-- MyNodeApplication
  	|-- MongoDB
        |-- C
            |--bin
                |-- mongod.exe
                |-- mongo.exe
                |-- etc.
        |-- config
            |--Settings.xml
        |-- ServiceManifest.xml
  	|-- ApplicationManifest.xml
```
Όπως μπορείτε να δείτε, το εργαλείο προστίθεται ένα νέο φάκελο, MongoDB, τον κατάλογο που περιέχει τα δυαδικά αρχεία MongoDB. Εάν ανοίξετε την `ApplicationManifest.xml` αρχείο, μπορείτε να δείτε ότι το πακέτο περιέχει τώρα τόσο η εφαρμογή Node.js και MongoDB. Ο παρακάτω κώδικας εμφανίζει το περιεχόμενο του τη δήλωση της εφαρμογής.

```xml
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="MyNodeApp" ApplicationTypeVersion="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="MongoDB" ServiceManifestVersion="1.0" />
   </ServiceManifestImport>
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="NodeService" ServiceManifestVersion="1.0" />
   </ServiceManifestImport>
   <DefaultServices>
      <Service Name="MongoDBService">
         <StatelessService ServiceTypeName="MongoDB">
            <SingletonPartition />
         </StatelessService>
      </Service>
      <Service Name="NodeServiceService">
         <StatelessService ServiceTypeName="NodeService">
            <SingletonPartition />
         </StatelessService>
      </Service>
   </DefaultServices>
</ApplicationManifest>  
```

### <a name="publishing-the-application"></a>Δημοσίευση της εφαρμογής
Το τελευταίο βήμα είναι να δημοσιεύσετε την εφαρμογή στο τοπικό σύμπλεγμα υπηρεσίας υφάσματος με τη χρήση των δεσμών ενεργειών του PowerShell παρακάτω:

```
Connect-ServiceFabricCluster localhost:19000

Write-Host 'Copying application package...'
Copy-ServiceFabricApplicationPackage -ApplicationPackagePath '[yourtargetdirectory]' -ImageStoreConnectionString 'file:C:\SfDevCluster\Data\ImageStoreShare' -ApplicationPackagePathInImageStore 'NodeAppType'

Write-Host 'Registering application type...'
Register-ServiceFabricApplicationType -ApplicationPathInImageStore 'NodeAppType'

New-ServiceFabricApplication -ApplicationName 'fabric:/NodeApp' -ApplicationTypeName 'NodeAppType' -ApplicationTypeVersion 1.0  
```

Μόλις η εφαρμογή δημοσιεύεται με επιτυχία στο τοπικό σύμπλεγμα, μπορείτε να αποκτήσετε πρόσβαση στην εφαρμογή Node.js τη θύρα που θα σας έχουν εισαχθεί στο δηλωτικό υπηρεσίας της εφαρμογής Node.js--για παράδειγμα http://localhost:3000.

Σε αυτό το πρόγραμμα εκμάθησης, που είδατε πώς μπορείτε να συσκευάσετε εύκολα δύο υπάρχουσες εφαρμογές ως μία εφαρμογή υπηρεσίας ύφασμα. Μάθατε επίσης πώς μπορείτε να αναπτύξετε για ύφασμα υπηρεσία, έτσι ώστε το μπορούν να επωφεληθούν από ορισμένες από τις δυνατότητες ύφασμα υπηρεσία, όπως υψηλή διαθεσιμότητα και την ενοποίηση εύρυθμης λειτουργίας συστήματος.

## <a name="next-steps"></a>Επόμενα βήματα

- Μάθετε περισσότερα σχετικά με την ανάπτυξη του κοντέινερ με [Επισκόπηση ύφασμα υπηρεσίας και κοντέινερ](service-fabric-containers-overview.md)
