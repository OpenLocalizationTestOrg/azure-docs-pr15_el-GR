<properties
   pageTitle="Δείγμα εφαρμογής για χρήση με ασφάλεια όριο περιβάλλοντα | Microsoft Azure"
   description="Ανάπτυξη αυτήν την εφαρμογή web απλό μετά τη δημιουργία ενός DMZ για να ελέγξετε σεναρίων ροής κυκλοφορίας"
   services="virtual-network"
   documentationCenter="na"
   authors="tracsman"
   manager="rossort"
   editor=""/>

<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/01/2016"
   ms.author="jonor"/>

# <a name="sample-application-for-use-with-security-boundary-environments"></a>Δείγμα εφαρμογής για χρήση με περιβάλλοντα όριο ασφαλείας

[Επιστρέψτε στη σελίδα βέλτιστες πρακτικές ασφαλείας όριο][HOME]

Αυτές οι δέσμες ενεργειών PowerShell μπορεί να εκτελεστεί τοπικά στους διακομιστές IIS01 και AppVM01 να εγκαταστήσετε και να ρυθμίσετε μια εφαρμογή web πολύ απλής που εμφανίζει μια σελίδα html από το διακομιστή IIS01 προσκηνίου με περιεχόμενο από το διακομιστή AppVM01 παρασκηνίου.

Αυτή η εφαρμογή θα παρέχει ένα απλό περιβάλλον δοκιμής για πολλά από τα παραδείγματα DMZ και με ποιον τρόπο τις αλλαγές στην τα τελικά σημεία, NSGs, UDR και τείχος προστασίας των κανόνων μπορεί να επηρεάσει ροών κίνησης.

## <a name="firewall-rule-to-allow-icmp"></a>Κανόνας τείχους προστασίας για να επιτρέψετε ICMP
Δήλωση απλό PowerShell μπορεί να εκτελεστεί σε οποιαδήποτε Εικονική των Windows για να επιτρέψετε την κυκλοφορία ICMP (Ping). Αυτό θα σας επιτρέψει για ευκολότερη δοκιμή και αντιμετώπιση προβλημάτων, επιτρέποντας το πρωτόκολλο ping για τη μεταβίβαση μέσω του τείχους προστασίας των windows (για τα περισσότερα Linux distros ICMP είναι ενεργοποιημένη από προεπιλογή).

    # Turn On ICMPv4
    New-NetFirewallRule -Name Allow_ICMPv4 -DisplayName "Allow ICMPv4" `
        -Protocol ICMPv4 -Enabled True -Profile Any -Action Allow

**Σημείωση:** Εάν χρησιμοποιείτε το παρακάτω δέσμες ενεργειών, αυτή η προσθήκη κανόνα τείχος προστασίας είναι η πρώτη πρόταση.

## <a name="iis01---web-application-installation-script"></a>IIS01 - δέσμη ενεργειών εγκατάστασης της εφαρμογής Web
Θα αυτήν τη δέσμη ενεργειών:

1.  Άνοιγμα IMCPv4 (Ping) του τοπικού διακομιστή τείχος προστασίας των windows για ευκολότερη δοκιμές
2.  Εγκατάσταση των υπηρεσιών IIS και του .net Framework v4.5
3.  Δημιουργήστε μια σελίδα web ASP.NET και ένα αρχείο Web.config
4.  Αλλάξτε το προεπιλεγμένο χώρο συγκέντρωσης εφαρμογών για να διευκολύνετε την πρόσβαση στο αρχείο
5.  Ορισμός του ανώνυμου χρήστη για το λογαριασμό διαχειριστή και τον κωδικό πρόσβασης

Αυτή η δέσμη ενεργειών PowerShell πρέπει να εκτελείται τοπικά ενώ είχατε RDP σε IIS01.

    # IIS Server Post Build Config Script
    # Get Admin Account and Password
        Write-Host "Please enter the admin account information used to create this VM:" -ForegroundColor Cyan
        $theAdmin = Read-Host -Prompt "The Admin Account Name (no domain or machine name)"
        $thePassword = Read-Host -Prompt "The Admin Password"
        
    # Turn On ICMPv4
        Write-Host "Creating ICMP Rule in Windows Firewall" -ForegroundColor Cyan
        New-NetFirewallRule -Name Allow_ICMPv4 -DisplayName "Allow ICMPv4" -Protocol ICMPv4 -Enabled True -Profile Any -Action Allow
        
    # Install IIS
        Write-Host "Installing IIS and .Net 4.5, this can take some time, like 15+ minutes..." -ForegroundColor Cyan
        add-windowsfeature Web-Server, Web-WebServer, Web-Common-Http, Web-Default-Doc, Web-Dir-Browsing, Web-Http-Errors, Web-Static-Content, Web-Health, Web-Http-Logging, Web-Performance, Web-Stat-Compression, Web-Security, Web-Filtering, Web-App-Dev, Web-ISAPI-Ext, Web-ISAPI-Filter, Web-Net-Ext, Web-Net-Ext45, Web-Asp-Net45, Web-Mgmt-Tools, Web-Mgmt-Console
        
    # Create Web App Pages
        Write-Host "Creating Web page and Web.Config file" -ForegroundColor Cyan
        $MainPage = '<%@ Page Language="vb" AutoEventWireup="false" %>
        <%@ Import Namespace="System.IO" %>
        <script language="vb" runat="server">
            Protected Sub Page_Load(ByVal sender As Object, ByVal e As System.EventArgs) Handles Me.Load
                Dim FILENAME As String = "\\10.0.2.5\WebShare\Rand.txt"
                Dim objStreamReader As StreamReader
                objStreamReader = File.OpenText(FILENAME)
                Dim contents As String = objStreamReader.ReadToEnd()
                lblOutput.Text = contents
                objStreamReader.Close()
                lblTime.Text = Now()
            End Sub
        </script>
            
        <!DOCTYPE html>
        <html xmlns="http://www.w3.org/1999/xhtml">
        <head runat="server">
            <title>DMZ Example App</title>
        </head>
        <body style="font-family: Optima,Segoe,Segoe UI,Candara,Calibri,Arial,sans-serif;">
          <form id="frmMain" runat="server">
            <div>
              <h1>Looks like you made it!</h1>
              This is a page from the inside (a web server on a private network),<br />
              and it is making its way to the outside! (If you are viewing this from the internet)<br />
              <br />
              The following sections show:
              <ul style="margin-top: 0px;">
                <li> Local Server Time - Shows if this page is or isnt cached anywhere</li>
                <li> File Output - Shows that the web server is reaching AppVM01 on the backend subnet and successfully returning content</li>
                <li> Image from the Internet - Doesnt really show anything, but it made me happy to see this when the app worked</li>
              </ul>
              <div style="border: 2px solid #8AC007; border-radius: 25px; padding: 20px; margin: 10px; width: 650px;">
                <b>Local Web Server Time</b>: <asp:Label runat="server" ID="lblTime" /></div>
              <div style="border: 2px solid #8AC007; border-radius: 25px; padding: 20px; margin: 10px; width: 650px;">
                <b>File Output from AppVM01</b>: <asp:Label runat="server" ID="lblOutput" /></div>
              <div style="border: 2px solid #8AC007; border-radius: 25px; padding: 20px; margin: 10px; width: 650px;">
                <b>Image File Linked from the Internet</b>:<br />
                <br />
                <img src="http://sd.keepcalm-o-matic.co.uk/i/keep-calm-you-made-it-7.png" alt="You made it!" width="150" length="175"/></div>
            </div>
          </form>
        </body>
        </html>'
        
        $WebConfig ='<?xml version="1.0" encoding="utf-8"?>
        <configuration>
          <system.web>
            <compilation debug="true" strict="false" explicit="true" targetFramework="4.5" />
            <httpRuntime targetFramework="4.5" />
            <identity impersonate="true" />
            <customErrors mode="Off"/>
          </system.web>
          <system.webServer>
            <defaultDocument>
              <files>
                <add value="Home.aspx" />
              </files>
            </defaultDocument>
          </system.webServer>
        </configuration>'
            
        $MainPage | Out-File -FilePath "C:\inetpub\wwwroot\Home.aspx" -Encoding ascii
        $WebConfig | Out-File -FilePath "C:\inetpub\wwwroot\Web.config" -Encoding ascii
    
    # Set App Pool to Clasic Pipeline to remote file access will work easier
        Write-Host "Updaing IIS Settings" -ForegroundColor Cyan
        c:\windows\system32\inetsrv\appcmd.exe set app "Default Web Site/" /applicationPool:".NET v4.5 Classic"
        c:\windows\system32\inetsrv\appcmd.exe set config "Default Web Site/" /section:system.webServer/security/authentication/anonymousAuthentication /userName:$theAdmin /password:$thePassword /commit:apphost
        
    # Make sure the IIS settings take
        Write-Host "Restarting the W3SVC" -ForegroundColor Cyan
        Restart-Service -Name W3SVC
        
        Write-Host
        Write-Host "Web App Creation Successfull!" -ForegroundColor Green
        Write-Host


## <a name="appvm01---file-server-installation-script"></a>AppVM01 - δέσμη ενεργειών εγκατάστασης διακομιστή αρχείων
Αυτή η δέσμη ενεργειών ρυθμίζει παρασκηνίου για αυτήν την απλή εφαρμογή. Θα αυτήν τη δέσμη ενεργειών:

1.  Άνοιγμα IMCPv4 (Ping) του τείχους προστασίας για ευκολότερη σκοπούς δοκιμής
2.  Δημιουργία ενός νέου καταλόγου
3.  Δημιουργήστε ένα αρχείο κειμένου για να απομακρυσμένη πρόσβαση από την ιστοσελίδα παραπάνω
4.  Ορισμός δικαιωμάτων στο στον κατάλογο και αρχείου για ανώνυμη για να επιτρέψετε την πρόσβαση
5.  Απενεργοποιήστε την επιλογή IE βελτιωμένη ασφάλεια για να επιτρέψετε σε πιο εύκολη περιήγηση από αυτόν το διακομιστή 

>[AZURE.IMPORTANT] **Βέλτιστη πρακτική**: Απενεργοποίηση ποτέ IE βελτιωμένη ασφάλεια σε ένα διακομιστή παραγωγής, καθώς και γενικά είναι καλή ιδέα να περιηγηθείτε στο web από ένα διακομιστή παραγωγής. Επίσης, Άνοιγμα κοινόχρηστων αρχείων για ανώνυμη πρόσβαση είναι καλή ιδέα, αλλά ολοκληρώθηκε εδώ για λόγους ευκολίας.

Αυτή η δέσμη ενεργειών PowerShell πρέπει να εκτελείται τοπικά ενώ είχατε RDP σε AppVM01. Εκτέλεση ως διαχειριστής για να εξασφαλίσετε την επιτυχημένη εκτέλεσης απαιτείται PowerShell.
    
    # AppVM01 Server Post Build Config Script
    # PowerShell must be run as Administrator for Net Share commands to work
    
    # Turn On ICMPv4
        New-NetFirewallRule -Name Allow_ICMPv4 -DisplayName "Allow ICMPv4" -Protocol ICMPv4 -Enabled True -Profile Any -Action Allow
    
    # Create Directory
        New-Item "C:\WebShare" -ItemType Directory
    
    # Write out Rand.txt
        $FileContent = "Hello, I'm the contents of a remote file on AppVM01."
        $FileContent | Out-File -FilePath "C:\WebShare\Rand.txt" -Encoding ascii
    
    # Set Permissions on share
        $Acl = Get-Acl "C:\WebShare"
        $AccessRule = New-Object system.security.accesscontrol.filesystemaccessrule("Everyone","ReadAndExecute, Synchronize","ContainerInherit, ObjectInherit","InheritOnly","Allow")
        $Acl.SetAccessRule($AccessRule)
        Set-Acl "C:\WebShare" $Acl
    
    # Create network share
        Net Share WebShare=C:\WebShare "/grant:Everyone,READ"
    
    # Turn Off IE Enhanced Security Configuration for Admins
        Set-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Active Setup\Installed Components\{A509B1A7-37EF-4b3f-8CFC-4F3A74704073}" -Name "IsInstalled" -Value 0
    
        Write-Host
        Write-Host "File Server Setup Successfull!" -ForegroundColor Green
        Write-Host
    

## <a name="dns01---dns-server-installation-script"></a>DNS01 - δέσμη ενεργειών εγκατάστασης διακομιστή DNS
Δεν υπάρχει καμία δέσμης ενεργειών που περιλαμβάνονται σε αυτήν την εφαρμογή δείγμα ρύθμισης του διακομιστή DNS. Εάν δοκιμές από τους κανόνες τείχους προστασίας, NSG ή UDR πρέπει να περιλαμβάνει την κυκλοφορία DNS, το διακομιστή DNS01 θα πρέπει να γίνει εγκατάσταση με μη αυτόματο τρόπο. Το αρχείο xml ρύθμισης παραμέτρων δικτύου για δύο παραδείγματα περιλαμβάνει DNS01 ως πρωτεύοντα διακομιστή DNS και τη δημόσια διακομιστή DNS που φιλοξενείται από επίπεδο 3 ως αντίγραφο ασφαλείας διακομιστή DNS. Ο διακομιστής DNS 3 επίπεδο αποτελεί πραγματική διακομιστή DNS που χρησιμοποιείται για την κυκλοφορία μη τοπικής και με DNS01 δεν εγκατάστασης, θα προκύψει χωρίς τοπικού DNS.

<!--Link References-->
[HOME]: ../best-practices-network-security.md
