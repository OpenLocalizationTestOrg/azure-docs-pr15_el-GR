<properties
    pageTitle="Δημιουργήστε μια εικόνα Azure RemoteApp | Microsoft Azure"
    description="Μάθετε περισσότερα σχετικά με τις διαθέσιμες επιλογές για τη δημιουργία εικόνων για Azure RemoteApp"
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



# <a name="create-an-azure-remoteapp-image"></a>Δημιουργήστε μια εικόνα Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp είναι που έχουν καταργηθεί. Διαβάστε την [ανακοίνωση](https://go.microsoft.com/fwlink/?linkid=821148) για λεπτομέρειες.

Azure RemoteApp χρησιμοποιούν εικόνες για τη διατήρηση των εφαρμογών που χρησιμοποιείτε από κοινού με τους χρήστες σας. (Θα σας λαμβάνουν την εικόνα σας και χρησιμοποιήστε το για να δημιουργήσετε ΣΠΣ - που του τι την πρόσβαση χρηστών κατά την είσοδο σε Azure RemoteApp.) Για να δημιουργήσετε μια συλλογή Azure RemoteApp με την επιλογή σας με τις εφαρμογές, είτε πρόκειται για cloud ή υβριδικό, μπορείτε να ξεκινήσετε με τη δημιουργία μιας εικόνας με αυτές τις εφαρμογές που έχουν εγκατασταθεί. Στη συνέχεια, δημιουργήστε μια συλλογή που χρησιμοποιεί αυτήν την εικόνα, εκχωρήσετε στους χρήστες για τη συλλογή και δημοσίευση εφαρμογών σε αυτούς τους χρήστες.

Έχετε πολλές επιλογές για τη δημιουργία ή τη χρήση εικόνων. Η βασική [απαίτηση](remoteapp-imagereqs.md) για μια εικόνα είναι ότι εκτέλεση Windows Server 2012 R2 και έχει εκχωρηθεί ο ρόλος Host απομακρυσμένη περίοδο λειτουργίας επιφάνειας εργασίας (RDSH) εγκατεστημένο. Τον τρόπο που λαμβάνετε είναι όπου ενδιαφέρουσα πράγματα.

Όταν θέλετε να εικόνες, έχετε τις εξής επιλογές:

- Μπορείτε να εισαγάγετε και να χρησιμοποιήσετε μια [εικόνα που βασίζεται σε μια εικονική μηχανή Azure](remoteapp-image-on-azurevm.md). Αυτό είναι καλό για εφαρμογές γραμμής εταιρικά που απαιτούν προσαρμοσμένες ρυθμίσεις. Μπορείτε να προσαρμόσετε την εικόνα ώστε να εργάζεται για την εφαρμογή.
- Μπορείτε να [δημιουργήσετε και να στείλετε μια προσαρμοσμένη εικόνα](remoteapp-create-custom-image.md). Αυτό είναι καλή, εάν έχετε ήδη μια εικόνα που χρησιμοποιείτε για την ανάπτυξη εσωτερικής εγκατάστασης Υπηρεσίες απομακρυσμένης επιφάνειας εργασίας.
- Μπορείτε να χρησιμοποιήσετε μία από τις [εικόνες των προτύπων](remoteapp-images.md) που περιλαμβάνονται στη συνδρομή σας RemoteApp. Αυτές οι εικόνες δημιουργούνται και διατηρούνται από την ομάδα RemoteApp και περιέχουν ορισμένες τυπικές εφαρμογές (όπως την οικογένεια προγραμμάτων του Office) που μπορείτε να κάνετε διαθέσιμο στους χρήστες σας. Σημειώστε ότι μπορεί να χρησιμοποιηθεί μόνο το Office 365 Pro Plus εικόνα σε μια ρύθμιση παραγωγής.

Ανεξάρτητα από όπου μπορείτε να λάβετε την εικόνα σας ή πώς να δημιουργήσετε, θα θέλετε να βεβαιωθείτε ότι μπορείτε να κατανοήσετε τις [απαιτήσεις εφαρμογής](remoteapp-appreqs.md) για να βεβαιωθείτε ότι η εφαρμογή σας λειτουργεί καλά στις RemoteApp. Στη συνέχεια, το επόμενο βήμα είναι να δημιουργήσετε μια συλλογή [cloud](remoteapp-create-cloud-deployment.md) ή [υβριδικό](remoteapp-create-hybrid-deployment.md) .
