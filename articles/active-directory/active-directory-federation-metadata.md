<properties
    pageTitle="Μετα-δεδομένα του Azure AD Ομοσπονδία | Microsoft Azure"
    description="Σε αυτό το άρθρο περιγράφει το έγγραφο μετα-δεδομένων Ομοσπονδία που δημοσιεύει Azure Active Directory για τις υπηρεσίες που αποδέχονται τα διακριτικά Azure Active Directory."
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


# <a name="federation-metadata"></a>Ομοσπονδία μετα-δεδομένων

Azure Active Directory (Azure AD) δημοσιεύει εγγράφου Ομοσπονδία μετα-δεδομένων για τις υπηρεσίες που έχει ρυθμιστεί ώστε να αποδέχονται τα διακριτικά ασφαλείας που ζητήματα Azure AD. Η μορφή Ομοσπονδία μετα-δεδομένα του εγγράφου περιγράφεται στη [γλώσσα Ομοσπονδία υπηρεσιών Web (WS-Ομοσπονδία) έκδοση 1.2](http://docs.oasis-open.org/wsfed/federation/v1.2/os/ws-federation-1.2-spec-os.html), που εκτείνεται [μετα-δεδομένων για το v2.0 OASIS ασφαλείας διεκδίκηση Markup Language (SAML)](http://docs.oasis-open.org/security/saml/v2.0/saml-metadata-2.0-os.pdf).

## <a name="tenant-specific-and-tenant-independent-metadata-endpoints"></a>Συγκεκριμένο μισθωτή και μισθωτή ανεξάρτητα από τα τελικά σημεία μετα-δεδομένων

Azure AD δημοσιεύει συγκεκριμένο μισθωτή και μισθωτή ανεξάρτητα από τα τελικά σημεία.

Τα τελικά σημεία συγκεκριμένο μισθωτή έχουν σχεδιαστεί για ένα συγκεκριμένο μισθωτή. Τα μετα-δεδομένα Ομοσπονδία συγκεκριμένο μισθωτή περιλαμβάνει πληροφορίες σχετικά με το μισθωτή, συμπεριλαμβανομένων των μισθωτή εκδότη και τελικού σημείου πληροφορίες σχετικά με. Οι εφαρμογές που περιορισμού της πρόσβασης σε ένα μεμονωμένο μισθωτή χρησιμοποιούν τα τελικά σημεία αφορούν συγκεκριμένα το μισθωτή.

Τελικά σημεία μισθωτή ανεξάρτητο παρέχει πληροφορίες που είναι κοινά και για μισθωτές όλα Azure AD. Αυτές οι πληροφορίες ισχύει για μισθωτές φιλοξενείται στο *login.microsoftonline.com* και είναι κοινόχρηστα σε όλο το μισθωτές. Τα τελικά σημεία μισθωτή ανεξάρτητο συνιστώνται για εφαρμογές πολλών μισθωτών, επειδή δεν είναι συσχετισμένα με οποιοδήποτε συγκεκριμένο μισθωτή.

## <a name="federation-metadata-endpoints"></a>Ομοσπονδία τα τελικά σημεία μετα-δεδομένων

Azure AD δημοσιεύει Ομοσπονδία μετα-δεδομένων στο `https://login.microsoftonline.com/<TenantDomainName>/FederationMetadata/2007-06/FederationMetadata.xml`.

Για **τα τελικά σημεία αφορούν συγκεκριμένα το μισθωτή**, το `TenantDomainName` μπορεί να είναι ένας από τους παρακάτω τύπους:

- Ένα όνομα τομέα καταχωρημένες από μια Azure AD μισθωτή, όπως είναι οι: `contoso.onmicrosoft.com`.
- Το αμετάβλητες μισθωτή Αναγνωριστικό του τομέα, όπως είναι οι `72f988bf-86f1-41af-91ab-2d7cd011db45`.

Για **τα τελικά σημεία ανεξάρτητο μισθωτή**, η `TenantDomainName` είναι `common`. Αυτό το έγγραφο παραθέτει μόνο τα στοιχεία Ομοσπονδία μετα-δεδομένα που είναι κοινά και για όλα τα μισθωτές Azure AD που φιλοξενούνται στο login.microsoftonline.com.

Για παράδειγμα, μπορεί να είναι ένα τελικό σημείο συγκεκριμένο μισθωτή `https:// login.microsoftonline.com/contoso.onmicrosoft.com/FederationMetadata/2007-06/FederationMetadata.xml`. Το τελικό σημείο μισθωτή ανεξάρτητο είναι [https://login.microsoftonline.com/common/FederationMetadata/2007-06/FederationMetadata.xml](https://login.microsoftonline.com/common/FederationMetadata/2007-06/FederationMetadata.xml). Μπορείτε να προβάλετε το έγγραφο μετα-δεδομένων ομοσπονδίας, πληκτρολογώντας αυτήν τη διεύθυνση URL στο πρόγραμμα περιήγησης.

## <a name="contents-of-federation-metadata"></a>Περιεχόμενα του Ομοσπονδία μετα-δεδομένων

Η παρακάτω ενότητα παρέχει πληροφορίες που απαιτούνται από τις υπηρεσίες που εκμετάλλευση των διακριτικών που εκδίδονται από Azure AD.

### <a name="entity-id"></a>Αναγνωριστικό οντότητας

Το `EntityDescriptor` στοιχείο περιέχει ένα `EntityID` χαρακτηριστικό. Η τιμή του το `EntityID` χαρακτηριστικό αντιπροσωπεύει τον εκδότη, αυτό σημαίνει ότι η υπηρεσία διακριτικού ασφαλείας (STS) που εξέδωσε το διακριτικό. Είναι σημαντικό να επικυρώσετε τον εκδότη, όταν λαμβάνετε ένα διακριτικό.

Το παρακάτω μετα-δεδομένων εμφανίζει ένα δείγμα συγκεκριμένο μισθωτή `EntityDescriptor` στοιχείο με μια `EntityID` στοιχείο.

```
<EntityDescriptor
xmlns="urn:oasis:names:tc:SAML:2.0:metadata"
ID="_b827a749-cfcb-46b3-ab8b-9f6d14a1294b"
entityID="https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db45/">
```
Μπορείτε να αντικαταστήσετε το Αναγνωριστικό μισθωτή στο το τελικό σημείο μισθωτή ανεξάρτητο με το Αναγνωριστικό μισθωτή για να δημιουργήσετε ένα συγκεκριμένο μισθωτή `EntityID` τιμή. Η τιμή που προκύπτει θα είναι η ίδια με τον εκδότη διακριτικού. Η στρατηγική επιτρέπει σε μια εφαρμογή πολλών μισθωτή για να επικυρώσετε τον εκδότη για μια δεδομένη μισθωτή.

Το παρακάτω μετα-δεδομένων εμφανίζει ένα δείγμα μισθωτή ανεξάρτητο `EntityID` στοιχείο. Σημειώστε, ότι το `{tenant}` είναι μια καθορισμένη τιμή, όχι ένα σύμβολο κράτησης θέσης.

```
<EntityDescriptor
xmlns="urn:oasis:names:tc:SAML:2.0:metadata"
ID="="_0e5bd9d0-49ef-4258-bc15-21ce143b61bd"
entityID="https://sts.windows.net/{tenant}/">
```

### <a name="token-signing-certificates"></a>Πιστοποιητικά υπογραφής διακριτικού

Όταν μια υπηρεσία λαμβάνει ένα διακριτικό που έχει εκδοθεί από ένα μισθωτή Azure AD, πρέπει να η επικύρωση της υπογραφής του διακριτικού με ένα κλειδί υπογραφής που είναι δημοσιευμένο στο έγγραφο Ομοσπονδία μετα-δεδομένων. Τα μετα-δεδομένα Ομοσπονδία περιλαμβάνει το δημόσιο τμήμα του τα πιστοποιητικά που χρησιμοποιούν το μισθωτές της υπογραφής διακριτικού. Εμφανίζονται τα ανεπεξέργαστα byte πιστοποιητικό στο το `KeyDescriptor` στοιχείο. Το διακριτικό πιστοποιητικό υπογραφής δεν είναι έγκυροι για την είσοδο μόνο όταν η τιμή του το `use` χαρακτηριστικό είναι `signing`.

Ένα έγγραφο μετα-δεδομένων Ομοσπονδία δημοσιεύτηκε από Azure AD μπορεί να έχει πολλά υπογραφής κλειδιά, όπως όταν γίνεται προετοιμασία Azure AD για να ενημερώσετε το πιστοποιητικό υπογραφής. Όταν ένα έγγραφο μετα-δεδομένων Ομοσπονδία περιλαμβάνει περισσότερες από μία πιστοποιητικό, μια υπηρεσία που επικύρωση τα διακριτικά πρέπει να υποστηρίζουν όλα τα πιστοποιητικά στο έγγραφο.

Το παρακάτω μετα-δεδομένων εμφανίζει ένα δείγμα `KeyDescriptor` στοιχείο με έναν αριθμό-κλειδί υπογραφής.

```
<KeyDescriptor use="signing">
<KeyInfo xmlns="http://www.w3.org/2000/09/xmldsig#">
<X509Data>
<X509Certificate>
MIIDPjCCAiqgAwIBAgIQVWmXY/+9RqFA/OG9kFulHDAJBgUrDgMCHQUAMC0xKzApBgNVBAMTImFjY291bnRzLmFjY2Vzc2NvbnRyb2wud2luZG93cy5uZXQwHhcNMTIwNjA3MDcwMDAwWhcNMTQwNjA3MDcwMDAwWjAtMSswKQYDVQQDEyJhY2NvdW50cy5hY2Nlc3Njb250cm9sLndpbmRvd3MubmV0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEArCz8Sn3GGXmikH2MdTeGY1D711EORX/lVXpr+ecGgqfUWF8MPB07XkYuJ54DAuYT318+2XrzMjOtqkT94VkXmxv6dFGhG8YZ8vNMPd4tdj9c0lpvWQdqXtL1TlFRpD/P6UMEigfN0c9oWDg9U7Ilymgei0UXtf1gtcQbc5sSQU0S4vr9YJp2gLFIGK11Iqg4XSGdcI0QWLLkkC6cBukhVnd6BCYbLjTYy3fNs4DzNdemJlxGl8sLexFytBF6YApvSdus3nFXaMCtBGx16HzkK9ne3lobAwL2o79bP4imEGqg+ibvyNmbrwFGnQrBc1jTF9LyQX9q+louxVfHs6ZiVwIDAQABo2IwYDBeBgNVHQEEVzBVgBCxDDsLd8xkfOLKm4Q/SzjtoS8wLTErMCkGA1UEAxMiYWNjb3VudHMuYWNjZXNzY29udHJvbC53aW5kb3dzLm5ldIIQVWmXY/+9RqFA/OG9kFulHDAJBgUrDgMCHQUAA4IBAQAkJtxxm/ErgySlNk69+1odTMP8Oy6L0H17z7XGG3w4TqvTUSWaxD4hSFJ0e7mHLQLQD7oV/erACXwSZn2pMoZ89MBDjOMQA+e6QzGB7jmSzPTNmQgMLA8fWCfqPrz6zgH+1F1gNp8hJY57kfeVPBiyjuBmlTEBsBlzolY9dd/55qqfQk6cgSeCbHCy/RU/iep0+UsRMlSgPNNmqhj5gmN2AFVCN96zF694LwuPae5CeR2ZcVknexOWHYjFM0MgUSw0ubnGl0h9AJgGyhvNGcjQqu9vd1xkupFgaN+f7P3p3EVN5csBg5H94jEcQZT7EKeTiZ6bTrpDAnrr8tDCy8ng
</X509Certificate>
</X509Data>
</KeyInfo>
</KeyDescriptor>
  ```

Το `KeyDescriptor` στοιχείο εμφανίζεται σε δύο σημεία στο έγγραφο Ομοσπονδία μετα-δεδομένων. στην ενότητα WS Ομοσπονδία συγκεκριμένο και την ενότητα SAML συγκεκριμένο. Τα πιστοποιητικά που έχουν δημοσιευτεί σε δύο ενότητες θα είναι τα ίδια.

Στην ενότητα WS Ομοσπονδία συγκεκριμένο, ένα πρόγραμμα ανάγνωσης μετα-δεδομένων WS Ομοσπονδία θα διαβάσει τα πιστοποιητικά από μια `RoleDescriptor` στοιχείο με το `SecurityTokenServiceType` τύπου.

Το παρακάτω μετα-δεδομένων εμφανίζει ένα δείγμα `RoleDescriptor` στοιχείο.

```
<RoleDescriptor xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:fed="http://docs.oasis-open.org/wsfed/federation/200706" xsi:type="fed:SecurityTokenServiceType"protocolSupportEnumeration="http://docs.oasis-open.org/wsfed/federation/200706">
```

Στην ενότητα SAML συγκεκριμένο, ένα πρόγραμμα ανάγνωσης μετα-δεδομένων WS Ομοσπονδία θα διαβάσει τα πιστοποιητικά από μια `IDPSSODescriptor` στοιχείο.

Το παρακάτω μετα-δεδομένων εμφανίζει ένα δείγμα `IDPSSODescriptor` στοιχείο.

```
<IDPSSODescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol">
```
Δεν υπάρχουν διαφορές στη μορφή πιστοποιητικών που αφορούν συγκεκριμένα το μισθωτή και ανεξάρτητο μισθωτή.

### <a name="ws-federation-endpoint-url"></a>Διεύθυνση URL τελικού σημείου WS Ομοσπονδία

Τα μετα-δεδομένα Ομοσπονδία περιλαμβάνει τη διεύθυνση URL που χρησιμοποιεί Azure AD για μία μόνο είσοδο και μίας sign-out στο πρωτόκολλο WS ομοσπονδίας. Αυτό το τελικό σημείο εμφανίζεται στο το `PassiveRequestorEndpoint` στοιχείο.

Το παρακάτω μετα-δεδομένων εμφανίζει ένα δείγμα `PassiveRequestorEndpoint` στοιχείο για ένα συγκεκριμένο μισθωτή τελικό σημείο.

```
<fed:PassiveRequestorEndpoint>
<EndpointReference xmlns="http://www.w3.org/2005/08/addressing">
<Address>
https://login.microsoftonline.com/72f988bf-86f1-41af-91ab-2d7cd011db45/wsfed
</Address>
</EndpointReference>
</fed:PassiveRequestorEndpoint>
```
Για το τελικό σημείο ανεξάρτητο μισθωτή, τη διεύθυνση URL WS Ομοσπονδία εμφανίζεται στο το τελικό σημείο WS ομοσπονδίας, όπως φαίνεται στο ακόλουθο δείγμα.

```
<fed:PassiveRequestorEndpoint>
<EndpointReference xmlns="http://www.w3.org/2005/08/addressing">
<Address>
https://login.microsoftonline.com/common/wsfed
</Address>
</EndpointReference>
</fed:PassiveRequestorEndpoint>
```

### <a name="saml-protocol-endpoint-url"></a>Διεύθυνση URL τελικού σημείου πρωτόκολλο SAML

Τα μετα-δεδομένα Ομοσπονδία περιλαμβάνει τη διεύθυνση URL που χρησιμοποιεί το Azure AD για μία μόνο είσοδο και μίας sign-out στο πρωτόκολλο SAML 2.0. Αυτά τα τελικά σημεία εμφανίζονται στο το `IDPSSODescriptor` στοιχείο.

Διευθύνσεις URL εισόδου και sign-out του εμφανίζονται στο το `SingleSignOnService` και `SingleLogoutService` στοιχεία.

Το παρακάτω μετα-δεδομένων εμφανίζει ένα δείγμα `PassiveResistorEndpoint` για ένα συγκεκριμένο μισθωτή τελικό σημείο.

```
<IDPSSODescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol">
…
    <SingleLogoutService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/contoso.onmicrosoft.com/saml2" />
    <SingleSignOnService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/contoso.onmicrosoft.com /saml2" />
  </IDPSSODescriptor>
```

Ομοίως τα τελικά σημεία για τα κοινά τελικά σημεία πρωτόκολλο SAML 2.0 δημοσιεύονται σε τα μετα-δεδομένα Ομοσπονδία ανεξάρτητο μισθωτή, όπως φαίνεται στο ακόλουθο δείγμα.

```
<IDPSSODescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol">
…
    <SingleLogoutService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/common/saml2" />
    <SingleSignOnService Binding="urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect" Location="https://login.microsoftonline.com/common/saml2" />
  </IDPSSODescriptor>
```
