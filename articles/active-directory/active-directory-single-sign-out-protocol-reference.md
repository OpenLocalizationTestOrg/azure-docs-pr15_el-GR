<properties
    pageTitle="Azure μία έξοδος πρωτόκολλο SAML | Microsoft Azure"
    description="Σε αυτό το άρθρο περιγράφει το μεμονωμένο πρωτόκολλο SAML Sign-Out στο Azure Active Directory"
    services="active-directory"
    documentationCenter=".net"
    authors="priyamohanram"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="priyamo"/>


# <a name="single-sign-out-saml-protocol"></a>Μία Sign-Out SAML πρωτοκόλλου

Azure Active Directory (Azure AD) υποστηρίζει το SAML 2.0 web περιήγησης μόνο sign-out προφίλ. Για μεμονωμένο sign-out για να λειτουργήσει σωστά, Azure AD πρέπει να καταχωρήσετε τη διεύθυνση URL μετα-δεδομένων κατά την εγγραφή εφαρμογών. Azure AD λαμβάνει τη διεύθυνση URL αποσυνδεθείτε και το κλειδί υπογραφής από την υπηρεσία cloud από τα μετα-δεδομένα. Azure AD χρησιμοποιεί τον αριθμό-κλειδί υπογραφής για να επιβεβαιώσετε την υπογραφή σε τα εισερχόμενα LogoutRequest και χρησιμοποιεί το LogoutURL για να ανακατευθύνετε τους χρήστες αφού πραγματοποιήσει έξοδο.

Εάν η υπηρεσία cloud δεν υποστηρίζει ένα τελικό σημείο μετα-δεδομένα, μετά την εφαρμογή έχει καταχωρηθεί, ο προγραμματιστής πρέπει να επικοινωνήσετε με υποστήριξης της Microsoft για την παροχή της διεύθυνσης URL αποσυνδεθείτε και κλειδί υπογραφής.

Σε αυτό το διάγραμμα δείχνει τη ροή εργασίας της διαδικασίας μία sign-out Azure AD.

![Μία έξοδος ροής εργασίας](media/active-directory-single-sign-out-protocol-reference/active-directory-saml-single-sign-out-workflow.png)

## <a name="logoutrequest"></a>LogoutRequest

Το στέλνει υπηρεσία cloud μια `LogoutRequest` μηνύματος σε Azure AD για να υποδείξει ότι έχει τερματιστεί μια περίοδο λειτουργίας. Το παρακάτω απόσπασμα εμφανίζει ένα δείγμα `LogoutRequest` στοιχείο.

```
<samlp:LogoutRequest xmlns="urn:oasis:names:tc:SAML:2.0:metadata" ID="idaa6ebe6839094fe4abc4ebd5281ec780" Version="2.0" IssueInstant="2013-03-28T07:10:49.6004822Z" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
  <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://www.workaad.com</Issuer>
  <NameID xmlns="urn:oasis:names:tc:SAML:2.0:assertion"> Uz2Pqz1X7pxe4XLWxV9KJQ+n59d573SepSAkuYKSde8=</NameID>
</samlp:LogoutRequest>
```

### <a name="logoutrequest"></a>LogoutRequest

Το `LogoutRequest` στοιχείο που αποστέλλονται σε Azure AD απαιτεί τα εξής χαρακτηριστικά:

- `ID`: Αυτό προσδιορίζει την αίτηση sign-out. Η τιμή του `ID` δεν θα πρέπει να ξεκινά με έναν αριθμό. Η τυπική πρακτική είναι να προσαρτήσετε **αναγνωριστικό** για την αναπαράσταση συμβολοσειράς GUID.

- `Version`: Ορίστε την τιμή αυτού του στοιχείου σε **2.0**. Αυτή η τιμή είναι απαραίτητο.

- `IssueInstant`: Αυτό είναι ένα `DateTime` συμβολοσειράς με μια τιμή συντονισμό παγκόσμια ώρα (UTC) και τη [μορφή ανταλλάξετε ("o")](https://msdn.microsoft.com/library/az4se3k1.aspx). Azure AD αναμένει τιμή αυτού του τύπου, αλλά δεν το επιβάλλουν.

- Το `Consent`, `Destination`, `NotOnOrAfter` και `Reason` χαρακτηριστικά λαμβάνονται υπόψη εάν έχουν εγγραφεί σε μια `LogoutRequest` στοιχείο.

### <a name="issuer"></a>Εκδότης

Το `Issuer` στοιχείο σε ένα `LogoutRequest` πρέπει να αντιστοιχεί ακριβώς ένα από τα **ServicePrincipalNames** στην υπηρεσία cloud στο Azure AD. Συνήθως, αυτό έχει οριστεί σε **URI Αναγνωριστικό εφαρμογής** που έχει καθοριστεί κατά την εγγραφή εφαρμογών.

### <a name="nameid"></a>NameID

Η τιμή του το `NameID` στοιχείο πρέπει να αντιστοιχεί ακριβώς το `NameID` του χρήστη που είναι γίνεται έξοδος.
## <a name="logoutresponse"></a>LogoutResponse

Azure AD στέλνει μια `LogoutResponse` απόκριση σε μια `LogoutRequest` στοιχείο. Το παρακάτω απόσπασμα εμφανίζει ένα δείγμα `LogoutResponse`.

```
<samlp:LogoutResponse ID="_f0961a83-d071-4be5-a18c-9ae7b22987a4" Version="2.0" IssueInstant="2013-03-18T08:49:24.405Z" InResponseTo="iddce91f96e56747b5ace6d2e2aa9d4f8c" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
  <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://sts.windows.net/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
  <samlp:Status>
    <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Success" />
  </samlp:Status>
</samlp:LogoutResponse>
```

### <a name="logoutresponse"></a>LogoutResponse

Azure AD σύνολα το `ID`, `Version` και `IssueInstant` τιμές σε το `LogoutResponse` στοιχείο. Ρυθμίζει επίσης το `InResponseTo` στοιχείο της τιμής του το `ID` χαρακτηριστικό το `LogoutRequest` που οποία δημιουργείται απάντηση.

### <a name="issuer"></a>Εκδότης

Azure AD ορίζει αυτήν την τιμή για να `https://login.microsoftonline.com/<TenantIdGUID>/` όπου <TenantIdGUID> είναι το Αναγνωριστικό μισθωτή του μισθωτή του Azure AD.

Για να υπολογιστεί η τιμή του το `Issuer` στοιχείο, χρησιμοποιήστε την τιμή του **URI Αναγνωριστικό εφαρμογής** που παρέχονται κατά την εγγραφή εφαρμογών.

### <a name="status"></a>Κατάσταση

Azure AD χρησιμοποιεί το `StatusCode` στοιχείο σε το `Status` στοιχείο για να υποδείξετε την επιτυχία ή την αποτυχία της sign-out. Όταν αποτύχει η προσπάθεια sign-out, το `StatusCode` στοιχείο μπορεί επίσης να περιέχει προσαρμοσμένα μηνύματα σφάλματος.
