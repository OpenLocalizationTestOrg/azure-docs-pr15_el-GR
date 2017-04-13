<properties
    pageTitle="Εφαρμογές σε στοίβα Azure - γνωστά προβλήματα και αντιμετώπιση προβλημάτων στο Web | Microsoft Azure"
    description="Λεπτομερείς οδηγίες για την ανάπτυξη εφαρμογών Web σε στοίβα Azure"
    services="azure-stack"
    documentationCenter=""
    authors="apwestgarth"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="app-service"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="anwestg"/>
    
# <a name="web-apps-resource-provider---known-issues-and-troubleshooting"></a>Υπηρεσία παροχής πόρων εφαρμογών Web - γνωστά προβλήματα και αντιμετώπιση προβλημάτων

> [AZURE.NOTE] Οι ακόλουθες πληροφορίες ισχύει μόνο για αναπτύξεις Azure στοίβα TP1.

Εάν αντιμετωπίζετε προβλήματα κατά την ανάπτυξη ή την εργασία με τα Web Apps σε στοίβα Azure ανατρέξτε τις παρακάτω οδηγίες.

## <a name="azure-stack-web-apps-not-available-in-the-portal"></a>Azure στοίβας Web Apps δεν είναι διαθέσιμη στην πύλη

![Εφαρμογές Web Azure στοίβας Δημιουργία νέας εφαρμογής Web][1]

Αφού ολοκληρώσετε την [καταχώρηση της υπηρεσίας παροχής εφαρμογών Web Azure στοίβας](azure-stack-webapps-deploy.md#register-the-newly-deployed-azure-stack-web-apps-provider-with-arm) εάν που δεν είναι δυνατό να βρείτε το Web + blade κινητές συσκευές, στη συνέχεια, δοκιμάστε τα εξής:
* **Αποσυνδεθείτε από την πύλη** και **Κλείστε το πρόγραμμα περιήγησης** και, στη συνέχεια, σε μια **νέα σύνδεση παράθυρο του προγράμματος περιήγησης με την πύλη** και προσπαθήστε ξανά.

Εάν ακόμη δεν μπορείτε να δείτε το web και κινητές συσκευές blade, δοκιμάστε τα εξής:

1.  Στον **Κεντρικό υπολογιστή του Azure στοίβας** στον **Hyper-V Manager** , εντοπίστε το **xRPVM** και να **συνδεθείτε** με την εικονική Μηχανή.
2.  Ανοίξτε μια **γραμμή εντολών ως διαχειριστής** και εκτελέστε την **εντολή IISRESET**
3.  Επιστροφή στο το **ClientVM** και ανοίξτε ένα **νέο παράθυρο του προγράμματος περιήγησης**, **συνδεθείτε στην πύλη** και προσπαθήστε ξανά.

## <a name="deployment-fails-when-creating-a-web-app-in-a-new-resource-group"></a>Ανάπτυξη αποτυγχάνει, όταν δημιουργείτε μια εφαρμογή Web σε μια νέα ομάδα πόρων

Στο πρώτο προεπισκόπηση του Web App, πρέπει να δημιουργηθούν **Όλες τις εφαρμογές Web** στην ίδια ομάδα πόρων με την υπηρεσία παροχής πόρων εφαρμογές Web (**ΤΟΠΙΚΉΣ AppService**).  Αυτό είναι ένα γνωστό θέμα και θα επιλυθεί σε μια μελλοντική προεπισκόπηση.

## <a name="other-issues"></a>Άλλα θέματα

Εάν αντιμετωπίσετε άλλα προβλήματα με τα Web Apps σε στοίβα Azure καταχωρήστε στο [φόρουμ στοίβας Azure] (https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack) όπου τα μέλη της ομάδας παρακολούθηση δημοσιεύσεις.


<!--Image references-->
[1]: ./media/azure-stack-webapps-troubleshoot-known-issues/NewWebandMobile.png



<!--Links-->
[Azure_Stack_App_Service_preview_installer]: http://go.microsoft.com/fwlink/?LinkID=717531
[WebAppsDeployment]: http://go.microsoft.com/fwlink/?LinkId=723982
[AppServiceHelperScripts]: http://go.microsoft.com/fwlink/?LinkId=733525
