<properties
    pageTitle="Azure καθολικής σύνδεσης πρωτόκολλο SAML | Microsoft Azure"
    description="Σε αυτό το άρθρο περιγράφει το πρωτόκολλο μία μόνο είσοδο σε SAML στο Azure Active Directory"
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

# <a name="single-sign-on-saml-protocol"></a>Καθολική σύνδεση SAML πρωτοκόλλου

Σε αυτό το άρθρο καλύπτει το SAML 2.0 ελέγχου ταυτότητας των προσκλήσεων και απαντήσεων που υποστηρίζει το Azure Active Directory (Azure AD) για καθολικής σύνδεσης.

Στο παρακάτω διάγραμμα πρωτόκολλο περιγράφει τη μία μόνο σειρά καθολικής σύνδεσης. Η υπηρεσία cloud (υπηρεσία παροχής) χρησιμοποιεί μια σύνδεση HTTP ανακατεύθυνση για τη μεταβίβαση μιας `AuthnRequest` στοιχείου (αίτηση ελέγχου ταυτότητας) για Azure AD (η υπηρεσία παροχής ταυτότητας). Στη συνέχεια, το Azure AD χρησιμοποιεί μια δημοσίευση HTTP σύνδεσης για να δημοσιεύσετε μια `Response` στοιχείου για την υπηρεσία cloud.

![Καθολική σύνδεση ροής εργασίας](media/active-directory-single-sign-on-protocol-reference/active-directory-saml-single-sign-on-workflow.png)

## <a name="authnrequest"></a>AuthnRequest

Για να ζητήσετε τον έλεγχο ταυτότητας ενός χρήστη, cloud services αποστολή μιας `AuthnRequest` στοιχείου για Azure AD. Δείγμα SAML 2.0 `AuthnRequest` μπορεί να μοιάζει κάπως έτσι:

```
<samlp:AuthnRequest
xmlns="urn:oasis:names:tc:SAML:2.0:metadata"
ID="id6c1c178c166d486687be4aaf5e482730"
Version="2.0" IssueInstant="2013-03-18T03:28:54.1839884Z"
xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
<Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://www.contoso.com</Issuer>
</samlp:AuthnRequest>
```


| Παράμετρος | | Περιγραφή |
| ----------------------- | ------------------------------- | --------------- |
| ΑΝΑΓΝΩΡΙΣΤΙΚΌ | απαιτείται | Azure AD χρησιμοποιεί αυτό το χαρακτηριστικό για να συμπληρώσετε το `InResponseTo` χαρακτηριστικό της επιστρέφεται απόκρισης. Αναγνωριστικό δεν πρέπει να ξεκινούν με έναν αριθμό, επομένως μια κοινές στρατηγική είναι να ξεκινούν μια συμβολοσειρά, όπως το "αναγνωριστικό" για την αναπαράσταση συμβολοσειράς GUID. Για παράδειγμα, `id6c1c178c166d486687be4aaf5e482730` είναι έγκυρο αναγνωριστικό. |
| Έκδοση | απαιτείται | Αυτό πρέπει να **2.0**.|
| IssueInstant | απαιτείται | Αυτή είναι μια συμβολοσειρά ημερομηνίας/ώρας με μια τιμή UTC και [ανταλλάξετε μορφή ("o")](https://msdn.microsoft.com/library/az4se3k1.aspx). Azure AD αναμένει μια τιμή DateTime αυτού του τύπου, αλλά δεν είναι ή χρησιμοποιήστε την τιμή. |
| AssertionConsumerServiceUrl | προαιρετικό | Εάν παρέχεται, αυτό πρέπει να συμφωνεί με το `RedirectUri` της υπηρεσίας cloud στο Azure AD. |
| ForceAuthn | προαιρετικό | Εάν παρέχεται, αυτό θα πρέπει να είναι false. Οποιαδήποτε άλλη τιμή προκαλεί σφάλμα.|
| IsPassive | προαιρετικό | Εάν παρέχεται, αυτό θα πρέπει να είναι false. Οποιαδήποτε άλλη τιμή προκαλεί σφάλμα. |  

Όλα τα άλλα `AuthnRequest` χαρακτηριστικά, όπως συγκατάθεση, προορισμού, AssertionConsumerServiceIndex, AttributeConsumerServiceIndex και ProviderName έχουν **παραβλεφθεί**.

Azure AD επίσης παραβλέπει το `Conditions` στοιχείο σε `AuthnRequest`.

### <a name="issuer"></a>Εκδότης

Το `Issuer` στοιχείο σε ένα `AuthnRequest` πρέπει να αντιστοιχεί ακριβώς ένα από τα **ServicePrincipalNames** στην υπηρεσία cloud στο Azure AD. Συνήθως, αυτό έχει οριστεί σε **URI Αναγνωριστικό εφαρμογής** που καθορίζεται κατά τη δήλωση εφαρμογής.

Ένα δείγμα SAML απόσπασμα που περιέχει το `Issuer` στοιχείο μοιάζει κάπως έτσι:

```
<Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://www.contoso.com</Issuer>
```

### <a name="nameidpolicy"></a>NameIDPolicy

Αυτό το στοιχείο προσκλήσεις μια μορφή Αναγνωριστικό συγκεκριμένο όνομα στην απάντηση και είναι προαιρετικό στο `AuthnRequest` στοιχείων που αποστέλλονται σε Azure AD.

Δείγμα `NameIdPolicy` στοιχείο μοιάζει κάπως έτσι:

```
<NameIDPolicy Format="urn:oasis:names:tc:SAML:2.0:nameid-format:persistent"/>
```

Εάν `NameIDPolicy` παρέχεται, μπορείτε να συμπεριλάβετε την προαιρετική `Format` χαρακτηριστικό. Το `Format` χαρακτηριστικό μπορεί να έχει μόνο μία από τις παρακάτω τιμές. οποιαδήποτε άλλη τιμή ως αποτέλεσμα ένα σφάλμα.

-  `urn:oasis:names:tc:SAML:2.0:nameid-format:persistent`: Azure Active Directory θέματα το αίτημα NameID ως ζεύγους αναγνωριστικό.
- `urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress`: Azure Active Directory θέματα το αίτημα NameID σε μορφή διεύθυνσης ηλεκτρονικού ταχυδρομείου.
- `urn:oasis:names:tc:SAML:1.1:nameid-format:unspecified`: Αυτή η τιμή επιτρέπει Azure Active Directory για να επιλέξετε τη μορφή διεκδίκηση. Azure Active Directory θέματα το NameID ως ζεύγους αναγνωριστικό.

Μην συμπεριλαμβάνετε τη `SPNameQualifer` χαρακτηριστικό. Azure AD παραβλέπει το `AllowCreate` χαρακτηριστικό.

### <a name="requestauthncontext"></a>RequestAuthnContext

Το `RequestedAuthnContext` στοιχείο καθορίζει τις μεθόδους ελέγχου ταυτότητας που θέλετε. Αυτό είναι προαιρετικό στο `AuthnRequest` στοιχείων που αποστέλλονται σε Azure AD. Azure AD υποστηρίζει μόνο μία `AuthnContextClassRef` τιμή: `urn:oasis:names:tc:SAML:2.0:ac:classes:Password`.

### <a name="scoping"></a>Εμβέλεια

Το `Scoping` στοιχείου, το οποίο περιλαμβάνει μια λίστα με υπηρεσίες παροχής ταυτότητας, είναι προαιρετικό στο `AuthnRequest` στοιχείων που αποστέλλονται σε Azure AD.

Εάν παρέχεται, μην συμπεριλαμβάνετε τη `ProxyCount` χαρακτηριστικό, `IDPListOption` ή `RequesterID` στοιχείο, καθώς δεν υποστηρίζονται.

### <a name="signature"></a>Υπογραφή

Μην συμπεριλαμβάνετε ένα `Signature` στοιχείο σε `AuthnRequest` στοιχεία, όπως Azure AD δεν υποστηρίζει πραγματοποιήσει αιτήσεις ελέγχου ταυτότητας.

### <a name="subject"></a>Θέμα

Azure AD παραβλέπει το `Subject` στοιχείο `AuthnRequest` στοιχεία.

## <a name="response"></a>Απόκριση

Όταν μια ζητήθηκε καθολικής σύνδεσης ολοκληρωθεί με επιτυχία, Azure AD δημοσιεύσεις μια απάντηση με την υπηρεσία cloud. Δείγμα απόκριση σε μια επιτυχημένη προσπάθεια καθολικής σύνδεσης μοιάζει κάπως έτσι:

```
<samlp:Response ID="_a4958bfd-e107-4e67-b06d-0d85ade2e76a" Version="2.0" IssueInstant="2013-03-18T07:38:15.144Z" Destination="https://contoso.com/identity/inboundsso.aspx" InResponseTo="id758d0ef385634593a77bdf7e632984b6" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
  <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion"> https://login.microsoftonline.com/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
  <ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
    ...
  </ds:Signature>
  <samlp:Status>
    <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Success" />
  </samlp:Status>
  <Assertion ID="_bf9c623d-cc20-407a-9a59-c2d0aee84d12" IssueInstant="2013-03-18T07:38:15.144Z" Version="2.0" xmlns="urn:oasis:names:tc:SAML:2.0:assertion">
    <Issuer>https://login.microsoftonline.com/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
    <ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
      ...
    </ds:Signature>
    <Subject>
      <NameID>Uz2Pqz1X7pxe4XLWxV9KJQ+n59d573SepSAkuYKSde8=</NameID>
      <SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer">
        <SubjectConfirmationData InResponseTo="id758d0ef385634593a77bdf7e632984b6" NotOnOrAfter="2013-03-18T07:43:15.144Z" Recipient="https://contoso.com/identity/inboundsso.aspx" />
      </SubjectConfirmation>
    </Subject>
    <Conditions NotBefore="2013-03-18T07:38:15.128Z" NotOnOrAfter="2013-03-18T08:48:15.128Z">
      <AudienceRestriction>
        <Audience>https://www.contoso.com</Audience>
      </AudienceRestriction>
    </Conditions>
    <AttributeStatement>
      <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name">
        <AttributeValue>testuser@contoso.com</AttributeValue>
      </Attribute>
      <Attribute Name="http://schemas.microsoft.com/identity/claims/objectidentifier">
        <AttributeValue>3F2504E0-4F89-11D3-9A0C-0305E82C3301</AttributeValue>
      </Attribute>
      ...
    </AttributeStatement>
    <AuthnStatement AuthnInstant="2013-03-18T07:33:56.000Z" SessionIndex="_bf9c623d-cc20-407a-9a59-c2d0aee84d12">
      <AuthnContext>
        <AuthnContextClassRef> urn:oasis:names:tc:SAML:2.0:ac:classes:Password</AuthnContextClassRef>
      </AuthnContext>
    </AuthnStatement>
  </Assertion>
</samlp:Response>
```

### <a name="response"></a>Απόκριση

Το `Response` στοιχείο περιλαμβάνει το αποτέλεσμα της αίτησης εξουσιοδότησης. Azure AD σύνολα το `ID`, `Version` και `IssueInstant` τιμές σε το `Response` στοιχείο. Επίσης, ορίζει τα εξής χαρακτηριστικά:

- `Destination`: Όταν η σύνδεση ολοκληρωθεί με επιτυχία, είναι ενεργοποιημένη η επιλογή το `RedirectUri` της υπηρεσίας παροχής (υπηρεσία cloud).
- `InResponseTo`: Αυτή έχει οριστεί σε το `ID` χαρακτηριστικό το `AuthnRequest` στοιχείο που ξεκίνησε την απάντηση.

### <a name="issuer"></a>Εκδότης

Azure AD σύνολα το `Issuer` στοιχείου για `https://login.microsoftonline.com/<TenantIDGUID>/` όπου <TenantIDGUID> είναι το Αναγνωριστικό μισθωτή του μισθωτή του Azure AD.

Για παράδειγμα, μια απάντηση δείγμα με στοιχείο εκδότη μπορεί να μοιάζει κάπως έτσι:

```
<Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion"> https://login.microsoftonline.com/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
```

### <a name="status"></a>Κατάσταση

Το `Status` μεταφέρει στοιχείο την επιτυχία ή την αποτυχία της καθολικής σύνδεσης. Περιλαμβάνει το `StatusCode` στοιχείου, το οποίο περιέχει έναν κωδικό ή ένα σύνολο ένθετη κωδικών που αντιπροσωπεύει την κατάσταση της αίτησης. Περιλαμβάνει επίσης την `StatusMessage` στοιχείου, το οποίο περιέχει προσαρμοσμένα μηνύματα σφάλματος που δημιουργούνται κατά τη διαδικασία σύνδεσης.

<!-- TODO: Add a authentication protocol error reference -->

Ακολουθεί SAML απόκριση σε μια ανεπιτυχής προσπάθεια καθολικής σύνδεσης.

```
<samlp:Response ID="_f0961a83-d071-4be5-a18c-9ae7b22987a4" Version="2.0" IssueInstant="2013-03-18T08:49:24.405Z" InResponseTo="iddce91f96e56747b5ace6d2e2aa9d4f8c" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
  <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://sts.windows.net/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
  <samlp:Status>
    <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Requester">
      <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:RequestUnsupported" />
    </samlp:StatusCode>
    <samlp:StatusMessage>AADSTS75006: An error occurred while processing a SAML2 Authentication request. AADSTS90011: The SAML authentication request property 'NameIdentifierPolicy/SPNameQualifier' is not supported.
Trace ID: 66febed4-e737-49ff-ac23-464ba090d57c
Timestamp: 2013-03-18 08:49:24Z</samlp:StatusMessage>
  </samlp:Status>
```

### <a name="assertion"></a>Διεκδίκηση

Μαζί με το `ID`, `IssueInstant` και `Version`, Azure AD ορίζει τα εξής στοιχεία σε το `Assertion` στοιχείο της απόκρισης.

#### <a name="issuer"></a>Εκδότης

Είναι ενεργοποιημένη η επιλογή `https://sts.windows.net/<TenantIDGUID>/`όπου <TenantIDGUID> είναι το Αναγνωριστικό μισθωτή του μισθωτή του Azure AD.

```
<Issuer>https://login.microsoftonline.com/82869000-6ad1-48f0-8171-272ed18796e9/</Issuer>
```

#### <a name="signature"></a>Υπογραφή

Azure AD υπογράφει τη διεκδίκηση απόκριση σε μια επιτυχημένη καθολικής σύνδεσης. Το `Signature` στοιχείο περιέχει μια ψηφιακή υπογραφή που μπορεί να χρησιμοποιήσει την υπηρεσία cloud για τον έλεγχο ταυτότητας της προέλευσης για να επιβεβαιώσετε την ακεραιότητα των τη διεκδίκηση.

Για να δημιουργήσετε αυτή η ψηφιακή υπογραφή, Azure AD χρησιμοποιεί το κλειδί υπογραφής του `IDPSSODescriptor` στοιχείο τα μετα-δεδομένα του εγγράφου.

```
<ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
      digital_signature_here
    </ds:Signature>
```

#### <a name="subject"></a>Θέμα

Καθορίζει το κεφάλαιο που είναι το υποκείμενο από τις προτάσεις σε τη διεκδίκηση. Περιέχει ένα `NameID` στοιχείου, το οποίο αντιπροσωπεύει το χρήστη με έλεγχο ταυτότητας. Το `NameID` τιμή είναι ένα αναγνωριστικό προορισμού που απευθύνεται μόνο για την υπηρεσία παροχής που είναι το κοινό για το διακριτικό. Είναι μόνιμη - μπορεί να ανακληθεί, αλλά είναι ποτέ εκ νέου. Επίσης, είναι αδιαφανές, επειδή δεν θα εμφανίζονται όλα τα στοιχεία σχετικά με το χρήστη και δεν μπορεί να χρησιμοποιηθεί ως αναγνωριστικό για ερωτήματα χαρακτηριστικό.

Το `Method` χαρακτηριστικό το `SubjectConfirmation` στοιχείο πάντα την τιμή `urn:oasis:names:tc:SAML:2.0:cm:bearer`.

```
<Subject>
      <NameID>Uz2Pqz1X7pxe4XLWxV9KJQ+n59d573SepSAkuYKSde8=</NameID>
      <SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer">
        <SubjectConfirmationData InResponseTo="id758d0ef385634593a77bdf7e632984b6" NotOnOrAfter="2013-03-18T07:43:15.144Z" Recipient="https://contoso.com/identity/inboundsso.aspx" />
      </SubjectConfirmation>
</Subject>
```

#### <a name="conditions"></a>Συνθήκες

Αυτό το στοιχείο καθορίζει τις συνθήκες που ορίζουν τη χρήση αποδεκτή διεκδικήσεων SAML.

```
<Conditions NotBefore="2013-03-18T07:38:15.128Z" NotOnOrAfter="2013-03-18T08:48:15.128Z">
      <AudienceRestriction>
        <Audience>https://www.contoso.com</Audience>
      </AudienceRestriction>
</Conditions>
```

Το `NotBefore` και `NotOnOrAfter` χαρακτηριστικά, καθορίστε το χρονικό διάστημα κατά το οποίο είναι έγκυρη τη διεκδίκηση.

- Η τιμή του το `NotBefore` χαρακτηριστικό είναι ίση προς ή ελαφρώς (λιγότερο από ένα δευτερόλεπτο) αργότερα από την τιμή του `IssueInstant` χαρακτηριστικό το `Assertion` στοιχείο. Azure AD δεν λογαριασμού για οποιαδήποτε χρονική διαφορά ανάμεσα στον ίδιο και την υπηρεσία cloud (υπηρεσία παροχής) και δεν προσθέτει οποιαδήποτε buffer αυτήν τη στιγμή.
- Η τιμή του το `NotOnOrAfter` χαρακτηριστικό είναι αργότερα από την τιμή του 70 λεπτά το `NotBefore` χαρακτηριστικό.

#### <a name="audience"></a>Ακροατηρίου

Περιέχει ένα URI που προσδιορίζει ένα ακροατήριο με το οποίο προορίζεται. Azure AD ορίζει την τιμή του αυτό το στοιχείο της τιμής του `Issuer` στοιχείου από το `AuthnRequest` που ξεκίνησε τη καθολικής σύνδεσης. Για να αξιολογήσετε το `Audience` τιμή, χρησιμοποιήστε την τιμή του το `App ID URI` που έχει καθοριστεί κατά την εγγραφή εφαρμογών.

```
<AudienceRestriction>
        <Audience>https://www.contoso.com</Audience>
</AudienceRestriction>
```

Όπως η `Issuer` τιμή, η `Audience` τιμή πρέπει να αντιστοιχεί ακριβώς ένα από τα κύρια ονόματα υπηρεσίας που αντιπροσωπεύει την υπηρεσία cloud στο Azure AD. Ωστόσο, εάν η τιμή του το `Issuer` στοιχείο δεν είναι μια τιμή URI, το `Audience` τιμή στην απάντηση είναι το `Issuer` τιμή με το πρόθεμα `spn:`.

#### <a name="attributestatement"></a>AttributeStatement

Περιέχει αξιώσεων σχετικά με το θέμα ή το χρήστη. Το παρακάτω απόσπασμα περιέχει ένα δείγμα `AttributeStatement` στοιχείο. Τα αποσιωπητικά υποδεικνύει ότι το στοιχείο μπορούν να περιλαμβάνουν πολλές χαρακτηριστικά και τιμές χαρακτηριστικών.

```
<AttributeStatement>
      <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name">
        <AttributeValue>testuser@contoso.com</AttributeValue>
      </Attribute>
      <Attribute Name="http://schemas.microsoft.com/identity/claims/objectidentifier">
        <AttributeValue>3F2504E0-4F89-11D3-9A0C-0305E82C3301</AttributeValue>
      </Attribute>
      ...
</AttributeStatement>
```     

- **Διεκδίκηση όνομα** : Η τιμή του το `Name` χαρακτηριστικό (`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`) είναι το κύριο όνομα χρήστη με έλεγχο ταυτότητας χρήστη, όπως `testuser@managedtenant.com`.
- **Διεκδίκηση ObjectIdentifier** : Η τιμή του το `ObjectIdentifier` χαρακτηριστικό (`http://schemas.microsoft.com/identity/claims/objectidentifier`) είναι το `ObjectId` του αντικειμένου καταλόγου που αντιπροσωπεύει το χρήστη με έλεγχο ταυτότητας στο Azure AD. `ObjectId`είναι ένα αμετάβλητες, καθολικά μοναδικό και εκ νέου χρήση ασφαλών αναγνωριστικό του χρήστη με έλεγχο ταυτότητας.

#### <a name="authnstatement"></a>AuthnStatement

Αυτό το στοιχείο διεκδικήσεις ότι το θέμα διεκδίκηση έχει ελεγχθεί η ταυτότητά με μια συγκεκριμένη μέσα σε μια συγκεκριμένη χρονική στιγμή.

- Το `AuthnInstant` χαρακτηριστικό καθορίζει την ώρα στην οποία έλεγχος ταυτότητας του χρήστη με Azure AD.
- Το `AuthnContext` στοιχείο Καθορίζει το περιβάλλον ελέγχου ταυτότητας που χρησιμοποιείται για τον έλεγχο ταυτότητας χρήστη.

```
<AuthnStatement AuthnInstant="2013-03-18T07:33:56.000Z" SessionIndex="_bf9c623d-cc20-407a-9a59-c2d0aee84d12">
      <AuthnContext>
        <AuthnContextClassRef> urn:oasis:names:tc:SAML:2.0:ac:classes:Password</AuthnContextClassRef>
      </AuthnContext>
</AuthnStatement>
```
