<properties 
   pageTitle="Πρόσβαση σε ιδιωτικά Azure σύννεφων με το Visual Studio | Microsoft Azure"
   description="Μάθετε πώς μπορείτε να αποκτήσετε πρόσβαση σε ιδιωτικά cloud πόρους με χρήση του Visual Studio."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags 
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="accessing-private-azure-clouds-with-visual-studio"></a>Πρόσβαση σε ιδιωτικά Azure σύννεφων με το Visual Studio

##<a name="overview"></a>Επισκόπηση

Από προεπιλογή, το Visual Studio υποστηρίζει τελικά σημεία ΥΠΌΛΟΙΠΑ δημόσια Azure cloud. Αυτό μπορεί να είναι ένα πρόβλημα, ωστόσο, εάν χρησιμοποιείτε το Visual Studio με μια ιδιωτική Azure cloud. Μπορείτε να χρησιμοποιήσετε πιστοποιητικά για ρύθμιση παραμέτρων του Visual Studio για πρόσβαση σε ιδιωτική Azure cloud ΥΠΌΛΟΙΠΑ τα τελικά σημεία. Μπορείτε να λάβετε αυτές τις αρχείο ρυθμίσεων δημοσίευση πιστοποιητικά μέσω του Azure.

## <a name="to-access-a-private-azure-cloud-in-visual-studio"></a>Για να αποκτήσετε πρόσβαση σε μια ιδιωτική Azure cloud στο Visual Studio

1. Στην [κλασική πύλη του Azure](http://go.microsoft.com/fwlink/?LinkID=213885) για το cloud ιδιωτικό, κάντε λήψη του αρχείου ρυθμίσεων δημοσίευση ή επικοινωνήστε με το διαχειριστή σας για ένα αρχείο ρυθμίσεων δημοσίευση. Στη δημόσια έκδοση του Azure, τη σύνδεση για να κάνετε λήψη αυτό είναι [https://manage.windowsazure.com/publishsettings/](https://manage.windowsazure.com/publishsettings/). (Το αρχείο λήψη θα πρέπει να έχει επέκταση .publishsettings.)

1. Στην **Εξερεύνηση Server** στο Visual Studio, επιλέξτε τον κόμβο **Azure** και, στο μενού συντόμευσης, κάντε κλικ στην εντολή **Διαχείριση εγγραφών** .

    ![Διαχείριση συνδρομές εντολής](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/IC790778.png)

1. Στο παράθυρο διαλόγου **Διαχείριση Microsoft Azure συνδρομές** , επιλέξτε την καρτέλα **πιστοποιητικά** και, στη συνέχεια, επιλέξτε το κουμπί " **Εισαγωγή** ".

    ![Εισαγωγή Azure πιστοποιητικών](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/IC790779.png)

1. Στο παράθυρο διαλόγου **Εισαγωγή Microsoft Azure συνδρομές** , μεταβείτε στο φάκελο όπου που αποθηκεύσατε το αρχείο ρυθμίσεων δημοσίευση και επιλέξτε το αρχείο και κατόπιν επιλέξτε το κουμπί " **Εισαγωγή** ". Αυτό θα εισαγάγει τα πιστοποιητικά στο αρχείο ρυθμίσεων δημοσίευση στο Visual Studio. Τώρα θα πρέπει να μπορείτε να αλληλεπιδράσετε με τους πόρους σας στο cloud ιδιωτικό.

    ![Εισαγωγή ρυθμίσεις δημοσίευσης](./media/vs-azure-tools-access-private-azure-clouds-with-visual-studio/IC790780.png)

## <a name="next-steps"></a>Επόμενα βήματα

[Δημοσίευση σε μια υπηρεσία Azure Cloud από το Visual Studio](https://msdn.microsoft.com/library/azure/ee460772.aspx)

[Πώς μπορείτε να: λήψη και εισαγωγή δημοσίευση ρυθμίσεις και τις πληροφορίες συνδρομής](https://msdn.microsoft.com/library/dn385850(v=nav.70).aspx)

