<properties
   pageTitle="Διαχείριση ρόλων στο Azure cloud services έργων με το Visual Studio | Microsoft Azure"
   description="Μάθετε πώς μπορείτε να προσθέσετε νέους ρόλους στο έργο σας υπηρεσία Azure cloud ή να καταργήσετε υπάρχοντα ρόλους από αυτό με χρήση του Visual Studio."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="managing-roles-in-the-azure-cloud-services-projects-with-visual-studio"></a>Διαχείριση ρόλων στα έργα υπηρεσιών Azure cloud με το Visual Studio

Αφού δημιουργήσετε το έργο υπηρεσιών Azure cloud, μπορείτε να προσθέσετε νέους ρόλους σε αυτό ή να καταργήσετε υπάρχοντα ρόλους από αυτή. Μπορείτε επίσης να εισαγάγετε ένα υπάρχον έργο και να μετατρέψετε σε ένα ρόλο. Για παράδειγμα, μπορείτε να εισαγάγετε μια εφαρμογή web ASP.NET και να ορίσετε ως ένα ρόλο web.

## <a name="adding-or-removing-roles"></a>Προσθήκη ή κατάργηση ρόλων

**Για να προσθέσετε ένα ρόλο**

Στην **Εξερεύνηση λύσεων**, ανοίξτε το μενού συντόμευσης για τον κόμβο **τους ρόλους** στο έργο σας υπηρεσία cloud και επιλέξτε **Προσθήκη**. Μπορείτε να επιλέξετε μια υπάρχουσα ρόλου web ή ρόλο εργασίας από την τρέχουσα λύση ή να δημιουργήσετε ένα νέο έργο ρόλο web ή εργασίας. Ή μπορείτε να επιλέξετε ένα κατάλληλο έργο, όπως ένα έργο της εφαρμογής web ASP.NET, και να συσχετίσετε με ένα έργο ρόλο.

**Για να καταργήσετε μια συσχέτιση ρόλων**

Στον κόμβο του έργου υπηρεσίας cloud στην Εξερεύνηση λύσεων **ρόλους** , ανοίξτε το μενού συντόμευσης για το ρόλο για να καταργήσετε και επιλέξτε **Κατάργηση**.

## <a name="removing-and-adding-roles-in-your-cloud-service"></a>Κατάργηση και προσθήκη ρόλων στην υπηρεσία cloud

Εάν καταργήσετε ένα ρόλο από το έργο υπηρεσία cloud, αλλά αργότερα αποφασίσετε για να προσθέσετε ξανά το ρόλο του έργου, μόνο το ρόλο δήλωσης και βασικά χαρακτηριστικά, όπως τα τελικά σημεία και τα Διαγνωστικά πληροφορίες, προστίθεται. Χωρίς επιπλέον πόρους ή αναφορές προστίθενται στο αρχείο ServiceDefinition.csdef ή το αρχείο ServiceConfiguration.cscfg. Εάν θέλετε να προσθέσετε αυτές τις πληροφορίες, θα πρέπει να την προσθέσετε με μη αυτόματο τρόπο σε αυτά τα αρχεία.

Για παράδειγμα, ενδέχεται να μπορείτε να καταργήσετε ένα ρόλο υπηρεσίας web και αργότερα αποφασίσετε να προσθέσετε και αυτός ο ρόλος πάλι στο τη λύση σας. Εάν το κάνετε αυτό, θα παρουσιαστεί σφάλμα. Για να αποτρέψετε αυτό το σφάλμα, πρέπει να προσθέσετε το `<LocalResources>` στοιχείου που εμφανίζεται στο παρακάτω XML επιστρέψετε στο αρχείο ServiceDefinition.csdef. Χρησιμοποιήστε το όνομα του ρόλου υπηρεσίας web που έχετε προσθέσει επιστρέψετε στο έργο ως μέρος του το χαρακτηριστικό όνομα για το **<LocalStorage>** στοιχείο. Σε αυτό το παράδειγμα το όνομα του ρόλου υπηρεσίας web είναι **WCFServiceWebRole1**.

    <WebRole name="WCFServiceWebRole1">
        <Sites>
          <Site name="Web">
            <Bindings>
              <Binding name="Endpoint1" endpointName="Endpoint1" />
            </Bindings>
          </Site>
        </Sites>
        <Endpoints>
          <InputEndpoint name="Endpoint1" protocol="http" port="80" />
        </Endpoints>
        <Imports>
          <Import moduleName="Diagnostics" />
        </Imports>
       <LocalResources>
          <LocalStorage name="WCFServiceWebRole1.svclog" sizeInMB="1000" cleanOnRoleRecycle="false" />
       </LocalResources>
    </WebRole>

## <a name="next-steps"></a>Επόμενα βήματα

Μάθετε περισσότερα σχετικά με τον τρόπο ρύθμισης παραμέτρων τους ρόλους στο Visual Studio, διαβάζοντας [Ρύθμιση παραμέτρων τους ρόλους για μια υπηρεσία Cloud Azure με το Visual Studio](vs-azure-tools-configure-roles-for-cloud-service.md).
