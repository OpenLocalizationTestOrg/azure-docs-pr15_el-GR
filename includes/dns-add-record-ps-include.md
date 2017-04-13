### <a name="create-an-aaaa-record-set-with-a-single-record"></a>Δημιουργία εγγραφής AAAA σύνολο με μία μόνο εγγραφή

    $rs = New-AzureRmDnsRecordSet -Name "test-aaaa" -RecordType AAAA -Ttl 60 -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Add-AzureRmDnsRecordConfig -RecordSet $rs -Ipv6Address "2607:f8b0:4009:1803::1005"
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="create-a-cname-record-set-with-a-single-record"></a>Δημιουργήστε μια εγγραφή CNAME που έχουν οριστεί με μία μόνο εγγραφή

    $rs = New-AzureRmDnsRecordSet -Name "test-cname" -RecordType CNAME -Ttl 60 -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Add-AzureRmDnsRecordConfig -RecordSet $rs -Cname "www.contoso.com"
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="create-an-mx-record-set-with-a-single-record"></a>Δημιουργήστε μια εγγραφή MX που έχουν οριστεί με μία μόνο εγγραφή

Σε αυτό το παράδειγμα, μπορούμε να χρησιμοποιήσουμε το όνομα του συνόλου εγγραφών "@" για να δημιουργήσετε μια εγγραφή MX στην κορυφή ζώνη (σε αυτήν την περίπτωση, "contoso.com"). Αυτό είναι κοινά για εγγραφές MX.

    $rs = New-AzureRmDnsRecordSet -Name "@" -RecordType MX -Ttl 60 -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Add-AzureRmDnsRecordConfig -RecordSet $rs -Exchange "mail.contoso.com" -Preference 5
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="create-an-ns-record-set-with-a-single-record"></a>Δημιουργήστε μια εγγραφή NS σύνολο με μία μόνο εγγραφή

    $rs = New-AzureRmDnsRecordSet -Name "test-ns" -RecordType NS -Ttl 60 -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Add-AzureRmDnsRecordConfig -RecordSet $rs -Nsdname "ns1.contoso.com"
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="create-a-ptr-record-set-with-a-single-record"></a>Δημιουργήστε μια εγγραφή PTR σύνολο με μία μόνο εγγραφή
Σε αυτήν την περίπτωση ' μου-arpa-τοποθεσία zone.com' αντιπροσωπεύει τη ζώνη ARPA που αντιπροσωπεύει την περιοχή διευθύνσεων IP.  Κάθε εγγραφή PTR ρύθμιση σε αυτήν τη ζώνη αντιστοιχεί σε μια διεύθυνση IP μέσα σε αυτήν την περιοχή διευθύνσεων IP.  

    $rs = New-AzureRmDnsRecordSet -Name "10" -RecordType PTR -Ttl 3600 -ZoneName my-arpa-zone.com -ResourceGroupName MyAzureResourceGroup
    Add-AzureRmDnsRecordConfig -RecordSet $rs -Ptrdname "myservice.contoso.com"
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="create-an-srv-record-set-with-a-single-record"></a>Δημιουργήστε μια εγγραφή SRV που έχουν οριστεί με μία μόνο εγγραφή

Εάν θέλετε να δημιουργήσετε μια εγγραφή SRV στη ρίζα της μια ζώνη, απλώς καθορίστε *_service* και *_protocol* στο όνομα της εγγραφής. Χωρίς να χρειάζεται να συμπεριλάβετε "@" στο όνομα της εγγραφής.

    $rs = New-AzureRmDnsRecordSet -Name "_sip._tls" -RecordType SRV -Ttl 60 -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Add-AzureRmDnsRecordConfig -RecordSet $rs –Priority 0 –Weight 5 –Port 8080 –Target "sip.contoso.com"
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="create-a-txt-record-set-with-a-single-record"></a>Δημιουργήστε μια εγγραφή TXT που έχουν οριστεί με μία μόνο εγγραφή

    $rs = New-AzureRmDnsRecordSet -Name "test-txt" -RecordType TXT -Ttl 60 -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Add-AzureRmDnsRecordConfig -RecordSet $rs -Value "This is a TXT record"
    Set-AzureRmDnsRecordSet -RecordSet $rs
