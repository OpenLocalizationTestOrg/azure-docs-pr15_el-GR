<properties
    pageTitle="Ανάπτυξη εικονική μηχανή κλίμακα ρύθμιση χρήση του Visual Studio | Microsoft Azure"
    description="Ανάπτυξη σύνολα κλίμακα εικονική μηχανή με χρήση του Visual Studio και ένα πρότυπο από διαχειριστή πόρων"
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="gbowerman"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/13/2016"
    ms.author="guybo"/>

# <a name="deploy-virtual-machine-scale-set-using-visual-studio"></a>Ανάπτυξη εικονική μηχανή κλίμακα ρύθμιση χρήση του Visual Studio

Αυτό το άρθρο σας δείχνει πώς να αναπτύξετε μια Azure εικονική μηχανή κλίμακα ρύθμιση χρησιμοποιώντας μια οπτική ανάπτυξη ομάδα πόρων Studio.


[Τα σύνολα κλίμακα εικονική μηχανή Azure](https://azure.microsoft.com/blog/azure-vm-scale-sets-public-preview/) είναι ένας πόρος τον υπολογισμό Azure για να αναπτύξετε και να διαχειριστείτε μια συλλογή από παρόμοια εικονικές μηχανές με εύκολα ενσωματωμένες επιλογές για αυτόματη κλίμακα και εξισορρόπηση φόρτου. Μπορείτε να προμηθεύσουν και να υλοποιήσετε Εικονική κλίμακα σύνολα με τη χρήση [προτύπων διαχείρισης πόρων Azure (ARM)](https://github.com/Azure/azure-quickstart-templates). Πρότυπα ARM μπορούν να αναπτυχθούν χρησιμοποιώντας ΥΠΌΛΟΙΠΑ CLI Azure, PowerShell, καθώς και απευθείας από το Visual Studio. Visual Studio παρέχει ένα σύνολο παράδειγμα πρότυπα τα οποία μπορούν να αναπτυχθούν ως μέρος ενός έργου Azure ανάπτυξης ομάδα πόρων.

Ομάδα πόρων Azure υλοποιήσεις είναι ένας τρόπος για να ομαδοποιήσετε και να δημοσιεύσετε ένα σύνολο σχετικών Azure πόρων σε μια λειτουργία μία ανάπτυξη. Να μάθετε περισσότερα σχετικά με τους εδώ: [Δημιουργία και ανάπτυξη ομάδων πόρων Azure μέσω του Visual Studio](../vs-azure-tools-resource-groups-deployment-projects-create-deploy.md).

## <a name="pre-requisites"></a>Προαπαιτούμενα

Για να ξεκινήσετε την ανάπτυξη σύνολα κλίμακα Εικονική στο Visual Studio χρειάζεστε τα εξής:

- Visual Studio 2013 ή του 2015
- Azure SDK 2.7, 2,8 ή 2.9

Σημείωση: Αυτές τις οδηγίες θεωρείται ότι χρησιμοποιείτε το Visual Studio 2015 με [2,8 SDK Azure](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/).

## <a name="creating-a-project"></a>Δημιουργία ενός έργου

1. Δημιουργήστε ένα νέο έργο στο Visual Studio 2015, επιλέγοντας **αρχείο | Δημιουργία | Έργο**.

    ![Δημιουργία αρχείου][file_new]

2. Στην περιοχή **Visual C# | Σύννεφο**, επιλέξτε **Διαχείριση πόρων Azure** για να δημιουργήσετε ένα έργο για την ανάπτυξη ενός προτύπου ARM.

    ![Δημιουργία έργου][create_project]

3.  Από τη λίστα των προτύπων, επιλέξτε είτε το Linux ή Windows εικονική μηχανή κλίμακα Ορισμός πρότυπο.

    ![Επιλέξτε το πρότυπο][select_Template]

4. Αφού δημιουργηθεί το έργο σας θα βλέπετε δεσμών ενεργειών του PowerShell ανάπτυξης ενός προτύπου για τη διαχείριση πόρων Azure και ένα αρχείο παραμέτρων για το σύνολο κλίμακα εικονική μηχανή.

    ![Εξερεύνηση λύσεων][solution_explorer]

## <a name="customize-your-project"></a>Προσαρμόστε το έργο σας

Τώρα μπορείτε να επεξεργαστείτε το πρότυπο για να το προσαρμόσετε για τις ανάγκες της εφαρμογής σας, όπως η προσθήκη ιδιοτήτων επέκταση Εικονική ή την επεξεργασία των κανόνων εξισορρόπησης φόρτου. Από προεπιλογή τα πρότυπα Ορισμός Εικονική κλίμακα έχουν ρυθμιστεί για να αναπτύξετε την επέκταση AzureDiagnostics που διευκολύνει την προσθήκη autoscale κανόνων. Την ανάπτυξη επίσης μια μονάδα εξισορρόπησης φόρτου με μια δημόσια διεύθυνση IP, έχουν ρυθμιστεί με τους κανόνες εισερχόμενων NAT που σας επιτρέπουν να συνδέεστε με τις παρουσίες Εικονική με SSH (Linux) ή RDP (Windows) – το εύρος θύρας προσκηνίου αρχίζει 50000, γεγονός που σημαίνει ότι στην περίπτωση Linux, εάν έχετε SSH στη θύρα 50000 του δημόσια διεύθυνση IP (ή το όνομα του τομέα) που θα δρομολογούνται στη θύρα 22 από την πρώτη εικονική Μηχανή στο σύνολο κλίμακα. Σύνδεση στη θύρα 50001 θα δρομολογούνται στη θύρα 22 από τη δεύτερη Εικονική και ούτω καθεξής.

 Ένας καλός τρόπος για να επεξεργαστείτε τα πρότυπά σας με το Visual Studio είναι να χρησιμοποιήσετε τη διάρθρωση JSON για να οργανώσετε τις παραμέτρους, μεταβλητές και τους πόρους. Με την κατανόηση του σχήματος Visual Studio μπορεί να οδηγεί σφάλματα στο πρότυπό σας, πριν να την αναπτύξετε.

![Εξερεύνηση JSON][json_explorer]

## <a name="deploy-the-project"></a>Αναπτύξτε το έργο

6. Ανάπτυξη προτύπου ARM Azure για να δημιουργήσετε τον πόρο Εικονική ρύθμιση κλίμακα. Κάντε δεξί κλικ στον κόμβο του έργου, επιλέξτε **ανάπτυξη | Ανάπτυξη νέας**.

    ![Ανάπτυξη προτύπου][5deploy_Template]

7. Επιλέξτε τη συνδρομή σας στο παράθυρο διαλόγου "Ανάπτυξη σε ομάδα πόρων".

    ![Ανάπτυξη προτύπου][6deploy_Template]

8. Από εδώ, μπορείτε επίσης να δημιουργήσετε μια νέα ομάδα πόρων Azure για να αναπτύξετε το πρότυπο για να.

    ![Νέα ομάδα πόρων][new_resource]

9. Στη συνέχεια επιλέξτε το κουμπί **Επεξεργασία παραμέτρους** για να εισαγάγετε τις παραμέτρους που θα περάσουν στο πρότυπό σας, ορισμένες τιμές όπως το όνομα χρήστη και τον κωδικό πρόσβασης για το λειτουργικό σύστημα απαιτούνται για να δημιουργήσετε την ανάπτυξη. Εάν δεν έχετε εργαλεία του PowerShell για εγκατεστημένο το Visual Studio, συνιστάται να ελέγξετε "Αποθήκευση κωδικών πρόσβασης" για να αποφύγετε ένα κρυφό παράθυρο γραμμής εντολών του PowerShell ή χρησιμοποιήστε [keyvault υποστήριξης](https://azure.microsoft.com/blog/keyvault-support-for-arm-templates/).

    ![Επεξεργασία των παραμέτρων][edit_parameters]

10. Τώρα, κάντε κλικ στην επιλογή **Ανάπτυξη**. Στο παράθυρο **εξόδου** θα εμφανίσει την πρόοδο της ανάπτυξης. Λάβετε υπόψη ότι η την ενέργεια εκτελείται η δέσμη ενεργειών **Ανάπτυξης AzureResourceGroup.ps1** .

    ![Παράθυρο εξόδου][output_window]

## <a name="exploring-your-vm-scale-set"></a>Εξερεύνηση του συνόλου κλίμακα Εικονική

Μόλις ολοκληρωθεί η ανάπτυξη, μπορείτε να προβάλετε το νέο σύνολο κλίμακα Εικονική στο Visual Studio **Cloud Explorer** (ανανέωση της λίστας). Εξερεύνηση cloud σάς επιτρέπει να διαχειρίζεστε Azure πόρους στο Visual Studio κατά την ανάπτυξη εφαρμογών. Μπορείτε επίσης να προβάλετε το σύνολο κλίμακα Εικονική σας στην [Πύλη του Azure](https://portal.azure.com) και [Εξερεύνηση πόρων Azure](https://resources.azure.com/).

![Εξερεύνηση cloud][cloud_explorer]

 Η πύλη παρέχει ο καλύτερος τρόπος για να διαχειριστείτε την υποδομή Azure σας με ένα πρόγραμμα περιήγησης web, ενώ Azure πόρων Explorer αποτελούν έναν εύκολο τρόπο να Εξερεύνηση οπτικά και να εντοπισμός σφαλμάτων Azure πόρων, παρέχοντας ένα παράθυρο σε "Προβολή παρουσία" και επίσης που εμφανίζει τις εντολές του PowerShell για τους πόρους που αναζητάτε. Ενώ είναι σύνολα κλίμακα Εικονική στην προεπισκόπηση, Εξερεύνηση του πόρου θα εμφανίζει τις περισσότερες λεπτομέρειες για τα σύνολα κλίμακα σας Εικονική.

## <a name="next-steps"></a>Επόμενα βήματα

Μόλις που έχετε αναπτύξει με επιτυχία σύνολα κλίμακα Εικονική μέσω του Visual Studio μπορείτε να προσαρμόσετε περαιτέρω το έργο σας να ταιριάζει με τις απαιτήσεις της εφαρμογής. Για παράδειγμα ρύθμιση autoscale, προσθέτοντας ένα πόρο ιδέες, προσθέτοντας υποδομή στο πρότυπό σας, όπως μεμονωμένη ΣΠΣ ή για την ανάπτυξη εφαρμογών χρησιμοποιώντας την επέκταση προσαρμοσμένων δεσμών ενεργειών. Μπορείτε να βρείτε μια καλή προέλευση παράδειγμα προτύπων στο αποθετήριο GitHub [Azure γρήγορη έναρξη πρότυπα](https://github.com/Azure/azure-quickstart-templates) (Αναζήτηση για "vmss").

[file_new]: ./media/virtual-machine-scale-sets-vs-create/1-FileNew.png
[create_project]: ./media/virtual-machine-scale-sets-vs-create/2-CreateProject.png
[select_Template]: ./media/virtual-machine-scale-sets-vs-create/3b-SelectTemplateLin.png
[solution_explorer]: ./media/virtual-machine-scale-sets-vs-create/4-SolutionExplorer.png
[json_explorer]: ./media/virtual-machine-scale-sets-vs-create/10-JsonExplorer.png
[5deploy_Template]: ./media/virtual-machine-scale-sets-vs-create/5-DeployTemplate.png
[6deploy_Template]: ./media/virtual-machine-scale-sets-vs-create/6-DeployTemplate.png
[new_resource]: ./media/virtual-machine-scale-sets-vs-create/7-NewResourceGroup.png
[edit_parameters]: ./media/virtual-machine-scale-sets-vs-create/8-EditParameter.png
[output_window]: ./media/virtual-machine-scale-sets-vs-create/9-Output.png
[cloud_explorer]: ./media/virtual-machine-scale-sets-vs-create/12-CloudExplorer.png
