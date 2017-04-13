<properties 
   pageTitle="Εντοπισμός σφαλμάτων υπηρεσίες Azure Cloud | Microsoft Azure"
   description="Υπηρεσίες Azure Cloud εντοπισμού σφαλμάτων"
   services="visual-studio-online"
   documentationCenter="n/a"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags 
   ms.service="visual-studio-online"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="multiple"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="debugging-cloud-services"></a>Υπηρεσίες cloud εντοπισμού σφαλμάτων

Μπορείτε να χρησιμοποιήσετε διαφορετικές μεθόδους για τον εντοπισμό σφαλμάτων σε μια εφαρμογή του Azure χρησιμοποιώντας τα εργαλεία Azure για το Microsoft Visual Studio και το Azure SDK:

- Μπορείτε να προσθέσετε μια εφαρμογή του Azure από το Visual Studio όταν σχεδιάζετε, ακριβώς όπως θα κάνατε με οποιαδήποτε εφαρμογή του Visual C# ή Visual Basic. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Εντοπισμός σφαλμάτων την υπηρεσία cloud στον τοπικό σας υπολογιστή](vs-azure-tools-debug-cloud-services-virtual-machines.md#debug-your-cloud-service-on-your-local-computer).

- Μπορείτε να χρησιμοποιήσετε τα Διαγνωστικά Azure για να καταγράψετε λεπτομερείς πληροφορίες από κώδικα που εκτελείται μέσα ρόλους, εάν εκτελούνται οι ρόλοι στο περιβάλλον ανάπτυξης ή στο Azure. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [συλλογή καταγραφή δεδομένων, χρησιμοποιώντας τα Διαγνωστικά Azure](http://go.microsoft.com/fwlink/p/?LinkId=400450).

- Εάν χρησιμοποιείτε το Visual Studio για μεγάλες επιχειρήσεις για να γράψετε στοχευμένες σε το 4 .NET Framework ή το διαίρεσης 4,5 .NET Framework ρόλους, μπορείτε να ενεργοποιήσετε IntelliTrace τη στιγμή να αναπτύξετε μια υπηρεσία Azure cloud από το Visual Studio. IntelliTrace παρέχει ένα αρχείο καταγραφής που μπορείτε να χρησιμοποιήσετε με το Visual Studio για τον εντοπισμό σφαλμάτων την εφαρμογή σας, όπως εάν εκτελείται στο Azure. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [τον εντοπισμό σφαλμάτων σε μια υπηρεσία cloud δημοσιευμένο με IntelliTrace και του Visual Studio]( http://go.microsoft.com/fwlink/p/?LinkId=623016).

- Μπορείτε να ενεργοποιήσετε απομακρυσμένο τον εντοπισμό σφαλμάτων σε τις υπηρεσίες cloud τη στιγμή όταν αναπτύσσετε την υπηρεσία cloud από το Visual Studio. Εάν επιλέξετε να ενεργοποιήσετε απομακρυσμένου εντοπισμού σφαλμάτων για μια ανάπτυξη, υπηρεσίες απομακρυσμένης εντοπισμού σφαλμάτων είναι εγκατεστημένα στον τις εικονικές μηχανές που εκτελείται κάθε παρουσία ρόλο. Αυτές τις υπηρεσίες, όπως msvsmon.exe, δεν επηρεάζουν τις επιδόσεις ή να έχει ως αποτέλεσμα επιπλέον κόστους. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Εντοπισμός σφαλμάτων σε μια υπηρεσία στο cloud στο Azure](vs-azure-tools-debug-cloud-services-virtual-machines.md#debug-a-cloud-service-in-azure).



