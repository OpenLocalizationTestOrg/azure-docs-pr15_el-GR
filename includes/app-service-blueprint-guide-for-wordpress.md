## <a name="wordpress-and-azure-app-service"></a>WordPress και Azure εφαρμογής υπηρεσίας

* [Τι είναι το WordPress;](https://wordpress.org/)
* [Πώς μπορείτε να εγκαταστήσετε την εφαρμογή web WordPress επιχειρηματικής κατηγορίας](../articles/app-service-web/web-sites-php-enterprise-wordpress.md)
* [Πώς μπορείτε να αγοράσετε MySQL θέσει σε κοινή χρήση ClearDB φιλοξενίας για την εφαρμογή σας WordPress](http://blog.syntaxc4.net/post/2012/12/03/provisioning-a-mysql-database-from-the-windows-azure-store.aspx)
* [Πώς να ClearDB αγοράς αποκλειστικό MySQL συμπλέγματος για την εφαρμογή σας WordPress](https://azure.microsoft.com/blog/announcing-new-mysql-premium-tiers-from-cleardb/)
* [Ανάπτυξη εφαρμογής web WordPress αντίγραφα με MySQL αναπαραγωγής συμπλέγματος](/documentation/templates/wordpress-mysql-replication/)
* [Δημιουργήστε το δικό σας σύμπλεγμα MySQL υπόδειγμα υπόδειγμα χρήσης συμπλέγματος Percona](/documentation/templates/mysql-ha-pxc/) και [Μάθετε περισσότερα σχετικά με τον τρόπο για τη διαχείριση του συμπλέγματος](https://github.com/fanjeffrey/axiom.articles/tree/master/pxc)
* [Ανάπτυξη WordPress αντίγραφα ασφαλείας των MySQL σύμπλεγμα αλληλεπίδραση με τη ρύθμιση παραμέτρων εξαρτώμενη υπόδειγμα](/documentation/templates/mysql-replication/)
* [Ανάπτυξη εφαρμογής WordPress αντίγραφα ασφαλείας των SQL Azure DB ελέγχονται από ProjectNami](/marketplace/partners/projectnami/projectnami/)
  
## <a name="troubleshooting-wordpress-application"></a>Αντιμετώπιση προβλημάτων εφαρμογής WordPress

* [Τρόπος αντιμετώπισης προβλημάτων της εφαρμογής σας WordPress](https://sunithamk.wordpress.com/2014/09/04/wordpress-troubleshooting-techniques-on-azure-websites/)
* [Συγκέντρωση τηλεμετρίας χρήση χρήση Azure ιδέες εφαρμογής υπηρεσίας](https://azure.microsoft.com/blog/usage-analytics-for-wordpress-with-azure-app-insights/)
* [Εκτέλεση profiler Zend Zray σε σχέση με την εφαρμογή web για τη διάγνωση προβλημάτων και επιδόσεων](https://sunithamk.wordpress.com/2015/08/04/profiling-php-application-on-azure-web-apps/)
* [Χρήση πύλη Kudu υποστήριξης για τη διάγνωση και να συμβάλει στην αντιμετώπιση προβλημάτων σε πραγματικό χρόνο](https://sunithamk.wordpress.com/2015/11/04/diagnose-and-mitigate-issues-with-azure-web-apps-support-portal/)
* [Χρήση διάφορα αυτόματης-διορθώσετε τους κανόνες για την αυτοματοποίηση επίλυση περιστατικά πραγματικό χρόνο](http://microsoftazurewebsitescheatsheet.info/#auto-heal)
* [Τρόπος δημιουργίας αντιγράφων ασφαλείας της εφαρμογής web σας](../articles/app-service-web/web-sites-backup.md) και [πώς μπορείτε να επαναφέρετε την εφαρμογή web της](../articles/app-service-web/web-sites-restore.md)

## <a name="performance"></a>Επιδόσεων

* [Πώς μπορείτε να επιταχύνετε την εφαρμογή web WordPress](https://sunithamk.wordpress.com/2014/08/01/10-ways-to-speed-up-your-wordpress-site-on-azure-websites/)
* [Πώς να cache με δυνατότητα redis](../articles/redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md) χρησιμοποιώντας [redis προσθήκης cache](https://wordpress.org/plugins/wp-redis/)
* [Πώς μπορείτε να ενεργοποιήσετε memcached cache αντικειμένων για WordPress](../articles/app-service-web/web-sites-connect-to-redis-using-memcache-protocol.md) με χρήση [προσθήκης memcached](https://wordpress.org/plugins/memcached/)
* [Ενεργοποίηση wincache με προσθήκη συνόλου cache W3](https://wordpress.org/plugins/w3-total-cache/)
* [Πώς μπορείτε να χρησιμοποιήσετε supercache Προσθήκη για να επιταχύνετε την εφαρμογή WordPress](http://ruslany.net/2008/12/speed-up-wordpress-on-iis-70/)
* [Πώς διακομιστή σε cache με χρήση των υπηρεσιών IIS εξόδου σε cache](http://blogs.msdn.com/b/brian_swan/archive/2011/06/08/performance-tuning-php-apps-on-windows-iis-with-output-caching.aspx)
* [Πώς μπορείτε να με δυνατότητα προγράμματος περιήγησης σε cache για στατικό περιεχόμενο](http://www.iis.net/configreference/system.webserver/staticcontent)
