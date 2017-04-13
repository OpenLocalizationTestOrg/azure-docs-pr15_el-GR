<properties
   pageTitle="Γρήγορα αποτελέσματα με το δείγμα"
   description="Power BI ενσωματωμένο, χρησιμοποιήστε το SDK για να προσθέσετε αλληλεπιδραστικές αναφορές του Power BI στην εφαρμογή σας επιχειρηματικής ευφυΐας"
   services="power-bi-embedded"
   documentationCenter=""
   authors="guyinacube"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="power-bi-embedded"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="powerbi"
   ms.date="10/04/2016"
   ms.author="asaxton"/>

# <a name="get-started-with-power-bi-embedded-sample"></a>Γρήγορα αποτελέσματα με το Power BI ενσωματωμένο δείγμα

Με το **Microsoft Power BI ενσωματωμένο**, μπορείτε να ενοποιήσετε αναφορές του Power BI απευθείας στο web ή εφαρμογές για κινητές συσκευές σας. Σε αυτό το άρθρο θα σας θα εξοικειωθείτε με το **Power BI ενσωματωμένο** δείγμα αποτελέσματα για λήψη.

Πριν από την προχωράμε οποιαδήποτε περαιτέρω, που πιθανότατα θα θέλετε να αποθηκεύσετε στους παρακάτω πόρους. Αυτές θα σας βοηθήσει να όταν ενοποίηση πολύ αναφορές του Power BI σε το δείγμα και τις δικές σας εφαρμογές.

 -  [Δείγμα πίνακα εργαλείων web app](http://go.microsoft.com/fwlink/?LinkId=761493)
 -  [Power BI ενσωματωμένου API αναφοράς](https://msdn.microsoft.com/library/mt711493.aspx)
 -  [Power BI ενσωματωμένο .NET SDK](http://go.microsoft.com/fwlink/?LinkId=746472) (διαθέσιμο μέσω NuGet)



> [AZURE.NOTE] Πριν να ρυθμίσετε τις παραμέτρους και να εκτελέσετε το Power BI ενσωματωμένο δείγμα αποτελέσματα λήψη, πρέπει να δημιουργήσετε τουλάχιστον μία **Συλλογή χώρου εργασίας** στο Azure τη συνδρομή σας. Για να μάθετε πώς μπορείτε να δημιουργήσετε μια **Συλλογή χώρου εργασίας** στην πύλη του Azure ανατρέξτε στο θέμα [Γρήγορα αποτελέσματα με το Power BI ενσωματωμένο](power-bi-embedded-get-started.md).

## <a name="configure-the-sample-app"></a>Ρύθμιση παραμέτρων της εφαρμογής δείγματος

Ας δούμε τη ρύθμιση του περιβάλλοντος ανάπτυξης του Visual Studio για να αποκτήσετε πρόσβαση τα στοιχεία που χρειάζονται για την εκτέλεση της εφαρμογής δείγμα.

1. Κάντε λήψη και αποσυμπιέστε το [Power BI ενσωματωμένου - ενοποίηση μια αναφορά σε μια εφαρμογή web](http://go.microsoft.com/fwlink/?LinkId=761493) δείγμα στην GitHub.

2. Άνοιγμα **PowerBI embedded.sln** στο Visual Studio. Ίσως χρειαστεί να εκτελέσει την εντολή **Πακέτου ενημέρωσης** στην κονσόλα διαχείρισης πακέτου NuGET προκειμένου να ενημερώσετε τα πακέτα που χρησιμοποιούνται σε αυτήν τη λύση.

3. Δημιουργία της λύσης.

4. Εκτελέστε την εφαρμογή κονσόλας **ProvisionSample** . Στην εφαρμογή κονσόλας δείγμα, παροχή του χώρου εργασίας και εισαγάγετε ένα αρχείο PBIX.

5. Για να παρέχετε ένα νέο **χώρο εργασίας**, ορίστε την επιλογή 5, **προμήθεια νέου χώρου εργασίας σε μια υπάρχουσα συλλογή χώρου εργασίας**.

    ![](media\powerbi-embedded-get-started-sample\console-option-5.png)

6. Πληκτρολογήστε το όνομα της **Συλλογής χώρου εργασίας** και **Πλήκτρο πρόσβασης**. Μπορείτε να λάβετε αυτές τις στην **Πύλη του Azure**. Για να μάθετε περισσότερα σχετικά με το πώς μπορείτε να λάβετε το **Πλήκτρο πρόσβασης**, ανατρέξτε στο θέμα [Προβολή Power BI API τα πλήκτρα πρόσβασης](power-bi-embedded-get-started-sample.md#view-access-keys) σε γρήγορα αποτελέσματα με το Microsoft Power BI ενσωματωμένο.

    ![](media\powerbi-embedded-get-started-sample\azure-portal.png)

7. Αντιγραφή και αποθηκεύστε το **Αναγνωριστικό χώρου εργασίας** που έχουν δημιουργηθεί πρόσφατα για να χρησιμοποιήσετε αργότερα σε αυτό το άρθρο. Αφού δημιουργηθεί το **Αναγνωριστικό χώρου εργασίας** , μπορείτε να το βρείτε την **Πύλη Azure**.

    ![](media\powerbi-embedded-get-started-sample\workspace-id.png)

8. Για να εισαγάγετε ένα αρχείο PBIX στο το **χώρο εργασίας**, ορίστε την επιλογή **6. Εισαγωγή αρχείου PBIX υπολογιστή σε έναν υπάρχοντα χώρο εργασίας**. Εάν δεν έχετε ένα αρχείο PBIX εύχρηστο, μπορείτε να κάνετε λήψη του [λιανική ανάλυσης δείγμα PBIX] (http://go.microsoft.com/fwlink/?LinkID=780547).

9. Εάν σας ζητηθεί, πληκτρολογήστε ένα φιλικό όνομα για το **σύνολο δεδομένων**.

Θα πρέπει να δείτε μια απάντηση, όπως:

````
Checking import state... Publishing
Checking import state... Succeeded
```

> [AZURE.NOTE] If your PBIX file contains any direct query connections, run option 7 to update the connection strings.

At this point, you have a Power BI PBIX report imported into your **Workspace**. Now, let's look at how to run the **Power BI Embedded** get started sample web app.

## Run the sample web app

The web app sample is a sample dashboard that renders reports imported into your **Workspace**. Here's how to configure the web app sample.

1. In the **PowerBI-embedded** Visual Studio solution, right click the **EmbedSample** web application, and choose **Set as StartUp project**.
2. In **web.config**, in the **EmbedSample** web application, edit the **appSettings**: **AccessKey**, **WorkspaceCollection** name, and **WorkspaceId**.

    ```
    <appSettings>
        <add key="powerbi:AccessKey" value="" />
        <add key="powerbi:ApiUrl" value="https://api.powerbi.com" />
        <add key="powerbi:WorkspaceCollection" value="" />
        <add key="powerbi:WorkspaceId" value="" />
    </appSettings>
    ```
3. Run the **EmbedSample** web application.

Once you run the **EmbedSample** web application, the left navigation panel should contain a **Reports** menu. To view the report you imported, expand **Reports**, and click a report. If you imported the [Retail Analysis Sample PBIX](http://go.microsoft.com/fwlink/?LinkID=780547), the sample web app would look like this:

![](media\powerbi-embedded-get-started-sample\power-bi-embedded-sample-left-nav.png)

After you click a report, the **EmbedSample** web application should look something this:

![](media\powerbi-embedded-get-started-sample\sample-web-app.png)


## Explore the sample code
The **Microsoft Power BI Embedded** sample is an example dashboard web app that shows you how to integrate **Power BI** reports into your app. It uses a Model-View-Controller (MVC) design pattern to demonstrate best practices. This section highlights parts of the sample code that you can explore within the **PowerBI-embedded** web app solution. The Model-View-Controller (MVC) pattern separates the modeling of the domain, the presentation, and the actions based on user input into three separate classes: Model, View, and Control. To learn more about MVC, see [Learn About ASP.NET](http://www.asp.net/mvc).

The **Microsoft Power BI Embedded** sample code is separated as follows. Each section includes the file name in the PowerBI-embedded.sln solution so that you can easily find the code in the sample.

> [AZURE.NOTE] This section is a summary of the sample code that shows how the code was written. To view the complete sample, please load the PowerBI-embedded.sln solution in Visual Studio.

### Model
The sample has a **ReportsViewModel** and **ReportViewModel**.

**ReportsViewModel.cs**: Represents Power BI Reports.

    public class ReportsViewModel
    {
        public List<Report> Reports { get; set; }
    }

**ReportViewModel.cs**: Represents a Power BI Report.

    public classReportViewModel
    {
        public IReport Report { get; set; }

        public string AccessToken { get; set; }
    }

### Connection string
The connection string must be in the following format:

```
Data Source=tcp:MyServer.database.windows.net,1433;Initial Catalog=MyDatabase
```

Using common server and database attributes will fail. For example: Server=tcp:MyServer.database.windows.net,1433;Database=MyDatabase,

### View
The **View** manages the display of Power BI **Reports** and a Power BI **Report**.

**Reports.cshtml**: Iterate over **Model.Reports** to create an **ActionLink**. The **ActionLink** is composed as follows:

|Part|Description
|---|---
|Title| Name of the Report.
|QueryString| A link to the Report ID.

    <div id="reports-nav" class="panel-collapse collapse">
        <div class="panel-body">
            <ul class="nav navbar-nav">
                @foreach (var report in Model.Reports)
                {
                    var reportClass = Request.QueryString["reportId"] == report.Id ? "active" : "";
                    <li class="@reportClass">
                        @Html.ActionLink(report.Name, "Report", new { reportId = report.Id })
                    </li>
                }
            </ul>
        </div>
    </div>

Report.cshtml: Set the **Model.AccessToken**, and the Lambda expression for **PowerBIReportFor**.

    @model ReportViewModel

    ...

    <div class="side-body padding-top">
        @Html.PowerBIAccessToken(Model.AccessToken)
        @Html.PowerBIReportFor(m => m.Report, new { style = "height:85vh" })
    </div>

### Controller

**DashboardController.cs**: Creates a PowerBIClient passing an **app token**. A JSON Web Token (JWT) is generated from the **Signing Key** to get the **Credentials**. The **Credentials** are used to create an instance of **PowerBIClient**. Once you have an instance of **PowerBIClient**, you can call GetReports() and GetReportsAsync().

CreatePowerBIClient()

    private IPowerBIClient CreatePowerBIClient()
    {
        var credentials = new TokenCredentials(accessKey, "AppKey");
        var client = new PowerBIClient(credentials)
        {
            BaseUri = new Uri(apiUrl)
        };

        return client;
    }

ActionResult Reports()

    public ActionResult Reports()
    {
        using (var client = this.CreatePowerBIClient())
        {
            var reportsResponse = client.Reports.GetReports(this.workspaceCollection, this.workspaceId);

            var viewModel = new ReportsViewModel
            {
                Reports = reportsResponse.Value.ToList()
            };

            return PartialView(viewModel);
        }
    }


Task<ActionResult> Report(string reportId)

    public async Task<ActionResult> Report(string reportId)
    {
        using (var client = this.CreatePowerBIClient())
        {
            var reportsResponse = await client.Reports.GetReportsAsync(this.workspaceCollection, this.workspaceId);
            var report = reportsResponse.Value.FirstOrDefault(r => r.Id == reportId);
            var embedToken = PowerBIToken.CreateReportEmbedToken(this.workspaceCollection, this.workspaceId, report.Id);

            var viewModel = new ReportViewModel
            {
                Report = report,
                AccessToken = embedToken.Generate(this.accessKey)
            };

            return View(viewModel);
        }
    }

### Integrate a report into your app

Once you have a **Report**, you use an **IFrame** to embed the Power BI **Report**. Here is a code snippet from  powerbi.js in the **Microsoft Power BI Embedded** sample.

![](media\powerbi-embedded-get-started-sample\power-bi-embedded-iframe-code.png)


## Filter reports embedded in your application

You can filter an embedded report using a URL syntax. To do this, you add a **$filter** query string parameter with an **eq** operator to your iFrame src url with the filter specified. Here is the filter query syntax:

```
https://app.powerbi.com/reportEmbed
?reportId=d2a0ea38-...-9673-ee9655d54a4a&
$filter={tableName/fieldName}%20eq%20'{fieldValue}'
```

> [AZURE.NOTE] {tableName/fieldName} cannot include spaces or special characters. The {fieldValue} accepts a single categorical value.  


## See also

- [Common Microsoft Power BI Embedded scenarios](power-bi-embedded-scenarios.md)
- [Authenticating and authorizing in Power BI Embedded](power-bi-embedded-app-token-flow.md)
