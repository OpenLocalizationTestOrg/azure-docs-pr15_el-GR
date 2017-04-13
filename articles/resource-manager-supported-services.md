<properties
   pageTitle="Διαχείριση πόρων υποστηρίζονται υπηρεσίες | Microsoft Azure"
   description="Περιγράφει τις υπηρεσίες παροχής πόρων που υποστηρίζουν διαχείριση πόρων, τα σχήματα και διαθέσιμες εκδόσεις API και τις περιοχές που μπορεί να φιλοξενήσει τους πόρους."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/25/2016"
   ms.author="magoedte;tomfitz"/>

# <a name="resource-manager-providers-regions-api-versions-and-schemas"></a>Διαχείριση πόρων υπηρεσίες παροχής, περιοχές, API εκδόσεις και οι διατάξεις

Azure διαχείριση πόρων παρέχει έναν νέο τρόπο για να αναπτύξετε και να διαχειριστείτε τις υπηρεσίες που απαρτίζουν τις εφαρμογές σας. Οι περισσότερες, αλλά όχι σε όλες τις υπηρεσίες υποστήριξης για τη διαχείριση πόρων και ορισμένες υπηρεσίες υποστήριξης για τη διαχείριση πόρων μόνο εν μέρει. Αυτό το θέμα παρέχει μια λίστα υπηρεσιών παροχής υποστηριζόμενες πόρων για τη διαχείριση πόρων Azure.

Κατά την ανάπτυξη τους πόρους σας, πρέπει επίσης να γνωρίζετε ποιες περιοχές υποστηρίζουν αυτούς τους πόρους και ποιες εκδόσεις API είναι διαθέσιμες για τους πόρους. Η ενότητα [περιοχές υποστηριζόμενα](#supported-regions) σας δείχνει πώς μπορείτε να μάθετε ποιες περιοχές εργασίας για τη συνδρομή σας και τους πόρους. Η ενότητα [εκδόσεις υποστηρίζονται API](#supported-api-versions) σας δείχνει πώς να καθορίσετε ποιες εκδόσεις API που μπορείτε να χρησιμοποιήσετε.

Για να δείτε ποιες υπηρεσίες που υποστηρίζονται στην πύλη του Azure και κλασική πύλη, ανατρέξτε στο θέμα [Azure διαθεσιμότητα πύλης γραφήματος](https://azure.microsoft.com/features/azure-portal/availability/). Για να δείτε ποιες υπηρεσίες κυλιόμενο πόρους υποστήριξης, ανατρέξτε στο θέμα [Μετακίνηση πόρων για νέα ομάδα πόρων ή τη συνδρομή](resource-group-move-resources.md).

Στους παρακάτω πίνακες παρατίθενται ποια Microsoft υπηρεσίες υποστήριξης ανάπτυξης και διαχείρισης μέσω του διαχειριστή πόρων και ποια όχι. Τη σύνδεση στη στήλη **Πρότυπα γρήγορη έναρξη** αποστέλλει ένα ερώτημα στο αποθετήριο Azure γρήγορη έναρξη πρότυπα για την υπηρεσία παροχής συγκεκριμένο πόρο. Γρήγορη έναρξη πρότυπα προστίθενται και ενημερώνονται συχνά. Η παρουσία μιας σύνδεσης για μια συγκεκριμένη υπηρεσία δεν σημαίνει απαραίτητα το ερώτημα επιστρέφει πρότυπα από το χώρο αποθήκευσης. Υπάρχουν επίσης πολλές υπηρεσίες παροχής πόρων τρίτων κατασκευαστών που υποστηρίζουν διαχείριση πόρων. Μάθετε πώς μπορείτε να δείτε όλες τις υπηρεσίες παροχής πόρων στην ενότητα [τύποι και υπηρεσίες παροχής πόρων](#resource-providers-and-types) .


## <a name="compute"></a>Υπολογισμός

| Υπηρεσία | Διαχείριση πόρων με δυνατότητα | REST API | Σχήμα | Γρήγορη έναρξη προτύπων |
| ------- | ------------------------ |-------- | ------ | ------ |
| Μαζική   | Ναι     | [Μαζική ΥΠΌΛΟΙΠΟ](https://msdn.microsoft.com/library/azure/dn820158.aspx) | [2015 12--01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-12-01/Microsoft.Batch.json)       |  |
|Κοντέινερ | Ναι | [Η υπηρεσία κοντέινερ ΥΠΌΛΟΙΠΟ](https://msdn.microsoft.com/library/azure/mt711470.aspx) | [2016-03-30](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-03-30/Microsoft.ContainerService.json) | [Microsoft.ContainerService](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.ContainerService%22&type=Code) |
| Υπηρεσίες Dynamics κύκλου ζωής | Ναι  |      |        |  |
| Σύνολα κλίμακας | Ναι | [Κλίμακα Ορισμός ΥΠΌΛΟΙΠΟ](https://msdn.microsoft.com/library/azure/mt705635.aspx) | [2015 08--01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Compute.json) | [virtualMachineScaleSets](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=virtualMachineScaleSets&type=Code) | 
| Υπηρεσία ύφασμα | Ναι  | [Υπόλοιπο ύφασμα υπηρεσίας](https://msdn.microsoft.com/library/azure/dn707692.aspx)  |      | [Microsoft.ServiceFabric](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.ServiceFabric%22&type=Code) |
| Εικονικές μηχανές | Ναι | [ΕΙΚΟΝΙΚΉ ΥΠΌΛΟΙΠΟ](https://msdn.microsoft.com/library/azure/mt163647.aspx) | [2015 08--01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Compute.json) | [virtualMachines](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Compute%2Fvirtualmachines%22&type=Code) |
| Εικονικές μηχανές (κλασικό) | Περιορισμένη | - | - | - |
| Εφαρμογή Remote | Όχι      | -  | - | - |
| Υπηρεσίες cloud (κλασικό) | Περιορίζεται (δείτε παρακάτω) | - | - | - |

Εικονικές μηχανές (κλασικό) αναφέρεται σε πόρους που είχαν αναπτυχθεί μέσω του μοντέλου κλασική ανάπτυξης, αντί μέσω του μοντέλου ανάπτυξης διαχείρισης πόρων. Σε γενικές γραμμές, αυτοί οι πόροι δεν υποστηρίζουν εργασίες διαχείρισης πόρων, αλλά υπάρχουν ορισμένες λειτουργίες που έχουν ενεργοποιηθεί. Για περισσότερες πληροφορίες σχετικά με αυτά τα μοντέλα ανάπτυξης, ανατρέξτε στο θέμα [Κατανόηση της διαχείρισης πόρων ανάπτυξης και κλασική ανάπτυξης](resource-manager-deployment-model.md). 

Υπηρεσίες cloud (κλασική) μπορούν να χρησιμοποιηθούν με άλλους πόρους κλασική; Ωστόσο, κλασική πόρους δεν εκμεταλλευτείτε όλες τις δυνατότητες διαχείρισης πόρων και δεν είναι μια καλή επιλογή για μελλοντικές λύσεις. Αντί για αυτό, μπορείτε να αλλάξετε την υποδομή εφαρμογή σας για να χρησιμοποιήσετε τους πόρους από τα πεδία ονομάτων Microsoft.Compute, Microsoft.Storage και Microsoft.Network.


## <a name="networking"></a>Δικτύωση

| Υπηρεσία | Διαχείριση πόρων με δυνατότητα | REST API | Σχήμα | Γρήγορη έναρξη προτύπων |
| ------- | -------  | -------- | ------ | ------ |
| Πύλη εφαρμογής | Ναι | [Πύλη εφαρμογής ΥΠΌΛΟΙΠΟ](https://msdn.microsoft.com/library/azure/mt684939.aspx)   |        | [applicationGateways](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2FapplicationGateways%22&type=Code) |
| DNS     | Ναι | [ΥΠΌΛΟΙΠΟ DNS](https://msdn.microsoft.com/library/azure/mt163862.aspx)         | [2016 04--01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-04-01/Microsoft.Network.json) | [dnsZones](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2FdnsZones%22&type=Code) |
| ExpressRoute | Ναι  | [ExpressRoute ΥΠΌΛΟΙΠΟ](https://msdn.microsoft.com/library/azure/mt586720.aspx)  |       | [expressRouteCircuits](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2FexpressRouteCircuits%22&type=Code) |
| Εξισορρόπηση φόρτου | Ναι  | [Εξισορρόπηση φόρτου ΥΠΌΛΟΙΠΟ](https://msdn.microsoft.com/library/azure/mt163651.aspx) | [2015 08--01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Network.json) | [loadBalancers](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2Floadbalancers%22&type=Code) |
| Διαχείριση κίνηση | Ναι | [Διαχείριση κίνηση ΥΠΌΛΟΙΠΟ](https://msdn.microsoft.com/library/azure/mt163667.aspx) | [2015 11--01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-11-01/Microsoft.Network.json)  | [trafficmanagerprofiles](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2Ftrafficmanagerprofiles%22&type=Code) |
| Εικονικό δίκτυα | Ναι| [Εικονικό δίκτυο ΥΠΌΛΟΙΠΟ](https://msdn.microsoft.com/en-us/library/azure/mt163650.aspx) | [2015 08--01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Network.json) | [virtualNetworks](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2FvirtualNetworks%22&type=Code) |
| Πύλη VPN | Ναι | [Πύλη δικτύου ΥΠΌΛΟΙΠΟ](https://msdn.microsoft.com/library/azure/mt163859.aspx) |  | [virtualNetworkGateways](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2FvirtualNetworkGateways%22&type=Code) <br /> [localNetworkGateways](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2FlocalNetworkGateways%22&type=Code) <br />[συνδέσεις](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2Fconnections%22&type=Code) |


## <a name="storage"></a>Χώρος αποθήκευσης

| Υπηρεσία | Διαχείριση πόρων με δυνατότητα | REST API | Σχήμα | Γρήγορη έναρξη προτύπων |
| ------- | ------- | -------- | ------ | ------- | ------ |
| Χώρος αποθήκευσης | Ναι  | [Χώρος αποθήκευσης ΥΠΌΛΟΙΠΟ](https://msdn.microsoft.com/library/azure/mt163683.aspx) | [Το λογαριασμό χώρου αποθήκευσης](resource-manager-template-storage.md) | [Microsoft.Storage](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Storage%22&type=Code) |
| StorSimple | Ναι  |         |        |  |

## <a name="databases"></a>Βάσεις δεδομένων

| Υπηρεσία | Διαχείριση πόρων με δυνατότητα | REST API | Σχήμα | Γρήγορη έναρξη προτύπων |
| ------- | ------- | -------- | ------ | ------- | ------ |
| DocumentDB | Ναι  | [DocumentDB ΥΠΌΛΟΙΠΟ](https://msdn.microsoft.com/library/azure/dn781481.aspx) | [2015-04-08](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-04-08/Microsoft.DocumentDB.json)  | [Microsoft.DocumentDB](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.DocumentDb%22&type=Code) |
| Redis Cache | Ναι |   | [2016 04--01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-04-01/Microsoft.Cache.json) | [Microsoft.Cache](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Cache%22&type=Code) |
| Βάση δεδομένων SQL | Ναι | [Βάση δεδομένων SQL ΥΠΌΛΟΙΠΟ](https://msdn.microsoft.com/library/azure/mt163571.aspx) | [2014-04-01-preview](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2014-04-01-preview/Microsoft.Sql.json) | [Microsoft.Sql](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Sql%22&type=Code) |
| Αποθήκη δεδομένων του SQL | Ναι |   |      |


## <a name="web--mobile"></a>Το Web & Mobile

| Υπηρεσία | Διαχείριση πόρων με δυνατότητα | REST API | Σχήμα | Γρήγορη έναρξη προτύπων |
| ------- | ------- | -------- | ------ | ------ |
| API εφαρμογές | Ναι |   | [2015 08--01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Web.json) | [API εφαρμογές](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22kind%22%3A+%22apiApp%22&type=Code) |
| API διαχείρισης | Ναι  | [API διαχείρισης ΥΠΌΛΟΙΠΟ](https://msdn.microsoft.com/library/azure/dn776326.aspx) | [2016-07-07](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-07-07/Microsoft.ApiManagement.json)       | [Microsoft.ApiManagement](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.ApiManagement%22&type=Code) | 
| Περιεχομένου επόπτη | Ναι |   |   |   |
| Συνάρτηση εφαρμογής | Ναι |   |   | [functionApp](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22functionApp%22&type=Code) |
| Λογική εφαρμογών | Ναι   | [Ροή εργασίας υπηρεσίας REST API](https://msdn.microsoft.com/library/azure/mt643787.aspx) | [2016-06-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-06-01/Microsoft.Logic.json) | [Microsoft.Logic](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Logic%22&type=Code) |
| Εφαρμογές για κινητές συσκευές | Ναι |  | [2015 08--01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Web.json)  | [mobileApp](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22mobileApp%22&type=Code)   |
| Κινητές συσκευές δεσμεύσεων | Ναι  |  [Κινητές συσκευές δέσμευση ΥΠΌΛΟΙΠΟ](https://msdn.microsoft.com/library/azure/mt683754.aspx)        |        | [Microsoft.MobileEngagements](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.MobileEngagement%22&type=Code) |
| Αναζήτηση | Ναι  | [Αναζήτηση ΥΠΌΛΟΙΠΟ](https://msdn.microsoft.com/library/azure/dn798935.aspx) |  | [Microsoft.Search](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Search%22&type=Code) |
| Εφαρμογές Web | Ναι |          | [2015 08--01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Web.json) | [Microsoft.Web](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Web%22&type=Code) |

## <a name="analytics"></a>Ανάλυση

| Υπηρεσία | Διαχείριση πόρων με δυνατότητα | REST API | Σχήμα | Γρήγορη έναρξη προτύπων |
| ------- | -------  | -------- | ------ | ------ |
| Κατάλογος δεδομένων | Ναι  | [Κατάλογος δεδομένων ΥΠΌΛΟΙΠΟ](https://msdn.microsoft.com/library/azure/mt267595.aspx)  | [2016-03-30](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-03-30/Microsoft.DataCatalog.json) |  |
| Προέλευση δεδομένων | Ναι | [Δεδομένα εργοστασίου ΥΠΌΛΟΙΠΟ](https://msdn.microsoft.com/library/azure/dn906738.aspx) |    | [Microsoft.DataFactory](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.DataFactory%22&type=Code) |
| Ανάλυση δεδομένων λίμνης | Ναι |   |   [2015-10-01-preview](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-10-01-preview/Microsoft.DataLakeAnalytics.json) | [Microsoft.DataLakeAnalytics](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.DataLakeAnalytics%22&type=Code) |
| Χώρος αποθήκευσης δεδομένων λίμνης | Ναι  | [ΥΠΌΛΟΙΠΟ του χώρου αποθήκευσης δεδομένων λίμνης](https://msdn.microsoft.com/library/azure/mt693424.aspx) | [2015-10-01-preview](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-10-01-preview/Microsoft.DataLakeAnalytics.json) | [Microsoft.DataLakeStore](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.DataLakeStore%22&type=Code) |
| HDInsights | Ναι | [HDInsights ΥΠΌΛΟΙΠΟ](https://msdn.microsoft.com/library/azure/mt622197.aspx) |        | [Microsoft.HDInsight](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.HDInsight%22&type=Code) |
| Μηχανικής εκμάθησης | Ναι | [Μηχανικής εκμάθησης ΥΠΌΛΟΙΠΟ](https://msdn.microsoft.com/library/azure/mt767538.aspx)        | [2016-05-01-preview](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-05-01-preview/Microsoft.MachineLearning.json) |
| Ροή ανάλυσης | Ναι  | [Ανάλυση ατμού ΥΠΌΛΟΙΠΟ](https://msdn.microsoft.com/library/azure/dn835031.aspx)   |        |  |
| Power BI | Ναι | [Power BI ενσωματωμένο ΥΠΌΛΟΙΠΟ](https://msdn.microsoft.com/library/azure/mt712303.aspx) | [2016-01-29](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-01-29/Microsoft.PowerBI.json) |  |

## <a name="intelligence"></a>Πληροφοριών

| Υπηρεσία | Διαχείριση πόρων με δυνατότητα | REST API | Σχήμα | Γρήγορη έναρξη προτύπων |
| ------- | ------- | -------- | ------ | ------ |
| Γνωστική υπηρεσιών | Ναι |  | [2016-02-01-preview](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-02-01-preview/Microsoft.CognitiveServices.json) |   |

## <a name="internet-of-things"></a>Internet πράγματα

| Υπηρεσία | Διαχείριση πόρων με δυνατότητα | REST API | Σχήμα | Γρήγορη έναρξη προτύπων |
| ------- | ------- | -------- | ------ | ------ |
| Ο διανομέας συμβάντος | Ναι  | [Συμβάν διανομέα ΥΠΌΛΟΙΠΟ](https://msdn.microsoft.com/library/azure/dn790674.aspx) | [2015 08--01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.EventHub.json) | [Microsoft.EventHub](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.EventHub%22&type=Code)  |
| IoTHubs | Ναι | [Ο διανομέας IoT ΥΠΌΛΟΙΠΟ](https://msdn.microsoft.com/library/azure/mt589014.aspx) | [2016-02-03](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-02-03/Microsoft.Devices.json) | [Microsoft.Devices](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Devices%22&type=Code) |
| Ειδοποίηση διανομείς | Ναι | [Ειδοποίηση διανομέα ΥΠΌΛΟΙΠΟ](https://msdn.microsoft.com/library/azure/dn495827.aspx) | [2015 04--01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-04-01/Microsoft.NotificationHubs.json) | [Microsoft.NotificationHubs](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.NotificationHubs%22&type=Code) |

## <a name="media--cdn"></a>Μέσα & CDN

| Υπηρεσία | Διαχείριση πόρων με δυνατότητα | REST API | Σχήμα | Γρήγορη έναρξη προτύπων |
| ------- | ------- | -------- | ------ | ------ |
| CDN | Ναι | [ΥΠΌΛΟΙΠΟ CDN](https://msdn.microsoft.com/library/azure/mt634456.aspx)  | [2016-04-02](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-04-02/Microsoft.Cdn.json) | [Microsoft.Cdn](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Cdn%22&type=Code) |
| Υπηρεσία πολυμέσων | Ναι | [ΥΠΌΛΟΙΠΟ υπηρεσίες πολυμέσων](https://msdn.microsoft.com/library/azure/hh973617.aspx)  | [2015 10--01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-10-01/Microsoft.Media.json) |


## <a name="hybrid-integration"></a>Ενοποίηση του υβριδικού

| Υπηρεσία | Διαχείριση πόρων με δυνατότητα | REST API | Σχήμα | Γρήγορη έναρξη προτύπων |
| ------- | ------- | -------- | ------ | ------ |
| Υπηρεσίες BizTalk | Ναι |          | [2014 04--01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2014-04-01/Microsoft.BizTalkServices.json) |  |
| Υπηρεσία ανάκτησης | Ναι | [Επαναφορά τοποθεσίας ΥΠΌΛΟΙΠΟ](https://msdn.microsoft.com/library/azure/mt750497.aspx) | [2016-06-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-06-01/Microsoft.RecoveryServices.json) | [Microsoft.RecoveryServices](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.RecoveryServices%22&type=Code) |
| Υπηρεσία Bus | Ναι | [Υπηρεσία Bus ΥΠΌΛΟΙΠΟ](https://msdn.microsoft.com/library/azure/mt639375.aspx) | [2015 08--01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.ServiceBus.json)       | [Microsoft.ServiceBus](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.ServiceBus%22&type=Code) |

## <a name="identity--access-management"></a>Ταυτότητα και διαχείριση πρόσβασης 

Azure Active Directory λειτουργεί με το διαχειριστή πόρων για να ενεργοποιήσετε τον έλεγχο πρόσβασης βάσει ρόλων για τη συνδρομή σας. Για να μάθετε πώς να χρησιμοποιείτε τον έλεγχο πρόσβασης βάσει ρόλων και υπηρεσίας καταλόγου Active Directory, ανατρέξτε στο θέμα [Έλεγχος πρόσβασης βάσει ρόλων Azure](./active-directory/role-based-access-control-configure.md).

## <a name="developer-services"></a>Υπηρεσίες για προγραμματιστές 

| Υπηρεσία | Διαχείριση πόρων με δυνατότητα | REST API | Σχήμα | Γρήγορη έναρξη προτύπων |
| ------- | ------- | -------- | ------ | ------ |
| Εφαρμογή ιδέες | Ναι  | [Ιδέες ΥΠΌΛΟΙΠΟ](https://msdn.microsoft.com/library/azure/dn931943.aspx)  | [2014 04--01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2014-04-01/Microsoft.Insights.json) | [Microsoft.insights](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.insights%22&type=Code) |
| Χάρτες Bing | Ναι  |          |        |  |
| DevTest Labs | Ναι |   | [2016-05-15](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-05-15/Microsoft.DevTestLab.json) | [Microsoft.DevTestLab](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.DevTestLab%22&type=Code)  |
| Visual Studio λογαριασμού | Ναι   |          | [2014-02-26](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2014-02-26/microsoft.visualstudio.json) |  |

## <a name="management-and-security"></a>Διαχείριση και την ασφάλεια

| Υπηρεσία | Διαχείριση πόρων με δυνατότητα | REST API | Σχήμα | Γρήγορη έναρξη προτύπων |
| ------- | ------- | -------- | ------ | ------ |
| Αυτοματοποίηση | Ναι | [Αυτοματοποίηση ΥΠΌΛΟΙΠΟ](https://msdn.microsoft.com/library/azure/mt662285.aspx) | [2015-10-31](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-10-31/Microsoft.Automation.json)       | [Microsoft.Automation](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Automation%22&type=Code) |
| Πλήκτρο θάλαμο | Ναι | [Πλήκτρο θάλαμο ΥΠΌΛΟΙΠΟ](https://msdn.microsoft.com/library/azure/dn903609.aspx) | [Πλήκτρο θάλαμο](resource-manager-template-keyvault.md)<br />[Μυστικό θάλαμο κλειδιού](resource-manager-template-keyvault-secret.md)   | [Microsoft.KeyVault](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.KeyVault%22&type=Code) |
| Λειτουργικές ιδέες | Ναι |          |        |  |
| Χρονοδιάγραμμα | Ναι  | [ΥΠΌΛΟΙΠΟ χρονοδιάγραμμα](https://msdn.microsoft.com/library/azure/mt629143.aspx)     | [2016 03--01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-03-01/Microsoft.Scheduler.json) | [Microsoft.Scheduler](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Scheduler%22&type=Code) |
| Ασφάλεια (έκδοση preview) | Ναι | [Ασφάλεια ΥΠΌΛΟΙΠΟ](https://msdn.microsoft.com/library/azure/mt704034.aspx)  |  | [Microsoft.Security](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Security%22&type=Code)  |

## <a name="resource-manager"></a>Διαχείριση πόρων

| Η δυνατότητα | Διαχείριση πόρων με δυνατότητα | REST API | Σχήμα | Γρήγορη έναρξη προτύπων |
| ------- | ------- | -------------- | -------- | ------ | ------ |
| Εξουσιοδότηση | Ναι | [Διαχείριση κλειδωμάτων](https://msdn.microsoft.com/library/azure/mt204563.aspx)<br >[Έλεγχος πρόσβασης βάσει ρόλων](https://msdn.microsoft.com/library/azure/dn906885.aspx)  | [Κλείδωμα πόρων](resource-manager-template-lock.md)<br />[Εκχωρήσεις ρόλων](resource-manager-template-role.md)  | [Microsoft.Authorization](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Authorization%22&type=Code) |
| Πόροι | Ναι | [Συνδεδεμένο πόροι](https://msdn.microsoft.com/library/azure/mt238499.aspx) | [Συνδέσεις πόρων](resource-manager-template-links.md) | [Microsoft.Resources](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Resources%22&type=Code) |


## <a name="resource-providers-and-types"></a>Τύποι και υπηρεσίες παροχής πόρων

Κατά την ανάπτυξη πόρους, συχνά πρέπει να ανακτήσετε πληροφορίες σχετικά με τις υπηρεσίες παροχής πόρων και τους τύπους. Μπορείτε να ανακτήσετε αυτές τις πληροφορίες μέσω REST API, Azure PowerShell ή Azure CLI.

Για να εργαστείτε με μια υπηρεσία παροχής πόρων, πρέπει να καταχωρούνται αυτήν την υπηρεσία παροχής πόρων με το λογαριασμό σας. Από προεπιλογή, πολλές υπηρεσίες παροχής πόρων καταχωρούνται αυτόματα; Ωστόσο, ίσως χρειαστεί να καταχωρήσετε με μη αυτόματο τρόπο ορισμένες υπηρεσίες παροχής πόρων. Στα παρακάτω παραδείγματα δείχνουν πώς μπορείτε να λάβετε την κατάσταση εγγραφής από μια υπηρεσία παροχής πόρων και να καταχωρήσετε την υπηρεσία παροχής πόρων, εάν είναι απαραίτητο.

### <a name="portal"></a>Πύλη

Μπορείτε εύκολα να δείτε μια λίστα υπηρεσιών παροχής υποστηριζόμενες πόρους με τα παρακάτω βήματα:

1. Επιλέξτε τα **δικαιώματά μου**.

    ![δικαιώματα μου](./media/resource-manager-supported-services/my-permissions.png)

2. Επιλέξτε **κατάσταση υπηρεσίας παροχής πόρων**

    ![Κατάσταση υπηρεσίας παροχής πόρων](./media/resource-manager-supported-services/resource-provider-status.png)

3. Μπορείτε να δείτε την πλήρη λίστα των υπηρεσιών παροχής πόρων για τη συνδρομή σας.

    ![λίστα υπηρεσιών παροχής πόρων](./media/resource-manager-supported-services/list-providers.png)

### <a name="rest-api"></a>REST API

Για να μεταφέρετε όλους του πόρου διαθέσιμες υπηρεσίες παροχής, περιλαμβανομένων τους τύπους, θέσεις, API εκδόσεις και κατάσταση εγγραφής, χρησιμοποιήστε τη λειτουργία [λίστα όλες τις υπηρεσίες παροχής πόρων](https://msdn.microsoft.com/library/azure/dn790524.aspx) . Εάν πρέπει να καταχωρήσετε μια υπηρεσία παροχής πόρων, ανατρέξτε στο θέμα [καταχώρηση μια συνδρομή με μια υπηρεσία παροχής του πόρου](https://msdn.microsoft.com/library/azure/dn790548.aspx).

### <a name="powershell"></a>PowerShell

Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να λάβετε όλες τις υπηρεσίες παροχής διαθέσιμων πόρων.

    Get-AzureRmResourceProvider -ListAvailable
    

Το επόμενο παράδειγμα δείχνει πώς μπορείτε να τους τύπους πόρων για μια υπηρεσία παροχής του συγκεκριμένου πόρου.

    (Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes
    
Το αποτέλεσμα είναι παρόμοια με:

    ResourceTypeName                Locations                                         
    ----------------                ---------                                         
    sites/extensions                {Brazil South, East Asia, East US, Japan East...} 
    sites/slots/extensions          {Brazil South, East Asia, East US, Japan East...} .
    ...
    
Για να καταχωρήσετε μια υπηρεσία παροχής πόρων, δώστε το χώρο ονομάτων:

    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.ApiManagement

### <a name="azure-cli"></a>Azure CLI

Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να λάβετε όλες τις υπηρεσίες παροχής διαθέσιμων πόρων.

    azure provider list
    
Το αποτέλεσμα είναι παρόμοια με:

    info:    Executing command provider list
    + Getting registered providers
    data:    Namespace                        Registered
    data:    -------------------------------  -------------
    data:    Microsoft.ApiManagement          Unregistered
    data:    Microsoft.AppService             Registered
    data:    Microsoft.Authorization          Registered
    ...

Μπορείτε να αποθηκεύσετε τις πληροφορίες για μια υπηρεσία παροχής του συγκεκριμένου πόρου σε ένα αρχείο με την ακόλουθη εντολή.

    azure provider show Microsoft.Web -vv --json > c:\temp.json

Για να καταχωρήσετε μια υπηρεσία παροχής πόρων, δώστε το χώρο ονομάτων:

    azure provider register -n Microsoft.ServiceBus

## <a name="supported-regions"></a>Υποστηριζόμενες περιοχές

Κατά την ανάπτυξη πόρους, συνήθως πρέπει να καθορίσετε μια περιοχή για τους πόρους. Διαχείριση πόρων υποστηρίζεται σε όλες τις περιοχές, αλλά οι πόροι αναπτύξετε ίσως δεν θα υποστηρίζεται σε όλες τις περιοχές. Επιπλέον, μπορεί να υπάρχουν περιορισμοί στη συνδρομή σας που επιτρέπουν τη χρήση ορισμένες περιοχές που υποστηρίζουν τον πόρο. Αυτές οι περιορισμοί μπορεί να σχετίζεται με φόρο θέματα για αρχική χώρα σας ή το αποτέλεσμα μιας πολιτικής που τοποθετείται από το διαχειριστή της συνδρομής σας για να χρησιμοποιήσετε μόνο ορισμένες περιοχές. 

Για μια πλήρη λίστα με όλες τις περιοχές που υποστηρίζονται για όλες τις υπηρεσίες του Azure, ανατρέξτε στο θέμα [υπηρεσίες ανά περιοχή](https://azure.microsoft.com/regions/#services). Ωστόσο, αυτή η λίστα μπορεί να περιλαμβάνει τις περιοχές που δεν υποστηρίζει τη συνδρομή σας. Μπορείτε να καθορίσετε τις περιοχές για ενός συγκεκριμένου τύπου που υποστηρίζει τη συνδρομή σας μέσω της πύλης, REST API, PowerShell ή Azure CLI.

### <a name="portal"></a>Πύλη

Μπορείτε να δείτε τις υποστηριζόμενες περιοχές για έναν τύπο πόρου καθοδηγεί στα παρακάτω βήματα:

1. Επιλέξτε **περισσότερες υπηρεσίες** > **Explorer πόρων**.

    ![Εξερεύνηση των πόρων](./media/resource-manager-supported-services/select-resource-explorer.png)

2. Ανοίξτε τον κόμβο **υπηρεσίες παροχής** .

    ![Επιλέξτε υπηρεσίες παροχής](./media/resource-manager-supported-services/select-providers.png)

3. Επιλέξτε μια υπηρεσία παροχής πόρων και δείτε τα υποστηριζόμενα περιοχές και τις εκδόσεις API του.

    ![υπηρεσία παροχής προβολής](./media/resource-manager-supported-services/view-provider.png)

### <a name="rest-api"></a>REST API

Για να ανακαλύψετε ποιες περιοχές είναι διαθέσιμες για ενός συγκεκριμένου τύπου στη συνδρομή σας, χρησιμοποιήστε τη λειτουργία [λίστα όλες τις υπηρεσίες παροχής πόρων](https://msdn.microsoft.com/library/azure/dn790524.aspx) . 

### <a name="powershell"></a>PowerShell

Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να λάβετε τις υποστηριζόμενες περιοχές για τοποθεσίες web.

    ((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).Locations
    
Το αποτέλεσμα είναι παρόμοια με:

    Brazil South
    East Asia
    East US
    Japan East
    Japan West
    North Central US
    North Europe
    South Central US
    West Europe
    West US
    Southeast Asia
    Central US
    East US 2

### <a name="azure-cli"></a>Azure CLI

Το παρακάτω παράδειγμα επιστρέφει όλες τις υποστηριζόμενες θέσεις για κάθε τύπο πόρου.

    azure location list

Μπορείτε επίσης να φιλτράρετε τα αποτελέσματα της θέσης με ένα βοηθητικό πρόγραμμα JSON όπως [jq](https://stedolan.github.io/jq/).

    azure location list --json | jq '.[] | select(.name == "Microsoft.Web/sites")'

Που επιστρέφει:

    {
      "name": "Microsoft.Web/sites",
      "location": "Brazil South,East Asia,East US,Japan East,Japan West,North Central US,
            North Europe,South Central US,West Europe,West US,Southeast Asia,Central US,East US 2"
    }

## <a name="supported-api-versions"></a>Υποστηριζόμενες εκδόσεις API

Κατά την ανάπτυξη ενός προτύπου, πρέπει να καθορίσετε μια έκδοση API για να χρησιμοποιήσετε για τη δημιουργία κάθε πόρο. Η έκδοση API αντιστοιχεί σε μια έκδοση των λειτουργιών REST API που διατίθενται από την υπηρεσία παροχής του πόρου. Με μια υπηρεσία παροχής πόρων ενεργοποιεί νέες δυνατότητες, εκδόσεις μια νέα έκδοση του το REST API. Γι ' αυτό, η έκδοση του API που καθορίσατε στο πρότυπό σας επηρεάζει τις ιδιότητες που μπορείτε να καθορίσετε στο πρότυπο. Σε γενικές γραμμές, που θέλετε να επιλέξετε την πιο πρόσφατη έκδοση API κατά τη δημιουργία προτύπων. Για υπάρχοντα πρότυπα, μπορείτε να αποφασίσετε εάν θέλετε να συνεχίσετε να χρησιμοποιείτε μια παλαιότερη έκδοση API ή ενημέρωση του προτύπου για την πιο πρόσφατη έκδοση για να εκμεταλλευτείτε τις νέες δυνατότητες.

### <a name="portal"></a>Πύλη

Μπορείτε να καθορίσετε τις υποστηριζόμενες εκδόσεις API με τον ίδιο τρόπο που καθορίζεται υποστηρίζονται περιοχές (απεικονίζεται προηγουμένως).

### <a name="rest-api"></a>REST API

Για να ανακαλύψετε ποιες εκδόσεις API είναι διαθέσιμες για τους τύπους πόρων, χρησιμοποιήστε τη λειτουργία [λίστα όλες τις υπηρεσίες παροχής πόρων](https://msdn.microsoft.com/library/azure/dn790524.aspx) . 

### <a name="powershell"></a>PowerShell

Το παρακάτω παράδειγμα δείχνει πώς μπορείτε να λάβετε τις διαθέσιμες εκδόσεις API για ενός συγκεκριμένου τύπου.

    ((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).ApiVersions
    
Το αποτέλεσμα είναι παρόμοια με:
    
    2015-08-01
    2015-07-01
    2015-06-01
    2015-05-01
    2015-04-01
    2015-02-01
    2014-11-01
    2014-06-01
    2014-04-01-preview
    2014-04-01

### <a name="azure-cli"></a>Azure CLI

Μπορείτε να αποθηκεύσετε τις πληροφορίες (όπως οι διαθέσιμες εκδόσεις API) για μια υπηρεσία παροχής πόρων σε ένα αρχείο με την ακόλουθη εντολή.

    azure provider show Microsoft.Web -vv --json > c:\temp.json

Μπορείτε να ανοίξετε το αρχείο και να βρείτε το στοιχείο **apiVersions**

## <a name="next-steps"></a>Επόμενα βήματα

- Για να μάθετε περισσότερα σχετικά με τη δημιουργία προτύπων για τη διαχείριση πόρων, ανατρέξτε στο θέμα [Διαχείριση πόρων σύνταξης Azure πρότυπα](resource-group-authoring-templates.md).
- Για να μάθετε περισσότερα σχετικά με τους πόρους για την ανάπτυξη, ανατρέξτε στο θέμα [ανάπτυξη μιας εφαρμογής με το πρότυπο διαχείρισης πόρων Azure](resource-group-template-deploy.md).
