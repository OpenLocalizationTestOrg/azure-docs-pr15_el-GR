<properties
    pageTitle="Προσθήκη νέου λογαριασμού μισθωτή Azure στοίβας στο Azure Active Directory | Microsoft Azure"
    description="Μετά την ανάπτυξη του Microsoft Azure στοίβας POC, θα χρειαστεί να δημιουργήσετε τουλάχιστον ένα μισθωτή του λογαριασμού χρήστη, ώστε να μπορείτε να εξερευνήσετε την πύλη του μισθωτή."
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

# <a name="add-a-new-azure-stack-tenant-account-in-azure-active-directory"></a>Προσθήκη νέου λογαριασμού μισθωτή Azure στοίβας στο Azure Active Directory

Μετά [την ανάπτυξη του Azure POC στοίβα](azure-stack-run-powershell-script.md), χρειάζεστε ένα λογαριασμό χρήστη μισθωτή ώστε να μπορείτε να εξερευνήσετε την πύλη του μισθωτή και να ελέγξετε τις προσφορές και τα προγράμματα. Μπορείτε να δημιουργήσετε ένα λογαριασμό μισθωτή, [χρησιμοποιώντας την πύλη του Azure](#create-an-azure-stack-tenant-account-using-the-azure-portal) ή με [χρήση του PowerShell](#create-an-azure-stack-tenant-account-using-powershell).

## <a name="create-an-azure-stack-tenant-account-using-the-azure-portal"></a>Δημιουργία λογαριασμού μισθωτή στοίβας Azure χρησιμοποιώντας την πύλη του Azure

Πρέπει να έχετε μια συνδρομή του Azure να χρησιμοποιήσουν την πύλη Azure.

1. Συνδεθείτε στο [Azure](http://manage.windowsazure.com).

2.  Στο Microsoft Azure αριστερή γραμμή περιήγησης, κάντε κλικ στην επιλογή **Υπηρεσία καταλόγου Active Directory**.

3.  Στη λίστα καταλόγου, κάντε κλικ στον κατάλογο που θέλετε να χρησιμοποιήσετε για τη στοίβα Azure ή δημιουργήστε ένα νέο.

4.  Σε αυτήν τη σελίδα καταλόγου, κάντε κλικ στην επιλογή **χρήστες**.

5.  Κάντε κλικ στην επιλογή **Προσθήκη χρήστη**.

6.  Στον οδηγό **Προσθήκη χρήστη** , στη λίστα **Τύπος χρήστη** , επιλέξτε **νέο χρήστη στην εταιρεία σας**.

7.  Στο πλαίσιο **όνομα χρήστη** , πληκτρολογήστε ένα όνομα για το χρήστη.

8.  Στο το **@** , επιλέξτε την κατάλληλη εισαγωγή.

9.  Κάντε κλικ στο βέλος Επόμενο.

10.  Στη σελίδα του **προφίλ χρήστη** του οδηγού, πληκτρολογήστε ένα **όνομα**, **Επώνυμο**και **εμφανιζόμενο όνομα**.

11. Στη λίστα **εργασιών** , επιλέξτε το **χρήστη**.

12. Κάντε κλικ στο βέλος Επόμενο.

13. Στη σελίδα **λήψη προσωρινό κωδικό πρόσβασης** , κάντε κλικ στην επιλογή **Δημιουργία**.

14. Αντιγράψτε το **νέο κωδικό πρόσβασης**.

15. Συνδεθείτε στο Microsoft Azure με το νέο λογαριασμό. Αλλαγή του κωδικού πρόσβασης όταν σας ζητηθεί.

16. Συνδεθείτε στο `https://portal.azurestack.local` με το νέο λογαριασμό για να δείτε την πύλη του μισθωτή.

## <a name="create-an-azure-stack-tenant-account-using-powershell"></a>Δημιουργία λογαριασμού μισθωτή στοίβας Azure μέσω του PowerShell

Εάν δεν έχετε μια συνδρομή του Azure, δεν μπορείτε να χρησιμοποιήσετε την πύλη του Azure για να προσθέσετε ένα λογαριασμό χρήστη μισθωτή. Σε αυτήν την περίπτωση, μπορείτε να χρησιμοποιήσετε αντί για αυτό το Azure Active Directory λειτουργικής μονάδας για το Windows PowerShell.

> [AZURE.NOTE] Εάν χρησιμοποιείτε λογαριασμό Microsoft (Live ID) για την ανάπτυξη του Azure PoC στοίβα, δεν μπορείτε να χρησιμοποιήσετε AAD PowerShell για τη δημιουργία λογαριασμού μισθωτή. 

1.  Εγκατάσταση του [Microsoft Online Services Βοηθού εισόδου για επαγγελματίες τεχνολογιών ΠΛΗΡΟΦΟΡΙΚΉΣ RTW](https://www.microsoft.com/en-us/download/details.aspx?id=41950).

2.  Εγκαταστήστε το [Azure Active Directory λειτουργικής μονάδας για το Windows PowerShell (έκδοση 64 bit)](http://go.microsoft.com/fwlink/p/?linkid=236297) και να το ανοίξετε.

3.  Εκτελέστε τα ακόλουθα cmdlet:




    ```powershell
    # Provide the AAD credential you use to deploy Azure Stack PoC
   
            $msolcred = get-credential
    
    # Add a tenant account "Tenant Admin <username>@<yourdomainname>" with the initial password "<password>".
    
            connect-msolservice -credential $msolcred
            $user = new-msoluser -DisplayName "Tenant Admin" -UserPrincipalName <username>@<yourdomainname> -Password <password>
            Add-MsolRoleMember -RoleName "Company Administrator" -RoleMemberType User -RoleMemberObjectId $user.ObjectId
    
    ```

4.  Πραγματοποιήστε είσοδο στο Microsoft Azure με το νέο λογαριασμό. Αλλαγή του κωδικού πρόσβασης όταν σας ζητηθεί.

5.  Πραγματοποιήστε είσοδο στο `https://portal.azurestack.local` με το νέο λογαριασμό για να δείτε την πύλη του μισθωτή.



