<properties
    pageTitle="Azure AD υπηρεσίας προς υπηρεσία Auth χρησιμοποιώντας OAuth2.0 | Microsoft Azure"
    description="Σε αυτό το άρθρο περιγράφει πώς μπορείτε να χρησιμοποιήσετε μηνύματα HTTP για την υλοποίηση της υπηρεσίας προς υπηρεσία ελέγχου ταυτότητας με χρήση της ροής εκχώρηση OAuth2.0 προγράμματος-πελάτη διαπιστευτήρια."
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

# <a name="service-to-service-calls-using-client-credentials"></a>Υπηρεσία για την υπηρεσία κλήσεων χρησιμοποιώντας τα διαπιστευτήρια προγράμματος-πελάτη

Το διακριτικό 2.0 προγράμματος-πελάτη διαπιστευτήρια εκχώρηση ροής επιτρέπει σε μια υπηρεσία web (μια *εμπιστευτικές προγράμματος-πελάτη*) για να χρησιμοποιήσετε το δικό της διαπιστευτήρια για τον έλεγχο ταυτότητας όταν καλείτε μια άλλη υπηρεσία web, αντί για απομίμηση ενός χρήστη. Σε αυτό το σενάριο, το πρόγραμμα-πελάτη είναι συνήθως μια υπηρεσία web μεσαίου επιπέδου, μια υπηρεσία μονού ή τοποθεσία web.

## <a name="client-credentials-grant-flow-diagram"></a>Τα διαπιστευτήρια προγράμματος-πελάτη εκχώρηση διαγράμματος ροής

Το παρακάτω διάγραμμα εξηγεί πώς τα διαπιστευτήρια προγράμματος-πελάτη εκχώρηση ροής λειτουργεί στο Azure Active Directory (Azure AD).

![OAuth2.0 προγράμματος-πελάτη διαπιστευτήρια εκχώρηση ροής](media/active-directory-protocols-oauth-service-to-service/active-directory-protocols-oauth-client-credentials-grant-flow.jpg)

1. Η εφαρμογή προγράμματος-πελάτη πραγματοποιεί έλεγχο ταυτότητας για το τελικό σημείο έκδοση κωδικού Azure AD και ζητά ένα διακριτικό πρόσβασης.
2. Το τελικό σημείο έκδοση κωδικού Azure AD θέματα το διακριτικό πρόσβασης.
3. Το διακριτικό πρόσβασης χρησιμοποιείται για τον έλεγχο ταυτότητας για την ασφαλή πόρων.
4. Δεδομένα από τον πόρο ασφαλή επιστρέφεται στην εφαρμογή web.

## <a name="register-the-services-in-azure-ad"></a>Καταχώρηση των υπηρεσιών στο Azure AD

Καταχώρηση τόσο την υπηρεσία κλήσης και τη λήψη της υπηρεσίας στο Azure Active Directory (Azure AD). Για λεπτομερείς οδηγίες, ανατρέξτε στο θέμα [Προσθήκη, ενημέρωση, και κατάργηση εφαρμογής](active-directory-integrating-applications.md#BKMK_Native)

## <a name="request-an-access-token"></a>Πρόσκληση σε ένα διακριτικό πρόσβασης

Για να ζητήσετε από ένα διακριτικό πρόσβασης, χρησιμοποιήστε μια ΔΗΜΟΣΊΕΥΣΗ HTTP για τη συγκεκριμένη μισθωτή Azure AD τελικού σημείου.

```
https://login.microsoftonline.com/<tenant id>/oauth2/token
```

## <a name="service-to-service-access-token-request"></a>Αίτηση πρόσβασης σε υπηρεσία διακριτικού

Μια αίτηση διακριτικού πρόσβασης υπηρεσίας εξυπηρέτησης περιέχει τις ακόλουθες παραμέτρους.

| Παράμετρος | | Περιγραφή |
|-----------|------|------------|
| response_type | απαιτείται | Καθορίζει τον τύπο ζητήθηκε απόκρισης. Σε μια ροή εκχώρηση διαπιστευτήρια προγράμματος-πελάτη, η τιμή πρέπει να **client_credentials**.|
| client_id | απαιτείται | Καθορίζει το αναγνωριστικό πελάτη Azure AD της κλήσης υπηρεσίας web. Για να βρείτε την εφαρμογή κλήσης Αναγνωριστικό υπολογιστή-πελάτη, στην πύλη διαχείρισης του Azure, κάντε κλικ στην επιλογή **Υπηρεσία καταλόγου Active Directory**, κάντε κλικ στον κατάλογο, κάντε κλικ στην εφαρμογή και, στη συνέχεια, κάντε κλικ στην επιλογή **Ρύθμιση παραμέτρων**.|
| client_secret | απαιτείται |  Πληκτρολογήστε έναν αριθμό-κλειδί καταχωρηθεί για την κλήση υπηρεσία web στο Azure AD. Για να δημιουργήσετε έναν αριθμό-κλειδί, στην πύλη διαχείρισης του Azure, κάντε κλικ στην επιλογή **Υπηρεσία καταλόγου Active Directory**, κάντε κλικ στον κατάλογο, κάντε κλικ στην εφαρμογή και, στη συνέχεια, κάντε κλικ στην επιλογή **Ρύθμιση παραμέτρων**. |
| πόρων | απαιτείται | Εισαγάγετε το URI Αναγνωριστικό εφαρμογής της υπηρεσίας web λήψη. Για να βρείτε το Αναγνωριστικό URI εφαρμογής, στην πύλη διαχείρισης του Azure, κάντε κλικ στην επιλογή **Υπηρεσία καταλόγου Active Directory**, κάντε κλικ στον κατάλογο, κάντε κλικ στην εφαρμογή και, στη συνέχεια, κάντε κλικ στην επιλογή **Ρύθμιση παραμέτρων**. |

## <a name="example"></a>Παράδειγμα

Στην παρακάτω ΚΑΤΑΧΏΡΗΣΗ HTTP ζητά ένα διακριτικό πρόσβασης για την υπηρεσία web https://service.contoso.com/. Το `client_id` προσδιορίζει την υπηρεσία web που ζητά το διακριτικό πρόσβασης.

```
POST contoso.com/oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=client_credentials&client_id=625bc9f6-3bf6-4b6d-94ba-e97cf07a22de&client_secret=qkDwDJlDfig2IpeuUZYKH1Wb8q1V0ju6sILxQQqhJ+s=&resource=https%3A%2F%2Fservice.contoso.com%2F
```

## <a name="service-to-service-access-token-response"></a>Πρόσβαση σε υπηρεσία διακριτικού απόκρισης

Απόκριση επιτυχίας περιέχει 2.0 διακριτικό JSON απόκριση με τις ακόλουθες παραμέτρους.

| Παράμετρος   | Περιγραφή |
|-------------|-------------|
|access_token |Το διακριτικό πρόσβασης ζητήθηκε. Κλήση υπηρεσίας web να χρησιμοποιήσετε αυτό το διακριτικό για τον έλεγχο ταυτότητας με την υπηρεσία web λήψη. |
|access_type  | Υποδεικνύει την τιμή διακριτικού τύπου. Ο μόνος τύπος που υποστηρίζει το Azure AD είναι **φορέα**. Για περισσότερες πληροφορίες σχετικά με τα διακριτικά φορέα, ανατρέξτε στο θέμα το [OAuth 2.0 εξουσιοδότησης Framework: χρήση διακριτικό φορέα (RFC 6750)](http://www.rfc-editor.org/rfc/rfc6750.txt).
|expires_in   | Πόσο χρόνο το διακριτικό πρόσβασης είναι έγκυρη (σε δευτερόλεπτα).|
|expires_on   |Η ώρα κατά την οποία λήγει το διακριτικό πρόσβασης. Η ημερομηνία αποδίδεται με τον αριθμό των δευτερολέπτων από 1970-01-01T0:0:0Z UTC μέχρι την ώρα λήξης. Αυτή η τιμή χρησιμοποιείται για τον καθορισμό της διάρκειας ζωής των διακριτικά στο cache. |
|πόρων     | Το Αναγνωριστικό εφαρμογής URI της υπηρεσίας web λήψη. |

## <a name="example"></a>Παράδειγμα

Το παρακάτω παράδειγμα εμφανίζει επιτυχίας απόκριση σε μια αίτηση για ένα διακριτικό πρόσβασης σε μια υπηρεσία web.

```
{
"access_token":"eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJodHRwczovL3NlcnZpY2UuY29udG9zby5jb20vIiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlLyIsImlhdCI6MTM4ODQ0ODI2NywibmJmIjoxMzg4NDQ4MjY3LCJleHAiOjEzODg0NTIxNjcsInZlciI6IjEuMCIsInRpZCI6IjdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZSIsIm9pZCI6ImE5OTE5MTYyLTkyMTctNDlkYS1hZTIyLWYxMTM3YzI1Y2RlYSIsInN1YiI6ImE5OTE5MTYyLTkyMTctNDlkYS1hZTIyLWYxMTM3YzI1Y2RlYSIsImlkcCI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZS8iLCJhcHBpZCI6ImQxN2QxNWJjLWM1NzYtNDFlNS05MjdmLWRiNWYzMGRkNThmMSIsImFwcGlkYWNyIjoiMSJ9.aqtfJ7G37CpKV901Vm9sGiQhde0WMg6luYJR4wuNR2ffaQsVPPpKirM5rbc6o5CmW1OtmaAIdwDcL6i9ZT9ooIIicSRrjCYMYWHX08ip-tj-uWUihGztI02xKdWiycItpWiHxapQm0a8Ti1CWRjJghORC1B1-fah_yWx6Cjuf4QE8xJcu-ZHX0pVZNPX22PHYV5Km-vPTq2HtIqdboKyZy3Y4y3geOrRIFElZYoqjqSv5q9Jgtj5ERsNQIjefpyxW3EwPtFqMcDm4ebiAEpoEWRN4QYOMxnC9OUBeG9oLA0lTfmhgHLAtvJogJcYFzwngTsVo6HznsvPWy7UP3MINA",
"token_type":"Bearer",
"expires_in":"3599",
"expires_on":"1388452167",
"resource":"https://service.contoso.com/"
}
```

## <a name="see-also"></a>Δείτε επίσης

* [Διακριτικό 2.0 στο Azure AD](active-directory-protocols-oauth-code.md)
