<properties
    pageTitle="Υποστήριξη βάσης δεδομένων SQL παλαιότερα προγράμματα-πελάτες και τελικού σημείου IP αλλάζει για το στοιχείο έλεγχος | Microsoft Azure"
    description="Μάθετε σχετικά με υποστήριξης προγράμματα-πελάτες προηγούμενης έκδοσης της βάσης δεδομένων SQL και IP αλλαγές τελικό σημείο για τον έλεγχο."
    services="sql-database"
    documentationCenter=""
    authors="ronitr"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/10/2016"
    ms.author="ronitr"/>

# <a name="sql-database----downlevel-clients-support-and-ip-endpoint-changes-for-auditing"></a>Βάση δεδομένων SQL - υποστήριξη προγράμματα-πελάτες προηγούμενων εκδόσεων και τις αλλαγές τελικού σημείου IP για το στοιχείο έλεγχος


[Έλεγχος](sql-database-auditing-get-started.md) λειτουργεί αυτόματα με τα προγράμματα-πελάτες SQL που υποστηρίζει ανακατεύθυνση ΚΔΤ.


##<a id="subheading-1"></a>Υποστήριξη προγράμματα-πελάτες προηγούμενης έκδοσης

Οποιοδήποτε πρόγραμμα-πελάτη που εφαρμόζει 7.4 ΚΔΤ επίσης πρέπει να υποστηρίζει ανακατεύθυνση. Εξαιρέσεις σε αυτό περιλαμβάνουν JDBC 4.0 στο οποίο η δυνατότητα ανακατεύθυνσης δεν υποστηρίζεται πλήρως και δεν έχει υλοποιηθεί Tedious για Node.JS σε ποια ανακατεύθυνσης.

Για "Πελάτες προηγούμενης έκδοσης", δηλαδή ποια υποστήριξη ΚΔΤ 7.3 και κάτω - το FQDN του διακομιστή στη σύνδεση η συμβολοσειρά έκδοσης πρέπει να είναι δυνατή η τροποποίηση:

Αρχικό διακομιστή FQDN στη συμβολοσειρά σύνδεσης: <*όνομα διακομιστή*>. database.windows.net

Έχουν τροποποιηθεί διακομιστή FQDN στη συμβολοσειρά σύνδεσης: <*όνομα διακομιστή*> .database. **ασφαλής**. των windows.net

Μερική λίστα "Παλαιότερα προγράμματα-πελάτες" περιλαμβάνει τα εξής:

- .NET 4.0 και παρακάτω,
- ODBC 10.0 και κάτω.
- JDBC (ενώ JDBC υποστηρίζει 7.4 ΚΔΤ, η δυνατότητα ανακατεύθυνσης ΚΔΤ δεν υποστηρίζεται πλήρως)
- Χρονοβόρα (για Node.JS)

**Παρατηρήσεις:** Το παραπάνω διακομιστή FDQN τροποποίηση ενδέχεται να είναι χρήσιμη επίσης για την εφαρμογή της πολιτικής τον έλεγχο του SQL Server επίπεδο χωρίς την ανάγκη για ένα βήμα ρύθμισης παραμέτρων σε κάθε βάση δεδομένων (προσωρινό μετριασμού).

##<a id="subheading-2"></a>Τελικό σημείο IP αλλάζει κατά την ενεργοποίηση του ελέγχου

Έχετε υπόψη ότι όταν ενεργοποιείτε έλεγχος, θα αλλάξει το τελικό σημείο IP της βάσης δεδομένων σας. Εάν έχετε ρυθμίσεις αυστηρών τείχος προστασίας, ενημερώστε τις ρυθμίσεις του τείχους προστασίας αντίστοιχα.

Το νέο τελικό σημείο IP βάσης δεδομένων εξαρτώνται από την περιοχή βάσης δεδομένων:

| Περιοχή βάσης δεδομένων | Πιθανές τελικά σημεία IP |
|----------|---------------|
| Βόρεια Κίνα  | 139.217.29.176, 139.217.28.254 |
| Κίνα Ανατολή  | 42.159.245.65, 42.159.246.245 |
| Ανατολική Αυστραλία  | 104.210.91.32, 40.126.244.159, 191.239.64.60, 40.126.255.94 |
| Αυστραλία νοτιοανατολικής | 191.239.184.223, 40.127.85.81, 191.239.161.83, 40.127.81.130 |
| Νότια Βραζιλίας  | 104.41.44.161, 104.41.62.230, 23.97.99.54, 104.41.59.191 |
| Κεντρική η.π.α.  | 104.43.255.70, 40.83.14.7, 23.99.128.244, 40.83.15.176 |
| Ανατολικής Ασίας   | 23.99.125.133, 13.75.40.42, 23.97.71.138, 13.94.43.245 |
| 2 Ανατολικής η.π.α. | 104.209.141.31, 104.208.238.177, 191.237.131.51, 104.208.235.50 |
| Ανατολικής η.π.α.   | 23.96.107.223, 104.41.150.122, 23.96.38.170, 104.41.146.44 |
| Κεντρική Ινδίας  | 104.211.98.219, 104.211.103.71 |
| Νότια Ινδίας   | 104.211.227.102, 104.211.225.157 |
| Δυτική Ινδίας  | 104.211.161.152, 104.211.162.21 |
| Ιαπωνία Ανατολή   | 104.41.179.1, 40.115.253.81, 23.102.64.207, 40.115.250.196 |
| Ιαπωνία δυτικές    | 104.214.140.140, 104.214.146.31, 191.233.32.34, 104.214.146.198 |
| Βόρεια κεντρική η.π.α.  | 191.236.155.178, 23.96.192.130, 23.96.177.169, 23.96.193.231 |
| Βόρεια Ευρώπη  | 104.41.209.221, 40.85.139.245, 137.116.251.66, 40.85.142.176 |
| Νότια κεντρικής η.π.α.  | 191.238.184.128, 40.84.190.84, 23.102.160.153, 40.84.186.66 |
| Νοτιοανατολικής Ασίας  | 104.215.198.156, 13.76.252.200, 23.97.51.109, 13.76.252.113 |
| Δυτική Ευρώπη  | 104.40.230.120, 13.80.23.64, 137.117.171.161, 13.80.8.37, 104.47.167.215, 40.118.56.193, 104.40.176.73, 40.118.56.20 |
| Δυτική η.π.α.  | 191.236.123.146, 138.91.163.240, 168.62.194.148, 23.99.6.91 |
| Κεντρική ώρα Καναδά  | 13.88.248.106 |
| Καναδάς Ανατολική  |  40.86.227.82 |
