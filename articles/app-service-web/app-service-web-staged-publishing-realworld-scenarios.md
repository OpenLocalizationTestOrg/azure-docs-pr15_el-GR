<properties
  pageTitle="Αποτελεσματική χρήση περιβάλλοντα DevOps για την εφαρμογή web"
  description="Μάθετε πώς να χρησιμοποιείτε υποδοχές ανάπτυξης για να ρυθμίσετε και να διαχειριστείτε πολλές περιβάλλοντα προγραμματισμού για την εφαρμογή σας"
  services="app-service\web"
  documentationCenter=""
  authors="sunbuild"
  manager="yochayk"
  editor=""/>

<tags
  ms.service="app-service"
  ms.devlang="na"
  ms.topic="article"
  ms.tgt_pltfrm="na"
  ms.workload="web"
  ms.date="10/24/2016"
  ms.author="sumuth"/>

# <a name="use-devops-environments-effectively-for-your-web-apps"></a>Αποτελεσματική χρήση περιβάλλοντα DevOps για τις εφαρμογές web

Αυτό το άρθρο περιγράφει τη ρύθμιση και διαχείριση αναπτύξεων εφαρμογών web για πολλαπλές εκδόσεις της εφαρμογής σας όπως ανάπτυξης, Ερωτήσεις και παραγωγής. Κάθε έκδοση της εφαρμογής σας να θεωρούνται ως περιβάλλον ανάπτυξης για συγκεκριμένες ανάγκες της εντός διαδικασίας ανάπτυξης. Για παράδειγμα Ερωτήσεις περιβάλλον μπορεί να χρησιμοποιηθεί από την ομάδα των προγραμματιστών για να ελέγξετε την ποιότητα της εφαρμογής πριν push τις αλλαγές σε παραγωγής.
Για τη ρύθμιση του πολλά περιβάλλοντα ανάπτυξης μπορεί να είναι δύσκολη εργασίας πρέπει να παρακολουθείτε, να διαχειριστείτε τους πόρους (υπολογισμού, εφαρμογή web, βάση δεδομένων, cache κ.λπ.) και ανάπτυξη κώδικα σε περιβάλλοντα.

## <a name="setting-up-a-non-production-environment-stagedevqa"></a>Ρύθμιση περιβάλλον μη παραγωγής (στάδιο, προγραμματιστές, Ερωτήσεις)
Όταν έχετε μια εφαρμογή web παραγωγής λειτουργία, το επόμενο βήμα είναι να δημιουργήσετε ένα περιβάλλον μη παραγωγής. Για να χρησιμοποιήσετε υποδοχές ανάπτυξης βεβαιωθείτε ότι χρησιμοποιείτε τη λειτουργία πρόγραμμα **τυπικής** ή **Premium** εφαρμογής υπηρεσίας. Ανάπτυξη υποδοχές είναι στην πραγματικότητα live web apps με το δικό τους ονόματα. Στοιχεία περιεχομένου και ρύθμισης παραμέτρων εφαρμογής Web μπορεί να γίνει αντιμετάθεση μεταξύ των δύο υποδοχές ανάπτυξης, συμπεριλαμβανομένης της παραγωγής υποδοχής. Ανάπτυξη της εφαρμογής σας σε μια υποδοχή ανάπτυξης περιλαμβάνει τα ακόλουθα πλεονεκτήματα:

1. Μπορείτε να επικυρώσετε web εφαρμογή αλλαγών σε μια υποδοχή ενδιάμεσου σταδίου ανάπτυξης πριν από την ανταλλαγή με την υποδοχή παραγωγής.
2. Την ανάπτυξη μιας εφαρμογής web σε μια υποδοχή και την εναλλαγή σε παραγωγής εξασφαλίζει ότι όλες τις εμφανίσεις της εσοχής είναι προθερμαίνονται πριν από την αντιμετάθεση σε παραγωγής. Αυτό καταργεί χρόνου εκτός λειτουργίας κατά την ανάπτυξη της εφαρμογής web σας. Η ανακατεύθυνση κίνηση είναι απρόσκοπτη και χωρίς αιτήσεις χάνονται λόγω αντιμετάθεσης λειτουργίες. Αυτή η ροή εργασίας ολόκληρο μπορεί να είναι αυτοματοποιημένη, ρυθμίζοντας τις παραμέτρους [Αυτόματη εναλλαγή](web-sites-staged-publishing.md#configure-auto-swap-for-your-web-app) όταν κάνετε προ-εναλλαγή επικύρωση δεν είναι απαραίτητο.
3. Μετά από μια ανταλλαγή, την υποδοχή με προηγουμένως σταδιακή web app περιλαμβάνει τώρα το προηγούμενο παραγωγής web app. Εάν οι αλλαγές Αντιμετάθεση στην υποδοχή παραγωγής δεν όπως θα περιμένατε, μπορείτε να εκτελέσετε την ίδια αντιμετάθεσης αμέσως, για να λάβετε το "τελευταία γνωστό καλό web app" ξανά.

Για να ρυθμίσετε μια υποδοχή ενδιάμεσου σταδίου ανάπτυξης, ανατρέξτε στο θέμα [Ρύθμιση του ενδιάμεσου περιβάλλοντα για τις εφαρμογές web στο Azure εφαρμογής υπηρεσίας](web-sites-staged-publishing.md). Κάθε περιβάλλον πρέπει να περιλαμβάνει το δικό της σύνολο των πόρων, για παράδειγμα εάν web app χρησιμοποιεί μια βάση δεδομένων, στη συνέχεια, παραγωγής και ενδιάμεσου σταδίου εφαρμογής web θα πρέπει να χρησιμοποιείτε διαφορετικές βάσεις δεδομένων. Προσθήκη ενδιάμεσου σταδίου πόρων περιβάλλον ανάπτυξης όπως βάσης δεδομένων, χώρος αποθήκευσης ή cache για τη ρύθμιση ενδιάμεσου σταδίου περιβάλλον ανάπτυξής σας.

## <a name="examples-of-using-multiple-development-environments"></a>Παραδείγματα χρήσης πολλά περιβάλλοντα ανάπτυξης

Οποιοδήποτε έργο πρέπει να ακολουθήσετε μια διαχείρισης κώδικα προέλευσης με τουλάχιστον δύο περιβάλλοντα, ένα περιβάλλον ανάπτυξης και παραγωγής, αλλά όταν χρησιμοποιώντας συστήματα διαχείρισης περιεχομένου, πλαίσια εφαρμογής κ.λπ. θα σας μπορεί να αντιμετωπίσετε ζητήματα όπου η εφαρμογή δεν υποστηρίζει αυτό το σενάριο από το πλαίσιο. Αυτό ισχύει για ορισμένα από τα πλαίσια δημοφιλείς αναφέρονται παρακάτω. Πολλές ερωτήσεις έρθει γνώμη όταν εργάζεστε με ένα CMS/πλαίσια όπως

1. Πώς μπορείτε να χωρίσετε σμίκρυνση σε διαφορετικά περιβάλλοντα
2. Ποια αρχεία μπορώ να αλλάξω και δεν θα επηρεάσει framework ενημερωμένες εκδόσεις
3. Διαχείριση παραμέτρων ανά περιβάλλον
4. Πώς μπορείτε να διαχειριστείτε ενημερώσεις έκδοση λειτουργικές μονάδες/προσθήκες, ενημερωμένες εκδόσεις framework πυρήνα

Υπάρχουν πολλοί τρόποι για να ρυθμίσετε ένα περιβάλλον πολλών για το έργο σας και στα παρακάτω παραδείγματα είναι μία μόνο μία τέτοιου είδους μέθοδο τις αντίστοιχες εφαρμογές.

### <a name="wordpress"></a>WordPress
Σε αυτήν την ενότητα θα μάθετε πώς μπορείτε να ρυθμίσετε μια ροή εργασίας ανάπτυξης χρησιμοποιώντας υποδοχές για WordPress. WordPress όπως οι περισσότερες λύσεις CMS δεν υποστηρίζει την εργασία με πολλά περιβάλλοντα ανάπτυξης από το πλαίσιο. Εφαρμογή υπηρεσίας Web Apps περιλαμβάνει μερικές δυνατότητες που σας διευκολύνουν να αποθηκεύσετε ρυθμίσεις παραμέτρων εκτός του κώδικα.

Πριν να δημιουργήσετε μια εσοχή ενδιάμεσου σταδίου, ρυθμίστε κώδικα της εφαρμογής σας για την υποστήριξη πολλών περιβάλλοντα. Για την υποστήριξη πολλών περιβάλλοντα στο WordPress πρέπει να επεξεργαστείτε `wp-config.php` στην εφαρμογή web της τοπικής ανάπτυξης, προσθέστε τον ακόλουθο κώδικα στην αρχή του αρχείου. Αυτό θα επιτρέπει την εφαρμογή σας για να επιλέξετε τις σωστές ρυθμίσεις παραμέτρων που βασίζεται στο επιλεγμένο περιβάλλον.

```
// Support multiple environments
// set the config file based on current environment
if (strpos($_SERVER['HTTP_HOST'],'localhost') !== false) {
// local development
 $config_file = 'config/wp-config.local.php';
}
elseif ((strpos(getenv('WP_ENV'),'stage') !== false) || (strpos(getenv('WP_ENV'),'prod' )!== false ))
//single file for all azure development environments
 $config_file = 'config/wp-config.azure.php';
}
$path = dirname(__FILE__). '/';
if (file_exists($path. $config_file)) {
// include the config file if it exists, otherwise WP is going to fail
require_once $path. $config_file;
```

Δημιουργήστε ένα φάκελο στη ρίζα εφαρμογής web που ονομάζεται `config` και να προσθέσετε ένα αρχείο δύο αρχεία: `wp-config.azure.php` και `wp-config.local.php` που αντιπροσωπεύει το περιβάλλον azure και τοπικές αντίστοιχα.

Αντιγράψτε τα εξής στο `wp-config.local.php` :

```
<?php
// MySQL settings
/** The name of the database for WordPress */

define('DB_NAME', 'yourdatabasename');

/** MySQL database username */
define('DB_USER', 'yourdbuser');

/** MySQL database password */
define('DB_PASSWORD', 'yourpassword');

/** MySQL hostname */
define('DB_HOST', 'localhost');
/**
 * For developers: WordPress debugging mode.
 * * Change this to true to enable the display of notices during development.
 * It is strongly recommended that plugin and theme developers use WP_DEBUG
 * in their development environments.
 */
define('WP_DEBUG', true);

//Security key settings
define('AUTH_KEY', 'put your unique phrase here');
define('SECURE_AUTH_KEY','put your unique phrase here');
define('LOGGED_IN_KEY','put your unique phrase here');
define('NONCE_KEY', 'put your unique phrase here');
define('AUTH_SALT', 'put your unique phrase here');
define('SECURE_AUTH_SALT', 'put your unique phrase here');
define('LOGGED_IN_SALT', 'put your unique phrase here');
define('NONCE_SALT', 'put your unique phrase here');

/**
 * WordPress Database Table prefix.
 *
 * You can have multiple installations in one database if you give each a unique
 * prefix. Only numbers, letters, and underscores please!
 */
$table_prefix = 'wp_';
```

Ρύθμιση των κλειδιών ασφαλείας του παραπάνω Βοήθεια εμποδίζουν που έγινε εισβολή εφαρμογή web της, επομένως Χρησιμοποιήστε μοναδικές τιμές. Εάν πρέπει να δημιουργήσει το κείμενο για τα πλήκτρα ασφαλείας που αναφέρονται παραπάνω, μπορείτε να μεταβείτε στο το αυτόματης δημιουργίας για να δημιουργήσετε νέες πλήκτρα/τιμές χρησιμοποιώντας αυτό [σύνδεση] (https://api.wordpress.org/secret-key/1.1/salt)

Αντιγράψτε τον παρακάτω κώδικα στο `wp-config.azure.php`:


``` <?php
    // MySQL settings
    /** The name of the database for WordPress */
    
    define('DB_NAME', getenv('DB_NAME'));
    
    /** MySQL database username */
    define('DB_USER', getenv('DB_USER'));
    
    /** MySQL database password */
    define('DB_PASSWORD', getenv('DB_PASSWORD'));
    
    /** MySQL hostname */
    define('DB_HOST', getenv('DB_HOST'));
    
    /**
    * For developers: WordPress debugging mode.
    *
    * Change this to true to enable the display of notices during development.
    * It is strongly recommended that plugin and theme developers use WP_DEBUG
    * in their development environments.
    * Turn on debug logging to investigate issues without displaying to end user. For WP_DEBUG_LOG to
    * do anything, WP_DEBUG must be enabled (true). WP_DEBUG_DISPLAY should be used in conjunction
    * with WP_DEBUG_LOG so that errors are not displayed on the page */
    
    */
    define('WP_DEBUG', getenv('WP_DEBUG'));
    define('WP_DEBUG_LOG', getenv('TURN_ON_DEBUG_LOG'));
    define('WP_DEBUG_DISPLAY',false);
    
    //Security key settings
    /** If you need to generate the string for security keys mentioned above, you can go the automatic generator to create new keys/values: https://api.wordpress.org/secret-key/1.1/salt **/
    define('AUTH_KEY',getenv('DB_AUTH_KEY'));
    define('SECURE_AUTH_KEY', getenv('DB_SECURE_AUTH_KEY'));
    define('LOGGED_IN_KEY', getenv('DB_LOGGED_IN_KEY'));
    define('NONCE_KEY', getenv('DB_NONCE_KEY'));
    define('AUTH_SALT', getenv('DB_AUTH_SALT'));
    define('SECURE_AUTH_SALT', getenv('DB_SECURE_AUTH_SALT'));
    define('LOGGED_IN_SALT',  getenv('DB_LOGGED_IN_SALT'));
    define('NONCE_SALT',  getenv('DB_NONCE_SALT'));
    
    /**
    * WordPress Database Table prefix.
    *
    * You can have multiple installations in one database if you give each a unique
    * prefix. Only numbers, letters, and underscores please!
    */
    $table_prefix = getenv('DB_PREFIX');
```

#### <a name="use-relative-paths"></a>Χρησιμοποιήστε σχετικές διαδρομές
Ένα τελευταίο σημείο είναι να ρυθμίσετε τις παραμέτρους της εφαρμογής WordPress να χρησιμοποιούν σχετικές διαδρομές. WordPress αποθηκεύει πληροφορίες διεύθυνσης URL στη βάση δεδομένων. Με αυτόν τον τρόπο μετακίνησης περιεχομένου από ένα περιβάλλον σε ένα άλλο δυσκολότερη χρειάζεστε για να ενημερώσετε τη βάση δεδομένων κάθε φορά που μετακινείτε από τοπικό σε στάδιο ή στάδιο για περιβάλλοντα παραγωγής. Για να μειώσετε τον κίνδυνο θέματα που μπορεί να προκληθεί με την ανάπτυξη μιας βάσης δεδομένων κάθε φορά που μπορείτε να αναπτύξετε από ένα περιβάλλον σε μια άλλη, χρησιμοποιήστε την [Προσθήκη συνδέσεων σχετική ρίζας](https://wordpress.org/plugins/root-relative-urls/) που μπορεί να εγκατασταθεί με χρήση πίνακα εργαλείων WordPress διαχειριστή ή κάντε λήψη του με μη αυτόματο τρόπο από [εδώ](https://downloads.wordpress.org/plugin/root-relative-urls.zip).


Προσθέστε τις ακόλουθες καταχωρήσεις για να σας `wp-config.php` αρχείου πριν από την `That's all, stop editing!` σχόλιο:

```

  define('WP_HOME', 'http://'. filter_input(INPUT_SERVER, 'HTTP_HOST', FILTER_SANITIZE_STRING));
    define('WP_SITEURL', 'http://'. filter_input(INPUT_SERVER, 'HTTP_HOST', FILTER_SANITIZE_STRING));
    define('WP_CONTENT_URL', '/wp-content');
    define('DOMAIN_CURRENT_SITE', filter_input(INPUT_SERVER, 'HTTP_HOST', FILTER_SANITIZE_STRING));
```

Ενεργοποίηση της προσθήκης μέσω του `Plugins` μενού στον πίνακα εργαλείων WordPress διαχειριστή. Αποθήκευση των ρυθμίσεών σας μόνιμη σύνδεση για την εφαρμογή WordPress.

#### <a name="the-final-wp-configphp-file"></a>Η τελική `wp-config.php` αρχείου
Οι ενημερώσεις πυρήνα WordPress δεν θα επηρεάσει σας `wp-config.php`, `wp-config.azure.php` και `wp-config.local.php` αρχεία. Στο τέλος αυτό τον τρόπο `wp-config.php` αρχείο θα μοιάζει κάπως έτσι

```
<?php
/**
 * The base configurations of the WordPress.
 *
 * This file has the following configurations: MySQL settings, Table Prefix,
 * Secret Keys, and ABSPATH. You can find more information by visiting
 *
 * Codex page. You can get the MySQL settings from your web host.
 *
 * This file is used by the wp-config.php creation script during the
 * installation. You don't have to use the web web app, you can just copy this file
 * to "wp-config.php" and fill in the values.
 *
 * @package WordPress
 */

// Support multiple environments
// set the config file based on current environment
if (strpos($_SERVER['HTTP_HOST'],'localhost') !== false) { // local development
  $config_file = 'config/wp-config.local.php';
}
elseif ((strpos(getenv('WP_ENV'),'stage') !== false) ||(strpos(getenv('WP_ENV'),'prod' )!== false )){
  $config_file = 'config/wp-config.azure.php';
}


$path = dirname(__FILE__). '/';
if (file_exists($path. $config_file)) {
  // include the config file if it exists, otherwise WP is going to fail
  require_once $path. $config_file;
}

/** Database Charset to use in creating database tables. */
define('DB_CHARSET', 'utf8');

/** The Database Collate type. Don't change this if in doubt. */
define('DB_COLLATE', '');


/* That's all, stop editing! Happy blogging. */

define('WP_HOME', 'http://'. $_SERVER['HTTP_HOST']);
define('WP_SITEURL', 'http://'. $_SERVER['HTTP_HOST']);
define('WP_CONTENT_URL', '/wp-content');
define('DOMAIN_CURRENT_SITE', $_SERVER['HTTP_HOST']);

/** Absolute path to the WordPress directory. */
if ( !defined('ABSPATH') )
    define('ABSPATH', dirname(__FILE__). '/');

/** Sets up WordPress vars and included files. */
require_once(ABSPATH. 'wp-settings.php');
```

#### <a name="set-up-a-staging-environment"></a>Ρύθμιση περιβάλλοντος ενδιάμεσου σταδίου
Υποθέτοντας ότι έχετε ήδη μια εφαρμογή web WordPress εκτελείται στο Web Azure, συνδεθείτε στην [πύλη Azure διαχείρισης προεπισκόπηση](http://portal.azure.com) και μεταβείτε στην εφαρμογή web της WordPress. Εάν οι εφαρμογές δεν μπορείτε να δημιουργήσετε έναν από το marketplace. Για να μάθετε περισσότερα, [κάντε κλικ εδώ](web-sites-php-web-site-gallery.md).
Κάντε κλικ στο στοιχείο ρυθμίσεις -> Ανάπτυξη υποδοχές -> Προσθήκη για να δημιουργήσετε μια εσοχή ανάπτυξη με το όνομα του σταδίου. Μια υποδοχή ανάπτυξης είναι μια άλλη εφαρμογή web κοινής χρήσης στους ίδιους πόρους με την εφαρμογή web κύρια δημιουργήθηκε παραπάνω.

![Δημιουργία υποδιαίρεση στάδιο ανάπτυξης](./media/app-service-web-staged-publishing-realworld-scenarios/1setupstage.png)

Προσθέστε μια άλλη βάση δεδομένων MySQL, ας υποθέσουμε `wordpress-stage-db` στη δική σας ομάδα πόρων `wordpressapp-group`.

 ![Προσθήκη βάσης δεδομένων MySQL σε ομάδα πόρων](./media/app-service-web-staged-publishing-realworld-scenarios/2addmysql.png)

Ενημερώστε τις συμβολοσειρές σύνδεσης για την υποδοχή του σταδίου ανάπτυξης ώστε να οδηγεί στη βάση δεδομένων που έχουν δημιουργηθεί πρόσφατα, `wordpress-stage-db`. Σημειώστε ότι παραγωγής σας στο web app, `wordpressprodapp` και ενδιάμεσου σταδίου web app `wordpressprodapp-stage` πρέπει να οδηγεί σε διαφορετικές βάσεις δεδομένων.

#### <a name="configure-environment-specific-app-settings"></a>Ρύθμιση παραμέτρων εφαρμογής συγκεκριμένο περιβάλλον
Οι προγραμματιστές μπορούν να αποθηκεύουν ζεύγη κλειδιού-τιμής συμβολοσειρά στο Azure ως μέρος των πληροφοριών ρύθμισης παραμέτρων που σχετίζεται με μια εφαρμογή web που ονομάζεται των ρυθμίσεων της εφαρμογής. Κατά το χρόνο εκτέλεσης, εφαρμογή υπηρεσίας Web Apps αυτόματα ανακτά αυτές τις τιμές για εσάς και είναι διαθέσιμες σε κώδικα που εκτελείται στην εφαρμογή web. Από ένα χρεόγραφο προοπτική που είναι μια καλή πλευρά επωφεληθείτε από ευαίσθητες πληροφορίες, όπως συμβολοσειρές σύνδεσης βάσης δεδομένων με τους κωδικούς πρόσβασης ποτέ εμφανίζεται ως απλό κείμενο σε ένα αρχείο όπως `wp-config.php`.

Αυτή η διαδικασία που ορίζονται από το κάτω από το στοιχείο είναι χρήσιμη όταν εκτελείτε όπως περιλαμβάνει αλλαγές αρχείων και αλλαγές της βάσης δεδομένων για την εφαρμογή WordPress:
- Αναβάθμιση της έκδοσης WordPress
- Προσθήκη νέων ή επεξεργασία ή αναβάθμιση προσθηκών
- Προσθήκη νέου ή να επεξεργαστείτε ή να αναβαθμίσετε θέματα

Ρυθμίστε τις παραμέτρους των ρυθμίσεων της εφαρμογής για:

- πληροφορίες βάσης δεδομένων
- Ενεργοποίηση/απενεργοποίηση WordPress καταγραφής
- Ρυθμίσεις ασφαλείας WordPress

![Εφαρμογή ρυθμίσεων για Wordpress web app](./media/app-service-web-staged-publishing-realworld-scenarios/3configure.png)

Βεβαιωθείτε ότι έχετε προσθέσει τις ακόλουθες ρυθμίσεις εφαρμογής για το υποδιαίρεση παραγωγής web app και το στάδιο. Σημειώστε ότι η εφαρμογή web παραγωγής και ενδιάμεσου σταδίου web app χρησιμοποιούν διαφορετικές βάσεις δεδομένων.
Καταργήστε την επιλογή **Ρύθμιση υποδιαίρεση** το πλαίσιο ελέγχου για όλες τις παραμέτρους ρυθμίσεις εκτός από WP_ENV. Αυτό θα εναλλαγή στη ρύθμιση παραμέτρων για την εφαρμογή web σας, μαζί με το περιεχόμενο του αρχείου και βάσεων δεδομένων. Εάν **Η ρύθμιση υποδιαίρεση** είναι **επιλεγμένη**, των ρυθμίσεων της εφαρμογής του web app και ρύθμιση παραμέτρων συμβολοσειρά σύνδεσης δεν θα μετακινηθεί σε περιβάλλοντα κατά την εκτέλεση μιας λειτουργίας ΑΝΤΙΜΕΤΆΘΕΣΗΣ και επομένως εάν υπάρχουν αλλαγές βάσης δεδομένων αυτό θα δεν διακόψει την εφαρμογή web της παραγωγής.

Ανάπτυξη της τοπικής ανάπτυξης περιβάλλον web app στο στάδιο web app και βάση δεδομένων με χρήση WebMatrix ή εργαλεία της επιλογής σας, όπως FTP, Git ή PhpMyAdmin.

![Παράθυρο διαλόγου δημοσίευση πίνακα για την εφαρμογή web WordPress στο Web](./media/app-service-web-staged-publishing-realworld-scenarios/4wmpublish.png)

Αναζήτηση και να ελέγξετε την εφαρμογή web της δοκιμής. Δεδομένων ένα σενάριο όπου το θέμα της εφαρμογής web είναι να ενημερωθούν, θα δείτε το ενδιάμεσου σταδίου web app.

![Αναζήτηση ενδιάμεσου εφαρμογής web πριν από την ανταλλαγή υποδοχές](./media/app-service-web-staged-publishing-realworld-scenarios/5wpstage.png)


 Εάν όλα σας αρέσει, κάντε κλικ στο κουμπί **Εναλλαγή** σε σας ενδιάμεσου σταδίου web app για να μεταφέρετε το περιεχόμενό σας στο περιβάλλον παραγωγής. Σε αυτήν την περίπτωση εναλλάσσετε την εφαρμογή web και τη βάση δεδομένων σε περιβάλλοντα κατά τη διάρκεια κάθε λειτουργίας **αντιμετάθεσης** .

![Εναλλαγή προεπισκόπηση αλλαγών για WordPress](./media/app-service-web-staged-publishing-realworld-scenarios/6swaps1.png)

 > [AZURE.NOTE]
 >Εάν έχετε ένα σενάριο όπου πρέπει να μόνο push αρχεία (δεν υπάρχουν ενημερωμένες εκδόσεις βάσης δεδομένων), στη συνέχεια, **Επιλέξτε** τη **Ρύθμιση υποδιαίρεση** για όλους τη βάση δεδομένων που σχετίζονται με *εφαρμογή ρυθμίσεις* και *Ρυθμίσεις συμβολοσειρές σύνδεσης* στο web app ρύθμιση blade εντός της πύλης Azure προεπισκόπηση πριν από την εκτέλεση της ΑΝΤΑΛΛΑΓΉΣ. Σε αυτό DB_NAME πεζών-κεφαλαίων, DB_HOST, DB_PASSWORD, DB_USER, προεπιλεγμένη ρύθμιση συμβολοσειρά σύνδεσης πρέπει να δεν εμφανίζονται στην προεπισκόπηση αλλαγών κατά την εκτέλεση μιας **Εναλλαγή**. At αυτό χρόνου, όταν ολοκληρωθεί η λειτουργία **Εναλλαγή** της εφαρμογής web WordPress θα έχετε τις ενημερώσεις που αρχεία **ΜΌΝΟ**.

Πριν να κάνετε μια ΑΝΤΑΛΛΑΓΉ, εδώ είναι η εφαρμογή web WordPress παραγωγής ![παραγωγής εφαρμογής web πριν από την ανταλλαγή υποδοχές](./media/app-service-web-staged-publishing-realworld-scenarios/7bfswap.png)

Μετά τη λειτουργία ΑΝΤΙΜΕΤΆΘΕΣΗΣ, το θέμα έχει ενημερωθεί στην εφαρμογή web της παραγωγής.

![Εφαρμογή web παραγωγής μετά την εναλλαγή υποδοχές](./media/app-service-web-staged-publishing-realworld-scenarios/8afswap.png)

Σε κατάσταση όταν πρέπει να **επαναφέρετε**, να μπορείτε να μεταβείτε στις ρυθμίσεις εφαρμογής web παραγωγής και να κάντε κλικ στο κουμπί **εναλλαγής** για να εναλλαγή του web app και βάση δεδομένων από την παραγωγή για ενδιάμεσου σταδίου υποδιαίρεση. Σημαντικό να θυμάστε είναι ότι εάν περιλαμβάνονται αλλαγές της βάσης δεδομένων με μια λειτουργία **Εναλλαγή** οποιαδήποτε στιγμή, στη συνέχεια, την επόμενη φορά που αναπτύξετε ξανά σε εφαρμογή web ενδιάμεσου σταδίου πρέπει να αναπτύξετε τη βάση δεδομένων αλλάζει στην τρέχουσα βάση δεδομένων για το ενδιάμεσου σταδίου web app θα μπορούσε να είναι προηγούμενης βάσης δεδομένων παραγωγής ή η βάση δεδομένων του σταδίου.

#### <a name="summary"></a>Σύνοψη
Γενίκευσης τη διαδικασία για κάθε εφαρμογή με μια βάση δεδομένων

1. Εγκατάσταση της εφαρμογής σε τοπικό περιβάλλον σας
2. Συμπεριλάβετε περιβάλλον συγκεκριμένη ρύθμιση παραμέτρων (τοπικό και Azure Web App)
3. Ρύθμιση του περιβάλλοντα στην εφαρμογή υπηρεσίας Web Apps – τοποθέτηση, παραγωγής
4. Εάν έχετε μια εφαρμογή παραγωγής ήδη εκτελείται σε Azure, συγχρονίστε παραγωγής το περιεχόμενό σας (αρχεία/κωδικός + βάσης δεδομένων) στο περιβάλλον τοπικού και ενδιάμεσου σταδίου.
5. Αναπτύξτε την εφαρμογή στο περιβάλλον του τοπικού
6. Τοποθετήσετε περιεχόμενο από την παραγωγή για περιβάλλοντα ενδιάμεσου σταδίου και την εφαρμογή web της παραγωγής στην περιοχή συντήρησης ή κλειδωμένη λειτουργία και συγχρονισμός βάσης δεδομένων
7. Ανάπτυξη περιβάλλοντος ενδιάμεσου σταδίου και έλεγχος
8. Ανάπτυξη περιβάλλον παραγωγής
9. Επαναλάβετε τα βήματα 4 έως 6

### <a name="umbraco"></a>Umbraco
Σε αυτήν την ενότητα θα μάθετε πώς το CMS Umbraco χρησιμοποιεί μια προσαρμοσμένη λειτουργική μονάδα για την ανάπτυξη από σε πολλές DevOps περιβάλλον. Αυτό το παράδειγμα σάς παρέχει μια διαφορετική προσέγγιση για τη διαχείριση πολλών περιβάλλοντα ανάπτυξης.

[Umbraco CMS](http://umbraco.com/) είναι μία από τις λύσεις CMS popular.NET χρησιμοποιούνται από πολλά προγραμματιστές το οποίο παρέχει λειτουργική μονάδα [Courier2](http://umbraco.com/products/more-add-ons/courier-2) για ανάπτυξη από ανάπτυξης ενδιάμεσου σταδίου για περιβάλλοντα παραγωγής. Μπορείτε να δημιουργήσετε εύκολα ένα περιβάλλον τοπικού ανάπτυξης για μια εφαρμογή web Umbraco CMS χρησιμοποιώντας το Visual Studio ή WebMatrix.

1. Δημιουργία εφαρμογής web Umbraco με το Visual Studio, [κάντε κλικ εδώ](https://our.umbraco.org/documentation/Installation/install-umbraco-with-nuget).
2. Για να δημιουργήσετε μια εφαρμογή web της Umbraco με WebMatrix, [κάντε κλικ εδώ](http://umbraco.com/help-and-support/video-tutorials/getting-started/working-with-webmatrix).

Να θυμάστε πάντα να καταργήσετε το `install` φακέλου στην περιοχή εφαρμογή σας και ποτέ στείλτε το στο στάδιο ή παραγωγής εφαρμογές web. Για αυτό το πρόγραμμα εκμάθησης, που θα χρησιμοποιούν WebMatrix

#### <a name="set-up-a-staging-environment"></a>Ρύθμιση ενδιάμεσου σταδίου περιβάλλοντος
- Δημιουργήστε μια υποδοχή ανάπτυξης που προαναφέρθηκαν για Umbraco CMS web app, υπό την προϋπόθεση ότι έχετε ήδη μια εφαρμογή web της Umbraco CMS προς τα επάνω και την εκτέλεση. Εάν δεν μπορείτε να δημιουργήσετε έναν από το marketplace.

- Ενημερώστε τη συμβολοσειρά σύνδεσης για την ανάπτυξη υποδοχή σας στάδιο ώστε να οδηγεί στη βάση δεδομένων που έχουν δημιουργηθεί πρόσφατα, **umbraco-στάδιο-db**. Σας παραγωγής web app (umbraositecms-1) και ενδιάμεσου σταδίου web app (umbracositecms-1-στάδιο) **πρέπει να** οδηγεί σε διαφορετικές βάσεις δεδομένων.

![Ενημέρωση συμβολοσειρά σύνδεσης για ενδιάμεσου web app με νέα βάση δεδομένων ενδιάμεσου σταδίου](./media/app-service-web-staged-publishing-realworld-scenarios/9umbconnstr.png)

- Κάντε κλικ στο **Ρυθμίσεις γρήγορα δημοσίευσης** υποδιαίρεση της ανάπτυξης του **σταδίου**. Αυτό θα λάβετε ένα αρχείο ρυθμίσεων δημοσίευση που αποθηκεύουν όλες τις πληροφορίες που απαιτούνται από Visual Studio ή Web πίνακα για να δημοσιεύσετε την εφαρμογή από το τοπικό ανάπτυξης web app με το Azure web app.

 ![Να δημοσιεύσετε τη ρύθμιση της εφαρμογής web ενδιάμεσου σταδίου](./media/app-service-web-staged-publishing-realworld-scenarios/10getpsetting.png)

- Ανοίξτε την εφαρμογή web της τοπικής ανάπτυξης σε **WebMatrix** ή **Visual Studio**. Σε αυτό το πρόγραμμα εκμάθησης χρησιμοποιώ μήτρα Web και πρέπει πρώτα να εισαγάγετε το αρχείο ρυθμίσεων δημοσίευση για την εφαρμογή web της ενδιάμεσου σταδίου

![Εισαγωγή ρυθμίσεων δημοσίευση για Umbraco χρήση πίνακα Web](./media/app-service-web-staged-publishing-realworld-scenarios/11import.png)

- Αναθεώρηση αλλαγών στο παράθυρο διαλόγου και ανάπτυξη της εφαρμογής σας τοπικό web σε εφαρμογή Azure web, *umbracositecms 1-φάσης*. Κατά την ανάπτυξη αρχείων απευθείας σε εφαρμογή web ενδιάμεσου σταδίου παραλείψετε τα αρχεία που βρίσκονται τα `~/app_data/TEMP/` φάκελο όπως αυτά θα είναι ξανά κατά την πρώτη εφαρμογή web στάδιο αποτελέσματα. Μπορείτε, επίσης, θα πρέπει να παραλείψετε το `~/app_data/umbraco.config` αρχείου ως αυτό, επίσης, θα είναι ξανά.

![Αναθεώρηση αλλαγών δημοσίευση σε μήτρα web](./media/app-service-web-staged-publishing-realworld-scenarios/12umbpublish.png)

- Μετά τη δημοσίευση με επιτυχία το Umbraco τοπικό web app για να ενδιάμεσου εφαρμογής web, αναζητήστε την εφαρμογή web της ενδιάμεσου σταδίου και εκτελέστε μερικά ελέγχους για να αποκλείσετε προβλήματα.

#### <a name="set-up-courier2-deployment-module"></a>Ρύθμιση του Courier2 λειτουργική μονάδα ανάπτυξης
Με [Courier2](http://umbraco.com/products/more-add-ons/courier-2) λειτουργική μονάδα που μπορεί να push περιεχόμενο, φύλλα στυλ, λειτουργικές μονάδες ανάπτυξης και περισσότερα σχετικά με μια απλή κάντε δεξί κλικ από μια εφαρμογή web ενδιάμεσου σταδίου παραγωγής εφαρμογή web για μια περισσότερες αναπτύξεις δωρεάν προβλήματα και μείωση κινδύνων του ορίου την εφαρμογή web της παραγωγής κατά την ανάπτυξη μια ενημερωμένη έκδοση.
Αγοράστε μια άδεια χρήσης για Courier2 για τον τομέα `*.azurewebsites.net` και τον προσαρμοσμένο τομέα σας (Καλώς http://abc.com) Αφού αγοράσετε την άδεια χρήσης, τοποθετήστε την άδεια χρήσης που έχετε λάβει (. Αρχείο LIC) στο το `bin` φακέλου.

![Απόθεση άδεια χρήσης του αρχείου στην περιοχή Ανακύκλωσης φακέλου](./media/app-service-web-staged-publishing-realworld-scenarios/13droplic.png)

Κάντε λήψη του πακέτου Courier2 από [εδώ](https://our.umbraco.org/projects/umbraco-pro/umbraco-courier-2/). Η σύνδεση στην εφαρμογή web της στάδιο, πείτε http://umbracocms-site-stage.azurewebsites.net/umbraco και κάντε κλικ στο μενού **Προγραμματιστής** και επιλέξτε **πακέτων**. Κάντε κλικ στην επιλογή **εγκατάσταση του** τοπικού πακέτου

![Πρόγραμμα εγκατάστασης του πακέτου Umbraco](./media/app-service-web-staged-publishing-realworld-scenarios/14umbpkg.png)

Αποστείλετε το πακέτο courier2 χρησιμοποιώντας το πρόγραμμα εγκατάστασης.

![Αποστολή πακέτου για courier λειτουργική μονάδα](./media/app-service-web-staged-publishing-realworld-scenarios/15umbloadpkg.png)

Για να ρυθμίσετε τις παραμέτρους που πρέπει να ενημερώσετε courier.config αρχείο στο φάκελο **ρύθμισης παραμέτρων** της εφαρμογής web.

```xml
<!-- Repository connection settings -->
 <!-- For each site, a custom repository must be configured, so Courier knows how to connect and authenticate-->
 <repositories>
    <!-- If a custom Umbraco Membership provider is used, specify login & password + set the passwordEncoding to clear: -->
    <repository name="production web app" alias="stage" type="CourierWebserviceRepositoryProvider" visible="true">
      <url>http://umbracositecms-1.azurewebsites.net</url>
      <user>0</user>
      <!--<login>user@email.com</login> -->
      <!-- <password>user_password</password>-->
      <!-- <passwordEncoding>Clear</passwordEncoding>-->
      </repository>
 </repositories>
 ```

Στην περιοχή `<repositories>`, εισαγάγετε τις πληροφορίες διεύθυνσης URL και χρήστη της τοποθεσίας παραγωγής. Εάν χρησιμοποιείτε την προεπιλεγμένη υπηρεσία παροχής μελών Umbraco, στη συνέχεια, προσθέστε το Αναγνωριστικό για το χρήστη διαχείρισης στο <user> ενότητας. Εάν χρησιμοποιείτε μια προσαρμοσμένη υπηρεσία παροχής συμμετοχής Umbraco, χρησιμοποιήστε `<login>`,`<password>` Courier2 λειτουργική μονάδα, μάθετε πώς μπορείτε να συνδεθείτε στην τοποθεσία παραγωγής. Για περισσότερες λεπτομέρειες, ανατρέξτε στην [τεκμηρίωση](http://umbraco.com/help-and-support/customer-area/courier-2-support-and-download/developer-documentation) για Courier λειτουργική μονάδα.

Ομοίως, εγκατάσταση Courier λειτουργικής μονάδας στην τοποθεσία σας παραγωγής και ρυθμίστε τις παραμέτρους του σημείου στο στάδιο web app στο αρχείο τα αντίστοιχα courier.config όπως φαίνεται εδώ

```xml
 <!-- Repository connection settings -->
 <!-- For each site, a custom repository must be configured, so Courier knows how to connect and authenticate-->
 <repositories>
    <!-- If a custom Umbraco Membership provider is used, specify login & password + set the passwordEncoding to clear: -->
    <repository name="Stage web app" alias="stage" type="CourierWebserviceRepositoryProvider" visible="true">
      <url>http://umbracositecms-1-stage.azurewebsites.net</url>
      <user>0</user>
      </repository>
 </repositories>
```

Κάντε κλικ στην καρτέλα Courier2 στον πίνακα εργαλείων εφαρμογής web Umbraco CMS και επιλέξτε θέσεις. Θα πρέπει να βλέπετε το όνομα του αποθετηρίου όπως αναφέρεται στο `courier.config`. Κάντε τα εξής τόσο για την παραγωγή και ενδιάμεσου εφαρμογές web.

![Προβολή προορισμού web app αποθετήριο δεδομένων](./media/app-service-web-staged-publishing-realworld-scenarios/16courierloc.png)

Τώρα σας επιτρέπει να αναπτύξετε ορισμένες περιεχόμενο από την τοποθεσία ενδιάμεσου σταδίου παραγωγής τοποθεσία. Μεταβείτε σε περιεχόμενο και επιλέξτε μια υπάρχουσα σελίδα ή δημιουργήστε μια νέα σελίδα. Θα επιλέξετε μια υπάρχουσα σελίδα από το web app όπου έχει αλλάξει τον τίτλο της σελίδας για **Γρήγορα αποτελέσματα – νέα** και τώρα, κάντε κλικ στην **Αποθήκευση και δημοσίευση**.

![Αλλαγή τίτλου της σελίδας και δημοσίευση](./media/app-service-web-staged-publishing-realworld-scenarios/17changepg.png)

Τώρα επιλέξτε η τροποποιημένη σελίδα και *κάντε δεξί κλικ* για να δείτε όλες τις επιλογές. Κάντε κλικ στο για να προβάλετε το παράθυρο διαλόγου ανάπτυξη **Courier** . Κάντε κλικ στην **Ανάπτυξη** για να ξεκινήσετε ανάπτυξης

![Παράθυρο διαλόγου ανάπτυξη λειτουργική μονάδα Courier](./media/app-service-web-staged-publishing-realworld-scenarios/18dialog1.png)

Εξετάστε τις αλλαγές και κάντε κλικ στη συνέχεια.

![Courier λειτουργική μονάδα ανάπτυξης παραθύρου διαλόγου αναθεώρηση αλλαγών](./media/app-service-web-staged-publishing-realworld-scenarios/19dialog2.png)

Αρχείο καταγραφής ανάπτυξης δείχνει αν ανάπτυξης ολοκληρώθηκε με επιτυχία.

 ![Προβολή αρχείων καταγραφής ανάπτυξης από Courier λειτουργική μονάδα](./media/app-service-web-staged-publishing-realworld-scenarios/20successdlg.png)

Αναζητήστε την εφαρμογή web της παραγωγής για να δείτε εάν οι αλλαγές θα εμφανιστούν.

 ![Αναζήτηση παραγωγής web app](./media/app-service-web-staged-publishing-realworld-scenarios/21umbpg.png)

Για να μάθετε περισσότερα σχετικά με τη χρήση Courier, διαβάστε την τεκμηρίωση.

#### <a name="how-to-upgrade-umbraco-cms-version"></a>Πώς μπορείτε να αναβαθμίσετε Umbraco CMS έκδοση

Courier δεν θα σας βοηθήσουν ανάπτυξη με την αναβάθμιση από μία εκδόσεις του Umbraco CMS σε μια άλλη. Κατά την αναβάθμιση Umbraco CMS έκδοση, θα πρέπει να ελέγχετε ασυμβατότητα με τις προσαρμοσμένες λειτουργικές μονάδες ή τρίτων κατασκευαστών λειτουργικές μονάδες και τις βιβλιοθήκες Umbraco πυρήνα. Ως βέλτιστη πρακτική

1. ΠΆΝΤΑ αντίγραφα ασφαλείας τα σας web app και των βάσεων δεδομένων πριν να κάνετε αναβάθμιση. Στην εφαρμογή Web Azure, μπορείτε να ρυθμίσετε αυτόματη δημιουργία αντιγράφων ασφαλείας για τις τοποθεσίες Web χρησιμοποιώντας το αντίγραφο ασφαλείας της δυνατότητας και να επαναφέρετε την τοποθεσία σας εάν απαιτείται χρήση επαναφέρετε τη δυνατότητα. Για περισσότερες λεπτομέρειες, ανατρέξτε στο θέμα [πώς μπορείτε να δημιουργήσετε αντίγραφα ασφαλείας της εφαρμογής web σας](web-sites-backup.md) και [πώς μπορείτε να επαναφέρετε την εφαρμογή web](web-sites-restore.md).

2. Ελέγξτε εάν τα πακέτα τρίτων κατασκευαστών που χρησιμοποιείτε είναι συμβατή με την έκδοση που πραγματοποιείτε αναβάθμιση σε. Στην το πακέτο σελίδα λήψης, ελέγξτε τη συμβατότητα έργου με την έκδοση Umbraco CMS.

Για περισσότερες λεπτομέρειες σχετικά με τον τρόπο για να αναβαθμίσετε την εφαρμογή web τοπικά, ακολουθήστε τις οδηγίες ως αναφέρεται [εδώ](https://our.umbraco.org/documentation/getting-started/set up/upgrading/general).

Μετά την αναβάθμιση της τοποθεσίας σας τοπικής ανάπτυξης, δημοσιεύστε τις αλλαγές ενδιάμεσου εφαρμογής web. Δοκιμάσετε την εφαρμογή σας και εάν όλα σας αρέσει, χρησιμοποιήστε το κουμπί **Εναλλαγή** για να **Εναλλαγή** ενδιάμεσου σταδίου τοποθεσία σας στο web app παραγωγής. Κατά την εκτέλεση της λειτουργίας **Εναλλαγή** , μπορείτε να προβάλετε τις αλλαγές που θα να αντιμετωπίσουν προβλήματα στη ρύθμιση παραμέτρων της εφαρμογής σας web. Με αυτήν τη **Εναλλαγή** λειτουργία, θα σας ανταλλαγή τις εφαρμογές web και βάσεων δεδομένων. Αυτό σημαίνει ότι, μετά το κάνετε ΕΝΑΛΛΑΓΉ της εφαρμογής web παραγωγής τώρα θα οδηγεί στη βάση δεδομένων umbraco-στάδιο-db και ενδιάμεσου σταδίου εφαρμογής web θα οδηγεί στη βάση δεδομένων umbraco-ειδών-db.

![Εναλλαγή προεπισκόπησης για την ανάπτυξη Umbraco CMS](./media/app-service-web-staged-publishing-realworld-scenarios/22umbswap.png)

Το πλεονέκτημα της ανταλλαγή του web app και το βάσης δεδομένων:
1. Σας δίνει τη δυνατότητα να επιστρέψετε στην προηγούμενη έκδοση της εφαρμογής web με μια άλλη **Εναλλαγή** εάν υπάρχουν ζητήματα εφαρμογής.
2. Για αναβάθμιση πρέπει να αναπτύξετε αρχεία και βάση δεδομένων από ενδιάμεσου web app για να παραγωγής web app και βάση δεδομένων. Υπάρχουν πολλά πράγματα που μπορεί να παρουσιαστεί κατά την ανάπτυξη αρχείων και βάσεων δεδομένων. Χρησιμοποιώντας τη δυνατότητα **αντιμετάθεσης** υποδοχών, να μειώσετε χρόνου εκτός λειτουργίας κατά την αναβάθμιση και μείωση του κινδύνου αποτυχιών που μπορεί να προκύψουν κατά την ανάπτυξη αλλαγές.
3. Σας δίνει τη δυνατότητα να κάνετε **A / B δοκιμές** χρησιμοποιώντας τη δυνατότητα [δοκιμή της παραγωγής](https://azure.microsoft.com/documentation/videos/introduction-to-azure-websites-testing-in-production-with-galin-iliev/)

Αυτό το παράδειγμα δείχνει την ευελιξία της πλατφόρμας όπου μπορείτε να δημιουργήσετε προσαρμοσμένες λειτουργικές μονάδες παρόμοια με λειτουργική μονάδα Umbraco Courier για τη Διαχείριση ανάπτυξης σε περιβάλλοντα.

## <a name="references"></a>Αναφορές
[Ανάπτυξη ευέλικτη λογισμικού με Azure εφαρμογής υπηρεσίας](app-service-agile-software-development.md)

[Ρύθμιση του ενδιάμεσου περιβάλλοντα για τις εφαρμογές web στο Azure εφαρμογής υπηρεσίας](web-sites-staged-publishing.md)

[Τρόπος αποκλεισμού της πρόσβασης web σε ανάπτυξη μη παραγωγής εσοχές](http://ruslany.net/2014/04/azure-web-sites-block-web-access-to-non-production-deployment-slots/)
