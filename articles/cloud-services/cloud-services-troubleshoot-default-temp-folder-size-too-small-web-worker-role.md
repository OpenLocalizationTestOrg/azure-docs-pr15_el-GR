<properties
   pageTitle="Προεπιλεγμένο μέγεθος φακέλου TEMP είναι πολύ μικρό για ένα ρόλο | Microsoft Azure"
   description="Ένα ρόλο υπηρεσία cloud έχει ένα περιορισμένο χρονικό διάστημα για το φάκελο TEMP. Σε αυτό το άρθρο παρέχει ορισμένες προτάσεις σχετικά με τον τρόπο για να αποφύγετε Έξοδος από το χώρο."
   services="cloud-services"
   documentationCenter=""
   authors="simonxjx"
   manager="felixwu"
   editor=""
   tags="top-support-issue"/>
<tags
   ms.service="cloud-services"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="tbd"
   ms.date="10/12/2016"
   ms.author="v-six" />

# <a name="default-temp-folder-size-is-too-small-on-a-cloud-service-webworker-role"></a>Προεπιλεγμένο μέγεθος φακέλου TEMP είναι πολύ μικρό σε ρόλο cloud υπηρεσίας web/εργαζόμενου

Το προεπιλεγμένο προσωρινό κατάλογο ενός ρόλου εργαζόμενος ή web της υπηρεσίας cloud έχει μέγιστο μέγεθος 100 MB, το οποίο μπορεί να γίνει πλήρης σε κάποιο σημείο. Σε αυτό το άρθρο περιγράφει τον τρόπο για να αποφύγετε την έλλειψη χώρου για τον προσωρινό κατάλογο.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="why-do-i-run-out-of-space"></a>Γιατί ελεύθερο χώρο;

Η τυπική μεταβλητές περιβάλλοντος των Windows TEMP και TMP είναι διαθέσιμες σε κώδικα που εκτελείται στην εφαρμογή σας. TEMP και TMP οδηγεί σε ένα μεμονωμένο κατάλογο, που έχει μέγιστο μέγεθος των 100 MB. Οποιαδήποτε δεδομένα που είναι αποθηκευμένα σε αυτόν τον κατάλογο δεν είναι μόνιμα κατά μήκος του κύκλου ζωής της υπηρεσίας cloud. Εάν τοποθέτησή τις παρουσίες ρόλο σε μια υπηρεσία cloud, εκκαθάριση του καταλόγου.

## <a name="suggestion-to-fix-the-problem"></a>Πρόταση για να διορθώσετε το πρόβλημα

Υλοποίηση μία από τις παρακάτω εναλλακτικές:

- Ρύθμιση παραμέτρων ενός πόρου τοπικού χώρου αποθήκευσης και έχετε πρόσβαση σε αυτά απευθείας αντί για τη χρήση TEMP ή TMP. Για να αποκτήσετε πρόσβαση σε έναν πόρο τοπικού χώρου αποθήκευσης από κώδικα που εκτελείται στην εφαρμογή σας, καλέστε τη μέθοδο [RoleEnvironment.GetLocalResource](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleenvironment.getlocalresource.aspx) . 

- Ρύθμιση παραμέτρων ενός πόρου τοπικού χώρου αποθήκευσης και τοποθετήστε το δείκτη του TEMP και TMP καταλόγων για να υποδείξετε τη διαδρομή του πόρου τοπικού χώρου αποθήκευσης. Αυτή η τροποποίηση θα πρέπει να εκτελεστεί μέσα από τη μέθοδο [RoleEntryPoint.OnStart](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx) .

Το ακόλουθο παράδειγμα κώδικα δείχνει πώς μπορείτε να τροποποιήσετε το καταλόγους προορισμού για TEMP και TMP από μέσα στη μέθοδο OnStart:


```csharp
using System;
using Microsoft.WindowsAzure.ServiceRuntime;

namespace WorkerRole1
{
    public class WorkerRole : RoleEntryPoint
    {
        public override bool OnStart()
        {
            // The local resource declaration must have been added to the
            // service definition file for the role named WorkerRole1:
            //
            // <LocalResources>
            //    <LocalStorage name="CustomTempLocalStore"
            //                  cleanOnRoleRecycle="false"
            //                  sizeInMB="1024" />
            // </LocalResources>

            string customTempLocalResourcePath =
            RoleEnvironment.GetLocalResource("CustomTempLocalStore").RootPath;
            Environment.SetEnvironmentVariable("TMP", customTempLocalResourcePath);
            Environment.SetEnvironmentVariable("TEMP", customTempLocalResourcePath);

            // The rest of your startup code goes here…

            return base.OnStart();
        }
    }
}
```

## <a name="next-steps"></a>Επόμενα βήματα

Διαβάστε το ιστολόγιο που περιγράφει [τον τρόπο για να αυξήσετε το μέγεθος του Azure ρόλων ASP.NET προσωρινό φάκελο Web](http://blogs.msdn.com/b/kwill/archive/2011/07/18/how-to-increase-the-size-of-the-windows-azure-web-role-asp-net-temporary-folder.aspx).

Προβολή περισσότερων [Αντιμετώπιση προβλημάτων άρθρα](/?tag=top-support-issue&product=cloud-services) για τις υπηρεσίες cloud.

Για να μάθετε τον τρόπο αντιμετώπισης προβλημάτων ρόλο υπηρεσία cloud, χρησιμοποιώντας δεδομένα Διαγνωστικά υπολογιστή Azure PaaS, προβολή [σειρά ιστολογίων του Kevin Williamson](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).
