<properties
   pageTitle="Πώς μπορώ να αποκτήσω AppSource πιστοποιηθεί για Azure Active Directory | Microsoft Azure"
   description="Λεπτομέρειες σχετικά με τον τρόπο για να λάβετε την εφαρμογή σας AppSource πιστοποιηθεί για Azure Active Directory."
   services="active-directory"
   documentationCenter=""
   authors="skwan"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="10/28/2016"
   ms.author="skwan;bryanla"/>

#<a name="how-to-get-appsource-certified-for-azure-active-directory-ad"></a>Πώς μπορώ να αποκτήσω AppSource πιστοποιηθεί για Azure Active Directory (AD) 

Για να λάβετε AppSource πιστοποίησης για Azure AD, η εφαρμογή σας πρέπει να υλοποιήσετε το σύμβολο πολλών μισθωτή στο μοτίβο με Azure AD χρησιμοποιώντας τα πρωτόκολλα OpenID σύνδεση, διακριτικό 2.0 ή SAML 2.0. 

Εάν δεν είστε εξοικειωμένοι με το Azure AD εισόδου ή ανάπτυξη της εφαρμογής πολλών μισθωτή:

1. Ξεκινήστε κάνοντας ανάγνωσης σχετικά με το πρόγραμμα [περιήγησης στο Web App σενάρια σε σενάρια ελέγχου ταυτότητας για το Azure AD][AAD-Auth-Scenarios-Browser-To-WebApp].  
2. Στη συνέχεια, δείτε το Azure AD [οδηγοί web εφαρμογής γρήγορης εκκίνησης][AAD-QuickStart-Web-Apps], που δείχνουν πώς μπορείτε να υλοποιήσετε εισόδου και περιλαμβάνει συνοδευτικό δείγματα κώδικα. 

    > [AZURE.TIP] Δοκιμάστε την προεπισκόπηση του μας νέα [πύλη για προγραμματιστές](https://identity.microsoft.com/Docs/Web) που θα σας βοηθήσει να ξεκινήσετε με το Azure Active Directory σε λίγα λεπτά!  Η πύλη για προγραμματιστές θα σας καθοδηγήσει της διαδικασίας την καταχώρηση εφαρμογής και ενοποίηση Azure AD μέσα στον κώδικα.  Όταν ολοκληρώσετε την εργασία, θα έχετε μια απλή εφαρμογή που μπορούν να ελέγχουν την ταυτότητα χρηστών στο μισθωτή σας και ένα παρασκηνίου που μπορούν να αποδέχονται τα διακριτικά και εκτελεί επικύρωση.

3. Για να μάθετε πώς μπορείτε να υλοποιήσετε το μισθωτή του πολλών εισόδου μοτίβο με Azure AD, ανατρέξτε στο θέμα [πώς μπορείτε να συνδεθείτε σε οποιονδήποτε χρήστη Azure Active Directory (AD) χρησιμοποιώντας το μοτίβο εφαρμογή πολλών μισθωτή][AAD-Howto-Multitenant-Overview]

## <a name="related-content"></a>Σχετικό περιεχόμενο
Για περισσότερες πληροφορίες σχετικά με τη δημιουργία εφαρμογών που υποστηρίζουν Azure AD-είσοδος, ή για να λάβετε Βοήθεια και υποστήριξη, ανατρέξτε στον [Οδηγό για προγραμματιστές του Azure AD][AAD-Dev-Guide].

Χρησιμοποιήστε την ενότητα σχόλια Disqus μετά από αυτό το άρθρο για να στείλετε τα σχόλιά σας και Βοηθήστε μας να περιορίσετε και να διαμορφώσετε μας περιεχόμενο.

<!--Reference style links -->
[AAD-Auth-Scenarios]: ./active-directory-authentication-scenarios.md
[AAD-Auth-Scenarios-Browser-To-WebApp]: ./active-directory-authentication-scenarios.md#web-browser-to-web-application
[AAD-Dev-Guide]: ./active-directory-developers-guide.md
[AAD-Howto-Multitenant-Overview]: ./active-directory-devhowto-multi-tenant-overview.md
[AAD-QuickStart-Web-Apps]: ./active-directory-developers-guide.md#web-application-quick-start-guides


<!--Image references-->










