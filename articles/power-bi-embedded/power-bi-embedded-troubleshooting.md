<properties
   pageTitle="Αντιμετώπιση προβλημάτων ενσωματωμένου προεπισκόπηση του Microsoft Power BI"
   description="Αντιμετώπιση προβλημάτων ενσωματωμένου προεπισκόπηση του Microsoft Power BI"
   services="power-bi-embedded"
   documentationCenter=""
   authors="guyinacube"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="power-bi-embedded"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="powerbi"
   ms.date="10/04/2016"
   ms.author="asaxton"/>

# <a name="microsoft-power-bi-embedded-preview-troubleshooting"></a>Αντιμετώπιση προβλημάτων ενσωματωμένου προεπισκόπηση του Microsoft Power BI
Σε αυτό το άρθρο παρέχει απαντήσεις για τον τρόπο αντιμετώπισης προβλημάτων **Ενσωματωμένου Power BI**.

<a name="connection-string"/>
## <a name="setting-sql-server-connection-strings"></a>Ρύθμιση συμβολοσειρές σύνδεσης του SQL Server
Για να ορίσετε τη σύνδεση συμβολοσειρά SQL Server, που πρέπει να ακολουθήσετε μια συγκεκριμένη μορφή. Ακολουθεί ένα παράδειγμα συμβολοσειράς σύνδεσης για τον SQL Server.

```
"Persist Security Info=False;Integrated Security=true;Initial Catalog=Northwind;server=(local)"
```

Για να μάθετε περισσότερα σχετικά με τις συμβολοσειρές σύνδεσης του SQL Server, ανατρέξτε στα ακόλουθα άρθρα:

-   [Συμβολοσειρές σύνδεσης του SQL Server](https://msdn.microsoft.com/library/jj653752.aspx)
-   [SqlConnection.ConnectionString](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.connectionstring.aspx)

<a name="credentials"/>
## <a name="setting-credentials"></a>Ορισμός διαπιστευτηρίων
Στην περίπτωση όπου έχετε τα διαπιστευτήρια για ανάπτυξη ή ενδιάμεσου σταδίου περιβάλλον, όπως το όνομα χρήστη και τον κωδικό πρόσβασης, ίσως χρειαστεί να ενημερώσετε τα διαπιστευτήρια που συμφωνούν με μια λύση παραγωγής.

## <a name="see-also"></a>Δείτε επίσης
- [Γρήγορα αποτελέσματα με το δείγμα](power-bi-embedded-get-started-sample.md)
- [Τι είναι το Power BI ενσωματωμένου](power-bi-embedded-what-is-power-bi-embedded.md)
