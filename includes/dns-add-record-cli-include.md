#### <a name="create-an-a-record-set-with-single-record"></a>Δημιουργήστε μια εγγραφή A που έχουν οριστεί με μία μόνο εγγραφή

Για να δημιουργήσετε ένα σύνολο εγγραφών, χρησιμοποιήστε `azure network dns record-set create`. Καθορίστε την ομάδα πόρων, όνομα ζώνης, εγγραφή ορίστε σχετικό όνομα, τύπο εγγραφής και ώρα για να live (TTL).

    azure network dns record-set create myresourcegroup  contoso.com "test-a"  A --ttl 300

Μετά τη δημιουργία Α εγγραφή σύνολο, προσθέστε τη διεύθυνση IPv4 στην εγγραφή που έχουν οριστεί με `azure network dns record-set add-record`.

    azure network dns record-set add-record myresourcegroup contoso.com "test-a" A -a 192.168.1.1

#### <a name="create-an-aaaa-record-set-with-a-single-record"></a>Δημιουργία εγγραφής AAAA σύνολο με μία μόνο εγγραφή

    azure network dns record-set create myresourcegroup contoso.com "test-aaaa" AAAA --ttl 300

    azure network dns record-set add-record myresourcegroup contoso.com "test-aaaa" AAAA -b "2607:f8b0:4009:1803::1005"

#### <a name="create-a-cname-record-set-with-a-single-record"></a>Δημιουργήστε μια εγγραφή CNAME που έχουν οριστεί με μία μόνο εγγραφή

Εγγραφές CNAME επιτρέπει μόνο μία τιμή μία συμβολοσειρά.


    azure network dns record-set create -g myresourcegroup contoso.com  "test-cname" CNAME --ttl 300

    azure network dns record-set add-record  myresourcegroup contoso.com  test-cname CNAME -c "www.contoso.com"


#### <a name="create-an-mx-record-set-with-a-single-record"></a>Δημιουργήστε μια εγγραφή MX που έχουν οριστεί με μία μόνο εγγραφή

Σε αυτό το παράδειγμα, μπορούμε να χρησιμοποιήσουμε το όνομα του συνόλου εγγραφών "@" για να δημιουργήσετε την εγγραφή MX στην κορυφή ζώνη (σε αυτήν την περίπτωση, "contoso.com"). Αυτό είναι κοινά για εγγραφές MX.

    azure network dns record-set create myresourcegroup contoso.com  "@"  MX --ttl 300

    azure network dns record-set add-record -g myresourcegroup contoso.com  "@" MX -e "mail.contoso.com" -f 5


#### <a name="create-an-ns-record-set-with-a-single-record"></a>Δημιουργήστε μια εγγραφή NS σύνολο με μία μόνο εγγραφή

    azure network dns record-set create myresourcegroup contoso.com test-ns  NS --ttl 300

    azure network dns record-set add-record myresourcegroup  contoso.com  "test-ns" NS -d "ns1.contoso.com"

#### <a name="create-a-ptr-record-set-with-a-single-record"></a>Δημιουργήστε μια εγγραφή PTR σύνολο με μία μόνο εγγραφή  
Σε αυτήν την περίπτωση ' μου-arpa-τοποθεσία zone.com' αντιπροσωπεύει τη ζώνη ARPA που αντιπροσωπεύει την περιοχή διευθύνσεων IP.  Κάθε εγγραφή PTR ρύθμιση σε αυτήν τη ζώνη αντιστοιχεί σε μια διεύθυνση IP μέσα σε αυτήν την περιοχή διευθύνσεων IP.    

    azure network dns record-set add-record myresourcegroup my-arpa-zone.com "10" PTR -P "myservice.contoso.com"   

#### <a name="create-an-srv-record-set-with-a-single-record"></a>Δημιουργήστε μια εγγραφή SRV που έχουν οριστεί με μία μόνο εγγραφή

Εάν θέλετε να δημιουργήσετε μια εγγραφή SRV στον ριζικό κατάλογο της ζώνης, μπορείτε να καθορίσετε "_service" και "_protocol" στο όνομά της εγγραφής. Χωρίς να χρειάζεται να συμπεριλάβετε "@" στο όνομα της εγγραφής.


    azure network dns record-set create myresourcegroup contoso.com "_sip._tls" SRV --ttl 300

    azure network dns record-set add-record myresourcegroup contoso.com  "_sip._tls" SRV -p 0 - w 5 -o 8080 -u "sip.contoso.com"

#### <a name="create-a-txt-record-set-with-single-record"></a>Δημιουργήστε μια εγγραφή TXT που έχουν οριστεί με μία μόνο εγγραφή

    azure network dns record-set create myresourcegroup contoso.com "test-TXT" TXT --ttl 300

    azure network dns record-set add-record myresourcegroup contoso.com "test-txt" TXT -x "this is a TXT record"
