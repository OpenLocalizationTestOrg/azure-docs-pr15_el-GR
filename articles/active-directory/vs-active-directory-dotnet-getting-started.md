<properties 
    pageTitle="Γρήγορα αποτελέσματα με το Azure Active Directory και το Visual Studio συνδεδεμένες υπηρεσίες (MVC έργα) | Microsoft Azure" 
    description="Πώς μπορείτε να ξεκινήσετε τη χρήση Azure Active Directory έργα MVC μετά τη σύνδεση ή τη δημιουργία ενός Azure AD χρήση του Visual Studio συνδεδεμένες υπηρεσίες" 
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

# <a name="getting-started-with-azure-active-directory-and-visual-studio-connected-services-mvc-projects"></a>Γρήγορα αποτελέσματα με το Azure Active Directory και του Visual Studio συνδεδεμένες υπηρεσίες (MVC έργα)

> [AZURE.SELECTOR]
> - [Γρήγορα αποτελέσματα](vs-active-directory-dotnet-getting-started.md)
> - [Τι έγινε](vs-active-directory-dotnet-what-happened.md)
 
##<a name="requiring-authentication-to-access-controllers"></a>Που απαιτεί έλεγχο ταυτότητας σε ελεγκτές πρόσβασης 

Όλοι οι ελεγκτές στο έργο σας έχουν adorned με το χαρακτηριστικό **εξουσιοδότηση** . Αυτό το χαρακτηριστικό θα απαιτούν από το χρήστη να γίνει έλεγχος ταυτότητας πριν από την πρόσβαση σε αυτούς τους ελεγκτές. Για να επιτρέψετε την ελεγκτή είναι η ανώνυμη πρόσβαση, καταργήστε αυτό το χαρακτηριστικό από τον ελεγκτή. Εάν θέλετε να ορίσετε τα δικαιώματα σε ένα πιο λεπτομερές επίπεδο, εφαρμόστε το χαρακτηριστικό σε κάθε μέθοδο που απαιτεί εξουσιοδότηση αντί για την εφαρμογή στην τάξη ελεγκτή.
 
##<a name="adding-signin--signout-controls"></a>Προσθήκη εισόδου / SignOut στοιχεία ελέγχου 

Για να προσθέσετε μια τα στοιχεία ελέγχου εισόδου/SignOut στην προβολή σας, μπορείτε να χρησιμοποιήσετε την προβολή μερική **_LoginPartial.cshtml** για να προσθέσετε τη λειτουργικότητα σε μία από τις προβολές. Ακολουθεί ένα παράδειγμα τις λειτουργίες που έχουν προστεθεί στην προβολή τυπικής **_Layout.cshtml** . (Σημειώστε το τελευταίο στοιχείο σε το div με σύμπτυξη στη γραμμή περιήγησης κλάσης):

<pre>
    &lt;! DOCTYPE html&gt; 
&lt;html&gt; 
&lt;head&gt; 
&lt;meta charset="utf-8" /&gt; 
&lt;meta name="viewport" content="width=device-width, initial-scale=1.0"&gt; 
&lt;title&gt;@ViewBag.Title - My ASP.NET Application&lt;/title&gt; 
@Styles.Render("~/Content/css") @Scripts.Render("~/bundles/modernizr") &lt;/head&gt; 
&lt;body&gt; 
&lt;div class="navbar navbar-inverse navbar-fixed-top"&gt; 
&lt;div class="container"&gt; 
&lt;div class="navbar-header"&gt; 
&lt;button type="button" class="navbar-toggle" data-toggle="collapse" data-target=".navbar-collapse"&gt; 
&lt;span class="icon-bar"&gt;&lt;/span&gt; 
&lt;span class="icon-bar"&gt;&lt;/span&gt; 
&lt;span class="icon-bar"&gt;&lt;/span&gt; 
&lt;/button&gt; 
@Html.ActionLink("Application name", "Index", "Home", new { area = "" }, new { @class = "navbar-brand" }) &lt;/div&gt; 
&lt;div class="navbar-collapse collapse"&gt; 
&lt;ul class="nav navbar-nav"&gt; 
&lt;li&gt;@Html.ActionLink("Home", "Index", "Home")&lt;/li&gt; 
&lt;li&gt;@Html.ActionLink("About", "About", "Home")&lt;/li&gt; 
&lt;li&gt;@Html.ActionLink("Contact", "Contact", "Home")&lt;/li&gt; 
&lt;/ul&gt; 
                    <span style="background-color:yellow">@Html.Partial("_LoginPartial")</span> 
                &lt;/ div&gt; 
&lt;/div&gt; 
&lt;/div&gt; 
&lt;div class="container body-content"&gt; 
@RenderBody() &lt;hr /&gt; 
&lt;footer&gt; 
&lt;p&gt;&amp;copy; @DateTime.Now.Year - My ASP.NET Application&lt;/p&gt; 
&lt;/footer&gt; 
&lt;/div&gt; 
@Scripts.Render("~/bundles/jquery") @Scripts.Render("~/bundles/bootstrap") @RenderSection("scripts", required: false) &lt;/body&gt; 
&lt;/html                                                                                                                                                                                                                                                                                                                                                                                                                                                           &gt;
</pre>

[Μάθετε περισσότερα σχετικά με το Azure Active Directory](https://azure.microsoft.com/services/active-directory/) 
