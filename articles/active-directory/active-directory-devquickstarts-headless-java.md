<properties
    pageTitle="Azure AD Java γρήγορα αποτελέσματα | Microsoft Azure"
    description="Πώς μπορείτε να δημιουργήσετε μια εφαρμογή της γραμμής εντολών Java που πραγματοποιεί χρήστες στο για να αποκτήσετε πρόσβαση API."
    services="active-directory"
    documentationCenter="java"
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
  ms.tgt_pltfrm="na"
    ms.devlang="java"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="brandwe"/>


# <a name="using-java-command-line-app-to-access-an-api-with-azure-ad"></a>Χρήση εφαρμογής Java γραμμής εντολών για να αποκτήσετε πρόσβαση API με Azure AD

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Azure AD καθιστά απλή και άμεση για να μεταβιβάζουν διαχείρισης των ταυτοτήτων της εφαρμογής web σας, παρέχοντας μία είσοδος και sign-out με λίγες μόνο γραμμές του κώδικα.  Στις εφαρμογές web της Java, μπορείτε να εκτελέσετε αυτό με υλοποίηση της το ADAL4J βάσει Κοινότητας της Microsoft.

  Εδώ θα χρησιμοποιήσουμε ADAL4J για να:
- Πραγματοποιήστε είσοδο στο χρήστη στην εφαρμογή χρησιμοποιώντας Azure AD ως την υπηρεσία παροχής ταυτότητας.
- Εμφάνιση ορισμένες πληροφορίες σχετικά με το χρήστη.
- Πραγματοποιήστε είσοδο στο χρήστη από την εφαρμογή.

Για να το κάνετε αυτό, θα πρέπει να:

1. Καταχωρήστε μια εφαρμογή του Azure AD
2. Ρύθμιση της εφαρμογής σας για να χρησιμοποιήσετε τη βιβλιοθήκη ADAL4J.
3. Χρησιμοποιήστε τη βιβλιοθήκη ADAL4J για την έκδοση αιτήσεις εισόδου και sign-out να Azure AD.
4. Εκτυπώστε δεδομένα σχετικά με το χρήστη.

Για να ξεκινήσετε, [κάντε λήψη του σκελετό εφαρμογής](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect/archive/skeleton.zip) ή [λήψη ολοκληρωμένο δείγμα](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect\/archive/complete.zip).  Θα χρειαστεί επίσης ένα μισθωτή του Azure AD στην οποία θέλετε να καταχωρήσετε την εφαρμογή σας.  Εάν δεν έχετε ήδη, [Μάθετε πώς μπορείτε να αποκτήσετε](active-directory-howto-tenant.md).

## <a name="1--register-an-application-with-azure-ad"></a>1. καταχώρηση μιας εφαρμογής με το Azure AD
Για να ενεργοποιήσετε την εφαρμογή σας για τον έλεγχο ταυτότητας χρηστών, θα πρέπει πρώτα να καταχωρήσετε μια νέα εφαρμογή στο μισθωτή σας.

- Είσοδος στην πύλη του Azure διαχείρισης.
- Στο αριστερό παράθυρο περιήγησης, κάντε κλικ στην **Υπηρεσία καταλόγου Active Directory**.
- Επιλέξτε το μισθωτή όπου θέλετε να καταχωρήσετε την εφαρμογή.
- Κάντε κλικ στην καρτέλα **εφαρμογές** και κάντε κλικ στην επιλογή Προσθήκη στο το σχέδιο κάτω.
- Ακολουθήστε τις οδηγίες και δημιουργήστε μια νέα **εφαρμογή Web ή/και WebAPI**.
    - Το **όνομα** της εφαρμογής θα περιγράφει την εφαρμογή σας στους τελικούς χρήστες
    - Το **Σύμβολο στη διεύθυνση URL** είναι τη βασική διεύθυνση URL της εφαρμογής.  Το σκελετό προεπιλογή είναι `http://localhost:8080/adal4jsample/`.
    - Το **URI Αναγνωριστικό εφαρμογής** είναι ένα μοναδικό αναγνωριστικό για την εφαρμογή σας.  Η σύμβαση είναι να χρησιμοποιήσετε `https://<tenant-domain>/<app-name>`, π.χ.`http://localhost:8080/adal4jsample/`
- Όταν ολοκληρώσετε την καταχώρηση, AAD θα εκχωρήσετε την εφαρμογή σας ένα αναγνωριστικό μοναδικό προγράμματος-πελάτη.  Θα χρειαστείτε αυτή την τιμή στις επόμενες ενότητες, επομένως, αντιγράψτε την από την καρτέλα ρύθμιση παραμέτρων.

Μία φορά στην πύλη για την εφαρμογή σας δημιουργία μιας **Εφαρμογής μυστικό** για την εφαρμογή σας και αντιγράψτε την προς τα κάτω.  Θα το χρειαστείτε λίγο.


## <a name="2-set-up-your-app-to-use-adal4j-library-and-prerequisites-using-maven"></a>2. ρύθμιση της εφαρμογής σας για χρήση ADAL4J βιβλιοθήκη και τις προϋποθέσεις χρησιμοποιώντας Maven
Εδώ, θα σας θα ρυθμίσετε τις παραμέτρους ADAL4J για να χρησιμοποιήσετε το πρωτόκολλο ελέγχου ταυτότητας OpenID σύνδεση.  ADAL4J θα χρησιμοποιηθεί για την έκδοση αιτήσεις εισόδου και sign-out, Διαχείριση περιόδου λειτουργίας του χρήστη, και λήψη πληροφοριών σχετικά με το χρήστη, μεταξύ άλλων.

-   Στον ριζικό κατάλογο του έργου σας, άνοιγμα/δημιουργία `pom.xml` και εντοπίστε το `// TODO: provide dependencies for Maven` και να τον αντικαταστήσετε με τα εξής:

```Java
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>public-client-adal4j-sample</artifactId>
    <packaging>jar</packaging>
    <version>0.0.1-SNAPSHOT</version>
    <name>public-client-adal4j-sample</name>
    <url>http://maven.apache.org</url>
    <properties>
        <spring.version>3.0.5.RELEASE</spring.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>com.microsoft.azure</groupId>
            <artifactId>adal4j</artifactId>
            <version>1.1.2</version>
        </dependency>
        <dependency>
            <groupId>com.nimbusds</groupId>
            <artifactId>oauth2-oidc-sdk</artifactId>
            <version>4.5</version>
        </dependency>
        <dependency>
            <groupId>org.json</groupId>
            <artifactId>json</artifactId>
            <version>20090211</version>
        </dependency>
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>javax.servlet-api</artifactId>
            <version>3.0.1</version>
            <scope>provided</scope>
        </dependency>
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-log4j12</artifactId>
            <version>1.7.5</version>
        </dependency>
</dependencies>
    <build>
        <finalName>public-client-adal4j-sample</finalName>
        <plugins>
                <plugin>
            <groupId>org.codehaus.mojo</groupId>
            <artifactId>exec-maven-plugin</artifactId>
            <version>1.2.1</version>
            <configuration>
                <mainClass>PublicClient</mainClass>
            </configuration>
        </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <configuration>
                    <source>1.7</source>
                    <target>1.7</target>
                    <encoding>UTF-8</encoding>
                </configuration>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-dependency-plugin</artifactId>
                <executions>
                    <execution>
                        <id>install</id>
                        <phase>install</phase>
                        <goals>
                            <goal>sources</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-resources-plugin</artifactId>
                <version>2.5</version>
                <configuration>
                    <encoding>UTF-8</encoding>
                </configuration>
            </plugin>
            <plugin>
        <artifactId>maven-assembly-plugin</artifactId>
        <executions>
          <execution>
            <phase>package</phase>
            <goals>
              <goal>single</goal>
            </goals>
          </execution>
        </executions>
        <configuration>
          <descriptorRefs>
            <descriptorRef>jar-with-dependencies</descriptorRef>
          </descriptorRefs>
        </configuration>
      </plugin>
      <plugin>
  <groupId>org.apache.maven.plugins</groupId>
  <artifactId>maven-assembly-plugin</artifactId>
  <configuration>
    <archive>
      <manifest>
        <mainClass>PublicClient</mainClass>
      </manifest>
    </archive>
  </configuration>
</plugin>
        </plugins>
    </build>

</project>


```



## <a name="3-create-the-java-publicclient-file"></a>3. δημιουργήσετε το αρχείο PublicClient java

Όπως φαίνεται παραπάνω, θα χρησιμοποιήσουμε το API του γραφήματος για να λάβετε δεδομένα σχετικά με ο συνδεδεμένος χρήστης. Για να είναι εύκολο για μας αυτό θα πρέπει να δημιουργήσουμε ένα αρχείο για να αντιπροσωπεύει ένα **Αντικείμενο καταλόγου** και ένα μεμονωμένο αρχείο για να αντιπροσωπεύει το **χρήστη** , ώστε να μπορεί να χρησιμοποιηθεί το μοτίβο OO της Java.

1. Δημιουργήστε ένα αρχείο που ονομάζεται `DirectoryObject.java` οποίοι θα χρησιμοποιηθούν για την αποθήκευση βασικές δεδομένα σχετικά με οποιαδήποτε DirectoryObject (που μπορεί να μην διστάσεις να χρησιμοποιήστε αυτήν την επιλογή αργότερα για οποιαδήποτε άλλα ερωτήματα γραφήματος μπορείτε να το κάνετε). Μπορείτε να αποκοπή και επικολλήστε αυτή από τις παρακάτω:

```Java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.Future;

import javax.naming.ServiceUnavailableException;

import com.microsoft.aad.adal4j.AuthenticationContext;
import com.microsoft.aad.adal4j.AuthenticationResult;

public class PublicClient {

    private final static String AUTHORITY = "https://login.microsoftonline.com/common/";
    private final static String CLIENT_ID = "2a4da06c-ff07-410d-af8a-542a512f5092";

    public static void main(String args[]) throws Exception {

        try (BufferedReader br = new BufferedReader(new InputStreamReader(
                System.in))) {
            System.out.print("Enter username: ");
            String username = br.readLine();
            System.out.print("Enter password: ");
            String password = br.readLine();

            AuthenticationResult result = getAccessTokenFromUserCredentials(
                    username, password);
            System.out.println("Access Token - " + result.getAccessToken());
            System.out.println("Refresh Token - " + result.getRefreshToken());
            System.out.println("ID Token - " + result.getIdToken());
        }
    }

    private static AuthenticationResult getAccessTokenFromUserCredentials(
            String username, String password) throws Exception {
        AuthenticationContext context = null;
        AuthenticationResult result = null;
        ExecutorService service = null;
        try {
            service = Executors.newFixedThreadPool(1);
            context = new AuthenticationContext(AUTHORITY, false, service);
            Future<AuthenticationResult> future = context.acquireToken(
                    "https://graph.windows.net", CLIENT_ID, username, password,
                    null);
            result = future.get();
        } finally {
            service.shutdown();
        }

        if (result == null) {
            throw new ServiceUnavailableException(
                    "authentication result was null");
        }
        return result;
    }
}


```


##<a name="compile-and-run-the-sample"></a>Μεταγλώττιση και να εκτελέσετε το δείγμα

Αλλαγή πάλι τον ριζικό κατάλογο και εκτελέστε την ακόλουθη εντολή για να δημιουργήσετε το δείγμα τοποθετείτε απλώς συνεργασία με `maven`. Αυτό θα χρησιμοποιήσει το `pom.xml` αρχείου που συντάξατε για εξαρτήσεις.

`$ mvn package`

Πρέπει τώρα να έχετε μια `adal4jsample.war` το αρχείο σας `/targets` καταλόγου. Ενδέχεται να μπορείτε να αναπτύξετε που σας Tomcat κοντέινερ και να επισκεφθείτε τη διεύθυνση URL 

`http://localhost:8080/adal4jsample/`


> [AZURE.NOTE] 
Είναι πολύ εύκολο να αναπτύξετε μια ΠΟΛΈΜΟΥ με την πιο πρόσφατη διακομιστές Tomcat. Απλώς μεταβείτε `http://localhost:8080/manager/` και ακολουθήστε τις οδηγίες στην αποστολή σας '' adal4jsample.war' αρχείο. Θα autodeploy για εσάς με το σωστό τελικό σημείο.

##<a name="next-steps"></a>Επόμενα βήματα

Συγχαρητήρια! Τώρα έχετε μια εργασία εφαρμογής Java που έχει τη δυνατότητα να ασφαλή έλεγχο ταυτότητας χρηστών, καλέστε APIs Web χρησιμοποιώντας διακριτικό 2.0 και λήψη βασικών πληροφοριών σχετικά με το χρήστη.  Εάν δεν το έχετε κάνει ήδη, τώρα είναι η ώρα για τη συμπλήρωση του μισθωτή με ορισμένων χρηστών.

Για αναφορά, το ολοκληρωμένο δείγμα (χωρίς τις τιμές παραμέτρων) [παρέχεται ως ένα .zip εδώ](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect/archive/complete.zip), ή μπορείτε να αντιγράψει το το GitHub:

```git clone --branch complete https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect.git```

