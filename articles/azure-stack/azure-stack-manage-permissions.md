<properties
    pageTitle="Διαχείριση δικαιωμάτων σε πόρους ανά χρήστη σε στοίβα Azure (διαχειριστή της υπηρεσίας και μισθωτή) | Microsoft Azure"
    description="Ως διαχειριστής της υπηρεσίας ή μισθωτή, μάθετε πώς να διαχειρίζεστε δικαιώματα για τους πόρους ανά χρήστη."
    services="azure-stack"
    documentationCenter=""
    authors="ErikjeMS"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="erikje"/>

# <a name="manage-user-permissions"></a>Διαχείριση δικαιωμάτων χρήστη

Ένας χρήστης σε στοίβα Azure μπορεί να είναι ένα πρόγραμμα ανάγνωσης, κατόχου ή συμβολής για κάθε παρουσία του μια συνδρομή, ομάδα πόρων ή της υπηρεσίας. Για παράδειγμα, ο χρήστης A ενδέχεται να έχετε δικαιώματα ανάγνωσης για τη συνδρομή 1, αλλά έχετε δικαιώματα κατόχου για να εικονική μηχανή 7.

-   Πρόγραμμα ανάγνωσης: Χρήστη να προβάλετε τα πάντα, αλλά να κάνετε αλλαγές.

-   Συνεργάτης: Χρήστη για να διαχειριστείτε τα πάντα εκτός από την πρόσβαση σε πόρους.

-   Κάτοχος: Χρήστη για να διαχειριστείτε τα πάντα, συμπεριλαμβανομένης της πρόσβασης σε πόρους.


## <a name="set-access-permissions-for-a-user"></a>Ορισμός δικαιωμάτων πρόσβασης για ένα χρήστη

1.  Πραγματοποιήστε είσοδο με ένα λογαριασμό που έχει δικαιώματα κατόχου για τον πόρο που θέλετε να διαχειριστείτε.

2.  Στο το blade για τον πόρο, κάντε κλικ στο εικονίδιο **Access** ![](media/azure-stack-manage-permissions/image1.png).

3.  Στο το blade **χρήστες** , κάντε κλικ στην επιλογή **ρόλων**.

4.  Στο το blade **ρόλων** , κάντε κλικ στην επιλογή **Προσθήκη** για να προσθέσετε δικαιώματα για το χρήστη.

## <a name="next-steps"></a>Επόμενα βήματα

[Προσθέστε ένα μισθωτή του Azure στοίβας](azure-stack-add-new-user-aad.md)
