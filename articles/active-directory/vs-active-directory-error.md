<properties 
    pageTitle="Σφάλμα κατά τον εντοπισμό ελέγχου ταυτότητας" 
    description="Ο Οδηγός σύνδεσης υπηρεσίας καταλόγου active directory εντόπισε έναν τύπο συμβατό έλεγχο ταυτότητας" 
    services="active-directory" 
    documentationCenter="" 
    authors="TomArcher" 
    manager="douge" 
    editor=""/>
  
<tags 
    ms.service="active-directory" 
    ms.workload="web" 
    ms.tgt_pltfrm="vs-getting-started" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="tarcher"/>

# <a name="error-during-authentication-detection"></a>Σφάλμα κατά τον εντοπισμό ελέγχου ταυτότητας

Κατά τον εντοπισμό προηγούμενο κωδικό ελέγχου ταυτότητας, ο οδηγός εντόπισε ένα τύπο συμβατό έλεγχο ταυτότητας.   

##<a name="what-is-being-checked"></a>Τι ελέγχεται;

**Σημείωση:** Για να εντοπίσει σωστά προηγούμενο κωδικό ελέγχου ταυτότητας σε ένα έργο, πρέπει να δημιουργηθούν του έργου.  Εάν συναντήσατε αυτό το σφάλμα και δεν έχετε ένα προηγούμενο κωδικό ελέγχου ταυτότητας στο έργο σας, δημιουργήστε ξανά και προσπαθήστε ξανά.

###<a name="project-types"></a>Τύπους έργων

Ο οδηγός ελέγχει τον τύπο του έργου αναπτύσσετε, ώστε το μπορούν να εισαχθούν της λογικής ελέγχου ταυτότητας δεξιά στο έργο.  Εάν υπάρχει οποιονδήποτε ελεγκτή που προέρχεται από `ApiController` στο έργο, το project θα ληφθούν υπόψη WebAPI έργου.  Εάν υπάρχουν μόνο ελεγκτές που προέρχεται από `MVC.Controller` στο έργο, το project θα ληφθούν υπόψη ένα έργο MVC.  Οτιδήποτε άλλο δεν υποστηρίζεται από τον οδηγό.  Έργα WebForms δεν υποστηρίζονται αυτήν τη στιγμή.

###<a name="compatible-authentication-code"></a>Κωδικός συμβατό έλεγχο ταυτότητας

Ο οδηγός ελέγχει επίσης για τις ρυθμίσεις ελέγχου ταυτότητας που έχουν ρυθμιστεί παλαιότερα με τον οδηγό ή είναι συμβατά με τον οδηγό.  Εάν όλες οι ρυθμίσεις, θεωρείται επανεισόδων υπόθεσης και ο οδηγός θα ανοίξετε και να εμφανίσετε τις ρυθμίσεις.  Εάν μόνο μερικές από τις ρυθμίσεις υπάρχουν, θεωρείται μια υπόθεση σφάλματος.

Σε ένα έργο MVC, ο οδηγός ελέγχει για οποιαδήποτε από τις ακόλουθες ρυθμίσεις, οι οποίες προκύπτουν από την προηγούμενη χρήση του οδηγού:

    <add key="ida:ClientId" value="" />
    <add key="ida:Tenant" value="" />
    <add key="ida:AADInstance" value="" />
    <add key="ida:PostLogoutRedirectUri" value="" />

Επιπλέον, ο οδηγός ελέγχει για οποιαδήποτε από τις παρακάτω ρυθμίσεις σε ένα έργο Web API, που έχει ως αποτέλεσμα από την προηγούμενη χρήση του οδηγού:

    <add key="ida:ClientId" value="" />
    <add key="ida:Tenant" value="" />
    <add key="ida:Audience" value="" />

###<a name="incompatible-authentication-code"></a>Κωδικός συμβατό έλεγχο ταυτότητας

Τέλος, ο οδηγός επιχειρεί να ανιχνεύσει τις εκδόσεις του κώδικα ελέγχου ταυτότητας που έχουν ρυθμιστεί με προηγούμενες εκδόσεις του Visual Studio. Εάν έχετε λάβει αυτό το σφάλμα, αυτό σημαίνει ότι το έργο σας περιέχει έναν τύπο συμβατό έλεγχο ταυτότητας. Ο οδηγός εντοπίζει τους ακόλουθους τύπους ελέγχου ταυτότητας από προηγούμενες εκδόσεις του Visual Studio:

* Έλεγχος ταυτότητας των Windows 
* Λογαριασμούς μεμονωμένων χρηστών 
* Εταιρικοί λογαριασμοί 
 

Για να εντοπίσετε ελέγχου ταυτότητας των Windows σε ένα έργο MVC, ο οδηγός αναζητά το `authentication` στοιχείου από το αρχείο **web.config** .

<pre>
    &lt;ρύθμιση παραμέτρων&gt;
        &lt;system.web&gt;
            <span style="background-color: yellow">&lt;λειτουργία ελέγχου ταυτότητας = "Των Windows" /&gt;</span>
        &lt;/system.web&gt;
    &lt;/configuration&gt;
</pre>

Για να εντοπίσετε ελέγχου ταυτότητας των Windows σε ένα έργο Web API, ο οδηγός αναζητά το `IISExpressWindowsAuthentication` στοιχείο από αρχείο **.csproj** του έργου σας:

<pre>
    &lt;Έργο&gt;
&lt;PropertyGroup&gt;
            <span style="background-color: yellow">&lt;IISExpressWindowsAuthentication&gt;με δυνατότητα&lt;/IISExpressWindowsAuthentication&gt;</span>
        &lt;/PropertyGroup > &lt;/του Project        &gt;
</pre>

Για να εντοπίσετε τον έλεγχο ταυτότητας μεμονωμένων λογαριασμών χρήστη, ο οδηγός αναζητά το στοιχείο πακέτου από το αρχείο **Packages.config** .

<pre>
    &lt;πακέτα&gt;
        <span style="background-color: yellow">&lt;συσκευάσετε id="Microsoft.AspNet.Identity.EntityFramework" έκδοση = "2.1.0" targetFramework = "net45" /&gt;</span>
    &lt;/πακέτων&gt;
</pre>

Για να εντοπίσετε μια παλιά φόρμα εταιρικός λογαριασμός ελέγχου ταυτότητας, ο οδηγός έχει για το ακόλουθο στοιχείο από **web.config**:

<pre>
    &lt;ρύθμιση παραμέτρων&gt;
        &lt;appSettings&gt;
            <span style="background-color: yellow">&lt;Προσθήκη κλειδιού = τιμή "ida: τομέα" = "***" /&gt;</span>
        &lt;/appSettings&gt;
    &lt;/configuration&gt;
</pre>

Για να αλλάξετε τον τύπο ελέγχου ταυτότητας, καταργήστε τον τύπο συμβατό έλεγχο ταυτότητας και εκτελέστε ξανά τον οδηγό.

Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [Σενάρια ελέγχου ταυτότητας για Azure AD](active-directory-authentication-scenarios.md).