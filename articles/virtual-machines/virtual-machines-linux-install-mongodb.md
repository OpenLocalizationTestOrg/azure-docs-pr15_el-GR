<properties
   pageTitle="Εγκατάσταση MongoDB σε μια Εικονική Linux | Microsoft Azure"
   description="Μάθετε πώς μπορείτε να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους MongoDB σε μια εικονική μηχανή Linux στο Azure χρησιμοποιώντας το μοντέλο ανάπτυξης διαχείρισης πόρων."
   services="virtual-machines-linux"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="09/29/2016"
   ms.author="iainfou"/>

# <a name="install-and-configure-mongodb-on-a-linux-vm-in-azure"></a>Εγκατάσταση και ρύθμιση παραμέτρων MongoDB σε μια Εικονική Linux στα Azure
[MongoDB](http://www.mongodb.org) είναι μια δημοφιλή ανοιχτού κώδικα, υψηλών επιδόσεων NoSQL βάση δεδομένων. Σε αυτό το άρθρο θα μάθετε πώς να εγκαταστήσετε και να ρυθμίσετε τις παραμέτρους MongoDB σε μια Εικονική Linux στα Azure χρησιμοποιώντας το μοντέλο ανάπτυξης διαχείρισης πόρων. Παραδείγματα εμφανίζονται οι λεπτομέρειες που πώς να:

- [Μη αυτόματη εγκατάσταση και ρύθμιση παραμέτρων μια βασική παρουσία MongoDB](#manually-install-and-configure-mongodb-on-a-vm)
- [Δημιουργήστε μια βασική παρουσία MongoDB χρησιμοποιώντας ένα πρότυπο από διαχειριστή πόρων](#create-basic-mongodb-instance-on-centos-using-a-template)
- [Δημιουργήστε μια σύνθετη MongoDB sharded σύμπλεγμα με ρεπλίκα σύνολα χρησιμοποιώντας ένα πρότυπο από διαχειριστή πόρων](#create-a-complex-mongodb-sharded-cluster-on-centos-using-a-template)


## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία
Σε αυτό το άρθρο απαιτεί τα εξής:

- Azure λογαριασμού ([λάβετε μια δωρεάν δοκιμαστική έκδοση](https://azure.microsoft.com/pricing/free-trial/)).
- έχετε συνδεθεί με [Azure CLI](../xplat-cli-install.md)`azure login`
- το Azure CLI *πρέπει να είναι* με λειτουργία από διαχειριστή πόρων Azure`azure config mode arm`


## <a name="manually-install-and-configure-mongodb-on-a-vm"></a>Μη αυτόματη εγκατάσταση και ρύθμιση παραμέτρων MongoDB σε μια Εικονική
MongoDB [παρέχουν οδηγίες εγκατάστασης](https://docs.mongodb.com/manual/administration/install-on-linux/) για distros Linux συμπεριλαμβανομένων καπέλο κόκκινο / CentOS, SUSE, Ubuntu και Debian. Το ακόλουθο παράδειγμα δημιουργεί μια `CoreOS` Εικονική χρησιμοποιώντας ένα πλήκτρο SSH που είναι αποθηκευμένα στο `.ssh/azure_id_rsa.pub`. Απαντήστε τις οδηγίες για το όνομα του λογαριασμού χώρου αποθήκευσης, όνομα DNS και διαπιστευτήρια διαχειριστή:

```bash
azure vm quick-create --ssh-publickey-file .ssh/azure_id_rsa.pub --image-urn CentOS
```

Συνδεθείτε με τη χρήση στη δημόσια διεύθυνση IP που εμφανίζεται στο τέλος της το προηγούμενο βήμα δημιουργίας Εικονική Εικονική:

```bash
ssh ops@40.78.23.145
```

Για να προσθέσετε αρχεία προέλευσης της εγκατάστασης για MongoDB, δημιουργήστε μια `yum` αρχείο φύλαξης των δεδομένων ως εξής:

```bash
sudo touch /etc/yum.repos.d/mongodb-org-3.2.repo
```

Ανοίξτε το αρχείο repo MongoDB για επεξεργασία. Προσθέστε τις ακόλουθες γραμμές:

```bash
[mongodb-org-3.2]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/3.2/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-3.2.asc
```

Εγκατάσταση με χρήση MongoDB `yum` ως εξής:

```bash
sudo yum install -y mongodb-org
```

Από προεπιλογή, SELinux τίθεται σε ισχύ σε εικόνες CentOS που αποτρέπει την πρόσβαση MongoDB. Εγκαταστήστε Εργαλεία διαχείρισης πολιτικής και ρύθμιση παραμέτρων SELinux για να επιτρέψετε MongoDB λειτουργίας με την προεπιλεγμένη θύρα TCP 27017 ως εξής. 

```bash
sudo yum install -y policycoreutils-python
sudo semanage port -a -t mongod_port_t -p tcp 27017
```

Εκκίνηση της υπηρεσίας MongoDB ως εξής:

```bash
sudo service mongod start
```

Επαληθεύστε την εγκατάσταση MongoDB, σύνδεση με χρήση του τοπικού `mongo` προγράμματος-πελάτη:

```bash
mongo
```

Τώρα μπορείτε να ελέγξετε την παρουσία MongoDB, προσθέτοντας ορισμένα δεδομένα και, στη συνέχεια, αναζήτηση:

```
> db
test
> db.foo.insert( { a : 1 } )  
> db.foo.find()  
{ "_id" : ObjectId("57ec477cd639891710b90727"), "a" : 1 }
> exit
```

Εάν θέλετε, ρυθμίστε τις παραμέτρους MongoDB ώστε να ξεκινά αυτόματα κατά την επανεκκίνηση του συστήματος:

```bash
sudo chkconfig mongod on
```


## <a name="create-basic-mongodb-instance-on-centos-using-a-template"></a>Δημιουργία βασικού MongoDB παρουσίας σε CentOS χρησιμοποιώντας ένα πρότυπο
Μπορείτε να δημιουργήσετε μια βασική παρουσία MongoDB σε ένα μεμονωμένο Εικονική CentOS χρησιμοποιώντας το ακόλουθο πρότυπο Azure γρήγορη έναρξη από Github. Αυτό το πρότυπο χρησιμοποιεί την επέκταση προσαρμοσμένη δέσμη ενεργειών για Linux για να προσθέσετε μια `yum` αποθετήριο δεδομένων για να σας που έχουν δημιουργηθεί πρόσφατα Εικονική CentOS και, στη συνέχεια, εγκαταστήστε το στοιχείο MongoDB.

- [Βασικές MongoDB παρουσία στον CentOS](https://github.com/Azure/azure-quickstart-templates/tree/master/mongodb-on-centos) - https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-on-centos/azuredeploy.json

Το παρακάτω παράδειγμα δημιουργεί μια ομάδα πόρων με το όνομα `myResourceGroup` στο το `WestUS` περιοχής. Εισαγάγετε τις δικές σας τιμές ως εξής:

```bash
azure group create --name myResourceGroup --location WestUS \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-on-centos/azuredeploy.json
```

> [AZURE.NOTE] Το Azure CLI σας επιστρέφει μια ερώτηση μέσα σε μερικά δευτερόλεπτα της δημιουργίας ανάπτυξης, αλλά την εγκατάσταση και ρύθμιση παραμέτρων χρειάζονται μερικά λεπτά για να ολοκληρωθεί. Ελέγξτε την κατάσταση της ανάπτυξης με `azure group deployment show myResourceGroup`, πληκτρολογώντας το όνομα της ομάδας σας πόρου αντίστοιχα. Περιμένετε έως ότου το `ProvisioningState` εμφανίζει 'Ολοκληρώθηκε με' πριν να επιχειρήσετε να SSH για την εικονική Μηχανή.

Όταν είναι ανάπτυξης ολοκλήρωση, SSH για την εικονική Μηχανή. Λήψη της διεύθυνσης IP του σας Εικονική χρησιμοποιώντας το `azure vm show` εντολής όπως στο ακόλουθο παράδειγμα:

```bash
azure vm show --resource-group myResourceGroup --name myVM
```

Κοντά στο τέλος του αποτελέσματος, το `Public IP address` εμφανίζεται. SSH για να σας Εικονική με τη διεύθυνση IP του Εικονική σας:

```bash
ssh ops@138.91.149.74
```

Επαληθεύστε την εγκατάσταση MongoDB, σύνδεση με χρήση του τοπικού `mongo` προγράμματος-πελάτη ως εξής:

```bash
mongo
```

Τώρα μπορείτε να ελέγξετε την παρουσία, προσθέτοντας ορισμένα δεδομένα και αναζήτηση ως εξής:

```
> db
test
> db.foo.insert( { a : 1 } )  
> db.foo.find()  
{ "_id" : ObjectId("57ec477cd639891710b90727"), "a" : 1 }
> exit
```


## <a name="create-a-complex-mongodb-sharded-cluster-on-centos-using-a-template"></a>Δημιουργήστε ένα σύνθετο σύμπλεγμα Sharded MongoDB σε CentOS χρησιμοποιώντας ένα πρότυπο
Μπορείτε να δημιουργήσετε μια σύνθετη MongoDB sharded σύμπλεγμα χρησιμοποιώντας το ακόλουθο πρότυπο Azure γρήγορη έναρξη από Github. Αυτό το πρότυπο ακολουθεί τις [βέλτιστες πρακτικές sharded σύμπλεγμα MongoDB](https://docs.mongodb.com/manual/core/sharded-cluster-components/) για την παροχή πλεονασμού και υψηλή διαθεσιμότητα. Το πρότυπο δημιουργεί δύο shards, με τρεις κόμβους σε κάθε σύνολο αντιγράφου. Μία ρύθμισης παραμέτρων διακομιστή που έχουν οριστεί με τρεις κόμβους επίσης δημιουργείται ρεπλίκα, συν δύο `mongos` δρομολογητή διακομιστές παρέχουν συνέπειας για τις εφαρμογές από το shards.

- [Σύμπλεγμα Sharding MongoDB σε CentOS](https://github.com/Azure/azure-quickstart-templates/tree/master/mongodb-sharding-centos) - https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-sharding-centos/azuredeploy.json

> [AZURE.WARNING] Ανάπτυξη αυτό σύνθετες σύμπλεγμα sharded MongoDB απαιτεί περισσότερους από 20 πυρήνων, που συνήθως είναι το πλήθος των πυρήνα προεπιλεγμένη ανά περιοχή για μια συνδρομή. Ανοίξτε μια πρόσκληση Azure υποστήριξη για να αυξήσετε το μέγεθος των κύριων count.

Το παρακάτω παράδειγμα δημιουργεί μια ομάδα πόρων με το όνομα `myResourceGroup` στο το `WestUS` περιοχής. Εισαγάγετε τις δικές σας τιμές ως εξής:

```bash
azure group create --name myResourceGroup --location WestUS \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-sharding-centos/azuredeploy.json
```

> [AZURE.NOTE] Το Azure CLI σας επιστρέφει μια ερώτηση μέσα σε μερικά δευτερόλεπτα της δημιουργίας ανάπτυξης, αλλά την εγκατάσταση και ρύθμιση παραμέτρων μπορεί να διαρκέσει περισσότερο από μία ώρα για να ολοκληρωθεί. Ελέγξτε την κατάσταση της ανάπτυξης με `azure group deployment show myResourceGroup`, ρυθμίζοντας το όνομα της ομάδας σας πόρων ανάλογα. Περιμένετε έως ότου το `ProvisioningState` εμφανίζει 'Ολοκληρώθηκε με' πριν από τη σύνδεση του ΣΠΣ.


## <a name="next-steps"></a>Επόμενα βήματα
Σε αυτά τα παραδείγματα, συνδεθείτε στην παρουσία MongoDB τοπικά από την εικονική Μηχανή. Εάν θέλετε να συνδεθείτε με την παρουσία MongoDB από άλλο Εικονική ή δικτύου, βεβαιωθείτε ότι το κατάλληλο [δημιουργούνται κανόνες ομάδα ασφαλείας δικτύου](virtual-machines-linux-nsg-quickstart.md).

Για περισσότερες πληροφορίες σχετικά με τη δημιουργία με τη χρήση προτύπων, ανατρέξτε στο θέμα η [Επισκόπηση της διαχείρισης πόρων Azure](../azure-resource-manager/resource-group-overview.md).

Τα πρότυπα διαχείρισης πόρων Azure χρησιμοποιήστε την επέκταση προσαρμοσμένη δέσμη ενεργειών να κάνετε λήψη και να εκτελέσει δέσμες ενεργειών σε VM σας. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [χρησιμοποιώντας το Azure προσαρμοσμένη δέσμη ενεργειών επέκταση με Linux εικονικές μηχανές](virtual-machines-linux-extensions-customscript.md).