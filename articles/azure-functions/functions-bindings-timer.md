<properties
    pageTitle="Azure έναυσμα χρονόμετρο συναρτήσεις | Microsoft Azure"
    description="Κατανόηση πώς μπορείτε να χρησιμοποιήσετε εναύσματα χρονόμετρο σε συναρτήσεις Azure."
    services="functions"
    documentationCenter="na"
    authors="christopheranderson"
    manager="erikre"
    editor=""
    tags=""
    keywords="Azure συναρτήσεις, συναρτήσεις, συμβάν επεξεργασία, δυναμική υπολογισμού, χωρίς αρχιτεκτονικής"/>

<tags
    ms.service="functions"
    ms.devlang="multiple"
    ms.topic="reference"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="08/22/2016"
    ms.author="chrande; glenga"/>

# <a name="azure-functions-timer-trigger"></a>Azure έναυσμα χρονόμετρο συναρτήσεις

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Σε αυτό το άρθρο εξηγεί τον τρόπο ρύθμισης εναύσματα χρονόμετρο σε συναρτήσεις Azure. Χρονόμετρο εκκινεί κλήση συναρτήσεων που βασίζεται σε ένα χρονοδιάγραμμα, μία φορά ή επαναλαμβανόμενα.  

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

## <a name="functionjson-for-timer-trigger"></a>Function.JSON για έναυσμα χρονομέτρησης

Το αρχείο *function.json* παρέχει μια παράσταση χρονοδιάγραμμα. Για παράδειγμα, το παρακάτω χρονοδιάγραμμα εκτελεί τη συνάρτηση κάθε λεπτό:

```json
{
  "bindings": [
    {
      "schedule": "0 * * * * *",
      "name": "myTimer",
      "type": "timerTrigger",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

Το χρονόμετρο έναυσμα χειρίζεται πολλών παρουσιών κλίμακα ανάληψης αυτόματα: μόνο μία παρουσία μιας συνάρτησης συγκεκριμένο χρονόμετρο πρόκειται να εκτελείται σε όλες τις εμφανίσεις.

## <a name="format-of-schedule-expression"></a>Μορφοποίηση της παράστασης χρονοδιάγραμμα

Η παράσταση χρονοδιάγραμμα είναι μια [παράσταση CRON](http://en.wikipedia.org/wiki/Cron#CRON_expression) που περιλαμβάνει πεδία 6: `{second} {minute} {hour} {day} {month} {day of the week}`. 

Σημειώστε ότι πολλές από τις παραστάσεις cron βρείτε online παραλειφθεί το όρισμα πεδίο {δεύτερο}, έτσι εάν αντιγράψετε από μία από αυτές θα πρέπει να προσαρμόσετε για το πεδίο επιπλέον. 

Ακολουθούν ορισμένες άλλες χρονοδιάγραμμα παραδείγματα παραστάσεων:

Για να ενεργοποιήσετε μία φορά κάθε 5 λεπτά:

```json
"schedule": "0 */5 * * * *"
```

Για να ενεργοποιήσετε μία φορά στο επάνω μέρος κάθε ώρα:

```json
"schedule": "0 0 * * * *",
```

Για να ενεργοποιήσετε μία φορά κάθε δύο ώρες:

```json
"schedule": "0 0 */2 * * *",
```

Για να ενεργοποιήσετε μία φορά κάθε ώρα από 9 ΠΜ σε 5 μ.μ.:

```json
"schedule": "0 0 9-17 * * *",
```

Για να ενεργοποιούν 9:30 πμ καθημερινά:

```json
"schedule": "0 30 9 * * *",
```

Για να ενεργοποιούν 9:30 πμ κάθε εργάσιμη ημέρα:

```json
"schedule": "0 30 9 * * 1-5",
```

## <a name="timer-trigger-c-code-example"></a>Παράδειγμα κώδικα χρονόμετρο έναυσμα C#

Το παράδειγμα αυτό κώδικα C# συντάσσει ένα μεμονωμένο αρχείο καταγραφής κάθε φορά που ενεργοποιείται η συνάρτηση.

```csharp
public static void Run(TimerInfo myTimer, TraceWriter log)
{
    log.Info($"C# Timer trigger function executed at: {DateTime.Now}");    
}
```

## <a name="next-steps"></a>Επόμενα βήματα

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)] 
