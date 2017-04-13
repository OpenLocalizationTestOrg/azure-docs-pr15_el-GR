<properties 
    pageTitle="Εργασίες που επιτρέπεται σε διαφορετικές καταστάσεις ή καταστάσεις στις υπηρεσίες BizTalk | Microsoft Azure" 
    description="Οι ενέργειες/λειτουργίες που επιτρέπεται σε διαφορετική MABS κατάσταση: διακοπή, Έναρξη, επανεκκινήστε, αναστολή, βιογραφικό σημείωμα, διαγραφή, κλιμάκωση, ενημέρωση ρύθμισης παραμέτρων και τη δημιουργία αντιγράφων ασφαλείας" 
    services="biztalk-services" 
    documentationCenter="" 
    authors="MandiOhlinger" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="biztalk-services" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="mandia"/>



# <a name="biztalk-services-service-state-chart"></a>BizTalk υπηρεσίες: Γράφημα κατάσταση υπηρεσίας
Ανάλογα με την τρέχουσα κατάσταση της υπηρεσίας BizTalk, υπάρχουν λειτουργίες που μπορούν ή δεν μπορεί να εκτελέσει την υπηρεσία του BizTalk.

Για παράδειγμα, μπορείτε να προμηθεύσουν μια νέα υπηρεσία BizTalk στην πύλη του Azure κλασική. Όταν ολοκληρωθεί με επιτυχία, η υπηρεσία BizTalk είναι σε ενεργή κατάσταση. Στην κατάσταση ενεργό, μπορείτε να διακόψετε την υπηρεσία BizTalk. Εάν διακόψετε είναι επιτυχής, η υπηρεσία BizTalk μεταβαίνει σε κατάσταση διακοπή. Εάν αποτύχει η διακοπή, η υπηρεσία BizTalk μεταβαίνει σε κατάσταση StopFailed. Σε κατάσταση StopFailed, μπορείτε να κάνετε επανεκκίνηση της υπηρεσίας BizTalk. Εάν προσπαθήσετε μια λειτουργία που δεν επιτρέπεται, όπως το βιογραφικό σημείωμα στην υπηρεσία BizTalk, παρουσιάζεται το παρακάτω σφάλμα:

**Η λειτουργία δεν επιτρέπεται**

Για να προμηθεύσουν υπηρεσίας BizTalk, μεταβείτε στο [υπηρεσίες BizTalk: πύλη κλασική προμήθεια Azure χρησιμοποιώντας](http://go.microsoft.com/fwlink/p/?LinkID=302280).

Στους παρακάτω πίνακες παρατίθενται τις λειτουργίες ή τις ενέργειες που μπορεί να ολοκληρωθεί όταν η υπηρεσία BizTalk είναι σε ένα συγκεκριμένο μέλος. Μια ✔ σημαίνει ότι η λειτουργία επιτρέπεται ενώ βρίσκεστε σε αυτήν την κατάσταση. Μια κενή καταχώρηση σημαίνει ότι η λειτουργία δεν είναι δυνατό να εκτελεστεί ενώ βρίσκεστε σε αυτήν την κατάσταση.

## <a name="start-stop-restart-suspend-resume-and-delete-operations"></a>Έναρξη, διακοπή, ξεκινήστε πάλι, αναστολή, βιογραφικό σημείωμα, και διαγραφή λειτουργίες
<table border="1">
<tr>
        <th colspan="15">Η λειτουργία ή την ενέργεια</th>
</tr>

<tr>
        <th rowspan="18">Κατάσταση υπηρεσίας BizTalk</th>
</tr>
<tr bgcolor="FAF9F9">
        <th> </th>
        <th>Έναρξη</th>
        <th>Σταμάτα</th>
        <th>Επανεκκίνηση</th>
        <th>Αναστολή</th>
        <th>Βιογραφικό σημείωμα</th>
        <th>Διαγραφή</th>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Ενεργό</b></td>
<td> </td>
<td><center>✔</center></td>
<td><center>✔</center></td>
<td><center>✔</center></td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Απενεργοποιημένα</b></td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Σε αναστολή</b></td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Έγινε διακοπή</b></td>
<td><center>✔</center></td>
<td> </td>
<td><center>✔</center></td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Απέτυχε η ενημέρωση υπηρεσίας</b></td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>DisableFailed</b></td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>EnableFailed</b></td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>StartFailed<br/>
StopFailed<br/>
RestartFailed</b></td>
<td><center>✔</center></td>
<td><center>✔</center></td>
<td><center>✔</center></td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>SuspendedFailed<br/>
ResumeFailed</b></td>
<td> </td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
<td><center>✔</center></td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>CreatedFailed<br/>
RestoreFailed<br/></b></td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>ConfigUpdateFailed</b></td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>ScaleFailed</b></td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
</table>
<br/>

## <a name="scale-update-configuration-and-backup-operations"></a>Κλίμακα, ενημέρωση ρύθμισης παραμέτρων και λειτουργίες δημιουργίας αντιγράφων ασφαλείας
<table border="1">
<tr>
        <th colspan="15">Η λειτουργία ή την ενέργεια</th>
</tr>

<tr>
        <th rowspan="18">Κατάσταση υπηρεσίας BizTalk</th>
</tr>
<tr bgcolor="FAF9F9">
        <th> </th>
        <th>Κλίμακα</th>
        <th>Ενημέρωση ρύθμισης παραμέτρων</th>
        <th>Δημιουργία αντιγράφων ασφαλείας</th>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Ενεργό</b></td>
<td><center>✔</center></td>
<td><center>✔</center></td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Απενεργοποιημένα</b></td>
<td> </td>
<td> </td>
<td> </td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Σε αναστολή</b></td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Έγινε διακοπή</b></td>
<td> </td>
<td> </td>
<td><center>✔</center></td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>Απέτυχε η ενημέρωση υπηρεσίας</b></td>
<td> </td>
<td> </td>
<td> </td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>DisableFailed</b></td>
<td> </td>
<td> </td>
<td> </td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>EnableFailed</b></td>
<td> </td>
<td> </td>
<td> </td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>StartFailed<br/>
StopFailed<br/>
RestartFailed</b></td>
<td> </td>
<td><center>✔</center></td>
<td> </td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>SuspendedFailed<br/>
ResumeFailed</b></td>
<td> </td>
<td> </td>
<td> </td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>CreatedFailed<br/>
RestoreFailed<br/></b></td>
<td> </td>
<td> </td>
<td> </td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>ConfigUpdateFailed</b></td>
<td> </td>
<td><center>✔</center></td>
<td> </td>
</tr>
<tr>
<td bgcolor="FAF9F9"><b>ScaleFailed</b></td>
<td><center>✔</center></td>
<td> </td>
<td> </td>
</tr>
</table>

## <a name="see-also"></a>Δείτε επίσης
- [BizTalk υπηρεσίες: Κλασική πύλη χρησιμοποιώντας Azure προμήθεια](http://go.microsoft.com/fwlink/p/?LinkID=302280)<br/>
- [BizTalk υπηρεσίες: Καρτέλες πίνακα εργαλείων, εποπτεία και κλίμακας](http://go.microsoft.com/fwlink/p/?LinkID=302281)<br/>
- [Υπηρεσίες BizTalk: Προγραμματιστής, Basic, τυπική και γράφημα εκδόσεις Premium](http://go.microsoft.com/fwlink/p/?LinkID=302279)<br/>
- [BizTalk υπηρεσίες: Δημιουργία αντιγράφων ασφαλείας και επαναφορά](http://go.microsoft.com/fwlink/p/?LinkID=329873)<br/>
- [Υπηρεσίες BizTalk: περιορισμού](http://go.microsoft.com/fwlink/p/?LinkID=302282)<br/>
- [BizTalk υπηρεσίες: Όνομα εκδότη και το κλειδί εκδότη](http://go.microsoft.com/fwlink/p/?LinkID=303941)<br/>
- [Πώς μπορώ να ξεκινήσετε με το Azure BizTalk υπηρεσίες SDK](http://go.microsoft.com/fwlink/p/?LinkID=302335)


 
