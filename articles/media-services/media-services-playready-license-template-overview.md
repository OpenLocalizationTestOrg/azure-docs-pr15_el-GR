<properties 
    pageTitle="Επισκόπηση πρότυπο άδειας χρήσης PlayReady υπηρεσίες πολυμέσων" 
    description="Αυτό το θέμα παρέχει μια επισκόπηση του PlayReady άδεια χρήσης του προτύπου που χρησιμοποιείται για τη ρύθμιση παραμέτρων PlayReady άδειες χρήσης." 
    authors="juliako" 
    manager="erikre" 
    editor="" 
    services="media-services" 
    documentationCenter=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016"  
    ms.author="juliako"/>

#<a name="media-services-playready-license-template-overview"></a>Επισκόπηση πρότυπο άδειας χρήσης PlayReady υπηρεσίες πολυμέσων

Azure Media Services παρέχει τώρα μια υπηρεσία για την παροχή του Microsoft PlayReady άδειες χρήσης. Όταν το πρόγραμμα αναπαραγωγής τελικού χρήστη (για παράδειγμα, Silverlight) προσπαθεί να αναπαραγάγετε το περιεχόμενό σας PlayReady προστατεύεται, αποστέλλεται μια αίτηση για την υπηρεσία παράδοσης άδειας χρήσης για να αποκτήσετε μια άδεια χρήσης. Εάν η υπηρεσία άδειας χρήσης απορρίψει την αίτηση, θέματα την άδεια χρήσης που αποστέλλονται στον υπολογιστή-πελάτη και μπορούν να χρησιμοποιηθούν για αποκρυπτογράφηση και αναπαραγωγή του περιεχομένου που καθορίζεται.

Υπηρεσίες πολυμέσων παρέχει επίσης API που σας επιτρέπουν να διαμορφώσετε τις άδειες χρήσης PlayReady. Άδειες χρήσης περιέχουν τα δικαιώματα και τους περιορισμούς που θέλετε για το περιβάλλον εκτέλεσης PlayReady DRM για να επιβάλετε όταν ένας χρήστης επιχειρεί να αναπαραγωγή προστασία περιεχομένου.
Ακολουθούν ορισμένα παραδείγματα περιορισμοί άδειας χρήσης PlayReady που μπορείτε να καθορίσετε:

- Η ημερομηνία/ώρα από την οποία η άδεια χρήσης είναι έγκυρες.
- Η τιμή DateTime όταν λήξει η άδεια χρήσης. 
- Για την άδεια χρήσης για να αποθηκευτούν σε μόνιμη χώρου αποθήκευσης του υπολογιστή-πελάτη. Μόνιμη αδειών χρήσης που χρησιμοποιούνται συνήθως για να επιτρέψετε σε εργασία χωρίς σύνδεση αναπαραγωγής του περιεχομένου.
- Το επίπεδο ελάχιστου ασφαλείας που πρέπει να έχετε ένα πρόγραμμα αναπαραγωγής για να αναπαραγάγετε το περιεχόμενό σας. 
- Το επίπεδο προστασίας εξόδου για τα στοιχεία ελέγχου εξόδου για audio\video περιεχόμενο. 
- Για περισσότερες πληροφορίες, ανατρέξτε στην ενότητα στοιχεία ελέγχου εξόδου (3,5) στο έγγραφο [PlayReady συμμόρφωσης κανόνων](https://www.microsoft.com/playready/licensing/compliance/) .

>[AZURE.NOTE]Προς το παρόν, μπορείτε να ρυθμίσετε το PlayRight της άδειας χρήσης PlayReady (απαιτείται αυτό το δικαίωμα). Το PlayRight δίνει το πρόγραμμα-πελάτη τη δυνατότητα να αναπαραγωγής του περιεχομένου. Το PlayRight επιτρέπει επίσης τη ρύθμιση των παραμέτρων τους περιορισμούς ειδικά για αναπαραγωγή. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα [PlayReadyPlayRight](media-services-playready-license-template-overview.md#PlayReadyPlayRight).

Για να ρυθμίσετε τις παραμέτρους PlayReady άδειες χρήσης χρησιμοποιώντας τις υπηρεσίες πολυμέσων, πρέπει να ρυθμίσετε το πρότυπο άδειας χρήσης PlayReady υπηρεσίες πολυμέσων. Το πρότυπο έχει οριστεί σε XML.

Το παρακάτω παράδειγμα εμφανίζει το πρότυπο απλούστερος (και πιο συνηθισμένες) που ρυθμίζει τις παραμέτρους μια βασική ροή άδεια χρήσης. Με αυτήν την άδεια χρήσης, τους πελάτες σας θα έχει τη δυνατότητα να αναπαραγάγετε το PlayReady προστασία περιεχομένου.
    
    <?xml version="1.0" encoding="utf-8"?>
    <PlayReadyLicenseResponseTemplate xmlns:i="http://www.w3.org/2001/XMLSchema-instance" 
                                      xmlns="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/PlayReadyTemplate/v1">
      <LicenseTemplates>
        <PlayReadyLicenseTemplate>
          <ContentKey i:type="ContentEncryptionKeyFromHeader" />
          <PlayRight />
        </PlayReadyLicenseTemplate>
      </LicenseTemplates>
    </PlayReadyLicenseResponseTemplate>

Το XML συμφωνεί με το PlayReady άδεια χρήσης του προτύπου σχήμα XML που ορίζονται από το στο πρότυπο άδειας χρήσης PlayReady ενότητα σχήματος XML.

Υπηρεσίες πολυμέσων επίσης ορίζει ένα σύνολο κλάσεις .NET που μπορεί να χρησιμοποιηθεί για σειριακά και αποσειριοποιηθούν προς και από το αρχείο XML. Για περιγραφή των κύριες κλάσεις, ανατρέξτε στο θέμα [κλάσεις .NET υπηρεσίες πολυμέσων](media-services-playready-license-template-overview.md#classes). που χρησιμοποιούνται για τη ρύθμιση παραμέτρων για πρότυπα άδεια χρήσης.

Για παράδειγμα να ολοκληρωμένες που χρησιμοποιεί κλάσεις .NET για να ρυθμίσετε τις παραμέτρους του προτύπου PlayReady άδειας χρήσης, ανατρέξτε στο θέμα [Χρήση δυναμική κρυπτογράφηση PlayReady και η υπηρεσία παράδοσης άδεια χρήσης](https://msdn.microsoft.com/library/azure/dn783467.aspx).

##<a id="classes"></a>Κλάσεις .NET υπηρεσίες πολυμέσων που χρησιμοποιούνται για τη ρύθμιση παραμέτρων για πρότυπα άδειας χρήσης

Οι παρακάτω είναι το κύριο κλάσεις .NET χρησιμοποιούνται για τη ρύθμιση παραμέτρων για πρότυπα άδειας χρήσης PlayReady υπηρεσίες πολυμέσων. Αυτές οι κλάσεις αντιστοίχιση με τους τύπους που προσδιορίζονται στο [σχήμα XML PlayReady άδεια χρήσης του προτύπου](media-services-playready-license-template-overview.md#schema).

Τάξη [MediaServicesLicenseTemplateSerializer](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mediaservices.client.contentkeyauthorization.mediaserviceslicensetemplateserializer.aspx) χρησιμοποιείται σειριοποίηση και αποσειριοποίηση προς και από το πρότυπο άδειας χρήσης Media Services XML.

###<a name="playreadylicenseresponsetemplate"></a>PlayReadyLicenseResponseTemplate

[PlayReadyLicenseResponseTemplate](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mediaservices.client.contentkeyauthorization.playreadylicenseresponsetemplate.aspx) - αυτή η κλάση αντιπροσωπεύει το πρότυπο για την απάντηση που αποστέλλονται του τελικού χρήστη. Περιέχει ένα πεδίο για μια συμβολοσειρά προσαρμοσμένα δεδομένα μεταξύ του διακομιστή αδειών χρήσης και της εφαρμογής (μπορεί να είναι χρήσιμο για την προσαρμοσμένη εφαρμογή λογικής), καθώς και μια λίστα με ένα ή περισσότερα πρότυπα άδεια χρήσης.

Αυτή είναι η κλάση "επάνω επιπέδου" στην ιεραρχία του προτύπου. Σημαίνει ότι το πρότυπο απάντησης περιλαμβάνει μια λίστα με τα πρότυπα άδεια χρήσης και τα πρότυπα άδειας χρήσης περιλαμβάνουν (άμεσα ή έμμεσα) όλες οι άλλες κατηγορίες που απαρτίζουν τα δεδομένα του προτύπου για σειριοποίηση.


###<a name="playreadylicensetemplate"></a>PlayReadyLicenseTemplate

[PlayReadyLicenseTemplate](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mediaservices.client.contentkeyauthorization.playreadylicensetemplate.aspx) - τάξη αντιπροσωπεύει ένα πρότυπο άδειας χρήσης για τη δημιουργία PlayReady άδειες χρήσης για να επιστραφεί στους τελικούς χρήστες. Περιέχει τα δεδομένα του περιεχομένου κλειδιού σε την άδεια χρήσης και δικαιωμάτων ή περιορισμοί για να επιβληθούν από το χρόνο εκτέλεσης PlayReady DRM κατά τη χρήση του περιεχομένου κλειδιού.


###<a id="PlayReadyPlayRight"></a>PlayReadyPlayRight

[PlayReadyPlayRight](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mediaservices.client.contentkeyauthorization.playreadyplayright.aspx) - αυτή η κλάση αντιπροσωπεύει το PlayRight μιας άδειας χρήσης PlayReady. Εκχωρεί στο χρήστη τη δυνατότητα να αναπαραγωγή περιεχομένου υπόκεινται τους περιορισμούς κανέναν ή περισσότερους έχει ρυθμιστεί με την άδεια χρήσης και στην PlayRight ίδιο (για το συγκεκριμένο πολιτική αναπαραγωγής). Ιδιαίτερα της πολιτικής στην το PlayRight έχει να κάνετε με τους περιορισμούς εξόδου που ελέγχουν τους τύπους των εξόδους που μπορούν να αναπαραχθούν στο περιεχόμενο μέσω και περιορισμοί που πρέπει να τοποθετηθούν στη θέση κατά τη χρήση μιας δεδομένης εξόδου. Για παράδειγμα, εάν είναι ενεργοποιημένη η DigitalVideoOnlyContentRestriction, στη συνέχεια, το περιβάλλον εκτέλεσης DRM επιτρέπει μόνο το βίντεο που θα εμφανίζεται επάνω από ψηφιακή εξόδους (αναλογική εξόδους βίντεο δεν θα επιτρέπεται η μεταβίβαση του περιεχομένου).

>[AZURE.IMPORTANT]Αυτοί οι τύποι των περιορισμών μπορεί να είναι πολύ ισχυρή, αλλά μπορεί επίσης να επηρεάσει την εμπειρία καταναλωτή. Εάν η προστασία εξόδου έχουν ρυθμιστεί πολύ περιοριστικό, το περιεχόμενο μπορεί να μην λειτουργεί σε ορισμένα προγράμματα-πελάτες. Για περισσότερες πληροφορίες, ανατρέξτε στο θέμα το έγγραφο [PlayReady συμμόρφωσης κανόνων](https://www.microsoft.com/playready/licensing/compliance/) .

Για ένα παράδειγμα του τι προστασίας επίπεδα υποστηρίζει το Silverlight, ανατρέξτε στο θέμα: [υποστήριξης του Silverlight για προστασία εξόδου](http://go.microsoft.com/fwlink/?LinkId=617318).

##<a id="schema"></a>Σχήμα XML PlayReady άδεια χρήσης του προτύπου
    
    <?xml version="1.0" encoding="utf-8"?>
    <xs:schema xmlns:tns="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/PlayReadyTemplate/v1" xmlns:ser="http://schemas.microsoft.com/2003/10/Serialization/" elementFormDefault="qualified" targetNamespace="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/PlayReadyTemplate/v1" xmlns:xs="http://www.w3.org/2001/XMLSchema">
      <xs:import namespace="http://schemas.microsoft.com/2003/10/Serialization/" />
      <xs:complexType name="AgcAndColorStripeRestriction">
        <xs:sequence>
          <xs:element minOccurs="0" name="ConfigurationData" type="xs:unsignedByte" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="AgcAndColorStripeRestriction" nillable="true" type="tns:AgcAndColorStripeRestriction" />
      <xs:simpleType name="ContentType">
        <xs:restriction base="xs:string">
          <xs:enumeration value="Unspecified" />
          <xs:enumeration value="UltravioletDownload" />
          <xs:enumeration value="UltravioletStreaming" />
        </xs:restriction>
      </xs:simpleType>
      <xs:element name="ContentType" nillable="true" type="tns:ContentType" />
      <xs:complexType name="ExplicitAnalogTelevisionRestriction">
        <xs:sequence>
          <xs:element minOccurs="0" name="BestEffort" type="xs:boolean" />
          <xs:element minOccurs="0" name="ConfigurationData" type="xs:unsignedByte" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="ExplicitAnalogTelevisionRestriction" nillable="true" type="tns:ExplicitAnalogTelevisionRestriction" />
      <xs:complexType name="PlayReadyContentKey">
        <xs:sequence />
      </xs:complexType>
      <xs:element name="PlayReadyContentKey" nillable="true" type="tns:PlayReadyContentKey" />
      <xs:complexType name="ContentEncryptionKeyFromHeader">
        <xs:complexContent mixed="false">
          <xs:extension base="tns:PlayReadyContentKey">
            <xs:sequence />
          </xs:extension>
        </xs:complexContent>
      </xs:complexType>
      <xs:element name="ContentEncryptionKeyFromHeader" nillable="true" type="tns:ContentEncryptionKeyFromHeader" />
      <xs:complexType name="ContentEncryptionKeyFromKeyIdentifier">
        <xs:complexContent mixed="false">
          <xs:extension base="tns:PlayReadyContentKey">
            <xs:sequence>
              <xs:element minOccurs="0" name="KeyIdentifier" type="ser:guid" />
            </xs:sequence>
          </xs:extension>
        </xs:complexContent>
      </xs:complexType>
      <xs:element name="ContentEncryptionKeyFromKeyIdentifier" nillable="true" type="tns:ContentEncryptionKeyFromKeyIdentifier" />
      <xs:complexType name="PlayReadyLicenseResponseTemplate">
        <xs:sequence>
          <xs:element name="LicenseTemplates" nillable="true" type="tns:ArrayOfPlayReadyLicenseTemplate" />
          <xs:element minOccurs="0" name="ResponseCustomData" nillable="true" type="xs:string">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
        </xs:sequence>
      </xs:complexType>
      <xs:element name="PlayReadyLicenseResponseTemplate" nillable="true" type="tns:PlayReadyLicenseResponseTemplate" />
      <xs:complexType name="ArrayOfPlayReadyLicenseTemplate">
        <xs:sequence>
          <xs:element minOccurs="0" maxOccurs="unbounded" name="PlayReadyLicenseTemplate" nillable="true" type="tns:PlayReadyLicenseTemplate" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="ArrayOfPlayReadyLicenseTemplate" nillable="true" type="tns:ArrayOfPlayReadyLicenseTemplate" />
      <xs:complexType name="PlayReadyLicenseTemplate">
        <xs:sequence>
          <xs:element minOccurs="0" name="AllowTestDevices" type="xs:boolean" />
          <xs:element minOccurs="0" name="BeginDate" nillable="true" type="xs:dateTime">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element name="ContentKey" nillable="true" type="tns:PlayReadyContentKey" />
          <xs:element minOccurs="0" name="ContentType" type="tns:ContentType">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="ExpirationDate" nillable="true" type="xs:dateTime">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="GracePeriod" nillable="true" type="ser:duration">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="LicenseType" type="tns:PlayReadyLicenseType" />
          <xs:element minOccurs="0" name="PlayRight" nillable="true" type="tns:PlayReadyPlayRight" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="PlayReadyLicenseTemplate" nillable="true" type="tns:PlayReadyLicenseTemplate" />
      <xs:simpleType name="PlayReadyLicenseType">
        <xs:restriction base="xs:string">
          <xs:enumeration value="Nonpersistent" />
          <xs:enumeration value="Persistent" />
        </xs:restriction>
      </xs:simpleType>
      <xs:element name="PlayReadyLicenseType" nillable="true" type="tns:PlayReadyLicenseType" />
      <xs:complexType name="PlayReadyPlayRight">
        <xs:sequence>
          <xs:element minOccurs="0" name="AgcAndColorStripeRestriction" nillable="true" type="tns:AgcAndColorStripeRestriction">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="AllowPassingVideoContentToUnknownOutput" type="tns:UnknownOutputPassingOption">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="AnalogVideoOpl" nillable="true" type="xs:int">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="CompressedDigitalAudioOpl" nillable="true" type="xs:int">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="CompressedDigitalVideoOpl" nillable="true" type="xs:int">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="DigitalVideoOnlyContentRestriction" type="xs:boolean">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="ExplicitAnalogTelevisionOutputRestriction" nillable="true" type="tns:ExplicitAnalogTelevisionRestriction">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="FirstPlayExpiration" nillable="true" type="ser:duration">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="ImageConstraintForAnalogComponentVideoRestriction" type="xs:boolean">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="ImageConstraintForAnalogComputerMonitorRestriction" type="xs:boolean">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="ScmsRestriction" nillable="true" type="tns:ScmsRestriction">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="UncompressedDigitalAudioOpl" nillable="true" type="xs:int">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
          <xs:element minOccurs="0" name="UncompressedDigitalVideoOpl" nillable="true" type="xs:int">
            <xs:annotation>
              <xs:appinfo>
                <DefaultValue EmitDefaultValue="false" xmlns="http://schemas.microsoft.com/2003/10/Serialization/" />
              </xs:appinfo>
            </xs:annotation>
          </xs:element>
        </xs:sequence>
      </xs:complexType>
      <xs:element name="PlayReadyPlayRight" nillable="true" type="tns:PlayReadyPlayRight" />
      <xs:simpleType name="UnknownOutputPassingOption">
        <xs:restriction base="xs:string">
          <xs:enumeration value="NotAllowed" />
          <xs:enumeration value="Allowed" />
          <xs:enumeration value="AllowedWithVideoConstriction" />
        </xs:restriction>
      </xs:simpleType>
      <xs:element name="UnknownOutputPassingOption" nillable="true" type="tns:UnknownOutputPassingOption" />
      <xs:complexType name="ScmsRestriction">
        <xs:sequence>
          <xs:element minOccurs="0" name="ConfigurationData" type="xs:unsignedByte" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="ScmsRestriction" nillable="true" type="tns:ScmsRestriction" />
    </xs:schema>



##<a name="media-services-learning-paths"></a>Διαδικασίες εκμάθησης Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Παροχή σχολίων

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
