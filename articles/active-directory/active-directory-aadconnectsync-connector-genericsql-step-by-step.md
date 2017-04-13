<properties
   pageTitle="Γενικό σύνδεσης SQL βήμα προς βήμα | Microsoft Azure"
   description="Σε αυτό το άρθρο είναι παρουσίαση μερικών μέσω ενός απλού συστήματος HR βήμα προς βήμα χρησιμοποιώντας το γενικό Connector SQL."
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.workload="identity"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="08/30/2016"
   ms.author="billmath"/>

# <a name="generic-sql-connector-step-by-step"></a>Γενικό SQL σύνδεσης βήμα προς βήμα
Αυτό το θέμα είναι ένας οδηγός βήμα προς βήμα. Δημιουργεί μια απλή δείγμα βάσης δεδομένων HR και το χρησιμοποιήσετε για την εισαγωγή ορισμένων χρηστών και την ιδιότητα μέλους ομάδας.

## <a name="prepare-the-sample-database"></a>Προετοιμασία του δείγματος βάσης δεδομένων
Σε ένα διακομιστή που εκτελεί τον SQL Server, εκτελέστε τη δέσμη ενεργειών SQL που βρέθηκε στο [παράρτημα A](#appendix-a). Αυτή η δέσμη ενεργειών δημιουργεί ένα δείγμα βάσης δεδομένων με το όνομα GSQLDEMO. Το μοντέλο αντικειμένου για τη βάση δεδομένων που έχουν δημιουργηθεί μοιάζει με αυτήν την εικόνα:  
![Μοντέλο αντικειμένου](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\objectmodel.png)

Επίσης, δημιουργήστε ένα χρήστη που θέλετε να χρησιμοποιήσετε για να συνδεθείτε με τη βάση δεδομένων. Σε αυτόν τον οδηγό, ο χρήστης είναι που ονομάζεται FABRIKAM\SQLUser και βρίσκεται στον τομέα.

## <a name="create-the-odbc-connection-file"></a>Δημιουργία αρχείου σύνδεσης ODBC
Το γενικό Connector SQL χρησιμοποιεί ODBC για να συνδεθείτε με τον απομακρυσμένο διακομιστή. Πρώτα πρέπει να δημιουργήσετε ένα αρχείο με τις πληροφορίες σύνδεσης ODBC.

1. Ξεκινήστε το βοηθητικό πρόγραμμα διαχείρισης ODBC στο διακομιστή σας:  
![ODBC](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\odbc.png)
2. Επιλέξτε την καρτέλα **DSN αρχείου**. Κάντε κλικ στην επιλογή **Προσθήκη...**.
![ODBC1](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\odbc1.png)
3. Λειτουργεί το πρόγραμμα οδήγησης της πρώτης λεπτομερή, επομένως, επιλέξτε την και κάντε κλικ στην επιλογή **Επόμενο >**.  
![ODBC2](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\odbc2.png)
4. Δώστε ένα όνομα, όπως **GenericSQL**το αρχείο.  
![ODBC3](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\odbc3.png)
5. Κάντε κλικ στο κουμπί **Τέλος**.  
![ODBC4](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\odbc4.png)
6. Ώρα για τη ρύθμιση παραμέτρων της σύνδεσης. Πληκτρολογήστε το αρχείο προέλευσης δεδομένων μια καλή περιγραφή και δώστε το όνομα του διακομιστή στον οποίο εκτελείται το SQL Server.  
![ODBC5](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\odbc5.png)
7. Επιλέξτε τον τρόπο για τον έλεγχο ταυτότητας με SQL. Σε αυτήν την περίπτωση, μπορούμε να χρησιμοποιήσουμε ελέγχου ταυτότητας των Windows.  
![ODBC6](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\odbc6.png)
8. Παροχή του ονόματος του δείγματος βάσης δεδομένων, **GSQLDEMO**.  
![ODBC7](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\odbc7.png)
9. Διατήρηση όλα όσα προεπιλεγμένη σε αυτή την οθόνη. Κάντε κλικ στο κουμπί **Τέλος**.  
![ODBC8](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\odbc8.png)
10. Για να επαληθεύσετε όλα λειτουργούν όπως αναμένεται, κάντε κλικ στην επιλογή **Δοκιμή προέλευσης δεδομένων**.  
![ODBC9](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\odbc9.png)
11. Βεβαιωθείτε ότι η δοκιμή είναι επιτυχής.  
![ODBC10](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\odbc10.png)
12. Το αρχείο ρύθμισης παραμέτρων ODBC θα πρέπει τώρα να είναι ορατή στο DSN αρχείου.  
![ODBC11](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\odbc11.png)

Έχουμε τώρα το αρχείο θα σας χρειαστεί και να ξεκινήσετε να δημιουργείτε τη γραμμή σύνδεσης.

## <a name="create-the-generic-sql-connector"></a>Δημιουργήστε τη γραμμή σύνδεσης γενικής χρήσης SQL

1. Το περιβάλλον εργασίας χρήστη Διαχείριση συγχρονισμού υπηρεσίας, επιλέξτε **γραμμές σύνδεσης** και **Δημιουργία**. Επιλέξτε **Γενικής χρήσης SQL (Microsoft)** και να της δώσετε ένα περιγραφικό όνομα.  
![Connector1](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\connector1.png)
2. Βρείτε το αρχείο DSN που δημιουργήσατε στην προηγούμενη ενότητα και στείλτε το στο διακομιστή. Δώστε τα διαπιστευτήρια για να συνδεθείτε με τη βάση δεδομένων.  
![Connector2](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\connector2.png)
3. Σε αυτές τις οδηγίες, διευκολύνοντας για μας και πούμε ότι υπάρχουν δύο τύποι αντικειμένων, **χρηστών** και **ομάδων**.
![Connector3](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\connector3.png)
4. Για να βρείτε τα χαρακτηριστικά, θέλουμε τη γραμμή σύνδεσης για τον εντοπισμό αυτά τα χαρακτηριστικά, εξετάζοντας στον ίδιο τον πίνακα. Καθώς **οι χρήστες** είναι μια δεσμευμένη λέξη στο SQL, πρέπει να παρέχουμε το σε αγκύλες [].  
![Connector4](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\connector4.png)
5. Χρόνος για να ορίσετε το χαρακτηριστικό αγκύρωσης και το χαρακτηριστικό DN. Για **τους χρήστες**, μπορούμε να χρησιμοποιήσουμε το συνδυασμό των δύο χαρακτηριστικά όνομα χρήστη και το EmployeeID. Για την **ομάδα**, χρησιμοποιούμε όνομα ομάδας (όχι ρεαλιστική σε πραγματικούς κύκλου ζωής, αλλά για αυτόν τον Οδηγό λειτουργεί).
![Connector5](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\connector5.png)
6. Δεν όλοι οι τύποι χαρακτηριστικό μπορεί να εντοπίζονται σε μια βάση δεδομένων SQL. Τον τύπο χαρακτηριστικό αναφοράς δεν είναι ιδιαίτερα. Για τον τύπο του αντικειμένου ομάδας, πρέπει να αλλάξετε το αναγνωριστικό κατόχου και MemberID για αναφορά.  
![Connector6](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\connector6.png)
7. Τα χαρακτηριστικά που θα σας επιλεγμένο ανάλογα με τον τύπο του αντικειμένου χαρακτηριστικά αναφορά στο προηγούμενο βήμα αυτές οι τιμές είναι μια αναφορά σε. Σε περίπτωση μας, πληκτρολογήστε το αντικείμενο χρήστη.  
![Connector7](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\connector7.png)
8. Στη σελίδα καθολικές παράμετροι, επιλέξτε **υδατογράφημα** ως τη στρατηγική delta. Πληκτρολογήστε επίσης τη μορφή ημερομηνίας/ώρας **εεεε-ηη HH:mm:ss**.
![Connector8](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\connector8.png)
9. Στη σελίδα **Ρύθμιση παραμέτρων διαμερίσματα και ιεραρχίες** , επιλέξτε και οι δύο τύποι αντικειμένων.
![Connector9](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\connector9.png)
10. Στην **Επιλογή τύποι αντικειμένων** και **Επιλέξτε χαρακτηριστικά**, επιλέξτε τύποι αντικειμένων και όλα τα χαρακτηριστικά. Στη σελίδα **Ρύθμιση παραμέτρων οι αγκυρώσεις** , κάντε κλικ στο κουμπί **Τέλος**.

## <a name="create-run-profiles"></a>Δημιουργία προφίλ εκτέλεση

1. Το περιβάλλον εργασίας χρήστη Διαχείριση συγχρονισμού υπηρεσίας, επιλέξτε **γραμμές σύνδεσης**και **Ρύθμιση παραμέτρων προφίλ εκτέλεση**. Κάντε κλικ στην επιλογή **νέο προφίλ**. Θα σας Ξεκινήστε με **Πλήρους εισαγωγής**.  
![Runprofile1](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\runprofile1.png)
2. Επιλέξτε τον τύπο **Πλήρους εισαγωγής (μόνο για το στάδιο)**.  
![Runprofile2](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\runprofile2.png)
3. Επιλέξτε τα διαμερίσματα **αντικείμενο = χρήστης**.  
![Runprofile3](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\runprofile3.png)
4. Επιλέξτε τον **πίνακα** και πληκτρολογήστε **[ΧΡΉΣΤΕΣ]**. Κάντε κύλιση προς τα κάτω στην ενότητα Τύπος αντικειμένου πολλαπλών τιμών και εισαγάγετε τα δεδομένα όπως στην παρακάτω εικόνα. Επιλέξτε **Τέλος** για να αποθηκεύσετε το βήμα.
![Runprofile4a](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\runprofile4a.png)  
![Runprofile4b](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\runprofile4b.png)  
5. Επιλέξτε **νέο βήμα**. Αυτήν τη στιγμή, επιλέξτε **αντικείμενο = ομάδα**. Στην τελευταία σελίδα, χρησιμοποιήστε τη ρύθμιση παραμέτρων όπως στην παρακάτω εικόνα. Κάντε κλικ στο κουμπί **Τέλος**.  
![Runprofile5a](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\runprofile5a.png)  
![Runprofile5b](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\runprofile5b.png)  
6. Προαιρετικό: Εάν θέλετε να, μπορείτε να ρυθμίσετε επιπλέον εκτέλεση προφίλ. Για αυτόν τον οδηγό, χρησιμοποιείται μόνο της πλήρους εισαγωγής.
7. Κάντε κλικ στο **κουμπί OK** για να ολοκληρώσετε την αλλαγή του εκτέλεση προφίλ.

## <a name="add-some-test-data-and-test-the-import"></a>Προσθήκη ορισμένα έλεγχος δεδομένων και να ελέγξετε την εισαγωγή
Συμπληρώστε ορισμένα δεδομένα δοκιμής το δείγμα βάσης δεδομένων. Όταν είστε έτοιμοι, επιλέξτε **Εκτέλεση** και **πλήρους εισαγωγής**.

Ακολουθεί ένας χρήστης με δύο αριθμούς τηλεφώνου και μια ομάδα με ορισμένα μέλη.  
![cs1](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\cs1.png)  
![CS2](.\media\active-directory-aadconnectsync-connector-genericsql-step-by-step\cs2.png)  

## <a name="appendix-a"></a>Παράρτημα A
**Δέσμη ενεργειών SQL για να δημιουργήσετε το δείγμα βάσης δεδομένων**

```SQL
---Creating the Database---------
Create Database GSQLDEMO
Go
-------Using the Database-----------
Use [GSQLDEMO]
Go
-------------------------------------
USE [GSQLDEMO]
GO
/****** Object:  Table [dbo].[GroupMembers]   ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [dbo].[GroupMembers](
    [MemberID] [int] NOT NULL,
    [Group_ID] [int] NOT NULL
) ON [PRIMARY]

GO
/****** Object:  Table [dbo].[GROUPS]   ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [dbo].[GROUPS](
    [GroupID] [int] NOT NULL,
    [GROUPNAME] [nvarchar](200) NOT NULL,
    [DESCRIPTION] [nvarchar](200) NULL,
    [WATERMARK] [datetime] NULL,
    [OwnerID] [int] NULL,
PRIMARY KEY CLUSTERED
(
    [GroupID] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]
) ON [PRIMARY]

GO
/****** Object:  Table [dbo].[USERPHONE]   ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
SET ANSI_PADDING ON
GO
CREATE TABLE [dbo].[USERPHONE](
    [USER_ID] [int] NULL,
    [Phone] [varchar](20) NULL
) ON [PRIMARY]

GO
SET ANSI_PADDING OFF
GO
/****** Object:  Table [dbo].[USERS]   ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [dbo].[USERS](
    [USERID] [int] NOT NULL,
    [USERNAME] [nvarchar](200) NOT NULL,
    [FirstName] [nvarchar](100) NULL,
    [LastName] [nvarchar](100) NULL,
    [DisplayName] [nvarchar](100) NULL,
    [ACCOUNTDISABLED] [bit] NULL,
    [EMPLOYEEID] [int] NOT NULL,
    [WATERMARK] [datetime] NULL,
PRIMARY KEY CLUSTERED
(
    [USERID] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]
) ON [PRIMARY]

GO
ALTER TABLE [dbo].[GroupMembers]  WITH CHECK ADD  CONSTRAINT [FK_GroupMembers_GROUPS] FOREIGN KEY([Group_ID])
REFERENCES [dbo].[GROUPS] ([GroupID])
GO
ALTER TABLE [dbo].[GroupMembers] CHECK CONSTRAINT [FK_GroupMembers_GROUPS]
GO
ALTER TABLE [dbo].[GroupMembers]  WITH CHECK ADD  CONSTRAINT [FK_GroupMembers_USERS] FOREIGN KEY([MemberID])
REFERENCES [dbo].[USERS] ([USERID])
GO
ALTER TABLE [dbo].[GroupMembers] CHECK CONSTRAINT [FK_GroupMembers_USERS]
GO
ALTER TABLE [dbo].[GROUPS]  WITH CHECK ADD  CONSTRAINT [FK_GROUPS_USERS] FOREIGN KEY([OwnerID])
REFERENCES [dbo].[USERS] ([USERID])
GO
ALTER TABLE [dbo].[GROUPS] CHECK CONSTRAINT [FK_GROUPS_USERS]
GO
ALTER TABLE [dbo].[USERPHONE]  WITH CHECK ADD  CONSTRAINT [FK_USERPHONE_USER] FOREIGN KEY([USER_ID])
REFERENCES [dbo].[USERS] ([USERID])
GO
ALTER TABLE [dbo].[USERPHONE] CHECK CONSTRAINT [FK_USERPHONE_USER]
GO
```
