<properties
   pageTitle="ExpressRoute θέσεις | Microsoft Azure"
   description="Σε αυτό το άρθρο παρέχει μια επισκόπηση λεπτομερείς θέσεις όπου διατίθενται υπηρεσίες και πώς να συνδεθείτε σε Azure περιοχές."
   services="expressroute"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor="" />
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/21/2016"
   ms.author="cherylmc" />

# <a name="expressroute-partners-and-peering-locations"></a>ExpressRoute συνεργάτες και διεισδύουν θέσεις

Οι πίνακες σε αυτό το άρθρο παρέχει πληροφορίες ExpressRoute συνδεσιμότητας υπηρεσίες παροχής, ExpressRoute γεωγραφικό πεδίο, υπηρεσίες cloud της Microsoft που υποστηρίζεται μέσω ExpressRoute και υπευθύνων ολοκλήρωσης καθολικών συστημάτων ExpressRoute (SIs).

## <a name="partners"></a>Υπηρεσίες παροχής συνδεσιμότητας ExpressRoute

ExpressRoute υποστηρίζεται σε όλες τις περιοχές Azure και θέσεις. Το παρακάτω χάρτη παρέχει μια λίστα με τις περιοχές Azure και ExpressRoute θέσεις. ExpressRoute θέσεις που αναφέρονται σε αυτές όπου Microsoft peers με πολλές υπηρεσίες παροχής.

![Χάρτης τοποθεσίας][0]

Θα έχετε πρόσβαση σε υπηρεσίες Azure σε όλες τις περιοχές μέσα σε μια περιοχή γεωπολιτική Εάν είστε συνδεδεμένοι σε τουλάχιστον μία θέση ExpressRoute μέσα στην περιοχή γεωπολιτική. Ο παρακάτω πίνακας παρέχει ένα χάρτη των περιοχών Azure ExpressRoute θέσεις μέσα σε μια περιοχή γεωπολιτική.

|**Γεωπολιτική περιοχής**|**Περιοχές Azure**|**ExpressRoute θέσεις**|
|---|---|---|
|**Βόρεια Αμερική**|Ανατολικής η.π.α., Δυτική η.π.α., Ανατολικής ΗΠΑ 2, κεντρική η.π.α., Νότιας κεντρικές ΗΠΑ, Βόρεια κεντρική η.π.α., Καναδά κεντρική Καναδά Ανατολικής|Ατλάντα, Σικάγο, Ντάλας, Λας Βέγκας, Λάρισα, Νέα Υόρκη, Σιάτλ, κοίλο πυριτίου, Washington ελεγκτή Τομέα, Μόντρεαλ +, Κεμπέκ πόλη +, Τορόντο|
|**Νότια Αμερική**|Νότια Βραζιλίας|Παύλος Σάο|
|**Ευρώπη**|Βόρεια Ευρώπη, Δυτική Ευρώπη, ΗΒ Δυτική, Νότια ΗΒ|Amsterdam, Dublin, Λονδίνο, Newport(Wales) +, Παρίσι|
|**Χώρες Ασίας και**|Ανατολικής Ασίας, νοτιοανατολικής Ασίας|Χονγκ Κονγκ, Σιγκαπούρη|
|**Ιαπωνία**|Ιαπωνία Δυτική, Ιαπωνία Ανατολή|Οσάκα, Τόκιο|
|**Αυστραλία**|Αυστραλία νοτιοανατολικής, Ανατολή Αυστραλίας|Melbourne, Σίντνεϋ|
|**Ινδία**|Δυτική Ινδίας, Νότια Central, Ινδίας Ινδίας|Τσενάι, Μουμπάι|



Ο παρακάτω πίνακας παρέχει πληροφορίες σε περιοχές και γεωπολιτική όρια για national σύννεφων.

|**Γεωπολιτική περιοχής**|**Περιοχές Azure**|**ExpressRoute θέσεις**|
|---|---|---|---|
|**Στο cloud για δημόσιους η.π.α.**|Βιρτζίνια Gov Gov Iowa, ΗΠΑ η.π.α.|Σικάγο, Ντάλας, Νέα Υόρκη, Washington ελεγκτή Τομέα|
|**Κίνα**|Βόρεια, Κίνα Ανατολή Κίνα|Πεκίνο, Shanghai|
|**Γερμανία**|Γερμανία Central, Γερμανία Ανατολή|Βερολίνο, Φραγκφούρτη|


Συνδεσιμότητα σε γεωπολιτική περιοχές δεν υποστηρίζεται σε την τυπική ExpressRoute SKU. Θα πρέπει να ενεργοποιήσετε το πρόσθετο premium ExpressRoute για την υποστήριξη καθολικής σύνδεσης. Σύνδεση με περιβάλλοντα national cloud δεν υποστηρίζεται. Εάν προκύψει όπως χρειάζεται, μπορείτε να εργαστείτε με την υπηρεσία παροχής σύνδεσης.


## <a name="connectivity-provider-locations"></a>Συνδεσιμότητα παροχής θέσεις

> [AZURE.SELECTOR]
[Θέσεις από την υπηρεσία παροχής](expressroute-locations.md#connectivity-provider-locations)
[υπηρεσίες παροχής κατά θέση](expressroute-locations-providers.md#connectivity-provider-locations)

### <a name="production-azure"></a>Παραγωγής Azure
| **Θέση**  | **Υπηρεσίες παροχής** |
|---------------|-----------------------|
| **Amsterdam** | Aryaka δίκτυα, AT & T NetBond, British Telecom, Colt, Equinix, euNetworks, GÉANT, InterCloud, λύσεις Internet - σύνδεση Cloud, Interxion, Verizon επικοινωνίες, πορτοκαλί, Tata επικοινωνίες, TeleCity ομάδα, Telenor, επίπεδο 3 |
| **Ατλάντα** | Equinix |
| **Τσενάι** | Επικοινωνίες Tata |
| **Σικάγο** | AT & T NetBond, Comcast, Equinix, επίπεδο 3 επικοινωνίες, Zayo ομάδα |
| **Ντάλας** | AT & T NetBond, Cologix, Equinix, επίπεδο 3 επικοινωνίες, Megaport |
| **Dublin** | Colt, Telecity ομάδα |
| **Χονγκ Κονγκ** | Καθολικό τηλεπικοινωνιακό British Telecom, Κίνα, Equinix, Megaport, πορτοκαλί, PCCW καθολικού περιορισμένο, Tata επικοινωνίες, Verizon |
| **Λονδίνο** | AT & T NetBond, British Telecom, Colt, Equinix, InterCloud, λύσεις Internet - σύνδεση Cloud, Interxion, Jisc, επίπεδο 3 επικοινωνίες, MTN, NTT επικοινωνίες, πορτοκαλί, Tata επικοινωνίες, Telecity ομάδα, Telenor, Verizon, Vodafone |
| **Λας Βέγκας** | Επίπεδο 3 επικοινωνίες +, Megaport
| **Λάρισα** | CoreSite, Equinix, Megaport, NTT, Zayo ομάδα |
| **Melbourne** | AARNet, Equinix, Megaport, NEXTDC, Telstra Corporation |
| **Νέα Υόρκη** | Equinix, Megaport, Zayo ομάδα |
| **Newport(Wales) +** | Επόμενη γενιάς δεδομένων + |
| **Το Μόντρεαλ** | Cologix + |
| **Μουμπάι** | Επικοινωνίες Tata |
| **Οσάκα** | Equinix, Internet πρωτοβουλία Ιαπωνία Inc. - IIJ, NTT επικοινωνίες, Softbank |
| **Παρίσι** | Interxion, Equinix + |
| **Παύλος Σάο** | Equinix, Telefonica |
| **Σιάτλ** | Megaport Equinix, επικοινωνίες επιπέδου 3, |
| **Κοίλο πυριτίου** | Aryaka δίκτυα, AT & T NetBond, British Telecom, CenturyLink +, Comcast, Equinix, επίπεδο 3 επικοινωνίες, πορτοκαλί, Tata επικοινωνίες, Verizon, Zayo ομάδα |
| **Σιγκαπούρη** | Aryaka δίκτυα, AT & T NetBond, British Telecom, Equinix, InterCloud, Megaport, πορτοκαλί, SingTel, Tata επικοινωνίες, Verizon |
| **Σίντνεϋ** | AARNet, AT & T NetBond, British Telecom, Equinix, Megaport, NEXTDC, πορτοκαλί, Telstra Corporation, Verizon |
| **Τόκυο** | Aryaka δίκτυα, British Telecom, Colt, Equinix, Internet πρωτοβουλία Ιαπωνία Inc. - IIJ, NTT επικοινωνίες, Softbank, Verizon |
| **Τορόντο** | Cologix, Equinix, Zayo ομάδα |
| **Washington ελεγκτή Τομέα** | Aryaka δίκτυα, AT & T NetBond, British Telecom, Comcast, Equinix, InterCloud, επίπεδο 3 επικοινωνίες, Megaport, πορτοκαλί, Tata επικοινωνίες, Verizon, Zayo ομάδα |

 **+**υποδηλώνει σύντομα διαθέσιμο

### <a name="national-cloud-environments"></a>National cloud περιβάλλοντα

#### <a name="us-government-cloud"></a>Στο cloud για δημόσιους η.π.α.

| **Θέση**  |**Υπηρεσίες παροχής** |
|---------------|--------------------|
| **Σικάγο** | AT & T NetBond, Equinix, επίπεδο 3 επικοινωνίες, Verizon |
| **Ντάλας** |  Equinix, Verizon + |
| **Νέα Υόρκη** | Verizon Equinix, επίπεδο 3 επικοινωνίες +, |
| **Washington ελεγκτή Τομέα** | AT & T NetBond, Equinix, επίπεδο 3 επικοινωνίες, Verizon |

#### <a name="china"></a>Κίνα

| **Θέση**  | **Υπηρεσίες παροχής** |
|---------------|-----------------------|
| **Πεκίνο** | Τηλεπικοινωνιακό Κίνα |
| **Shanghai** |  Τηλεπικοινωνιακό Κίνα |
Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [ExpressRoute στην Κίνα](http://www.windowsazure.cn/home/features/expressroute/)

#### <a name="germany"></a>Γερμανία

| **Θέση**  | **Υπηρεσίες παροχής** |
|---------------|-----------------------|
| **Βερολίνο** | Colt + e-shelter |
| **Φραγκφούρτη** | Colt, Equinix, Interxion |

## <a name="nonpartners"></a>Τη σύνδεση μέσω υπηρεσίες παροχής που δεν περιλαμβάνεται στη λίστα

Εάν δεν παρατίθεται η υπηρεσία παροχής συνδεσιμότητας στις προηγούμενες ενότητες, εξακολουθείτε να μπορείτε να δημιουργήσετε μια σύνδεση.

- Επικοινωνήστε με την υπηρεσία παροχής σύνδεσης για να δείτε αν είστε συνδεδεμένοι σε οποιαδήποτε από τις ανταλλαγές στον παραπάνω πίνακα. Μπορείτε να ελέγξετε τις παρακάτω συνδέσεις για να συλλέξετε περισσότερες πληροφορίες σχετικά με τις υπηρεσίες που παρέχονται από υπηρεσίες παροχής exchange. Πολλές υπηρεσίες παροχής συνδεσιμότητας είστε ήδη συνδεδεμένοι με κέντρα Ethernet.

    - [Σύννεφο Equinix Exchange](http://www.equinix.com/services/interconnection-connectivity/cloud-exchange/)
    - [TeleCity CloudIX](http://www.telecitygroup.com/colocation-services/cloud-ix.htm)
    - [InterXion](http://www.interxion.com/)
    - [NextDC](http://www.nextdc.com/)
    - [CoreSite](http://www.coresite.com/)
    - [Cologix](http://www.cologix.com/)
- Έχει επέκταση το δίκτυό σας στη θέση του peering της επιλογής υπηρεσία παροχής σύνδεσης.
    - Βεβαιωθείτε ότι παροχής συνδεσιμότητας επεκτείνει τη σύνδεσή σας σε ιδιαίτερα διαθέσιμα, ώστε να υπάρχουν χωρίς μία σημεία αποτυχίας.
- Διάταξη ένα κύκλωμα ExpressRoute με το exchange ως υπηρεσία παροχής σύνδεσης για να συνδεθείτε στη Microsoft.
    - Ακολουθήστε τα βήματα στο θέμα [Δημιουργία μιας κυκλώματος ExpressRoute](expressroute-howto-circuit-classic.md) για να ρυθμίσετε τη σύνδεση.

|**Θέση**|**Exchange**|**Υπηρεσίες παροχής συνδεσιμότητας**|
|-------------|------------|-------------------------|
| **Νέα Υόρκη** | Equinix | Lightower |
| **Σιάτλ** | Equinix | Την Αλάσκα επικοινωνιών |
| **Κοίλο πυριτίου** | Equinix | Επικοινωνίες XO |
| **Σιγκαπούρη** | Equinix | 1CLOUDSTAR |
| **Washington ελεγκτή Τομέα** | Equinix | Lightower |

## <a name="expressroute-system-integrators"></a>ExpressRoute υπευθύνων ολοκλήρωσης καθολικών συστημάτων

Ενεργοποίηση ιδιωτικό συνδεσιμότητας στις ανάγκες σας μπορεί να είναι δύσκολη, βάσει της κλίμακας του δικτύου σας. Μπορείτε να εργαστείτε με οποιοδήποτε από τα ενοποιητές συστήματος που παρατίθενται στον παρακάτω πίνακα για να σας βοηθήσουν να προσθήκης λογαριασμών για να ExpressRoute.

|**Ήπειρο**|**Υπευθύνων ολοκλήρωσης καθολικών συστημάτων**|
|-------------|---------------------|
| **Χώρες Ασίας και** | Avanade Inc., OneAs1a|
| **Ευρώπη** | Avanade Inc., Dotnet λύσεων|
| **ΜΑΣ** | Avanade Inc., επαγγελματικές Equinix υπηρεσίες, Perficient, ηγετική θέση έργου|

## <a name="next-steps"></a>Επόμενα βήματα

- Για περισσότερες πληροφορίες σχετικά με το ExpressRoute, ανατρέξτε στο θέμα [Συνήθεις Ερωτήσεις ExpressRoute](expressroute-faqs.md).
- Βεβαιωθείτε ότι πληρούνται όλες οι προϋποθέσεις. Ανατρέξτε στο θέμα [προαπαιτούμενα στοιχεία ExpressRoute](expressroute-prerequisites.md).

<!--Image References-->
[0]: ./media/expressroute-locations/expressroute-locations-map.png "Χάρτης τοποθεσίας"
