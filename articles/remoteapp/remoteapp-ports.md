
<properties
    pageTitle="Λίστα θύρες και διευθύνσεις URL για να whitelist για αναπτυχθεί RemoteApp Azure στο δίκτυο πελατών του εικονικού | Microsoft Azure"
    description="Μάθετε ποιες θύρες και διευθύνσεις URL θα πρέπει να ρυθμίσετε τις παραμέτρους για επικοινωνία μέσω του Azure RemoteApp."
    services="remoteapp"
    documentationCenter=""
    authors="mghosh1616"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/16/2016"
    ms.author="elizapo" />



# <a name="list-of-ports-and-urls-to-permit-access-for-azure-remoteapp-deployed-in-customer-virtual-network"></a>Λίστα με τις θύρες και διευθύνσεις URL για να επιτρέπεται η πρόσβαση για Azure RemoteApp αναπτυχθεί σε πελατών εικονικού δικτύου 

> [AZURE.IMPORTANT]
> Azure RemoteApp είναι που έχουν καταργηθεί. Διαβάστε την [ανακοίνωση](https://go.microsoft.com/fwlink/?linkid=821148) για λεπτομέρειες.

Το παρακάτω ισχύει για το Azure RemoteApp μια συλλογή cloud ή υβριδικό Εάν αναπτύσσετε σε ένα εικονικό δίκτυο (VNET). Για περισσότερες πληροφορίες σχετικά με εικονικού δίκτυα, διαβάστε [Εικονικού Επισκόπηση δικτύου](../virtual-network/virtual-networks-overview.md). Εάν έχετε δημιουργήσει μια ομάδα ασφαλείας δικτύου (NSG) περιορισμός κίνηση για τους πόρους σας εικονικού δικτύου που έχετε επιλέξει για Azure RemoteApp, βεβαιωθείτε ότι οι παρακάτω είναι προσβάσιμα και επιτρεπόμενες έως τις πολιτικές ασφαλείας του εικονικού δικτύου. Για περισσότερες πληροφορίες σχετικά με τις ομάδες ασφαλείας δικτύου, διαβάστε [Τι είναι μια ομάδα ασφαλείας δικτύου; (NSG)](../virtual-network/virtual-networks-nsg.md).

##  <a name="azure-remoteapp-subnet-needs-access-to-these-endpoints-and-urls"></a>Azure υποδικτύου RemoteApp χρειάζονται πρόσβαση σε αυτά τα τελικά σημεία και διευθύνσεις URL: 
*   *. servicebus.windows.net
*    *. servicebus.net
*    https://*.RemoteApp.windwsazure.com  
*    https://www.RemoteApp.windowsazure.com 
*    https://*RemoteApp.windowsazure.com  
*    https://*.Core.Windows.NET  
*    Εξερχομένων: TCP: 443, TCP: 10101 10175 
*    Προαιρετικό – UDP: 10201 10275  
 
## <a name="azure-remoteapp-clients-need-access-to-these-endpoints-and-urls"></a>Προγράμματα-πελάτες του Azure RemoteApp πρέπει να έχετε πρόσβαση σε αυτά τα τελικά σημεία και διευθύνσεις URL: 

Οι υπολογιστές-πελάτες να σημαίνουν ότι οι επιτραπέζιοι υπολογιστές, κ.λπ. που οι χρήστες θα χρησιμοποιήσετε για να συνδεθείτε στις εφαρμογές αναπτυχθεί στη συλλογή Azure RemoteApp συσκευές.

-  https://telemetry.RemoteApp.windowsazure.com  
-  https://*.RemoteApp.windowsazure.com (είναι το προαιρετικό θύρες UDP για αυτήν τη διεύθυνση) 
-  https://Login.Windows.NET  
-  https://Login.microsoftonline.com  
-  https://www.RemoteApp.windowsazure.com 
-  https://*.Core.Windows.NET  
-  Εξερχομένων: TCP: 443  
-  Προαιρετικό - UDP: 3391 