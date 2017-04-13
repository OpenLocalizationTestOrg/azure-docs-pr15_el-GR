<properties 
    pageTitle="Παρακολούθηση εξαρτήσεις, εξαιρέσεις και ώρες εκτέλεσης στις εφαρμογές web της Java" 
    description="Εκτεταμένη παρακολούθηση της τοποθεσίας Web Java με ιδέες εφαρμογής" 
    services="application-insights" 
    documentationCenter="java"
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/24/2016" 
    ms.author="awills"/>
 
# <a name="monitor-dependencies-exceptions-and-execution-times-in-java-web-apps"></a>Παρακολούθηση εξαρτήσεις, εξαιρέσεις και ώρες εκτέλεσης στις εφαρμογές web της Java

*Εφαρμογή ιδέες είναι σε προεπισκόπηση.*

Εάν έχετε [όργανα, εφαρμογή web της Java με εφαρμογή ιδέες][java], μπορείτε να χρησιμοποιήσετε τον παράγοντα Java για τη λήψη βαθύτερη ιδεών, χωρίς αλλαγές κώδικα:


* **Εξαρτήσεις:** Δεδομένα σχετικά με τις κλήσεις που κάνει την εφαρμογή σας σε άλλα στοιχεία, όπως:
 * **ΥΠΌΛΟΙΠΟ κλήσεις** που έγιναν μέσω HttpClient, OkHttp και RestTemplate (Ανοιξιάτικο).
 * **Redis** πραγματοποιημένες κλήσεις μέσω του προγράμματος-πελάτη Jedis. Εάν η κλήση διαρκεί περισσότερο από δεκάδες, τον παράγοντα λαμβάνει επίσης τα ορίσματα κλήσης.
 * **[Κλήσεις JDBC](http://docs.oracle.com/javase/7/docs/technotes/guides/jdbc/)** - MySQL, SQL Server, PostgreSQL, SQLite, Oracle DB ή Apache Derby DB. υποστηρίζονται οι κλήσεις "executeBatch". Για MySQL και PostgreSQL, εάν η κλήση διαρκεί περισσότερο από δεκάδες, τον παράγοντα αναφέρει το πρόγραμμα στο ερώτημα. 
* **Αποκλείονται εξαιρέσεις:** Δεδομένα σχετικά με τις εξαιρέσεις που αντιμετωπίζονται από τον κώδικά σας.
* **Χρόνος εκτέλεσης μέθοδο:** Δεδομένα σχετικά με το χρόνο που χρειάζεται να εκτελέσει συγκεκριμένες μεθόδους.

Για να χρησιμοποιήσετε τον παράγοντα Java, να εγκαταστήσετε στο διακομιστή σας. Τις εφαρμογές web πρέπει να έχει τοποθετηθεί αισθητήρας για την [Εφαρμογή ιδέες Java SDK][java].

## <a name="install-the-application-insights-agent-for-java"></a>Εγκαταστήστε την εφαρμογή ιδέες παράγοντας για Java

1. Στον υπολογιστή εκτελεί Java διακομιστή σας, [κάντε λήψη του παράγοντα](https://aka.ms/aijavasdk).
2. Επεξεργαστείτε τη δέσμη ενεργειών εφαρμογής διακομιστή εκκίνησης και προσθέστε το παρακάτω JVM:

    `javaagent:`*πλήρης διαδρομή προς το αρχείο ΒΆΖΟ παράγοντα*

    Για παράδειγμα, στο Tomcat σε υπολογιστή Linux:

    `export JAVA_OPTS="$JAVA_OPTS -javaagent:<full path to agent JAR file>"`


3. Επανεκκινήστε το διακομιστή εφαρμογών.

## <a name="configure-the-agent"></a>Ρύθμιση παραμέτρων τον παράγοντα

Δημιουργήστε ένα αρχείο με το όνομα `AI-Agent.xml` και να την τοποθετήσετε στον ίδιο φάκελο με το αρχείο ΒΆΖΟ παράγοντα.

Ορίστε το περιεχόμενο του αρχείου xml. Το παρακάτω παράδειγμα, για να συμπεριλάβετε ή να παραλείψετε τις δυνατότητες που θέλετε να επεξεργαστείτε. 

```XML

    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationInsightsAgent>
      <Instrumentation>
        
        <!-- Collect remote dependency data -->
        <BuiltIn enabled="true">
           <!-- Disable Redis or alter threshold call duration above which arguments are sent.
               Defaults: enabled, 10000 ms -->
           <Jedis enabled="true" thresholdInMS="1000"/>
           
           <!-- Set SQL query duration above which query plan is reported (MySQL, PostgreSQL). Default is 10000 ms. -->
           <MaxStatementQueryLimitInMS>1000</MaxStatementQueryLimitInMS>
        </BuiltIn>

        <!-- Collect data about caught exceptions 
             and method execution times -->

        <Class name="com.myCompany.MyClass">
           <Method name="methodOne" 
               reportCaughtExceptions="true"
               reportExecutionTime="true"
               />

           <!-- Report on the particular signature
                void methodTwo(String, int) -->
           <Method name="methodTwo"
              reportExecutionTime="true"
              signature="(Ljava/lang/String;I)V" />
        </Class>
        
      </Instrumentation>
    </ApplicationInsightsAgent>

```

Πρέπει να ενεργοποιήσετε την εξαίρεση αναφορών και του χρονισμού μέθοδο για μεμονωμένες μεθόδους.

Από προεπιλογή, `reportExecutionTime` είναι αληθής και `reportCaughtExceptions` είναι ψευδής.

## <a name="view-the-data"></a>Προβολή των δεδομένων

Στον πόρο εφαρμογής ιδέες, συγκεντρωτική απομακρυσμένο εξάρτηση μέθοδο εκτέλεσης ώρες και εμφανίζεται [κάτω από το πλακίδιο επιδόσεων][metrics]. 

Για να αναζητήσετε ξεχωριστές παρουσίες εξάρτηση, εξαίρεσης και μέθοδο αναφορές, Άνοιγμα [αναζήτησης][diagnostic]. 

[Διάγνωση προβλημάτων εξάρτησης - μάθετε περισσότερα](app-insights-dependencies.md#diagnosis).



## <a name="questions-problems"></a>Ερωτήσεις; Αντιμετωπίζετε προβλήματα;

* Δεν υπάρχουν δεδομένα; [Ορισμός εξαιρέσεων τείχους προστασίας](app-insights-ip-addresses.md)
* [Αντιμετώπιση προβλημάτων Java](app-insights-java-troubleshoot.md)



<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[apiexceptions]: app-insights-api-custom-events-metrics.md#track-exception
[availability]: app-insights-monitor-web-app-availability.md
[diagnostic]: app-insights-diagnostic-search.md
[eclipse]: app-insights-java-eclipse.md
[java]: app-insights-java-get-started.md
[javalogs]: app-insights-java-trace-logs.md
[metrics]: app-insights-metrics-explorer.md
[usage]: app-insights-web-track-usage.md

 
