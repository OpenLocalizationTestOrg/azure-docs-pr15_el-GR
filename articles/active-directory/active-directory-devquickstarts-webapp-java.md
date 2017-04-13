<properties
    pageTitle="Azure AD Java γρήγορα αποτελέσματα | Microsoft Azure"
    description="Πώς μπορείτε να δημιουργήσετε μια εφαρμογή web Java που πραγματοποιεί χρήστες με ένα εταιρικό ή σχολικό λογαριασμό."
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


# <a name="java-web-app-sign-in--sign-out-with-azure-ad"></a>Java Web App είσοδος και έξοδος με Azure AD

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


## <a name="2-set-up-your-app-to-use-adal4j-library-and-prerequisities-using-maven"></a>2. ρύθμιση της εφαρμογής σας για να χρησιμοποιήσετε τη βιβλιοθήκη ADAL4J και prerequisities χρησιμοποιώντας Maven
Εδώ, θα σας θα ρυθμίσετε τις παραμέτρους ADAL4J για να χρησιμοποιήσετε το πρωτόκολλο ελέγχου ταυτότητας OpenID σύνδεση.  ADAL4J θα χρησιμοποιηθεί για την έκδοση αιτήσεις εισόδου και sign-out, Διαχείριση περιόδου λειτουργίας του χρήστη, και λήψη πληροφοριών σχετικά με το χρήστη, μεταξύ άλλων.

-   Στον ριζικό κατάλογο του έργου σας, άνοιγμα/δημιουργία `pom.xml` και εντοπίστε το `// TODO: provide dependencies for Maven` και να τον αντικαταστήσετε με τα εξής:

```Java
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>adal4jsample</artifactId>
    <packaging>war</packaging>
    <version>0.0.1-SNAPSHOT</version>
    <name>adal4jsample</name>
    <url>http://maven.apache.org</url>
    <properties>
        <spring.version>3.0.5.RELEASE</spring.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>com.microsoft.azure</groupId>
            <artifactId>adal4j</artifactId>
            <version>1.1.1</version>
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
        <!-- Spring 3 dependencies -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-core</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-web</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>${spring.version}</version>
        </dependency>
    </dependencies>

    <build>
        <finalName>sample-for-adal4j</finalName>
        <plugins>
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
                <artifactId>maven-war-plugin</artifactId>
                <version>2.4</version>
                <configuration>
                    <warName>${project.artifactId}</warName>
                    <source>${project.basedir}\src</source>
                    <target>${maven.compiler.target}</target>
                    <encoding>utf-8</encoding>
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
        </plugins>
    </build>

</project>

```


## <a name="3-create-the-java-web-application-files-web-inf"></a>3. Δημιουργία Java web αρχεία εφαρμογών (WEB-Πληροφορίες)

Εδώ, θα σας θα ρυθμίσετε τις παραμέτρους της εφαρμογής web Java για να χρησιμοποιήσετε το πρωτόκολλο ελέγχου ταυτότητας OpenID σύνδεση.  Η βιβλιοθήκη ADAL4J θα χρησιμοποιηθεί για αιτήσεις εισόδου και sign-out θεμάτων, Διαχείριση περιόδου λειτουργίας του χρήστη και λήψη πληροφοριών σχετικά με το χρήστη, μεταξύ άλλων.

-   Για να ξεκινήσετε, ανοίξτε το `web.xml` αρχείο που βρίσκεται στην περιοχή `\webapp\WEB-INF\`, και εισαγάγετε τιμές παραμέτρων της εφαρμογής σας στην xml.

Το αρχείο πρέπει να είναι ως εξής:

```xml
<?xml version="1.0"?>
<web-app id="WebApp_ID" version="2.4"
    xmlns="http://java.sun.com/xml/ns/j2ee" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://java.sun.com/xml/ns/j2ee 
    http://java.sun.com/xml/ns/j2ee/web-app_2_4.xsd">
    <display-name>Archetype Created Web Application</display-name>
    <context-param>
        <param-name>authority</param-name>
        <param-value>https://login.windows.net/</param-value>
    </context-param>
    <context-param>
        <param-name>tenant</param-name>
        <param-value>YOUR_TENANT_NAME</param-value>
    </context-param>

    <filter>
        <filter-name>BasicFilter</filter-name>
        <filter-class>com.microsoft.aad.adal4jsample.BasicFilter</filter-class>
        <init-param>
            <param-name>client_id</param-name>
            <param-value>YOUR_CLIENT_ID</param-value>
        </init-param>
        <init-param>
            <param-name>secret_key</param-name>
            <param-value>YOUR_CLIENT_SECRET</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>BasicFilter</filter-name>
        <url-pattern>/secure/*</url-pattern>
    </filter-mapping>

    <servlet>
        <servlet-name>mvc-dispatcher</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <load-on-startup>1</load-on-startup>
    </servlet>

    <servlet-mapping>
        <servlet-name>mvc-dispatcher</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>

    <context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>/WEB-INF/mvc-dispatcher-servlet.xml</param-value>
    </context-param>

    <listener>
        <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
    </listener>
</web-app>

```


    -   Η `YOUR_CLIENT_ID` είναι το **Αναγνωριστικό εφαρμογής** που έχουν εκχωρηθεί σε εφαρμογή σας στην πύλη εγγραφής.
    -   Η `YOUR_CLIENT_SECRET` είναι το **Μυστικό εφαρμογής** που δημιουργήσατε στην πύλη.
    - Η `YOUR_TENANT_NAME` είναι το **όνομα του μισθωτή** του την εφαρμογή σας, π.χ. contoso.onmicrosoft.com

Αφήστε τα υπόλοιπα τη ρύθμιση παραμέτρων από μόνο του.

> [AZURE.NOTE]
Όπως μπορείτε να δείτε από το αρχείο XML, θα σας συντάσσετε μια webapp JSP/Servlet που ονομάζεται `mvc-dispatcher` που θα χρησιμοποιήσει το `BasicFilter` κάθε φορά που θα σας, επισκεφθείτε το / ασφαλούς διεύθυνση URL. Θα δείτε τα υπόλοιπα με την ίδια μας γράψετε που θα σας θα χρησιμοποιήσετε / ασφαλούς ως θέση όπου βρίσκεται το προστατευμένο περιεχόμενο και θα επιβάλει Azure Active Directory του ελέγχου ταυτότητας.

-   Στη συνέχεια, δημιουργήστε το `mvc-dispatcher-servlet.xml` αρχείο που βρίσκεται στην περιοχή `\webapp\WEB-INF\`, και πληκτρολογήστε τα εξής:

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:context="http://www.springframework.org/schema/context"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="
        http://www.springframework.org/schema/beans     
        http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
        http://www.springframework.org/schema/context 
        http://www.springframework.org/schema/context/spring-context-3.0.xsd">

    <context:component-scan base-package="com.microsoft.aad.adal4jsample" />

    <bean
        class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix">
            <value>/</value>
        </property>
        <property name="suffix">
            <value>.jsp</value>
        </property>
    </bean>

</beans>
```

Αυτό θα σας ενημερώσει το webapp για να χρησιμοποιήσετε Ανοιξιάτικο και από πού να βρείτε το αρχείο .jsp που θα σας θα γράψετε κάτω από.

## <a name="4-create-the-java-jsp-view-files-for-basicfilter-mvc"></a>4. Δημιουργία αρχείων Java JSP προβολής (για BasicFilter MVC)

Συνεχίζουμε να μόνο τα μισά αλίευσαν μας ρύθμιση των μας webapp στο WEB-Πληροφορίες. Στη συνέχεια θα πρέπει να δημιουργήσετε τα ίδια τα αρχεία σελίδες διακομιστή Java που μας webapp θα εκτελεστεί που θα σας με υπόδειξη στο με τη ρύθμιση παραμέτρων.

Εάν θυμάστε, θα σας πει Java σε μας xml αρχεία ρύθμισης παραμέτρων που έχουμε μια `/` πόρων που θα πρέπει να φορτώσετε τα αρχεία .jsp, και μια `/secure` πόρων που θα πρέπει να διέρχονται από ένα φίλτρο που θα σας ονομάζεται `BasicFilter`. 

Ας κάνουμε αυτές τώρα.

-   Για να ξεκινήσετε, δημιουργήστε το `index.jsp` αρχείο που βρίσκεται στην περιοχή `\webapp\`, και αποκοπή και επικολλήστε τα εξής:

```jsp
<html>
<body>
    <h2>Hello World!</h2>
    <ul>
    <li><a href="secure/aad">Secure Page</a></li>
    </ul>
</body>
</html>

```

Ανακατευθύνει απλώς σε ασφαλή σελίδα που προστατεύεται από το φίλτρο.

- Στη συνέχεια, στο το ίδιο καταλόγου, σάς επιτρέπει να δημιουργήσετε ένα `error.jsp` αρχείο για να ενημερωθείτε τυχόν σφάλματα που μπορεί να συμβεί:

```jsp
<html>
<body>
    <h2>ERROR PAGE!</h2>
    <p>
        Exception -
        <%=request.getAttribute("error")%></p>
    <ul>
        <li><a href="<%=request.getContextPath()%>/index.jsp">Go Home</a></li>
    </ul>
</body>
</html>
```

- Τέλος, ας κάνουμε αυτό ασφαλή ιστοσελίδα θέλουμε, δημιουργώντας ένα φάκελο στην περιοχή `\webapp` που ονομάζεται `\secure` έτσι, ώστε ο κατάλογος μας τώρα `\webapp\secure`. 

- Μέσα σε αυτόν τον κατάλογο, στη συνέχεια, δημιουργήστε μια `aad.jsp` αρχείου και αποκοπή και επικολλήστε τα εξής:

```jsp
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>AAD Secure Page</title>
</head>
<body>

    <h1>Directory - Users List</h1>
    <p>${users}</p>

    <ul>
        <li><a href="<%=request.getContextPath()%>/secure/aad?cc=1">Get
                new Access Token via Client Credentials</a></li>
    </ul>
    <ul>
        <li><a href="<%=request.getContextPath()%>/secure/aad?refresh=1">Get
                new Access Token via Refresh Token</a></li>
    </ul>
    <ul>
        <li><a href="<%=request.getContextPath()%>/index.jsp">Go Home</a></li>
    </ul>
</body>
</html>
```

Μπορείτε να δείτε ότι αυτή η σελίδα θα σας ανακατευθύνουν σε συγκεκριμένες αιτήσεις που μας servlet BasicFilter θα ανάγνωσης και, στη συνέχεια, εκτέλεσης σχετικά με τη χρήση του `ADAJ4J` βιβλιοθήκη. Προτιμάτε να κάνετε απλή, huh;

Φυσικά, πρέπει τώρα πρέπει να ορίσετε μας αρχεία Java έτσι ώστε το servlet να κάνετε την εργασία.

## <a name="5-create-some-java-helper-files-for-basicfilter-mvc"></a>5. Δημιουργία ορισμένα αρχεία Βοήθειας Java (για BasicFilter MVC)

Στόχος μας είναι να δημιουργήσετε ορισμένα αρχεία Java που θα:

1. Να επιτρέπεται για είσοδο και sign-out του χρήστη
2. Λάβετε ορισμένα δεδομένα σχετικά με το χρήστη.

> [AZURE.NOTE] 
Για να λάβετε δεδομένα σχετικά με το χρήστη, πρέπει να χρησιμοποιήσετε το API Graph από το Azure Active Directory. Το API Graph είναι μια ασφαλής webservice που μπορείτε να χρησιμοποιήσετε για να καταγράψετε δεδομένα σχετικά με την εταιρεία σας, συμπεριλαμβανομένων των μεμονωμένων χρηστών. Αυτό είναι καλύτερη από Προ-συμπλήρωση ευαίσθητα δεδομένα διακριτικά, όπως το εξασφαλίζει ότι ο χρήστης που σας ρωτά για τα δεδομένα είναι εξουσιοδοτημένοι και όλα τα άτομα που μπορεί να συμβεί για να καταγράψετε το διακριτικό (από μια jailbroken τηλέφωνο ή το web cache του προγράμματος περιήγησης σε μια επιφάνεια εργασίας) δεν θα λάβετε μια ομάδα σημαντικές λεπτομέρειες σχετικά με το χρήστη ή την εταιρεία.

Ας εγγραφή ορισμένα αρχεία Java για να εκτελέσετε αυτή την εργασία για μας:

1. Δημιουργήστε ένα φάκελο του ριζικού καταλόγου που ονομάζεται 'adal4jsample' για την αποθήκευση όλων των αρχείων μας java. 

Θα χρησιμοποιήσουμε το χώρο ονομάτων `com.microsoft.aad.adal4jsample` σε αρχεία μας java. Οι περισσότερες IDEs δημιουργήσετε μια δομή ένθετη φακέλου για αυτό (π.χ. `/com/microsoft/aad/adal4jsample`). Που είναι δωρεάν να το κάνετε αυτό, αλλά δεν είναι απαραίτητο.

2. Μέσα σε αυτόν το φάκελο, δημιουργήστε ένα αρχείο που ονομάζεται `JSONHelper.java` που θα χρησιμοποιηθούν για τη Βοηθήστε μας να ανάλυση από δεδομένων JSON από μας διακριτικά. Μπορείτε να αποκοπή και επικολλήστε αυτή από τις παρακάτω:

```Java

package com.microsoft.aad.adal4jsample;

import java.lang.reflect.Field;
import java.util.Arrays;
import java.util.Enumeration;
import java.util.List;

import javax.servlet.http.HttpServletRequest;

import org.apache.commons.lang3.text.WordUtils;
import org.apache.log4j.Logger;
import org.json.JSONArray;
import org.json.JSONException;
import org.json.JSONObject;

/**
 * This class provides the methods to parse JSON Data from a JSON Formatted
 * String.
 * 
 * @author Azure Active Directory Contributor
 * 
 */
public class JSONHelper {

    private static Logger logger = Logger.getLogger(JSONHelper.class);

    JSONHelper() {
        // PropertyConfigurator.configure("log4j.properties");
    }

    /**
     * This method parses an JSON Array out of a collection of JSON Objects
     * within a string.
     * 
     * @param jSonData
     *            The JSON String that holds the collection.
     * @return An JSON Array that would contains all the collection object.
     * @throws Exception
     */
    public static JSONArray fetchDirectoryObjectJSONArray(JSONObject jsonObject) throws Exception {
        JSONArray jsonArray = new JSONArray();
        jsonArray = jsonObject.optJSONObject("responseMsg").optJSONArray("value");
        return jsonArray;
    }

    /**
     * This method parses an JSON Object out of a collection of JSON Objects
     * within a string
     * 
     * @param jsonObject
     * @return An JSON Object that would contains the DirectoryObject.
     * @throws Exception
     */
    public static JSONObject fetchDirectoryObjectJSONObject(JSONObject jsonObject) throws Exception {
        JSONObject jObj = new JSONObject();
        jObj = jsonObject.optJSONObject("responseMsg");
        return jObj;
    }

    /**
     * This method parses the skip token from a json formatted string.
     * 
     * @param jsonData
     *            The JSON Formatted String.
     * @return The skipToken.
     * @throws Exception
     */
    public static String fetchNextSkiptoken(JSONObject jsonObject) throws Exception {
        String skipToken = "";
        // Parse the skip token out of the string.
        skipToken = jsonObject.optJSONObject("responseMsg").optString("odata.nextLink");

        if (!skipToken.equalsIgnoreCase("")) {
            // Remove the unnecessary prefix from the skip token.
            int index = skipToken.indexOf("$skiptoken=") + (new String("$skiptoken=")).length();
            skipToken = skipToken.substring(index);
        }
        return skipToken;
    }

    /**
     * @param jsonObject
     * @return
     * @throws Exception
     */
    public static String fetchDeltaLink(JSONObject jsonObject) throws Exception {
        String deltaLink = "";
        // Parse the skip token out of the string.
        deltaLink = jsonObject.optJSONObject("responseMsg").optString("aad.deltaLink");
        if (deltaLink == null || deltaLink.length() == 0) {
            deltaLink = jsonObject.optJSONObject("responseMsg").optString("aad.nextLink");
            logger.info("deltaLink empty, nextLink ->" + deltaLink);

        }
        if (!deltaLink.equalsIgnoreCase("")) {
            // Remove the unnecessary prefix from the skip token.
            int index = deltaLink.indexOf("deltaLink=") + (new String("deltaLink=")).length();
            deltaLink = deltaLink.substring(index);
        }
        return deltaLink;
    }

    /**
     * This method would create a string consisting of a JSON document with all
     * the necessary elements set from the HttpServletRequest request.
     * 
     * @param request
     *            The HttpServletRequest
     * @return the string containing the JSON document.
     * @throws Exception
     *             If there is any error processing the request.
     */
    public static String createJSONString(HttpServletRequest request, String controller) throws Exception {
        JSONObject obj = new JSONObject();
        try {
            Field[] allFields = Class.forName(
                    "com.microsoft.windowsazure.activedirectory.sdk.graph.models." + controller).getDeclaredFields();
            String[] allFieldStr = new String[allFields.length];
            for (int i = 0; i < allFields.length; i++) {
                allFieldStr[i] = allFields[i].getName();
            }
            List<String> allFieldStringList = Arrays.asList(allFieldStr);
            Enumeration<String> fields = request.getParameterNames();

            while (fields.hasMoreElements()) {

                String fieldName = fields.nextElement();
                String param = request.getParameter(fieldName);
                if (allFieldStringList.contains(fieldName)) {
                    if (param == null || param.length() == 0) {
                        if (!fieldName.equalsIgnoreCase("password")) {
                            obj.put(fieldName, JSONObject.NULL);
                        }
                    } else {
                        if (fieldName.equalsIgnoreCase("password")) {
                            obj.put("passwordProfile", new JSONObject("{\"password\": \"" + param + "\"}"));
                        } else {
                            obj.put(fieldName, param);

                        }
                    }
                }
            }
        } catch (JSONException e) {
            e.printStackTrace();
        } catch (SecurityException e) {
            e.printStackTrace();
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
        return obj.toString();
    }

    /**
     * 
     * @param key
     * @param value
     * @return string format of this JSON obje
     * @throws Exception
     */
    public static String createJSONString(String key, String value) throws Exception {

        JSONObject obj = new JSONObject();
        try {
            obj.put(key, value);
        } catch (JSONException e) {
            e.printStackTrace();
        }

        return obj.toString();
    }

    /**
     * This is a generic method that copies the simple attribute values from an
     * argument jsonObject to an argument generic object.
     * 
     * @param jsonObject
     *            The jsonObject from where the attributes are to be copied.
     * @param destObject
     *            The object where the attributes should be copied into.
     * @throws Exception
     *             Throws a Exception when the operation are unsuccessful.
     */
    public static <T> void convertJSONObjectToDirectoryObject(JSONObject jsonObject, T destObject) throws Exception {

        // Get the list of all the field names.
        Field[] fieldList = destObject.getClass().getDeclaredFields();

        // For all the declared field.
        for (int i = 0; i < fieldList.length; i++) {
            // If the field is of type String, that is
            // if it is a simple attribute.
            if (fieldList[i].getType().equals(String.class)) {
                // Invoke the corresponding set method of the destObject using
                // the argument taken from the jsonObject.
                destObject
                        .getClass()
                        .getMethod(String.format("set%s", WordUtils.capitalize(fieldList[i].getName())),
                                new Class[] { String.class })
                        .invoke(destObject, new Object[] { jsonObject.optString(fieldList[i].getName()) });
            }
        }
    }

    public static JSONArray joinJSONArrays(JSONArray a, JSONArray b) {
        JSONArray comb = new JSONArray();
        for (int i = 0; i < a.length(); i++) {
            comb.put(a.optJSONObject(i));
        }
        for (int i = 0; i < b.length(); i++) {
            comb.put(b.optJSONObject(i));
        }
        return comb;
    }

}

```

3. Στη συνέχεια, δημιουργήστε ένα αρχείο που ονομάζεται `HttpClientHelper.java` που θα χρησιμοποιηθούν για τη Βοηθήστε μας να ανάλυση από HTTP δεδομένα από το τελικό σημείο AAD. Μπορείτε να αποκοπή και επικολλήστε αυτή από τις παρακάτω:

```Java

package com.microsoft.aad.adal4jsample;

import java.io.BufferedReader;
import java.io.ByteArrayOutputStream;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.net.HttpURLConnection;

import org.json.JSONException;
import org.json.JSONObject;

/**
 * This is Helper class for all RestClient class.
 * 
 * @author Azure Active Directory Contributor
 * 
 */
public class HttpClientHelper {

    public HttpClientHelper() {
        super();
    }

    public static String getResponseStringFromConn(HttpURLConnection conn, boolean isSuccess) throws IOException {

        BufferedReader reader = null;
        if (isSuccess) {
            reader = new BufferedReader(new InputStreamReader(conn.getInputStream()));
        } else {
            reader = new BufferedReader(new InputStreamReader(conn.getErrorStream()));
        }
        StringBuffer stringBuffer = new StringBuffer();
        String line = "";
        while ((line = reader.readLine()) != null) {
            stringBuffer.append(line);
        }

        return stringBuffer.toString();
    }

    public static String getResponseStringFromConn(HttpURLConnection conn, String payLoad) throws IOException {

        // Send the http message payload to the server.
        if (payLoad != null) {
            conn.setDoOutput(true);
            OutputStreamWriter osw = new OutputStreamWriter(conn.getOutputStream());
            osw.write(payLoad);
            osw.flush();
            osw.close();
        }

        // Get the message response from the server.
        BufferedReader br = new BufferedReader(new InputStreamReader(conn.getInputStream()));
        String line = "";
        StringBuffer stringBuffer = new StringBuffer();
        while ((line = br.readLine()) != null) {
            stringBuffer.append(line);
        }

        br.close();

        return stringBuffer.toString();
    }

    public static byte[] getByteaArrayFromConn(HttpURLConnection conn, boolean isSuccess) throws IOException {

        InputStream is = conn.getInputStream();
        ByteArrayOutputStream baos = new ByteArrayOutputStream();
        byte[] buff = new byte[1024];
        int bytesRead = 0;

        while ((bytesRead = is.read(buff, 0, buff.length)) != -1) {
            baos.write(buff, 0, bytesRead);
        }

        byte[] bytes = baos.toByteArray();
        baos.close();
        return bytes;
    }

    /**
     * for bad response, whose responseCode is not 200 level
     * 
     * @param responseCode
     * @param errorCode
     * @param errorMsg
     * @return
     * @throws JSONException
     */
    public static JSONObject processResponse(int responseCode, String errorCode, String errorMsg) throws JSONException {
        JSONObject response = new JSONObject();
        response.put("responseCode", responseCode);
        response.put("errorCode", errorCode);
        response.put("errorMsg", errorMsg);

        return response;
    }

    /**
     * for bad response, whose responseCode is not 200 level
     * 
     * @param responseCode
     * @param errorCode
     * @param errorMsg
     * @return
     * @throws JSONException
     */
    public static JSONObject processGoodRespStr(int responseCode, String goodRespStr) throws JSONException {
        JSONObject response = new JSONObject();
        response.put("responseCode", responseCode);
        if (goodRespStr.equalsIgnoreCase("")) {
            response.put("responseMsg", "");
        } else {
            response.put("responseMsg", new JSONObject(goodRespStr));
        }

        return response;
    }

    /**
     * for good response
     * 
     * @param responseCode
     * @param responseMsg
     * @return
     * @throws JSONException
     */
    public static JSONObject processBadRespStr(int responseCode, String responseMsg) throws JSONException {

        JSONObject response = new JSONObject();
        response.put("responseCode", responseCode);
        if (responseMsg.equalsIgnoreCase("")) { // good response is empty string
            response.put("responseMsg", "");
        } else { // bad response is json string
            JSONObject errorObject = new JSONObject(responseMsg).optJSONObject("odata.error");

            String errorCode = errorObject.optString("code");
            String errorMsg = errorObject.optJSONObject("message").optString("value");
            response.put("responseCode", responseCode);
            response.put("errorCode", errorCode);
            response.put("errorMsg", errorMsg);
        }

        return response;
    }

}

```

## <a name="6-create-the-java-graph-api-model-files-for-basicfilter-mvc"></a>6. Δημιουργία java της μοντέλο API Graph αρχείων (για BasicFilter MVC)

Όπως φαίνεται παραπάνω, θα χρησιμοποιήσουμε το API του γραφήματος για να λάβετε δεδομένα σχετικά με ο συνδεδεμένος χρήστης. Για να είναι εύκολο για μας αυτό θα πρέπει να δημιουργήσουμε ένα αρχείο για να αντιπροσωπεύει ένα **Αντικείμενο καταλόγου** και ένα μεμονωμένο αρχείο για να αντιπροσωπεύει το **χρήστη** , ώστε να μπορεί να χρησιμοποιηθεί το μοτίβο OO της Java.

1. Δημιουργήστε ένα αρχείο που ονομάζεται `DirectoryObject.java` οποίοι θα χρησιμοποιηθούν για την αποθήκευση βασικά δεδομένα σχετικά με οποιαδήποτε DirectoryObject (που μπορεί να μην διστάσεις να χρησιμοποιήστε αυτήν την επιλογή αργότερα για οποιαδήποτε άλλα ερωτήματα γραφήματος μπορείτε να το κάνετε). Μπορείτε να αποκοπή και επικολλήστε αυτή από τις παρακάτω:

```Java

package com.microsoft.aad.adal4jsample;

/**
 * @author Azure Active Directory Contributor
 *
 */
public abstract class DirectoryObject {
    
    public DirectoryObject() {
        super();
    }
    
    /**
     * 
     * @return
     */
    public abstract String getObjectId();
    
    /**
     * @param objectId
     */
    public abstract void setObjectId(String objectId);

    /**
     * 
     * @return
     */
    public abstract String getObjectType();

    /**
     * 
     * @param objectType
     */
    public abstract void setObjectType(String objectType);
    
    /**
     * 
     * @return
     */
    public abstract String getDisplayName();

    /**
     * 
     * @param displayName
     */
    public abstract void setDisplayName(String displayName);

}

```

2. Δημιουργήστε ένα αρχείο που ονομάζεται `User.java` οποίοι θα χρησιμοποιηθούν για την αποθήκευση βασικά δεδομένα σχετικά με οποιονδήποτε χρήστη από τον κατάλογο. Ξανά, αυτό είναι πολύ βασικά στοιχεία επικέντρωσης/setters για καταλόγου δεδομένων, ώστε να μπορείτε να αποκοπή και επικολλήστε αυτή από τις παρακάτω:

```Java

package com.microsoft.aad.adal4jsample;

import java.security.acl.Group;
import java.util.ArrayList;

import javax.xml.bind.annotation.XmlRootElement;

import org.json.JSONObject;

/**
 *  The User Class holds together all the members of a WAAD User entity and all the access methods and set methods
 *  @author Azure Active Directory Contributor
 */
@XmlRootElement
public class User extends DirectoryObject{
    
    // The following are the individual private members of a User object that holds
    // a particular simple attribute of an User object.
    protected String objectId;
    protected String objectType;
    protected String accountEnabled;
    protected String city;
    protected String country;
    protected String department;
    protected String dirSyncEnabled;
    protected String displayName;
    protected String facsimileTelephoneNumber;
    protected String givenName;
    protected String jobTitle;
    protected String lastDirSyncTime;
    protected String mail;
    protected String mailNickname;
    protected String mobile;
    protected String password;
    protected String passwordPolicies;
    protected String physicalDeliveryOfficeName;
    protected String postalCode;
    protected String preferredLanguage;
    protected String state;
    protected String streetAddress;
    protected String surname;
    protected String telephoneNumber;
    protected String usageLocation;
    protected String userPrincipalName;
    protected boolean isDeleted;  // this will move to dto

    /**
     * below 4 properties are for future use
     */
    // managerDisplayname of this user
    protected String managerDisplayname;
    
    // The directReports holds a list of directReports
    private ArrayList<User> directReports;
    
    // The groups holds a list of group entity this user belongs to. 
    private ArrayList<Group> groups;
    
    // The roles holds a list of role entity this user belongs to. 
    private ArrayList<Group> roles;
    
    
    /**
     * The constructor for the User class. Initializes the dynamic lists and managerDisplayname variables.
     */
    public User(){
        directReports = null;
        groups = new ArrayList<Group>();
        roles = new ArrayList<Group>();
        managerDisplayname = null;
    }
//  
//  public User(String displayName, String objectId){
//      setDisplayName(displayName);
//      setObjectId(objectId);
//  }
//  
//  public User(String displayName, String objectId, String userPrincipalName, String accountEnabled){
//      setDisplayName(displayName);
//      setObjectId(objectId);
//      setUserPrincipalName(userPrincipalName);
//      setAccountEnabled(accountEnabled);
//  }
//  

    /**
     * @return The objectId of this user.
     */
    public String getObjectId() {
        return objectId;
    }
    
    /**
     * @param objectId The objectId to set to this User object.
     */
    public void setObjectId(String objectId) {
        this.objectId = objectId;
    }


    /**
     * @return The objectType of this User.
     */
    public String getObjectType() {
        return objectType;
    }

    /**
     * @param objectType The objectType to set to this User object.
     */
    public void setObjectType(String objectType) {
        this.objectType = objectType;
    }

    /**
     * @return The userPrincipalName of this User.
     */
    public String getUserPrincipalName() {
        return userPrincipalName;
    }

    /**
     * @param userPrincipalName The userPrincipalName to set to this User object.
     */
    public void setUserPrincipalName(String userPrincipalName) {
        this.userPrincipalName = userPrincipalName;
    }

    
    /**
     * @return The usageLocation of this User.
     */
    public String getUsageLocation() {
        return usageLocation;
    }

    /**
     * @param usageLocation The usageLocation to set to this User object.
     */
    public void setUsageLocation(String usageLocation) {
        this.usageLocation = usageLocation;
    }

    /**
     * @return The telephoneNumber of this User.
     */
    public String getTelephoneNumber() {
        return telephoneNumber;
    }

    /**
     * @param telephoneNumber The telephoneNumber to set to this User object.
     */
    public void setTelephoneNumber(String telephoneNumber) {
        this.telephoneNumber = telephoneNumber;
    }

    /**
     * @return The surname of this User.
     */
    public String getSurname() {
        return surname;
    }

    /**
     * @param surname The surname to set to this User Object.
     */
    public void setSurname(String surname) {
        this.surname = surname;
    }

    /**
     * @return The streetAddress of this User.
     */
    public String getStreetAddress() {
        return streetAddress;
    }

    /**
     * @param streetAddress The streetAddress to set to this User.
     */
    public void setStreetAddress(String streetAddress) {
        this.streetAddress = streetAddress;
    }

    /**
     * @return The state of this User.
     */
    public String getState() {
        return state;
    }

    /**
     * @param state The state to set to this User object.
     */
    public void setState(String state) {
        this.state = state;
    }

    /**
     * @return The preferredLanguage of this User.
     */
    public String getPreferredLanguage() {
        return preferredLanguage;
    }

    /**
     * @param preferredLanguage The preferredLanguage to set to this User.
     */
    public void setPreferredLanguage(String preferredLanguage) {
        this.preferredLanguage = preferredLanguage;
    }

    /**
     * @return The postalCode of this User.
     */
    public String getPostalCode() {
        return postalCode;
    }

    /**
     * @param postalCode The postalCode to set to this User.
     */
    public void setPostalCode(String postalCode) {
        this.postalCode = postalCode;
    }

    /**
     * @return The physicalDeliveryOfficeName of this User.
     */
    public String getPhysicalDeliveryOfficeName() {
        return physicalDeliveryOfficeName;
    }

    /**
     * @param physicalDeliveryOfficeName The physicalDeliveryOfficeName to set to this User Object.
     */
    public void setPhysicalDeliveryOfficeName(String physicalDeliveryOfficeName) {
        this.physicalDeliveryOfficeName = physicalDeliveryOfficeName;
    }

    /**
     * @return The passwordPolicies of this User.
     */
    public String getPasswordPolicies() {
        return passwordPolicies;
    }

    /**
     * @param passwordPolicies The passwordPolicies to set to this User object.
     */
    public void setPasswordPolicies(String passwordPolicies) {
        this.passwordPolicies = passwordPolicies;
    }

    /**
     * @return The mobile of this User.
     */
    public String getMobile() {
        return mobile;
    }

    /**
     * @param mobile The mobile to set to this User object.
     */
    public void setMobile(String mobile) {
        this.mobile = mobile;
    }
    
    /**
     * @return The Password of this User.
     */
    public String getPassword() {
        return password;
    }

    /**
     * @param password The mobile to set to this User object.
     */
    public void setPassword(String password) {
        this.password = password;
    }

    /**
     * @return The mail of this User.
     */
    public String getMail() {
        return mail;
    }

    /**
     * @param mail The mail to set to this User object.
     */
    public void setMail(String mail) {
        this.mail = mail;
    }
    
    /**
     * @return The MailNickname of this User.
     */
    public String getMailNickname() {
        return mailNickname;
    }

    /**
     * @param mail The MailNickname to set to this User object.
     */
    public void setMailNickname(String mailNickname) {
        this.mailNickname = mailNickname;
    }


    /**
     * @return The jobTitle of this User.
     */
    public String getJobTitle() {
        return jobTitle;
    }

    /**
     * @param jobTitle The jobTitle to set to this User Object.
     */
    public void setJobTitle(String jobTitle) {
        this.jobTitle = jobTitle;
    }

    /**
     * @return The givenName of this User.
     */
    public String getGivenName() {
        return givenName;
    }

    /**
     * @param givenName The givenName to set to this User.
     */
    public void setGivenName(String givenName) {
        this.givenName = givenName;
    }

    /**
     * @return The facsimileTelephoneNumber of this User.
     */
    public String getFacsimileTelephoneNumber() {
        return facsimileTelephoneNumber;
    }

    /**
     * @param facsimileTelephoneNumber The facsimileTelephoneNumber to set to this User Object.
     */
    public void setFacsimileTelephoneNumber(String facsimileTelephoneNumber) {
        this.facsimileTelephoneNumber = facsimileTelephoneNumber;
    }

    /**
     * @return The displayName of this User.
     */
    public String getDisplayName() {
        return displayName;
    }

    /**
     * @param displayName The displayName to set to this User Object.
     */
    public void setDisplayName(String displayName) {
        this.displayName = displayName;
    }

    /**
     * @return The dirSyncEnabled of this User.
     */
    public String getDirSyncEnabled() {
        return dirSyncEnabled;
    }

    /**
     * @param dirSyncEnabled The dirSyncEnabled to set to this User.
     */
    public void setDirSyncEnabled(String dirSyncEnabled) {
        this.dirSyncEnabled = dirSyncEnabled;
    }

    /**
     * @return The department of this User.
     */
    public String getDepartment() {
        return department;
    }

    /**
     * @param department The department to set to this User.
     */
    public void setDepartment(String department) {
        this.department = department;
    }

    /**
     * @return The lastDirSyncTime of this User.
     */
    public String getLastDirSyncTime() {
        return lastDirSyncTime;
    }

    /**
     * @param lastDirSyncTime The lastDirSyncTime to set to this User.
     */
    public void setLastDirSyncTime(String lastDirSyncTime) {
        this.lastDirSyncTime = lastDirSyncTime;
    }

    /**
     * @return The country of this User.
     */
    public String getCountry() {
        return country;
    }

    /**
     * @param country The country to set to this User.
     */
    public void setCountry(String country) {
        this.country = country;
    }

    /**
     * @return The city of this User.
     */
    public String getCity() {
        return city;
    }

    /**
     * @param city The city to set to this User.
     */
    public void setCity(String city) {
        this.city = city;
    }

    /**
     * @return The accountEnabled attribute of this User.
     */
    public String getAccountEnabled() {
        return accountEnabled;
    }

    /**
     * @param accountEnabled The accountEnabled to set to this User.
     */
    public void setAccountEnabled(String accountEnabled) {
        this.accountEnabled = accountEnabled;
    }
    
    public boolean isIsDeleted() {
        return this.isDeleted;
    }

    public void setIsDeleted(boolean isDeleted) {
        this.isDeleted = isDeleted;
    }

    @Override
    public String toString() {
        return new JSONObject(this).toString();
    }
    
    public String getManagerDisplayname(){
        return managerDisplayname;
    }
    
    public void setManagerDisplayname(String managerDisplayname){
        this.managerDisplayname = managerDisplayname;
    }
}


/**
 * The Class DirectReports Holds the essential data for a single DirectReport entry. Namely,
 * it holds the displayName and the objectId of the direct entry. Furthermore, it provides the
 * access methods to set or get the displayName and the ObjectId of this entry.
 */
//class DirectReport extends User{
//
//  private String displayName;
//  private String objectId;
//   
//  /**
//   * Two arguments Constructor for the DirectReport Class.
//   * @param displayName
//   * @param objectId
//   */
//  public DirectReport(String displayName, String objectId){
//      this.displayName = displayName;
//      this.objectId = objectId;
//  }
//
//  /**
//   * @return The diaplayName of this direct report entry.
//   */
//  public String getDisplayName() {
//      return displayName;
//  }
//
//  
//  /**
//   *  @return The objectId of this direct report entry. 
//   */
//  public String getObjectId() {
//      return objectId;
//  }
//
//}

```

## <a name="7-create-the-authentication-modelcontroller-files-for-basicfilter"></a>7. Δημιουργία τα αρχεία ελέγχου ταυτότητας μοντέλο/ελεγκτή (για BasicFilter)

Ναι, Java είναι προτιμάτε λεπτομερές, αλλά θα σας σχεδόν είστε έτοιμοι. Δίπλα στην επιλογή επώνυμο, πριν από την μας γράψετε το servlet BasicFilter χειρισμού των αιτήσεων μας, ας γράψετε ορισμένες περισσότερες Βοήθειας αρχεία στα οποία το `ADAL4J` βιβλιοθήκη χρειάζεται. 

1. Δημιουργήστε ένα αρχείο που ονομάζεται `AuthHelper.java` που θα σας δώσει μας μεθόδους που θα χρησιμοποιήσουμε για να προσδιορίσετε την κατάσταση της συνδεδεμένος χρήσης. Περιλαμβάνουν τα εξής:

- `isAuthenticated()`μέθοδος η οποία επιστρέφει εάν ο χρήστης είναι συνδεδεμένος ή όχι
- `containsAuthenticationData()`που θα πείτε μας εάν το διακριτικό έχει δεδομένα ή όχι
- `isAuthenticationSuccessful()`μας που θα σας ενημερώσει εάν ο έλεγχος ταυτότητας ολοκληρώθηκε με επιτυχία για το χρήστη.

Αποκοπή και επικολλήστε τον παρακάτω κώδικα:

```Java
package com.microsoft.aad.adal4jsample;

import java.util.Map;

import javax.servlet.http.HttpServletRequest;

import com.microsoft.aad.adal4j.AuthenticationResult;
import com.nimbusds.openid.connect.sdk.AuthenticationResponse;
import com.nimbusds.openid.connect.sdk.AuthenticationResponseParser;
import com.nimbusds.openid.connect.sdk.AuthenticationSuccessResponse;

public final class AuthHelper {

    public static final String PRINCIPAL_SESSION_NAME = "principal";

    private AuthHelper() {
    }

    public static boolean isAuthenticated(HttpServletRequest request) {
        return request.getSession().getAttribute(PRINCIPAL_SESSION_NAME) != null;
    }

    public static AuthenticationResult getAuthSessionObject(
            HttpServletRequest request) {
        return (AuthenticationResult) request.getSession().getAttribute(
                PRINCIPAL_SESSION_NAME);
    }

    public static boolean containsAuthenticationData(
            HttpServletRequest httpRequest) {
        Map<String, String[]> map = httpRequest.getParameterMap();
        return httpRequest.getMethod().equalsIgnoreCase("POST") && (httpRequest.getParameterMap().containsKey(
                        AuthParameterNames.ERROR)
                        || httpRequest.getParameterMap().containsKey(
                                AuthParameterNames.ID_TOKEN) || httpRequest
                        .getParameterMap().containsKey(AuthParameterNames.CODE));
    }

    public static boolean isAuthenticationSuccessful(
            AuthenticationResponse authResponse) {
        return authResponse instanceof AuthenticationSuccessResponse;
    }
}
```

2. Δημιουργήστε ένα αρχείο που ονομάζεται `AuthParameterNames.java` που θα σας δώσει μας ορισμένες αμετάβλητες μεταβλητές `ADAL4J` θα απαιτούν. Αποκοπή/pate τα εξής:

```Java
package com.microsoft.aad.adal4jsample;

public final class AuthParameterNames {

    private AuthParameterNames() {
    }

    public static String ERROR = "error";
    public static String ERROR_DESCRIPTION = "error_description";
    public static String ERROR_URI = "error_uri";
    public static String ID_TOKEN = "id_token";
    public static String CODE = "code";
}
```

3. Τέλος, να δημιουργήσετε ένα αρχείο που ονομάζεται `AadController.java` που είναι του ελεγκτή μας μοτίβου MVC που θα σας δώσει μας μας ελεγκτή JSP και εκθέτει το `secure/aad` τελικού σημείου διεύθυνση URL για την εφαρμογή. Επιπλέον, το ερώτημα graph θέτει σε αυτό το αρχείο.

Αποκοπή και επικολλήστε τα εξής:

```Java
package com.microsoft.aad.adal4jsample;

import java.net.HttpURLConnection;
import java.net.URL;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpSession;

import org.json.JSONArray;
import org.json.JSONObject;
import org.springframework.stereotype.Controller;
import org.springframework.ui.ModelMap;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;

import com.microsoft.aad.adal4j.AuthenticationResult;

@Controller
@RequestMapping("/secure/aad")
public class AadController {

    @RequestMapping(method = { RequestMethod.GET, RequestMethod.POST })
    public String getDirectoryObjects(ModelMap model, HttpServletRequest httpRequest) {
        HttpSession session = httpRequest.getSession();
        AuthenticationResult result = (AuthenticationResult) session.getAttribute(AuthHelper.PRINCIPAL_SESSION_NAME);
        if (result == null) {
            model.addAttribute("error", new Exception("AuthenticationResult not found in session."));
            return "/error";
        } else {
            String data;
            try {
                data = this.getUsernamesFromGraph(result.getAccessToken(), session.getServletContext()
                        .getInitParameter("tenant"));
                model.addAttribute("users", data);
            } catch (Exception e) {
                model.addAttribute("error", e);
                return "/error";
            }
        }
        return "/secure/aad";
    }

    private String getUsernamesFromGraph(String accessToken, String tenant) throws Exception {
        URL url = new URL(String.format("https://graph.windows.net/%s/users?api-version=2013-04-05", tenant,
                accessToken));

        HttpURLConnection conn = (HttpURLConnection) url.openConnection();
        // Set the appropriate header fields in the request header.
        conn.setRequestProperty("api-version", "2013-04-05");
        conn.setRequestProperty("Authorization", accessToken);
        conn.setRequestProperty("Accept", "application/json;odata=minimalmetadata");
        String goodRespStr = HttpClientHelper.getResponseStringFromConn(conn, true);
        // logger.info("goodRespStr ->" + goodRespStr);
        int responseCode = conn.getResponseCode();
        JSONObject response = HttpClientHelper.processGoodRespStr(responseCode, goodRespStr);
        JSONArray users = new JSONArray();

        users = JSONHelper.fetchDirectoryObjectJSONArray(response);

        StringBuilder builder = new StringBuilder();
        User user = null;
        for (int i = 0; i < users.length(); i++) {
            JSONObject thisUserJSONObject = users.optJSONObject(i);
            user = new User();
            JSONHelper.convertJSONObjectToDirectoryObject(thisUserJSONObject, user);
            builder.append(user.getUserPrincipalName() + "<br/>");
        }
        return builder.toString();
    }

}

```

## <a name="8-create-the-basicfilter-file-for-basicfilter-mvc"></a>8. δημιουργήσετε το αρχείο BasicFilter (για BasicFilter MVC)

Συνεχίζουμε να τέλος είστε έτοιμοι να δημιουργήσετε το αρχείο BasicFilter για να χειριστείτε μας αιτήσεις από την προβολή (JSP αρχεία).

Δημιουργήστε ένα αρχείο που ονομάζεται `BasicFilter.java` που περιέχει τα εξής:

```Java

package com.microsoft.aad.adal4jsample;

import java.io.IOException;
import java.io.UnsupportedEncodingException;
import java.net.URI;
import java.net.URLEncoder;
import java.util.Date;
import java.util.HashMap;
import java.util.Map;
import java.util.UUID;
import java.util.concurrent.ExecutionException;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.Future;

import javax.naming.ServiceUnavailableException;
import javax.servlet.Filter;
import javax.servlet.FilterChain;
import javax.servlet.FilterConfig;
import javax.servlet.ServletException;
import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import com.microsoft.aad.adal4j.AuthenticationContext;
import com.microsoft.aad.adal4j.AuthenticationResult;
import com.microsoft.aad.adal4j.ClientCredential;
import com.nimbusds.oauth2.sdk.AuthorizationCode;
import com.nimbusds.openid.connect.sdk.AuthenticationErrorResponse;
import com.nimbusds.openid.connect.sdk.AuthenticationResponse;
import com.nimbusds.openid.connect.sdk.AuthenticationResponseParser;
import com.nimbusds.openid.connect.sdk.AuthenticationSuccessResponse;

public class BasicFilter implements Filter {

    private String clientId = "";
    private String clientSecret = "";
    private String tenant = "";
    private String authority;

    public void destroy() {

    }

    public void doFilter(ServletRequest request, ServletResponse response,
            FilterChain chain) throws IOException, ServletException {

        if (request instanceof HttpServletRequest) {
            HttpServletRequest httpRequest = (HttpServletRequest) request;
            HttpServletResponse httpResponse = (HttpServletResponse) response;
            try {

                String currentUri = request.getScheme()
                        + "://"
                        + request.getServerName()
                        + ("http".equals(request.getScheme())
                                && request.getServerPort() == 80
                                || "https".equals(request.getScheme())
                                && request.getServerPort() == 443 ? "" : ":"
                                + request.getServerPort())
                        + httpRequest.getRequestURI();
                String fullUrl = currentUri
                        + (httpRequest.getQueryString() != null ? "?"
                                + httpRequest.getQueryString() : "");
                // check if user has a session
                if (!AuthHelper.isAuthenticated(httpRequest)) {
                    if (AuthHelper.containsAuthenticationData(httpRequest)) {
                        Map<String, String> params = new HashMap<String, String>();
                        for (String key : request.getParameterMap().keySet()) {
                            params.put(key,
                                    request.getParameterMap().get(key)[0]);
                        }
                        AuthenticationResponse authResponse = AuthenticationResponseParser
                                .parse(new URI(fullUrl), params);
                        if (AuthHelper.isAuthenticationSuccessful(authResponse)) {

                            AuthenticationSuccessResponse oidcResponse = (AuthenticationSuccessResponse) authResponse;
                            AuthenticationResult result = getAccessToken(
                                    oidcResponse.getAuthorizationCode(),
                                    currentUri);
                            createSessionPrincipal(httpRequest, result);
                        } else {
                            AuthenticationErrorResponse oidcResponse = (AuthenticationErrorResponse) authResponse;
                            throw new Exception(String.format(
                                    "Request for auth code failed: %s - %s",
                                    oidcResponse.getErrorObject().getCode(),
                                    oidcResponse.getErrorObject()
                                            .getDescription()));
                        }
                    } else {
                            // not authenticated
                            httpResponse.setStatus(302);
                            httpResponse
                                    .sendRedirect(getRedirectUrl(currentUri));
                            return;
                    }
                } else {
                    // if authenticated, how to check for valid session?
                    AuthenticationResult result = AuthHelper
                            .getAuthSessionObject(httpRequest);

                    if (httpRequest.getParameter("refresh") != null) {
                        result = getAccessTokenFromRefreshToken(
                                result.getRefreshToken(), currentUri);
                    } else {
                        if (httpRequest.getParameter("cc") != null) {
                            result = getAccessTokenFromClientCredentials();
                        } else {
                            if (result.getExpiresOnDate().before(new Date())) {
                                result = getAccessTokenFromRefreshToken(
                                        result.getRefreshToken(), currentUri);
                            }
                        }
                    }
                    createSessionPrincipal(httpRequest, result);
                }
            } catch (Throwable exc) {
                httpResponse.setStatus(500);
                request.setAttribute("error", exc.getMessage());
                httpResponse.sendRedirect(((HttpServletRequest) request)
                        .getContextPath() + "/error.jsp");
            }
        }
        chain.doFilter(request, response);
    }

    private AuthenticationResult getAccessTokenFromClientCredentials()
            throws Throwable {
        AuthenticationContext context = null;
        AuthenticationResult result = null;
        ExecutorService service = null;
        try {
            service = Executors.newFixedThreadPool(1);
            context = new AuthenticationContext(authority + tenant + "/", true,
                    service);
            Future<AuthenticationResult> future = context.acquireToken(
                    "https://graph.windows.net", new ClientCredential(clientId,
                            clientSecret), null);
            result = future.get();
        } catch (ExecutionException e) {
            throw e.getCause();
        } finally {
            service.shutdown();
        }

        if (result == null) {
            throw new ServiceUnavailableException(
                    "authentication result was null");
        }
        return result;
    }

    private AuthenticationResult getAccessTokenFromRefreshToken(
            String refreshToken, String currentUri) throws Throwable {
        AuthenticationContext context = null;
        AuthenticationResult result = null;
        ExecutorService service = null;
        try {
            service = Executors.newFixedThreadPool(1);
            context = new AuthenticationContext(authority + tenant + "/", true,
                    service);
            Future<AuthenticationResult> future = context
                    .acquireTokenByRefreshToken(refreshToken,
                            new ClientCredential(clientId, clientSecret), null,
                            null);
            result = future.get();
        } catch (ExecutionException e) {
            throw e.getCause();
        } finally {
            service.shutdown();
        }

        if (result == null) {
            throw new ServiceUnavailableException(
                    "authentication result was null");
        }
        return result;

    }

    private AuthenticationResult getAccessToken(
            AuthorizationCode authorizationCode, String currentUri)
            throws Throwable {
        String authCode = authorizationCode.getValue();
        ClientCredential credential = new ClientCredential(clientId,
                clientSecret);
        AuthenticationContext context = null;
        AuthenticationResult result = null;
        ExecutorService service = null;
        try {
            service = Executors.newFixedThreadPool(1);
            context = new AuthenticationContext(authority + tenant + "/", true,
                    service);
            Future<AuthenticationResult> future = context
                    .acquireTokenByAuthorizationCode(authCode, new URI(
                            currentUri), credential, null);
            result = future.get();
        } catch (ExecutionException e) {
            throw e.getCause();
        } finally {
            service.shutdown();
        }

        if (result == null) {
            throw new ServiceUnavailableException(
                    "authentication result was null");
        }
        return result;
    }

    private void createSessionPrincipal(HttpServletRequest httpRequest,
            AuthenticationResult result) throws Exception {
        httpRequest.getSession().setAttribute(
                AuthHelper.PRINCIPAL_SESSION_NAME, result);
    }

    private String getRedirectUrl(String currentUri)
            throws UnsupportedEncodingException {
        String redirectUrl = authority
                + this.tenant
                + "/oauth2/authorize?response_type=code%20id_token&scope=openid&response_mode=form_post&redirect_uri="
                + URLEncoder.encode(currentUri, "UTF-8") + "&client_id="
                + clientId + "&resource=https%3a%2f%2fgraph.windows.net"
                + "&nonce=" + UUID.randomUUID() + "&site_id=500879";
        return redirectUrl;
    }

    public void init(FilterConfig config) throws ServletException {
        clientId = config.getInitParameter("client_id");
        authority = config.getServletContext().getInitParameter("authority");
        tenant = config.getServletContext().getInitParameter("tenant");
        clientSecret = config.getInitParameter("secret_key");
    }

}
```

Σε αυτό το servlet εκθέτει όλες τις μεθόδους που `ADAL4J` θα περιμένετε από την εφαρμογή για να εκτελέσετε. Αυτό περιλαμβάνει:

- `getAccessTokenFromClientCredentials()`-αποκτά πρόσβαση διακριτικού από το μυστικό
- `getAccessTokenFromRefreshToken()`-αποκτά πρόσβαση διακριτικού από μια ανανέωση διακριτικού
- `getAccessToken()`-αποκτά πρόσβαση διακριτικού από μια σύνδεση OpenID τη ροή (το οποίο χρησιμοποιούμε)
- `createSessionPrincipal()`-δημιουργεί αρχής χρησιμοποιούμε για πρόσβαση Graph API
- `getRedirectUrl()`-λαμβάνει το redirectURL για να συγκρίνετε με την τιμή που καταχωρήσατε στην πύλη του.

##<a name="compile-and-run-the-sample-in-tomcat"></a>Μεταγλώττιση και να εκτελέσετε το δείγμα στο Tomcat

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

