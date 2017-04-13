<properties
    pageTitle="Γρήγορα αποτελέσματα με το συνδέσεις υβριδική μεταγωγής | Microsoft Azure"
    description="Πώς να συντάξετε μια εφαρμογή κονσόλας κόμβου για τις συνδέσεις του υβριδικού"
    services="service-bus"
    documentationCenter="node"
    authors="jtaubensee"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.devlang="tbd"
    ms.topic="hero-article"
    ms.tgt_pltfrm="node"
    ms.workload="na"
    ms.date="10/28/2016"
    ms.author="jotaub"/>

# <a name="get-started-with-relay-hybrid-connections"></a>Γρήγορα αποτελέσματα με το συνδέσεις υβριδική μεταγωγής

[AZURE.INCLUDE [relay-selector-hybrid-connections](../../includes/relay-selector-hybrid-connections.md)]

## <a name="what-will-be-accomplished"></a>Τι θα πραγματοποιηθεί

Επειδή το υβριδικό συνδέσεις απαιτεί ένα πρόγραμμα-πελάτη και ένα στοιχείο διακομιστή, θα δημιουργήσουμε δύο κονσόλα εφαρμογών σε αυτό το πρόγραμμα εκμάθησης. Ακολουθούν τα βήματα:

1. Δημιουργήστε ένα χώρο ονομάτων αναμετάδοσης, με την πύλη Azure.

2. Δημιουργήστε μια σύνδεση υβριδική, με την πύλη Azure.

3. Συντάξτε ένα διακομιστή εφαρμογής κονσόλας να λαμβάνετε μηνύματα.

4. Συντάξτε ένα πρόγραμμα-πελάτη εφαρμογής κονσόλας για την αποστολή μηνυμάτων.

## <a name="prerequisites"></a>Προαπαιτούμενα στοιχεία

1. [Node.js](https://nodejs.org/en/) (αυτό το δείγμα χρησιμοποιεί 7.0 κόμβο).

2. Μια συνδρομή του Azure.

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="1-create-a-namespace-using-the-azure-portal"></a>1. Δημιουργήστε ένα χώρο ονομάτων με την πύλη Azure

Εάν έχετε ήδη ένα χώρο ονομάτων αναμετάδοσης δημιουργήσατε, μεταβείτε στην ενότητα [Δημιουργία υβριδικού σύνδεσης με την πύλη Azure](#2-create-a-hybrid-connection-using-the-azure-portal) .

[AZURE.INCLUDE [relay-create-namespace-portal](../../includes/relay-create-namespace-portal.md)]

## <a name="2-create-a-hybrid-connection-using-the-azure-portal"></a>2. Δημιουργία υβριδικού σύνδεσης με την πύλη Azure

Εάν έχετε ήδη μια σύνδεση υβριδική που δημιουργήσατε, μεταβείτε στην ενότητα [Δημιουργία εφαρμογής διακομιστή](#3-create-a-server-application-listener) .

[AZURE.INCLUDE [relay-create-hybrid-connection-portal](../../includes/relay-create-hybrid-connection-portal.md)]

## <a name="3-create-a-server-application-listener"></a>3. Δημιουργία εφαρμογής διακομιστή (ακρόασης)

Για να ακούσετε και λήψη μηνυμάτων από τη μετάδοση, θα σας θα Συντάξτε μια εφαρμογή κονσόλας Node.js.

[AZURE.INCLUDE [relay-hybrid-connections-dotnet-get-started-server](../../includes/relay-hybrid-connections-node-get-started-server.md)]

## <a name="4-create-a-client-application-sender"></a>4. δημιουργήσετε μια εφαρμογή προγράμματος-πελάτη (αποστολέα)

Για να στείλετε μηνύματα η μετάδοση, θα σας θα Συντάξτε μια εφαρμογή κονσόλας Node.js.

[AZURE.INCLUDE [relay-hybrid-connections-dotnet-get-started-client](../../includes/relay-hybrid-connections-node-get-started-client.md)]

## <a name="5-run-the-applications"></a>5. Εκτελέστε τις εφαρμογές

1. Εκτελέστε την εφαρμογή διακομιστή.

2. Εκτελέστε την εφαρμογή-πελάτη και πληκτρολογήστε κάποιο κείμενο.

3. Βεβαιωθείτε ότι η κονσόλα εφαρμογών διακομιστή εξάγει το κείμενο που έχει εισαχθεί στην εφαρμογή υπολογιστή-πελάτη.

![εκτέλεση εφαρμογών](./media/relay-hybrid-connections-node-get-started/running-applications.png)

Συγχαρητήρια, έχετε δημιουργήσει μια εφαρμογή του υβριδικού συνδέσεις σε ολοκληρωμένες.

## <a name="next-steps"></a>Επόμενα βήματα:

- [Μετάδοση συνήθεις Ερωτήσεις](relay-faq.md)
- [Δημιουργία ενός χώρου ονομάτων](relay-create-namespace-portal.md)
- [Γρήγορα αποτελέσματα με το .NET](relay-hybrid-connections-dotnet-get-started.md)
- [Γρήγορα αποτελέσματα με το κόμβου](relay-hybrid-connections-node-get-started.md)