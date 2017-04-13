<properties
    pageTitle="Σύνδεση σε στοίβα Azure με CLI | Microsoft Azure"
    description="Μάθετε πώς να χρησιμοποιείτε το περιβάλλον γραμμής εντολών πλατφόρμες (CLI) για τη διαχείριση και ανάπτυξη των πόρων σε στοίβα Azure"
    services="azure-stack"
    documentationCenter=""
    authors="HeathL17"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="helaw"/>

# <a name="install-and-configure-azure-stack-cli"></a>Εγκατάσταση και ρύθμιση παραμέτρων CLI στοίβας Azure

Σε αυτό το έγγραφο, θα σας καθοδηγήσει κατά τη διαδικασία χρησιμοποιώντας Azure περιβάλλον γραμμής εντολών (CLI) για να διαχειριστείτε πόρους στοίβας Azure Linux και Mac πλατφόρμες προγράμματος-πελάτη.  

## <a name="install-azure-stack-cli"></a>Εγκατάσταση του Azure στοίβας CLI

Εάν είστε σε Mac ή Linux, μπορείτε να λάβετε το CLI, χρησιμοποιώντας την ακόλουθη εντολή:
  
    `npm install -g azure-cli@0.10.4`.


## <a name="connect-to-azure-stack"></a>Σύνδεση σε στοίβα Azure
Στα παρακάτω βήματα, μπορείτε να ρυθμίσετε CLI Azure για σύνδεση σε στοίβα Azure. Στη συνέχεια, πραγματοποιήστε είσοδο και ανάκτηση πληροφοριών σχετικά με τη συνδρομή.

1.  Ανάκτηση της τιμής για active directory-πόρων-αναγνωριστικού με την εκτέλεση αυτού του PowerShell:
        
          (Invoke-RestMethod -Uri https://api.azurestack.local/metadata/endpoints?api-version=1.0 -Method Get).authentication.audiences[0]

2.  Χρησιμοποιήστε την ακόλουθη εντολή CLI για να προσθέσετε το περιβάλλον Azure στοίβα, φροντίζοντας να ενημερώσετε *--active directory-πόρων-αναγνωριστικού* με τα δεδομένα ανακτήθηκαν διεύθυνση URL στο προηγούμενο βήμα:

           azure account env add AzureStack --resource-manager-endpoint-url "https://api.azurestack.local" --management-endpoint-url "https://api.azurestack.local" --active-directory-endpoint-url  "https://login.windows.net" --portal-url "https://portal.azurestack.local" --gallery-endpoint-url "https://portal.azurestack.local" --active-directory-resource-id "https://azurestack.local-api/" --active-directory-graph-resource-id "https://graph.windows.net/"

3.  Συνδεθείτε χρησιμοποιώντας την ακόλουθη εντολή (αντικατάσταση *όνομα χρήστη* με το όνομα χρήστη):

        azure login -e AzureStack -u “<username>”

    >[AZURE.NOTE]Αν λαμβάνετε θέματα επικύρωσης πιστοποιητικού, απενεργοποιήσετε την επικύρωση πιστοποιητικών, εκτελέστε την εντολή `set        NODE_TLS_REJECT_UNAUTHORIZED=0`.

4.  Ορίστε τη λειτουργία Azure ρύθμισης παραμέτρων για τη διαχείριση πόρων Azure χρησιμοποιώντας την ακόλουθη εντολή:

        azure config mode arm

5.  Ανακτήστε μια λίστα των συνδρομών.

        azure account list     

## <a name="next-steps"></a>Επόμενα βήματα

[Ανάπτυξη προτύπων με Azure CLI](azure-stack-deploy-template-command-line.md)

[Σύνδεση με το PowerShell](azure-stack-connect-powershell.md)

[Διαχείριση δικαιωμάτων χρήστη](azure-stack-manage-permissions.md)
