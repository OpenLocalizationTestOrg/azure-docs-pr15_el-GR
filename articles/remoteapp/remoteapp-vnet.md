
<properties
    pageTitle="Επικυρώστε τη VNET Azure για χρήση με το Azure RemoteApp | Microsoft Azure"
    description="Μάθετε πώς μπορείτε να βεβαιωθείτε ότι είναι έτοιμη για χρήση με το Azure RemoteApp σας VNET Azure"
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />



# <a name="validate-the-azure-vnet-to-use-with-azure-remoteapp"></a>Επικυρώστε τη VNET Azure για χρήση με το Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp είναι που έχουν καταργηθεί. Διαβάστε την [ανακοίνωση](https://go.microsoft.com/fwlink/?linkid=821148) για λεπτομέρειες.

Πριν χρησιμοποιήσετε ένα VNET Azure με Azure RemoteApp, μπορείτε να επικυρώσετε τη VNET. Αυτό σας βοηθά να αποφύγετε προβλήματα με τη σύνδεση.

Για να επικυρώσετε σας VNET Azure, κάντε τα εξής:

1. Δημιουργήστε μια εικονική μηχανή Azure μέσα στο υποδίκτυο από το Azure VNET που θέλετε να χρησιμοποιήσετε με το Azure RemoteApp.

2. Σύνδεση σε συγκεκριμένη Εικονική χρησιμοποιώντας την επιλογή **σύνδεση** στην πύλη διαχείρισης.
3. Συμμετοχή η εικονική μηχανή στον ίδιο τομέα που θέλετε να χρησιμοποιήσετε με το Azure RemoteApp. Εάν θέλετε να δημιουργήσετε μια συλλογή υβριδική που συνδέεται με το δίκτυο εσωτερικής εγκατάστασης, συμμετοχή η εικονική μηχανή στον τοπικό τομέα σας.

Εάν αυτή είναι επιτυχής, το VNET Azure είναι έτοιμες για χρήση με RemoteApp.

Για περισσότερες πληροφορίες σχετικά με τη ροή εργασίας συλλογής υβριδική για ολοκληρωμένες, ανατρέξτε στα ακόλουθα άρθρα:

- [Πώς να σχεδιάσετε το εικονικό δίκτυο για Azure RemoteApp](remoteapp-planvnet.md)
- [Δημιουργία συλλογής υβριδική](remoteapp-create-hybrid-deployment.md)
- [Ανάπτυξη συλλογής Azure RemoteApp σας Azure εικονικού δικτύου (με την υποστήριξη για ExpressRoute)](http://blogs.msdn.com/b/rds/archive/2015/04/23/deploy-azure-remoteapp-collection-to-your-azure-virtual-network-with-support-for-expressroute.aspx)
