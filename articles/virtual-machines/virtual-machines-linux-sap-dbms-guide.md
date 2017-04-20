<properties
   pageTitle="NetWeaver SAP σε Linux εικονικές μηχανές (ΣΠΣ)-Οδηγός ανάπτυξης του DBMS | Microsoft Azure"
   description="NetWeaver SAP σε Linux εικονικές μηχανές (ΣΠΣ) – DBMS Οδηγός ανάπτυξης"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="MSSedusch"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"
   keywords=""/>
<tags
   ms.service="virtual-machines-linux"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="08/18/2016"
   ms.author="sedusch"/>

# <a name="sap-netweaver-on-azure-virtual-machines-vms--dbms-deployment-guide"></a>NetWeaver SAP στο Azure εικονικές μηχανές (ΣΠΣ) – DBMS Οδηγός ανάπτυξης

[767598]:https://service.sap.com/sap/support/notes/767598
[773830]:https://service.sap.com/sap/support/notes/773830
[826037]:https://service.sap.com/sap/support/notes/826037
[965908]:https://service.sap.com/sap/support/notes/965908
[1031096]:https://service.sap.com/sap/support/notes/1031096
[1139904]:https://service.sap.com/sap/support/notes/1139904
[1173395]:https://service.sap.com/sap/support/notes/1173395
[1245200]:https://service.sap.com/sap/support/notes/1245200
[1409604]:https://service.sap.com/sap/support/notes/1409604
[1558958]:https://service.sap.com/sap/support/notes/1558958
[1585981]:https://service.sap.com/sap/support/notes/1585981
[1588316]:https://service.sap.com/sap/support/notes/1588316
[1590719]:https://service.sap.com/sap/support/notes/1590719
[1597355]:https://service.sap.com/sap/support/notes/1597355
[1605680]:https://service.sap.com/sap/support/notes/1605680
[1619720]:https://service.sap.com/sap/support/notes/1619720
[1619726]:https://service.sap.com/sap/support/notes/1619726
[1619967]:https://service.sap.com/sap/support/notes/1619967
[1750510]:https://service.sap.com/sap/support/notes/1750510
[1752266]:https://service.sap.com/sap/support/notes/1752266
[1757924]:https://service.sap.com/sap/support/notes/1757924
[1757928]:https://service.sap.com/sap/support/notes/1757928
[1758182]:https://service.sap.com/sap/support/notes/1758182
[1758496]:https://service.sap.com/sap/support/notes/1758496
[1772688]:https://service.sap.com/sap/support/notes/1772688
[1814258]:https://service.sap.com/sap/support/notes/1814258
[1882376]:https://service.sap.com/sap/support/notes/1882376
[1909114]:https://service.sap.com/sap/support/notes/1909114
[1922555]:https://service.sap.com/sap/support/notes/1922555
[1928533]:https://service.sap.com/sap/support/notes/1928533
[1941500]:https://service.sap.com/sap/support/notes/1941500
[1956005]:https://service.sap.com/sap/support/notes/1956005
[1973241]:https://service.sap.com/sap/support/notes/1973241
[1984787]:https://service.sap.com/sap/support/notes/1984787
[1999351]:https://service.sap.com/sap/support/notes/1999351
[2002167]:https://service.sap.com/sap/support/notes/2002167
[2015553]:https://service.sap.com/sap/support/notes/2015553
[2039619]:https://service.sap.com/sap/support/notes/2039619
[2121797]:https://service.sap.com/sap/support/notes/2121797
[2134316]:https://service.sap.com/sap/support/notes/2134316
[2178632]:https://service.sap.com/sap/support/notes/2178632
[2191498]:https://service.sap.com/sap/support/notes/2191498
[2233094]:https://service.sap.com/sap/support/notes/2233094
[2243692]:https://service.sap.com/sap/support/notes/2243692

[azure-cli]:../xplat-cli-install.md
[azure-portal]:https://portal.azure.com
[azure-ps]:../powershell-install-configure.md
[azure-quickstart-templates-github]:https://github.com/Azure/azure-quickstart-templates
[azure-script-ps]:https://go.microsoft.com/fwlink/p/?LinkID=395017
[azure-subscription-service-limits]:../azure-subscription-service-limits.md
[azure-subscription-service-limits-subscription]:../azure-subscription-service-limits.md#subscription

[dbms-guide]:virtual-machines-linux-sap-dbms-guide.md (SAP NetWeaver στην Linux εικονικές μηχανές (ΣΠΣ)-Οδηγός ανάπτυξης του DBMS) [dbms-guide-2.1]:virtual-machines-linux-sap-dbms-guide.md#c7abf1f0-c927-4a7c-9c1d-c7b5b3b7212f (προσωρινή αποθήκευση για ΣΠΣ και VHD) [dbms-guide-2.2]:virtual-machines-linux-sap-dbms-guide.md#c8e566f9-21b7-4457-9f7f-126036971a91 (λογισμικό RAID) [dbms-guide-2.3]:virtual-machines-linux-sap-dbms-guide.md#10b041ef-c177-498a-93ed-44b3441ab152 (χώρος αποθήκευσης Microsoft Azure) [dbms-guide-2]:virtual-machines-linux-sap-dbms-guide.md#65fa79d6-a85f-47ee-890b-22e794f51a64 (δομή μιας ανάπτυξης RDBMS) [dbms-guide-3]:virtual-machines-linux-sap-dbms-guide.md#871dfc27-e509-4222-9370-ab1de77021c3 (υψηλή διαθεσιμότητα και αποκατάσταση ΣΠΣ Azure) [dbms-guide-5.5.1]:virtual-machines-linux-sap-dbms-guide.md# 0fef0e79-d3fe-4ae2-85af-73666a6f7268 (SQL Server 2012 SP1 CU4 και νεότερες εκδόσεις) [dbms-guide-5.5.2]:virtual-machines-linux-sap-dbms-guide.md#f9071eff-9d72-4f47-9da4-1852d782087b (SQL Server 2012 SP1 CU3 και παλαιότερες εκδόσεις) [dbms-guide-5.6]:virtual-machines-linux-sap-dbms-guide.md#1b353e38-21b3-4310-aeb6-a77e7c8e81c8 (χρησιμοποιώντας μια SQL Server εικόνες από το Microsoft Azure Marketplace) [dbms-guide-5.8]:virtual-machines-linux-sap-dbms-guide.md#9053f720-6f3b-4483-904d-15dc54141e30 (γενικά SQL Server για SAP στη σύνοψη Azure) [dbms-guide-5]:virtual-machines-linux-sap-dbms-guide.md#3264829e-075e-4d25-966e-a49dad878737 (συγκεκριμένες απαιτήσεις για SQL Server RDBMS) [dbms-guide-8.4.1]:virtual-machines-linux-sap-dbms-guide.md#b48cfe3b-48e9-4f5b-a783-1d29155bd573 (ρύθμιση παραμέτρων αποθήκευσης) [dbms-guide-8.4.2]:virtual-machines-linux-sap-dbms-guide.md# 23c78d3b-ca5a-4e72-8a24-645d141a3f5d (δημιουργία αντιγράφων ασφαλείας και επαναφορά) [dbms-guide-8.4.3]:virtual-machines-linux-sap-dbms-guide.md#77cd2fbb-307e-4cbf-a65f-745553f72d2c (ζητήματα επιδόσεων για δημιουργία αντιγράφων ασφαλείας και επαναφορά) [dbms-guide-8.4.4]:virtual-machines-linux-sap-dbms-guide.md#f77c1436-9ad8-44fb-a331-8671342de818 (άλλο) [dbms-guide-900-sap-cache-server-on-premises]:virtual-machines-linux-sap-dbms-guide.md#642f746c-e4d4-489d-bf63-73e80177a0a8

[dbms-guide-figure-100]:./media/virtual-machines-shared-sap-dbms-guide/100_storage_account_types.png
[dbms-guide-figure-200]:./media/virtual-machines-shared-sap-dbms-guide/200-ha-set-for-dbms-ha.png
[dbms-guide-figure-300]:./media/virtual-machines-shared-sap-dbms-guide/300-reference-config-iaas.png
[dbms-guide-figure-400]:./media/virtual-machines-shared-sap-dbms-guide/400-sql-2012-backup-to-blob-storage.png
[dbms-guide-figure-500]:./media/virtual-machines-shared-sap-dbms-guide/500-sql-2012-backup-to-blob-storage-different-containers.png
[dbms-guide-figure-600]:./media/virtual-machines-shared-sap-dbms-guide/600-iaas-maxdb.png
[dbms-guide-figure-700]:./media/virtual-machines-shared-sap-dbms-guide/700-livecach-prod.png
[dbms-guide-figure-800]:./media/virtual-machines-shared-sap-dbms-guide/800-azure-vm-sap-content-server.png
[dbms-guide-figure-900]:./media/virtual-machines-shared-sap-dbms-guide/900-sap-cache-server-on-premises.png

[deployment-guide]:virtual-machines-linux-sap-deployment-guide.md (SAP NetWeaver στην Linux εικονικές μηχανές (ΣΠΣ)-Οδηγός ανάπτυξης) [deployment-guide-2.2]:virtual-machines-linux-sap-deployment-guide.md#42ee2bdb-1efc-4ec7-ab31-fe4c22769b94 (SAP πόρους) [deployment-guide-3.1.2]:virtual-machines-linux-sap-deployment-guide.md#3688666f-281f-425b-a312-a77e7db2dfab (μια Εικονική με μια προσαρμοσμένη εικόνα για την ανάπτυξη) [deployment-guide-3.2]:virtual-machines-linux-sap-deployment-guide.md#db477013-9060-4602-9ad4-b0316f8bb281 (σενάριο 1: ανάπτυξη μια Εικονική από το Azure Marketplace για SAP) [deployment-guide-3.3]:virtual-machines-linux-sap-deployment-guide.md#54a1fc6d-24fd-4feb-9c57-ac588a55dff2 (σενάριο 2: ανάπτυξη μια Εικονική με μια προσαρμοσμένη εικόνα για SAP) [deployment-guide-3.4]:virtual-machines-linux-sap-deployment-guide.md#a9a60133-a763-4de8-8986-ac0fa33aa8c1 () Σενάριο 3: Μετακίνηση μια Εικονική από εσωτερικής εγκατάστασης με χρήση ενός VHD Azure μη γενίκευση με SAP) [deployment-guide-3]:virtual-machines-linux-sap-deployment-guide.md#b3253ee3-d63b-4d74-a49b-185e76c4088e (ανάπτυξη σενάρια του ΣΠΣ για SAP στο Microsoft Azure) [deployment-guide-4.1]:virtual-machines-linux-sap-deployment-guide.md#604bcec2-8b6e-48d2-a944-61b0f5dee2f7 (cmdlet του PowerShell για την ανάπτυξη Azure) [deployment-guide-4.2]:virtual-machines-linux-sap-deployment-guide.md#7ccf6c3e-97ae-4a7a-9c75-e82c37beb18e (λήψη και τη SAP, εισαγωγή σχετικές cmdlet του PowerShell) [deployment-guide-4.3]:virtual-machines-linux-sap-deployment-guide.md#31d9ecd6-b136-4c73-b61e-da4a29bbc9cc (Εικονική συμμετοχή σε τομέα εσωτερικής - μόνο για Windows) [deployment-guide-4.4.2]:virtual-machines-linux-sap-deployment-guide.md#6889ff12-eaaf-4f3c-97e1-7c9edc7f7542 (Linux) [ανάπτυξης-Οδηγός-4.4]: Virtual-machines-Linux-SAP-Deployment-Guide.MD#c7cbb0dc-52a4-49db-8e03-83e7edc2927d (λήψη, εγκατάσταση και ενεργοποίηση παράγοντας Εικονική Azure) [deployment-guide-4.5.1]:virtual-machines-linux-sap-deployment-guide.md#987cf279-d713-4b4c-8143-6b11589bb9d4 (Azure PowerShell) [deployment-guide-4.5.2]:virtual-machines-linux-sap-deployment-guide.md#408f3779-f422-4413-82f8-c57a23b4fc2f (Azure CLI) [deployment-guide-4.5]:virtual-machines-linux-sap-deployment-guide.md#d98edcd3-f2a1-49f7-b26a-07448ceb60ca (ρύθμιση παραμέτρων Azure βελτιωμένη παρακολούθηση επέκταση για SAP) [deployment-guide-5.1]:virtual-machines-linux-sap-deployment-guide.md#bb61ce92-8c5c-461f-8c53-39f5e5ed91f2 (Έλεγχος ετοιμότητας για βελτιωμένη παρακολούθηση Azure για SAP) [deployment-guide-5.2]:virtual-machines-linux-sap-deployment-guide.md#e2d592ff-b4ea-4a53-a91a-e5521edb6cd1 (Έλεγχος εύρυθμης λειτουργίας για Azure Παρακολούθηση ρύθμισης παραμέτρων υποδομή) [deployment-guide-5.3]:virtual-machines-linux-sap-deployment-guide.md#fe25a7da-4e4e-4388-8907-8abc2d33cfd8 (περαιτέρω βήματα αντιμετώπισης προβλημάτων της υποδομής παρακολούθηση Azure SAP)

[deployment-guide-configure-monitoring-scenario-1]:virtual-machines-linux-sap-deployment-guide.md#ec323ac3-1de9-4c3a-b770-4ff701def65b (Configure Monitoring)
[deployment-guide-configure-proxy]:virtual-machines-linux-sap-deployment-guide.md#baccae00-6f79-4307-ade4-40292ce4e02d (Configure Proxy)
[deployment-guide-figure-100]:./media/virtual-machines-shared-sap-deployment-guide/100-deploy-vm-image.png
[deployment-guide-figure-1000]:./media/virtual-machines-shared-sap-deployment-guide/1000-service-properties.png
[deployment-guide-figure-11]:virtual-machines-linux-sap-deployment-guide.md#figure-11
[deployment-guide-figure-1100]:./media/virtual-machines-shared-sap-deployment-guide/1100-azperflib.png
[deployment-guide-figure-1200]:./media/virtual-machines-shared-sap-deployment-guide/1200-cmd-test-login.png
[deployment-guide-figure-1300]:./media/virtual-machines-shared-sap-deployment-guide/1300-cmd-test-executed.png
[deployment-guide-figure-14]:virtual-machines-linux-sap-deployment-guide.md#figure-14
[deployment-guide-figure-1400]:./media/virtual-machines-shared-sap-deployment-guide/1400-azperflib-error-servicenotstarted.png
[deployment-guide-figure-300]:./media/virtual-machines-shared-sap-deployment-guide/300-deploy-private-image.png
[deployment-guide-figure-400]:./media/virtual-machines-shared-sap-deployment-guide/400-deploy-using-disk.png
[deployment-guide-figure-5]:virtual-machines-linux-sap-deployment-guide.md#figure-5
[deployment-guide-figure-50]:./media/virtual-machines-shared-sap-deployment-guide/50-forced-tunneling-suse.png
[deployment-guide-figure-500]:./media/virtual-machines-shared-sap-deployment-guide/500-install-powershell.png
[deployment-guide-figure-6]:virtual-machines-linux-sap-deployment-guide.md#figure-6
[deployment-guide-figure-600]:./media/virtual-machines-shared-sap-deployment-guide/600-powershell-version.png
[deployment-guide-figure-7]:virtual-machines-linux-sap-deployment-guide.md#figure-7
[deployment-guide-figure-700]:./media/virtual-machines-shared-sap-deployment-guide/700-install-powershell-installed.png
[deployment-guide-figure-760]:./media/virtual-machines-shared-sap-deployment-guide/760-azure-cli-version.png
[deployment-guide-figure-900]:./media/virtual-machines-shared-sap-deployment-guide/900-cmd-update-executed.png
[deployment-guide-figure-azure-cli-installed]:virtual-machines-linux-sap-deployment-guide.md#402488e5-f9bb-4b29-8063-1c5f52a892d0
[deployment-guide-figure-azure-cli-version]:virtual-machines-linux-sap-deployment-guide.md#0ad010e6-f9b5-4c21-9c09-bb2e5efb3fda
[deployment-guide-install-vm-agent-windows]:virtual-machines-linux-sap-deployment-guide.md#b2db5c9a-a076-42c6-9835-16945868e866
[deployment-guide-troubleshooting-chapter]:virtual-machines-linux-sap-deployment-guide.md#564adb4f-5c95-4041-9616-6635e83a810b (Checks and Troubleshooting for End-to-End Monitoring Setup for SAP on Azure)

[deploy-template-cli]:../resource-group-template-deploy.md#deploy-with-azure-cli-for-mac-linux-and-windows
[deploy-template-portal]:../resource-group-template-deploy.md#deploy-with-the-preview-portal
[deploy-template-powershell]:../resource-group-template-deploy.md#deploy-with-powershell

[dr-guide-classic]:http://go.microsoft.com/fwlink/?LinkID=521971

[getting-started]:virtual-machines-linux-sap-get-started.md
[getting-started-dbms]:virtual-machines-linux-sap-get-started.md#1343ffe1-8021-4ce6-a08d-3a1553a4db82
[getting-started-deployment]:virtual-machines-linux-sap-get-started.md#6aadadd2-76b5-46d8-8713-e8d63630e955
[getting-started-planning]:virtual-machines-linux-sap-get-started.md#3da0389e-708b-4e82-b2a2-e92f132df89c

[getting-started-windows-classic]:virtual-machines-windows-classic-sap-get-started.md
[getting-started-windows-classic-dbms]:virtual-machines-windows-classic-sap-get-started.md#c5b77a14-f6b4-44e9-acab-4d28ff72a930
[getting-started-windows-classic-deployment]:virtual-machines-windows-classic-sap-get-started.md#f84ea6ce-bbb4-41f7-9965-34d31b0098ea
[getting-started-windows-classic-dr]:virtual-machines-windows-classic-sap-get-started.md#cff10b4a-01a5-4dc3-94b6-afb8e55757d3
[getting-started-windows-classic-ha-sios]:virtual-machines-windows-classic-sap-get-started.md#4bb7512c-0fa0-4227-9853-4004281b1037
[getting-started-windows-classic-planning]:virtual-machines-windows-classic-sap-get-started.md#f2a5e9d8-49e4-419e-9900-af783173481c

[ha-guide-classic]:http://go.microsoft.com/fwlink/?LinkId=613056

[install-extension-cli]:virtual-machines-linux-enable-aem.md

[Logo_Linux]:./media/virtual-machines-shared-sap-shared/Linux.png
[Logo_Windows]:./media/virtual-machines-shared-sap-shared/Windows.png

[msdn-set-azurermvmaemextension]:https://msdn.microsoft.com/library/azure/mt670598.aspx

[planning-guide]:virtual-machines-linux-sap-planning-guide.md (SAP NetWeaver στην Linux εικονικές μηχανές (ΣΠΣ) – Προγραμματισμός και τον Οδηγό υλοποίησης) [planning-guide-1.2]:virtual-machines-linux-sap-planning-guide.md#e55d1e22-c2c8-460b-9897-64622a34fdff (πόρους) [planning-guide-11]:virtual-machines-linux-sap-planning-guide.md#7cf991a1-badd-40a9-944e-7baae842a058 (υψηλής διαθεσιμότητας (HA) και αποκατάστασης από καταστροφή (DR) για NetWeaver SAP εκτελείται σε εικονικές μηχανές Windows Azure) [planning-guide-11.4.1]:virtual-machines-linux-sap-planning-guide.md#5d9d36f9-9058-435d-8367-5ad05f00de77 (υψηλή διαθεσιμότητα για διακομιστές εφαρμογών SAP) [planning-guide-11.5]:virtual-machines-linux-sap-planning-guide.md#4e165b58-74ca-474f-a7f4-5e695a93204f (με αυτόματη εκκίνηση για παρουσίες SAP) [planning-guide-2.1]:virtual-machines-linux-sap-planning-guide.md# 1625df66-4cc6-4d60-9202-de8a0b77f803 (μόνο στο Cloud - εικονική μηχανή αναπτύξεις σε Azure χωρίς εξαρτήσεις στο δίκτυο πελατών εσωτερικής εγκατάστασης) [planning-guide-2.2]:virtual-machines-linux-sap-planning-guide.md#f5b3b18c-302c-4bd8-9ab2-c388f1ab3d10 (διασταυρούμενο εσωτερικής εγκατάστασης - ανάπτυξη του μίας ή πολλών ΣΠΣ SAP σε Azure με την απαίτηση είναι πλήρως ενσωματωμένο στο δίκτυο εσωτερικής εγκατάστασης) [planning-guide-3.1]:virtual-machines-linux-sap-planning-guide.md#be80d1b9-a463-4845-bd35-f4cebdb5424a [planning-guide-3.2.1]:virtual-machines-linux-sap-planning-guide.md#df49dc09-141b-4f34-a4a2-990913b30358 (σφάλμα τομέων) [planning-guide-3.2.2]:virtual-machines-linux-sap-planning-guide.md#fc1ac8b2-e54a-487c-8581-d3cc6625e560 (αναβάθμιση τομέων) [planning-guide-3.2.3]:virtual-machines-linux-sap-planning-guide.md#18810088-(Azure περιοχές) F9BE - 4c 97-958a - 27996255c 665 (σύνολα διαθεσιμότητα Azure) [planning-guide-3.2]:virtual-machines-linux-sap-planning-guide.md#8d8ad4b8-6093-4b91-ac36-ea56d80dbf77 (το Microsoft Azure εικονική μηχανή έννοια) [planning-guide-3.3.2]:virtual-machines-linux-sap-planning-guide.md#ff5ad0f9-f7f4-4022-9102-af07aef3bc92 (χώρος αποθήκευσης Premium Azure) [planning-guide-5.1.1]:virtual-machines-linux-sap-planning-guide.md#4d175f1b-7353-4137-9d2f-817683c26e53 (Μετακίνηση μια Εικονική από εσωτερικής εγκατάστασης στο Azure με ένα δίσκο μη γενίκευση) [planning-guide-5.1.2]:virtual-machines-linux-sap-planning-guide.md#e18f7839-c0e2-4385-b1e6-4538453a285c (ανάπτυξη μια Εικονική με μια συγκεκριμένη εικόνα πελάτη) [planning-guide-5.2.1]:virtual-machines-linux-sap-planning-guide.md#1b287330-944b-495d-9ea7-94b83aff73ef (προετοιμασίας για μετακίνηση μια Εικονική από εσωτερικής εγκατάστασης στο Azure με ένα δίσκο μη γενίκευση) [ Planning-Guide-5.2.2]:Virtual-machines-Linux-SAP-Planning-Guide.MD#57f32b1c-0cba-4e57-ab6e-c39fe22b6ec3 (προετοιμασίας για την ανάπτυξη μια Εικονική με μια συγκεκριμένη εικόνα πελατών για SAP) [planning-guide-5.2]:virtual-machines-linux-sap-planning-guide.md#6ffb9f41-a292-40bf-9e70-8204448559e7 (προετοιμασία ΣΠΣ με SAP για Azure) [planning-guide-5.3.1]:virtual-machines-linux-sap-planning-guide.md#6e835de8-40b1-4b71-9f18-d45b20959b79 (διαφορά μεταξύ του Azure δίσκου και εικόνα Azure) [planning-guide-5.3.2]:virtual-machines-linux-sap-planning-guide.md#a43e40e6-1acc-4633-9816-8f095d5a7b6a (αποστολή ενός VHD από εσωτερικής εγκατάστασης σε Azure) [planning-guide-5.4.2]:virtual-machines-linux-sap-planning-guide.md#9789b076-2011-4afa-b2fe-b07a8aba58a1 (αντιγραφή δίσκων μεταξύ λογαριασμοί Azure χώρο αποθήκευσης) [planning-guide-5.5.1]:virtual-machines-linux-sap-planning-guide.md# 4efec401-91e0-40c0-8e64-f2dceadff646 (Εικονική/VHD δομή για αναπτύξεις SAP) [planning-guide-5.5.3]:virtual-machines-linux-sap-planning-guide.md#17e0d543-7e8c-4160-a7da-dd7117a1ad9d (ρύθμιση automount για προσαρτημένο δίσκο) [planning-guide-7.1]:virtual-machines-linux-sap-planning-guide.md#3e9c3690-da67-421a-bc3f-12c520d99a30 (μία Εικονική με SAP NetWeaver επίδειξη/εκπαίδευση σενάριο) [planning-guide-7]:virtual-machines-linux-sap-planning-guide.md#96a77628-a05e-475d-9df3-fb82217e8f14 (έννοιες του Cloud-Only ανάπτυξη των παρουσιών SAP) [planning-guide-9.1]:virtual-machines-linux-sap-planning-guide.md#6f0a47f3-a289-4090-a053-2521618a28c3 (Azure παρακολούθηση λύση για το SAP) [planning-guide-azure-premium-storage]:virtual-machines-linux-sap-planning-guide.md#ff5ad0f9-f7f4-4022-9102-af07aef3bc92 (χώρος αποθήκευσης Premium Azure)

[planning-guide-figure-100]:./media/virtual-machines-shared-sap-planning-guide/100-single-vm-in-azure.png
[planning-guide-figure-1300]:./media/virtual-machines-shared-sap-planning-guide/1300-ref-config-iaas-for-sap.png
[planning-guide-figure-1400]:./media/virtual-machines-shared-sap-planning-guide/1400-attach-detach-disks.png
[planning-guide-figure-1600]:./media/virtual-machines-shared-sap-planning-guide/1600-firewall-port-rule.png
[planning-guide-figure-1700]:./media/virtual-machines-shared-sap-planning-guide/1700-single-vm-demo.png
[planning-guide-figure-1900]:./media/virtual-machines-shared-sap-planning-guide/1900-vm-set-vnet.png
[planning-guide-figure-200]:./media/virtual-machines-shared-sap-planning-guide/200-multiple-vms-in-azure.png
[planning-guide-figure-2100]:./media/virtual-machines-shared-sap-planning-guide/2100-s2s.png
[planning-guide-figure-2200]:./media/virtual-machines-shared-sap-planning-guide/2200-network-printing.png
[planning-guide-figure-2300]:./media/virtual-machines-shared-sap-planning-guide/2300-sapgui-stms.png
[planning-guide-figure-2400]:./media/virtual-machines-shared-sap-planning-guide/2400-vm-extension-overview.png
[planning-guide-figure-2500]:./media/virtual-machines-shared-sap-planning-guide/2500-vm-extension-details.png
[planning-guide-figure-2600]:./media/virtual-machines-shared-sap-planning-guide/2600-sap-router-connection.png
[planning-guide-figure-2700]:./media/virtual-machines-shared-sap-planning-guide/2700-exposed-sap-portal.png
[planning-guide-figure-2800]:./media/virtual-machines-shared-sap-planning-guide/2800-endpoint-config.png
[planning-guide-figure-2900]:./media/virtual-machines-shared-sap-planning-guide/2900-azure-ha-sap-ha.png
[planning-guide-figure-300]:./media/virtual-machines-shared-sap-planning-guide/300-vpn-s2s.png
[planning-guide-figure-3000]:./media/virtual-machines-shared-sap-planning-guide/3000-sap-ha-on-azure.png
[planning-guide-figure-3200]:./media/virtual-machines-shared-sap-planning-guide/3200-sap-ha-with-sql.png
[planning-guide-figure-400]:./media/virtual-machines-shared-sap-planning-guide/400-vm-services.png
[planning-guide-figure-600]:./media/virtual-machines-shared-sap-planning-guide/600-s2s-details.png
[planning-guide-figure-700]:./media/virtual-machines-shared-sap-planning-guide/700-decision-tree-deploy-to-azure.png
[planning-guide-figure-800]:./media/virtual-machines-shared-sap-planning-guide/800-portal-vm-overview.png
[planning-guide-microsoft-azure-networking]:virtual-machines-linux-sap-planning-guide.md#61678387-8868-435d-9f8c-450b2424f5bd (Microsoft Azure Networking)
[planning-guide-storage-microsoft-azure-storage-and-data-disks]:virtual-machines-linux-sap-planning-guide.md#a72afa26-4bf4-4a25-8cf7-855d6032157f (Storage: Microsoft Azure Storage and Data Disks)

[powershell-install-configure]:../powershell-install-configure.md
[resource-group-authoring-templates]:../resource-group-authoring-templates.md
[resource-group-overview]:../resource-group-overview.md
[resource-groups-networking]:../virtual-network/resource-groups-networking.md
[sap-pam]:https://support.sap.com/pam (SAP Product Availability Matrix)
[sap-templates-2-tier-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-marketplace-image%2Fazuredeploy.json
[sap-templates-2-tier-os-disk]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-user-disk%2Fazuredeploy.json
[sap-templates-2-tier-user-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-2-tier-user-image%2Fazuredeploy.json
[sap-templates-3-tier-marketplace-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image%2Fazuredeploy.json
[sap-templates-3-tier-user-image]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-user-image%2Fazuredeploy.json
[storage-azure-cli]:../storage/storage-azure-cli.md
[storage-azure-cli-copy-blobs]:../storage/storage-azure-cli.md#copy-blobs
[storage-introduction]:../storage/storage-introduction.md
[storage-powershell-guide-full-copy-vhd]:../storage/storage-powershell-guide-full.md#how-to-copy-blobs-from-one-storage-container-to-another
[storage-premium-storage-preview-portal]:../storage/storage-premium-storage.md
[storage-redundancy]:../storage/storage-redundancy.md
[storage-scalability-targets]:../storage/storage-scalability-targets.md
[storage-use-azcopy]:../storage/storage-use-azcopy.md
[template-201-vm-from-specialized-vhd]:https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd
[templates-101-simple-windows-vm]:https://github.com/Azure/azure-quickstart-templates/tree/master/101-simple-windows-vm
[templates-101-vm-from-user-image]:https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image
[virtual-machines-linux-attach-disk-portal]:virtual-machines-linux-attach-disk-portal.md
[virtual-machines-azure-resource-manager-architecture]:../resource-manager-deployment-model.md
[virtual-machines-azurerm-versus-azuresm]:virtual-machines-linux-compare-deployment-models.md
[virtual-machines-windows-classic-configure-oracle-data-guard]:virtual-machines-windows-classic-configure-oracle-data-guard.md
[virtual-machines-linux-cli-deploy-templates]:virtual-machines-linux-cli-deploy-templates.md (Deploy and manage virtual machines by using Azure Resource Manager templates and the Azure CLI)
[virtual-machines-deploy-rmtemplates-powershell]:virtual-machines-windows-ps-manage.md (Manage virtual machines using Azure Resource Manager and PowerShell)
[virtual-machines-linux-agent-user-guide]:virtual-machines-linux-agent-user-guide.md
[virtual-machines-linux-agent-user-guide-command-line-options]:virtual-machines-linux-agent-user-guide.md#command-line-options
[virtual-machines-linux-capture-image]:virtual-machines-linux-capture-image.md
[virtual-machines-linux-capture-image-resource-manager]:virtual-machines-linux-capture-image.md
[virtual-machines-linux-capture-image-resource-manager-capture]:virtual-machines-linux-capture-image.md#capture-the-vm
[virtual-machines-linux-configure-raid]:virtual-machines-linux-configure-raid.md
[virtual-machines-linux-configure-lvm]:virtual-machines-linux-configure-lvm.md
[virtual-machines-linux-classic-create-upload-vhd-step-1]:virtual-machines-linux-classic-create-upload-vhd.md#step-1-prepare-the-image-to-be-uploaded
[virtual-machines-linux-create-upload-vhd-suse]:virtual-machines-linux-suse-create-upload-vhd.md
[virtual-machines-linux-redhat-create-upload-vhd]:virtual-machines-linux-redhat-create-upload-vhd.md
[virtual-machines-linux-how-to-attach-disk]:virtual-machines-linux-add-disk.md
[virtual-machines-linux-how-to-attach-disk-how-to-initialize-a-new-data-disk-in-linux]:virtual-machines-linux-add-disk.md#connect-to-the-linux-vm-to-mount-the-new-disk
[virtual-machines-linux-tutorial]:virtual-machines-linux-quick-create-cli.md
[virtual-machines-linux-update-agent]:virtual-machines-linux-update-agent.md
[virtual-machines-manage-availability]:virtual-machines-linux-manage-availability.md
[virtual-machines-ps-create-preconfigure-windows-resource-manager-vms]:virtual-machines-windows-create-powershell.md
[virtual-machines-sizes]:virtual-machines-linux-sizes.md
[virtual-machines-windows-classic-ps-sql-alwayson-availability-groups]:virtual-machines-windows-classic-ps-sql-alwayson-availability-groups.md
[virtual-machines-windows-classic-ps-sql-int-listener]:virtual-machines-windows-classic-ps-sql-int-listener.md
[virtual-machines-sql-server-high-availability-and-disaster-recovery-solutions]:virtual-machines-windows-sql-high-availability-dr.md
[virtual-machines-sql-server-infrastructure-services]:virtual-machines-windows-sql-server-iaas-overview.md
[virtual-machines-sql-server-performance-best-practices]:virtual-machines-windows-sql-performance.md
[virtual-machines-upload-image-windows-resource-manager]:virtual-machines-windows-upload-image.md
[virtual-machines-windows-tutorial]:virtual-machines-windows-hero-tutorial.md
[virtual-machines-workload-template-sql-alwayson]:https://azure.microsoft.com/documentation/templates/sql-server-2014-alwayson-dsc/
[virtual-network-deploy-multinic-arm-cli]:../virtual-network/virtual-network-deploy-multinic-arm-cli.md
[virtual-network-deploy-multinic-arm-ps]:../virtual-network/virtual-network-deploy-multinic-arm-ps.md
[virtual-network-deploy-multinic-arm-template]:../virtual-network/virtual-network-deploy-multinic-arm-template.md
[virtual-networks-configure-vnet-to-vnet-connection]:../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md
[virtual-networks-create-vnet-arm-pportal]:../virtual-network/virtual-networks-create-vnet-arm-pportal.md
[virtual-networks-manage-dns-in-vnet]:../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md
[virtual-networks-multiple-nics]:../virtual-network/virtual-networks-multiple-nics.md
[virtual-networks-nsg]:../virtual-network/virtual-networks-nsg.md
[virtual-networks-reserved-private-ip]:../virtual-network/virtual-networks-static-private-ip-arm-ps.md
[virtual-networks-static-private-ip-arm-pportal]:../virtual-network/virtual-networks-static-private-ip-arm-pportal.md
[virtual-networks-udr-overview]:../virtual-network/virtual-networks-udr-overview.md
[vpn-gateway-about-vpn-devices]:../vpn-gateway/vpn-gateway-about-vpn-devices.md
[vpn-gateway-create-site-to-site-rm-powershell]:../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md
[vpn-gateway-cross-premises-options]:../vpn-gateway/vpn-gateway-plan-design.md
[vpn-gateway-site-to-site-create]:../vpn-gateway/vpn-gateway-site-to-site-create.md
[vpn-gateway-vpn-faq]:../vpn-gateway/vpn-gateway-vpn-faq.md
[xplat-cli]:../xplat-cli-install.md
[xplat-cli-azure-resource-manager]:../xplat-cli-azure-resource-manager.md

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]μοντέλο κλασική ανάπτυξης.

Αυτός ο οδηγός αποτελεί μέρος την τεκμηρίωση σχετικά με την εφαρμογή και την ανάπτυξη του λογισμικού SAP στο Microsoft Azure. Πριν από την ανάγνωση αυτού του οδηγού, διαβάστε [Προγραμματισμός και τον Οδηγό υλοποίησης] [-Οδηγός σχεδιασμού]. Αυτό το έγγραφο καλύπτει την ανάπτυξη του διάφορα συστήματα διαχείρισης σχεσιακή βάση δεδομένων (RDBMS) και τα σχετικά προϊόντα σε συνδυασμό με SAP στη Microsoft εικονικές μηχανές Windows Azure (ΣΠΣ) χρησιμοποιώντας την υποδομή Azure ως ένα δυνατότητες των υπηρεσιών (IaaS).

Το χαρτί συμπληρώνει την τεκμηρίωση εγκατάστασης SAP και SAP σημειώσεις που αντιπροσωπεύουν τα κύρια πόρους για εγκαταστάσεις και υλοποιήσεις του λογισμικού SAP στη δεδομένη πλατφόρμες

[AZURE.INCLUDE [windows-warning](../../includes/virtual-machines-linux-sap-warning.md)]

## <a name="general-considerations"></a>Γενικά θέματα
Σε αυτό το κεφάλαιο, ζητήματα που εκτελούνται SAP που σχετίζονται με τα συστήματα DBMS ΣΠΣ Azure εισάγονται. Υπάρχουν μερικές αναφορές σε συγκεκριμένα συστήματα DBMS σε αυτό το κεφάλαιο. Αντί για αυτό το συγκεκριμένο συστήματα DBMS αντιμετωπίζονται μέσα σε αυτό το έγγραφο, μετά από αυτό το κεφάλαιο.

### <a name="definitions-upfront"></a>Ορισμοί προκαταβολικά
Σε ολόκληρο το έγγραφο, θα χρησιμοποιήσουμε τους εξής όρους:

* IaaS: Υποδομή ως υπηρεσία.
* PaaS: Πλατφόρμα ως υπηρεσία.
* Απα: Λογισμικό ως υπηρεσία.
* Στοιχείο SAP: μια μεμονωμένη SAP εφαρμογή όπως ECC, εύρους ζώνης που ΈΧΕΙ, Διαχείριση λύσεων ή ΕΡ.  Στοιχεία SAP μπορεί να βασίζονται σε παραδοσιακά τεχνολογίες ABAP ή Java ή μια εφαρμογή μη-NetWeaver βάσει όπως αντικείμενα εργασίας.
* Περιβάλλον SAP: ένα ή περισσότερα στοιχεία SAP ομαδοποιημένα λογικά να εκτελεί μια επιχειρηματική λειτουργία όπως ανάπτυξης, QAS, εκπαίδευση, DR ή παραγωγής.
* Οριζόντιος SAP: Αυτό αναφέρεται σε ολόκληρο πόρους SAP ενός πελάτη Οριζόντιος IT. Τοπίου SAP περιλαμβάνει όλα παραγωγής και μη παραγωγής περιβάλλοντα.
* Σύστημα SAP: Ο συνδυασμός DBMS επιπέδου και τα επίπεδα εφαρμογών π.χ. ένα σύστημα ανάπτυξης SAP ERP, SAP εύρους ζώνης που ΈΧΕΙ δοκιμή συστήματος, σύστημα παραγωγής SAP CRM, κ.λπ. Στο Azure αναπτύξεις δεν υποστηρίζεται για να διαιρέσετε αυτά τα δύο επίπεδα μεταξύ εσωτερικής εγκατάστασης και του Azure. Αυτό σημαίνει ότι είναι είτε ένα σύστημα SAP αναπτυχθεί εσωτερικής εγκατάστασης ή το έχει αναπτυχθεί στο Azure. Ωστόσο, μπορείτε να αναπτύξετε τα διάφορα συστήματα ένα τοπίο SAP στο Azure ή εσωτερικής εγκατάστασης. Για παράδειγμα, μπορείτε θα μπορούσε να αναπτύξετε την ανάπτυξη SAP CRM και να ελέγξετε συστήματα στο Azure, αλλά το SAP CRM παραγωγής σύστημα εσωτερικής εγκατάστασης.
* Ανάπτυξη μόνο στο cloud: μια ανάπτυξη όπου το Azure συνδρομής δεν είναι συνδεδεμένο μέσω μιας τοποθεσίας σε τοποθεσία ή ExpressRoute σύνδεσης με την υποδομή δικτύου εσωτερικής εγκατάστασης. Κοινά Azure τεκμηρίωση αυτά τα είδη των αναπτύξεις περιγράφονται επίσης ως "Μόνο για Cloud" αναπτύξεις. Εικονικές μηχανές αναπτυχθεί με αυτήν τη μέθοδο είναι δυνατή η πρόσβαση μέσω του Internet και δημόσια τελικά σημεία Internet που έχουν εκχωρηθεί σε του ΣΠΣ στο Azure. Το εσωτερικής εγκατάστασης υπηρεσίας καταλόγου Active Directory (AD) και DNS δεν έχει επεκταθεί για να Azure σε αυτούς τους τύπους αναπτύξεις. Ως εκ τούτου του ΣΠΣ δεν αποτελούν μέρος του Active Directory εσωτερικής εγκατάστασης. Σημείωση: Μόνο στο Cloud αναπτύξεις σε αυτό το έγγραφο ορίζονται ως ολοκληρωμένη τοπίων SAP που εκτελούνται αποκλειστικά στο Azure χωρίς επέκταση της υπηρεσίας καταλόγου Active Directory ή επίλυση ονομάτων από εσωτερικής εγκατάστασης στο cloud δημόσια. Ρυθμίσεις παραμέτρων μόνο στο cloud δεν υποστηρίζονται για συστήματα SAP παραγωγής ή ρυθμίσεις παραμέτρων όπου ειδικά συγκροτήματα Μετάδοσης SAP ή άλλους πόρους εσωτερικής εγκατάστασης πρέπει να χρησιμοποιηθεί ανάμεσα σε συστήματα SAP φιλοξενούνται σε Azure και τους πόρους που κατοικούν εσωτερικής εγκατάστασης.
* Διασταυρούμενο εσωτερικής εγκατάστασης: Περιγράφει ένα σενάριο όπου ΣΠΣ αναπτύσσονται σε Azure συνδρομή που έχει--τοποθεσίας, πολλές τοποθεσίες ή ExpressRoute συνδεσιμότητας μεταξύ του datacenter(s) εσωτερικής εγκατάστασης και του Azure. Κοινά Azure τεκμηρίωση, τα παρακάτω είδη αναπτύξεις είναι επίσης περιγράφεται ως σενάρια σταυρό εσωτερικής εγκατάστασης. Ο λόγος για τη σύνδεση είναι να επεκτείνετε εσωτερικής εγκατάστασης τομέων, εσωτερικής εγκατάστασης υπηρεσίας καταλόγου Active Directory και εσωτερικής DNS σε Azure. Τοπίου εσωτερικής εγκατάστασης έχει επεκταθεί για να το Azure περιουσιακών στοιχείων της συνδρομής. Με αυτήν την επέκταση, του ΣΠΣ μπορεί να είναι μέρος του τομέα εσωτερικής εγκατάστασης. Οι χρήστες τομέα του τομέα εσωτερικής εγκατάστασης μπορούν να έχουν πρόσβαση οι διακομιστές και να εκτελέσετε τις υπηρεσίες σε αυτές τις VM (όπως υπηρεσίες DBMS). Επίλυση επικοινωνίας και όνομα μεταξύ ΣΠΣ αναπτύσσεται εσωτερικής εγκατάστασης και ΣΠΣ αναπτυχθεί στο Azure είναι δυνατό. Αναμένεται έτσι ώστε να είναι η πιο συνηθισμένο σενάριο για την ανάπτυξη περιουσιακών στοιχείων SAP στο Azure. Ανατρέξτε στο θέμα [αυτό] [ vpn-gateway-cross-premises-options] άρθρο και [αυτό] [ vpn-gateway-site-to-site-create] για περισσότερες πληροφορίες.

> [AZURE.NOTE] Αναπτύξεις εσωτερικής εγκατάστασης διασταυρούμενο συστήματα SAP όπου εικονικές μηχανές Windows Azure εκτελούν συστήματα SAP είναι μέλη ενός τομέα εσωτερικής εγκατάστασης που υποστηρίζονται για συστήματα SAP παραγωγής. Ρυθμίσεις παραμέτρων σταυρό εσωτερικής εγκατάστασης που υποστηρίζονται για την ανάπτυξη τμημάτων ή ολοκλήρωση τοπίων SAP σε Azure. Ακόμα και εκτελεί την πλήρη Οριζόντιος SAP στο Azure απαιτεί αντιμετωπίζετε αυτά τα ΣΠΣ γίνεται μέρος του τομέα εσωτερικής εγκατάστασης και ΔΙΑΦΗΜΊΣΕΙΣ. Σε προηγούμενες εκδόσεις του στην τεκμηρίωση συνομιλία σχετικά με τις υβριδικές-IT σενάρια, όπου βρίσκεται ο όρος "Υβριδική" στο το γεγονός ότι υπάρχει μια σύνδεση μεταξύ εσωτερικής εγκατάστασης μεταξύ εσωτερικής εγκατάστασης και του Azure. Σε αυτήν την περίπτωση 'Υβριδική' σημαίνει επίσης ότι το ΣΠΣ στο Azure αποτελούν μέρος του Active Directory εσωτερικής εγκατάστασης.

Ορισμένα έγγραφα του Microsoft περιγράφει σενάρια σταυρό εσωτερικής εγκατάστασης λίγο διαφορετικά, ειδικά για DBMS HA διαμορφώσεις. Στην περίπτωση του SAP σχετικά έγγραφα, το σταυρό εσωτερικής εγκατάστασης σενάριο απλώς να αρχίσει να βράζει προς τα κάτω, υπάρχει μια συνδεσιμότητα (ExpressRoute)--τοποθεσίας ή ιδιωτικών και να το γεγονός ότι τοπίου SAP κατανέμεται μεταξύ εσωτερικής εγκατάστασης και του Azure.

### <a name="resources"></a>Πόροι
Οι παρακάτω οδηγοί είναι διαθέσιμες για το θέμα της αναπτύξεις SAP σε Azure:

* [NetWeaver SAP στο Azure εικονικές μηχανές (ΣΠΣ) – Προγραμματισμός και τον Οδηγό υλοποίησης] [-Οδηγός σχεδιασμού]
* [NetWeaver SAP στο Azure εικονικές μηχανές (ΣΠΣ)-Οδηγός ανάπτυξης του] [Οδηγός ανάπτυξης]
* [NetWeaver SAP στο Azure εικονικές μηχανές (ΣΠΣ)-Οδηγός ανάπτυξης του DBMS (αυτό το έγγραφο)] [dbms-Οδηγός]

Οι παρακάτω σημειώσεις SAP σχετίζονται με το θέμα της SAP στο Azure:

| Αριθμός σημείωσης   | Τίτλος
|------------|--------
| [1928533] | Των εφαρμογών SAP στο Azure: οι τύποι υποστηρίζονται προϊόντα και Εικονική Azure
| [2015553] | SAP στο Microsoft Azure: υποστηρίζει τις προϋποθέσεις
| [1999351] | Αντιμετώπιση προβλημάτων αυξημένη Azure παρακολούθηση για SAP
| [2178632] | Παρακολούθηση μετρικά για SAP στο Microsoft Azure αριθμού-κλειδιού
| [1409604] | Virtualization στα Windows: βελτιωμένη παρακολούθηση
| [2191498] | SAP σε Linux με Azure: βελτιωμένη παρακολούθηση
| [2039619] | Των εφαρμογών SAP στο Microsoft Azure χρησιμοποιώντας τη βάση δεδομένων της Oracle: υποστηριζόμενες προϊόντα και εκδόσεις
| [2233094] | DB6: Των εφαρμογών SAP στο Azure χρησιμοποιώντας IBM DB2 για Linux, UNIX και Windows - πρόσθετες πληροφορίες
| [2243692] | Linux στο Microsoft Azure (IaaS) Εικονική: θέματα άδειας χρήσης SAP
| [1984787] | SUSE LINUX Enterprise Server 12: Σημειώσεις εγκατάστασης
| [2002167] | Κόκκινο καπέλο Enterprise Linux 7.x: εγκατάσταση και αναβάθμιση

Διαβάστε επίσης τη [ΣΆΡΩΣΗ Wiki](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) που περιέχει όλες τις σημειώσεις SAP για Linux.

Θα πρέπει να έχετε κάποια εμπειρία σχετικά με την αρχιτεκτονική Microsoft Azure και τον τρόπο εικονικές μηχανές Windows Azure Microsoft αναπτύσσεται και λειτουργεί. Μπορείτε να βρείτε περισσότερες πληροφορίες εδώ <https://azure.microsoft.com/documentation/>
 
> [AZURE.NOTE] Θα σας **δεν** συζητάτε πλατφόρμας Microsoft Azure ως ένα προσφορές υπηρεσιών (PaaS) της πλατφόρμας Microsoft Azure. Αυτό το έγγραφο είναι σχετικά με την εκτέλεση ενός συστήματος διαχείρισης βάσεων δεδομένων (DBMS) στο Microsoft εικονικές μηχανές Windows Azure (IaaS), όπως ακριβώς θα έπρεπε να εκτελέσετε το DBMS στο περιβάλλον σας εσωτερικής εγκατάστασης. Βάσης δεδομένων δυνατότητες και λειτουργίες μεταξύ των δύο αυτές τις προσφορές είναι πολύ διαφορετικές και δεν θα πρέπει να είναι μεικτή προς τα επάνω μεταξύ τους. Δείτε επίσης: <https://azure.microsoft.com/services/sql-database/>

Εφόσον μας συζητάτε IaaS, σε γενικές γραμμές τα Windows, Linux και DBMS εγκατάστασης και ρύθμισης παραμέτρων είναι ουσιαστικά ίδια όπως και οποιαδήποτε εικονική ή απολύτως μέταλλο υπολογιστή πρέπει να εγκαταστήσετε εσωτερικής εγκατάστασης. Ωστόσο, υπάρχουν ορισμένες αρχιτεκτονική και το σύστημα διαχείρισης υλοποίηση αποφάσεων που θα είναι διαφορετικές όταν χρησιμοποιείτε IaaS. Είναι το σκοπό αυτού του εγγράφου για να εξηγήσετε η συγκεκριμένη αρχιτεκτονική και σύστημα διαχείρισης διαφορές που πρέπει να είναι έτοιμοι για κατά τη χρήση IaaS.

Σε γενικές γραμμές, η συνολική περιοχές της διαφοράς που ασχολείται με αυτό το έγγραφο είναι:

* Σχεδιασμός τη σωστή διάταξη Εικονική/VHD συστήματα SAP για να βεβαιωθείτε ότι έχετε τα κατάλληλα δεδομένα αρχείων διάταξη και να επιτύχετε αρκετό IOP Προέλευσης για το φόρτο εργασίας.
* Ζητήματα δικτύωσης κατά τη χρήση IaaS.
* Δυνατότητες συγκεκριμένη βάση δεδομένων για να χρησιμοποιήσετε για να βελτιστοποιήσετε τη διάταξη της βάσης δεδομένων.
* Δημιουργία αντιγράφων ασφαλείας και επαναφορά ζητήματα στο IaaS.
* Χρήση διαφορετικών τύπων εικόνες για ανάπτυξη.
* Υψηλή διαθεσιμότητα στο Azure IaaS.

## <a name="65fa79d6-a85f-47ee-890b-22e794f51a64"></a>Δομή μιας ανάπτυξης RDBMS
Με τη σειρά, ακολουθήστε αυτού του κεφαλαίου, είναι απαραίτητο να κατανοήσετε τι υποβλήθηκε στο [αυτό] [ανάπτυξης-Οδηγός-3] κεφάλαιο της [οδηγό ανάπτυξης] [Οδηγός ανάπτυξης]. Γνώσεων σχετικά με διαφορετική σειρά Εικονική και τους διαφορές και διαφορές τυπική Azure και Premium αποθήκευσης θα πρέπει να είναι κατανοητή και γνωστά πριν να διαβάζετε αυτό το κεφάλαιο.

Μέχρι την Μαρτίου 2015, VHD Azure που περιέχουν σε λειτουργικό σύστημα ήταν περιορίζεται σε 127 GB μέγεθος. Αυτός ο περιορισμός έχετε αίρεται στις Μαρτίου 2015 (για περισσότερες πληροφορίες ελέγχου <https://azure.microsoft.com/blog/2015/03/25/azure-vm-os-drive-limit-octupled/> ). Από εκεί στην VHD που περιέχει το λειτουργικό σύστημα μπορούν να έχουν το ίδιο μέγεθος με οποιαδήποτε άλλα VHD. Ωστόσο, θα σας εξακολουθούν να προτιμούν μια δομή ανάπτυξης όπου το λειτουργικό σύστημα, DBMS και ενδεχόμενες δυαδικά δεδομένα SAP είναι ξεχωριστά από τα αρχεία βάσης δεδομένων. Επομένως, αναμένεται συστήματα SAP που εκτελούνται σε εικονικές μηχανές Windows Azure θα έχει την βάσης Εικονική (ή VHD) εγκαθίσταται με το λειτουργικό σύστημα, βάση δεδομένων διαχείρισης συστήματος εκτελέσιμων αρχείων και εκτελέσιμα SAP. Τα αρχεία δεδομένων και καταγραφής DBMS θα είναι αποθηκευμένα στο χώρο αποθήκευσης Azure (τυπική ή χώρο αποθήκευσης Premium) σε ξεχωριστά αρχεία VHD και συνημμένη ως λογικών δίσκων στην αρχική εικόνα Azure λειτουργικό σύστημα Εικονική. 

Εξαρτάται από εκεί αξιοποίηση τυπική Azure ή Premium αποθήκευσης (π.χ., χρησιμοποιώντας τον σειράς DS ή ΣΠΣ GS σειράς) είναι άλλα όρια μεγέθους στο Azure τεκμηριώνονται [εδώ][virtual-machines-sizes]. Κατά το σχεδιασμό σας VHD Azure, θα πρέπει να βρείτε το υπόλοιπο καλύτερα με τα όρια μεγέθους για τα εξής:

* Ο αριθμός των αρχείων δεδομένων.
* Ο αριθμός των VHD που περιέχουν τα αρχεία.
* Τα όρια IOP Προέλευσης από μια μεμονωμένη VHD.
* Μεταγωγή των δεδομένων ανά VHD.
* Ο αριθμός των επιπλέον πιθανών VHD ανά Εικονική μέγεθος.
* Η συνολική μεταγωγή χώρου αποθήκευσης που μπορούν να παρέχουν μια Εικονική.
 
Azure θα επιβάλει μια ορίου IOP Προέλευσης ανά μονάδα δίσκου VHD. Αυτά τα όρια είναι διαφορετικές για VHD που φιλοξενούνται στο χώρο αποθήκευσης τυπική Azure και Premium χώρου αποθήκευσης. Των αδρανειών εισόδου/εξόδου θα είναι επίσης πολύ διαφορετικές μεταξύ των δύο τύπων χώρου αποθήκευσης με την παροχή παράγοντες βελτίωση των αδρανειών εισόδου/εξόδου αποθήκευσης Premium. Κάθε έναν από τους διαφορετικούς τύπους Εικονική έχουν περιορισμένο αριθμό VHD που μπορείτε να επισυνάψετε. Μια άλλη περιορισμός είναι ότι μόνο ορισμένων τύπων Εικονική να αξιοποιήσετε αποθήκευσης Premium Azure. Αυτό σημαίνει ότι η απόφαση για έναν συγκεκριμένο τύπο Εικονική ενδέχεται να δεν μόνο ελέγχεται από τις απαιτήσεις CPU και μνήμης, αλλά επίσης με το IOP Προέλευσης, απαιτήσεις μετάδοσης λανθάνων χρόνος και δίσκου που συνήθως η κλιμάκωση γίνεται με τον αριθμό των VHD ή τον τύπο του χώρου αποθήκευσης Premium δίσκων. Ειδικά με το χώρο αποθήκευσης Premium το μέγεθος του VHD επίσης ενδέχεται να υπαγορεύεται από τον αριθμό των IOP Προέλευσης και μετάδοσης που χρειάζεται πρέπει να επιτυγχάνει κάθε VHD.

Το γεγονός ότι η συνολική ταχύτητα IOP Προέλευσης, τον αριθμό των VHD ενεργοποιηθεί και το μέγεθος του η Εικονική όλες συνδέονται μεταξύ τους, ενδέχεται να προκαλέσει μια Azure ρύθμιση παραμέτρων ενός συστήματος SAP για να είναι διαφορετικά από την ανάπτυξη εσωτερικής εγκατάστασης. Τα όρια IOP Προέλευσης ανά LUN είναι συνήθως με δυνατότητα ρύθμισης παραμέτρων σε αναπτύξεις εσωτερικής εγκατάστασης. Ενώ το με το χώρο αποθήκευσης Azure αυτά τα όρια είναι σταθερό ή όπως στο χώρο αποθήκευσης Premium εξαρτάται από τον τύπο του δίσκου. Επομένως, με αναπτύξεις εσωτερικής βλέπουμε ρυθμίσεις παραμέτρων πελάτη των διακομιστών βάσης δεδομένων που χρησιμοποιούν πολλές διαφορετικές όγκους για ειδική εκτελέσιμων αρχείων όπως SAP και του DBMS ή ειδική όγκους για προσωρινή βάσεις δεδομένων ή κενά διαστήματα πίνακα. Όταν όπως ένα σύστημα εσωτερικής μετακινείται Azure αυτό μπορεί να προκαλέσει σπαταλούν πιθανά εύρους ζώνης IOP Προέλευσης με καταναλώνεται VHD για εκτελέσιμα αρχεία ή βάσεις δεδομένων που δεν εκτελεί οποιαδήποτε ή δεν πολλές IOP Προέλευσης. Επομένως, σε VM Azure συνιστάται ότι το εκτελέσιμα DBMS και τη SAP, να εγκατασταθεί στο δίσκο OS εάν είναι δυνατό.

Τη θέση των αρχείων της βάσης δεδομένων και αρχεία καταγραφής και τον τύπο του χώρου αποθήκευσης Azure που χρησιμοποιείται, θα πρέπει να καθοριστούν από IOP Προέλευσης, λανθάνων χρόνος και τις απαιτήσεις μετάδοσης. Για να έχετε αρκετό IOP Προέλευσης για το αρχείο καταγραφής συναλλαγών, μπορεί να είναι υποχρεωμένοι να αξιοποιήσετε πολλών VHD για το αρχείο καταγραφής συναλλαγών ή χρησιμοποιήστε ένα μεγαλύτερο δίσκο Premium χώρου αποθήκευσης. Σε αυτήν την περίπτωση μία απλώς να δημιουργήσετε ένα λογισμικό RAID (π.χ., Windows αποθήκευσης χώρου συγκέντρωσης για Windows ή MDADM και LVM (Διαχείριση λογικού τόμου) για Linux) με το VHD που θα περιέχει το αρχείο καταγραφής συναλλαγών.

___

> ![Windows][Logo_Windows] Windows
>
> Μονάδα δίσκου D:\ σε μια Εικονική Azure είναι μια μονάδα μη διατηρηθεί η οποία υποστηρίζεται από ορισμένες τοπικών δίσκων στον κόμβο Azure υπολογισμού. Επειδή πρόκειται για μη διατηρηθεί, αυτό σημαίνει ότι τυχόν αλλαγές που κάνατε στο περιεχόμενο στη μονάδα δίσκου D:\ χάνονται όταν γίνει επανεκκίνηση του Εικονική. Από "αλλαγές", θα σας σημαίνει αποθηκευμένα αρχεία, κατάλογοι που δημιουργούνται, εγκατεστημένες εφαρμογές, κ.λπ.
>
> ![Linux][Logo_Linux] Linux
>
> Linux Azure ΣΠΣ τοποθετήσετε αυτόματα μια μονάδα δίσκου στο /mnt/resource που είναι μια μονάδα δίσκου μη διατηρηθεί αντίγραφα ασφαλείας των τοπικών δίσκων στον κόμβο Azure υπολογισμού. Επειδή πρόκειται για μη διατηρηθεί, αυτό σημαίνει ότι τυχόν αλλαγές που κάνατε στο περιεχόμενο /mnt/resource χάνονται όταν γίνει επανεκκίνηση του Εικονική. Με τις αλλαγές, θα σας σημαίνει αρχεία που έχουν αποθηκευτεί, οι κατάλογοι που δημιουργούνται, εγκατεστημένες εφαρμογές, κ.λπ.

___

Εξαρτάται από το Azure Εικονική σειρά, το τοπικών δίσκων στον κόμβο υπολογισμού εμφάνιση διαφορετικό επιδόσεις, τα οποία μπορούν να καταταχθούν όπως:

* A0-A7: Πολύ περιορισμένη απόδοση. Δεν μπορεί να χρησιμοποιηθεί για οτιδήποτε πέρα από το αρχείο σελιδοποίησης των windows
* A8 A11: Χαρακτηριστικά πολύ καλές επιδόσεις με ορισμένες IOP Προέλευσης δέκα χιλιάδων και > μετάδοσης 1GB/sec
* Δ-σειρά: Χαρακτηριστικά πολύ καλές επιδόσεις με ορισμένες IOP Προέλευσης δέκα χιλιάδων και > μετάδοσης 1GB/sec
* Σειράς DS: Χαρακτηριστικά πολύ καλές επιδόσεις με ορισμένες IOP Προέλευσης δέκα χιλιάδων και > μετάδοσης 1GB/sec
* G-σειρά: Χαρακτηριστικά πολύ καλές επιδόσεις με ορισμένες IOP Προέλευσης δέκα χιλιάδων και > μετάδοσης 1GB/sec
* GS-σειρά: Χαρακτηριστικά πολύ καλές επιδόσεις με ορισμένες IOP Προέλευσης δέκα χιλιάδων και > μετάδοσης 1GB/sec

Δηλώσεις παραπάνω συσχετίζετε με τους τύπους Εικονική που έχουν πιστοποιηθεί με SAP. Η σειρά Εικονική με εξαιρετική IOP Προέλευσης και απόδοση πληροίτε τις προϋποθέσεις για αξιοποίηση από ορισμένες δυνατότητες DBMS, όπως tempdb ή το χώρο προσωρινό πίνακα.

### <a name="c7abf1f0-c927-4a7c-9c1d-c7b5b3b7212f"></a>Προσωρινή αποθήκευση για ΣΠΣ και VHD
Όταν δημιουργούμε αυτές τις δίσκων/VHD μέσω της πύλης ή όταν θα σας ενεργοποίησης αποσταλεί VHD σε ΣΠΣ, θα σας να επιλέξετε αν η κίνηση εισόδου/εξόδου μεταξύ του Εικονική και αυτές που βρίσκεται στο χώρο αποθήκευσης Azure VHD αποθηκεύονται στο cache. Azure τυπικές και Premium αποθήκευσης χρησιμοποιούν δύο διαφορετικές τεχνολογίες για αυτόν τον τύπο της μνήμης cache. Και στις δύο περιπτώσεις του cache του εαυτού θα είναι δίσκου αντίγραφα σε το ίδιο μονάδες που χρησιμοποιούνται από τον προσωρινό δίσκο (D:\ στα Windows) ή /mnt/resource σε Linux του η Εικονική.
 
Για το χώρο αποθήκευσης τυπική Azure οι τύποι πιθανές cache είναι:

* Χωρίς αποθήκευση σε cache
* Διαβάστε σε cache
* Ανάγνωση και εγγραφή σε cache

Για να λάβετε συνεπή και ντετερμινιστική επιδόσεων, θα πρέπει να ορίσετε την προσωρινή αποθήκευση στο χώρο αποθήκευσης τυπική Azure για όλους τους VHD που περιέχουν **DBMS που σχετίζονται με τα αρχεία δεδομένων, τα αρχεία καταγραφής και διάστημα πίνακα για να 'NONE'**. Την προσωρινή αποθήκευση των η Εικονική μπορούν να παραμείνουν με την προεπιλεγμένη.

Για το χώρο αποθήκευσης Premium Azure υπάρχουν τις παρακάτω επιλογές σε cache:

* Χωρίς αποθήκευση σε cache
* Διαβάστε σε cache

Σύσταση για το χώρο αποθήκευσης Premium Azure είναι να αξιοποιήσετε **ανάγνωση σε cache για τα αρχεία δεδομένων** της βάσης δεδομένων SAP και επιλέξατε **χωρίς αποθήκευση σε cache για την VHD(s) των αρχείων καταγραφής**.

### <a name="c8e566f9-21b7-4457-9f7f-126036971a91"></a>RAID λογισμικού
Όπως ήδη αναφέρεται παραπάνω, θα πρέπει να εξισορροπήσετε τον αριθμό των IOP Προέλευσης που απαιτείται για τα αρχεία βάσης δεδομένων κατά μήκος του αριθμού των VHD μπορείτε να ρυθμίσετε τις παραμέτρους και πληκτρολογήστε τη μέγιστη IOP Προέλευσης θα παρέχει μια Εικονική Azure ανά δίσκο VHD ή Premium χώρου αποθήκευσης. Ευκολότερος τρόπος για να ασχοληθείτε με τη φόρτωση IOP Προέλευσης μέσω VHD είναι για να δημιουργήσετε ένα λογισμικό RAID πάνω από το διαφορετικό VHD. Στη συνέχεια, τοποθετήστε έναν αριθμό των αρχείων δεδομένων του το DBMS SAP σε το τα LUN σκαλισμένη εκτός του λογισμικού RAID. Εξαρτάται από τις απαιτήσεις που ενδέχεται να θέλετε να λάβετε υπόψη τη χρήση του χώρου αποθήκευσης Premium καθώς και από δύο από τα τρία διαφορετικά δίσκων αποθήκευσης Premium παρέχουν ανώτερο όριο IOP Προέλευσης από VHD που βασίζονται σε τυπική χώρου αποθήκευσης. Εκτός από τη σημαντική καλύτερη εισόδου/εξόδου λανθάνων χρόνος που παρέχεται από χώρο αποθήκευσης Premium Azure. 

Ίδιο ισχύει για το αρχείο καταγραφής συναλλαγών του τα διάφορα συστήματα DBMS. Με πολλές από αυτές απλώς Προσθήκη περισσότερων αρχείων Tlog δεν βοηθά επειδή τα συστήματα DBMS γράψετε σε ένα από τα αρχεία κάθε φορά μόνο. Εάν χρειάζονται υψηλότερες χρεώσεις IOP Προέλευσης από μια τυπική ενός χώρου αποθήκευσης που βασίζεται VHD μπορούν να παρέχουν, μπορείτε να διαγραμμίσεων μέσω πολλών τυπική VHD χώρου αποθήκευσης ή μπορείτε να χρησιμοποιήσετε έναν μεγαλύτερο Premium δίσκου τύπο αποθήκευσης πέρα από τις υψηλότερες χρεώσεις IOP Προέλευσης επίσης παρέχει παράγοντες κάτω λανθάνων χρόνος για την εγγραφή εισόδων/εξόδων στο αρχείο καταγραφής συναλλαγών.
 
Καταστάσεις έμπειρος στο Azure αναπτύξεις, οι οποίες θα προτιμήσει χρησιμοποιώντας ένα λογισμικό RAID είναι:

* Αρχείο καταγραφής συναλλαγών του αρχείου καταγραφής/Ακύρωση αναίρεσης απαιτούν περισσότερες IOP Προέλευσης από Azure παρέχει για μια μεμονωμένη VHD. Όπως προαναφέρθηκε αυτό είναι δυνατό να επιλυθούν δημιουργώντας ένα LUN μέσω πολλών VHD χρησιμοποιώντας ένα λογισμικό RAID.
* Άνισος εισόδου/εξόδου φόρτο εργασίας κατανομή επάνω από τα αρχεία διαφορετικά δεδομένα της βάσης δεδομένων SAP. Σε αυτές τις περιπτώσεις μία μπορεί να αντιμετωπίσετε ένα αρχείο δεδομένων του πάτημα του ορίου προτιμάτε συχνά. Ότι άλλα αρχεία δεδομένων δεν ακόμα γρήγορα κοντά του ορίου IOP Προέλευσης από μια μεμονωμένη VHD. Σε αυτές τις περιπτώσεις, η ευκολότερος λύση είναι να δημιουργήσετε μία LUN μέσω πολλών VHD χρησιμοποιώντας ένα λογισμικό RAID. 
* Δεν γνωρίζετε τι το ακριβές φόρτο εργασίας εισόδου/εξόδου ανά αρχείο δεδομένων και μόνο περίπου γνωρίζετε τι είναι το συνολικό φόρτο εργασίας IOP Προέλευσης σε σχέση με το DBMS. Πιο εύκολο να κάνετε είναι να δημιουργήσετε μία LUN με τη Βοήθεια του ένα λογισμικό RAID. Το άθροισμα των ορίων του πολλών VHD πίσω από αυτόν τον LUN, στη συνέχεια, θα πρέπει να ικανοποιεί το γνωστό επιτόκιο IOP Προέλευσης.

___

> ![Windows][Logo_Windows] Windows
>
> Χρήση του Windows Server 2012 ή νεότερη έκδοση κενά διαστήματα χώρου αποθήκευσης είναι προτιμότερη καθώς πρόκειται για πιο αποτελεσματική από Windows επιμερισμό από προηγούμενες εκδόσεις των Windows. Λάβετε υπόψη ότι ενδέχεται να πρέπει να δημιουργήσετε τα σύνολα χώρου αποθήκευσης των Windows και χώροι αποθήκευσης από τις εντολές του PowerShell όταν χρησιμοποιείτε το Windows Server 2012 ως λειτουργικό σύστημα. Οι εντολές του PowerShell θα βρείτε εδώ <https://technet.microsoft.com/library/jj851254.aspx>

> 
> ![Linux][Logo_Linux] Linux
>
> Για να δημιουργήσετε ένα λογισμικό RAID σε Linux υποστηρίζονται μόνο MDADM και LVM (Διαχείριση λογικού τόμου). Για περισσότερες πληροφορίες, διαβάστε στα ακόλουθα άρθρα:
>
> * [Ρύθμιση παραμέτρων του λογισμικού RAID σε Linux] [ virtual-machines-linux-configure-raid] (για MDADM)
> * [Ρύθμιση παραμέτρων LVM σε μια Εικονική Linux στα Azure][virtual-machines-linux-configure-lvm]


___

Ζητήματα για αξιοποίηση Εικονική σειράς που μπορούν να λειτουργούν με το χώρο αποθήκευσης Premium Azure συνήθως είναι οι εξής:

* Απαιτήσεις για των αδρανειών εισόδου/εξόδου που είναι κοντά τι κάνουν ΣΑΝ/NAS συσκευές.
* Αίτημα για παράγοντες καλύτερη εισόδου/εξόδου λανθάνων χρόνος από χώρο αποθήκευσης τυπική Azure μπορούν να κάνουν.
* Υψηλότερη IOP Προέλευσης ανά Εικονική από το τι θα μπορούσε να είναι επίτευξη με πολλές τυπική VHD χώρου αποθήκευσης σε σχέση με έναν συγκεκριμένο τύπο Εικονική.

Επειδή το υποκείμενο αποθήκευσης Azure αναπαράγει κάθε VHD σε τουλάχιστον τρεις κόμβους χώρου αποθήκευσης, απλή RAID 0 επιμερισμό μπορεί να χρησιμοποιηθεί. Δεν χρειάζεται για την υλοποίηση RAID5 ή RAID1.

### <a name="10b041ef-c177-498a-93ed-44b3441ab152"></a>Χώρος αποθήκευσης του Windows Azure
Χώρος αποθήκευσης Microsoft Azure θα αποθηκεύσει βάσης Εικονική (με OS) και VHD ή αντικείμενα BLOB σε τουλάχιστον 3 κόμβους ξεχωριστή χώρου αποθήκευσης. Όταν δημιουργείτε ένα λογαριασμό του χώρου αποθήκευσης, υπάρχει μια επιλογή προστασίας, όπως φαίνεται εδώ:

![Παν αναπαραγωγής που είναι ενεργοποιημένες για το λογαριασμό χώρου αποθήκευσης Azure][dbms-guide-figure-100]

Azure (τοπικά πλεονάζοντα) τοπική αναπαραγωγή χώρου αποθήκευσης παρέχει επίπεδα προστασίας από απώλεια δεδομένων λόγω αποτυχία υποδομή που λίγους πελάτες μπορεί να πρέπει να αναπτύξετε. Όπως φαίνεται παραπάνω υπάρχουν 4 διαφορετικές επιλογές με μια Πέμπτη είναι μια παραλλαγή ενός από τα τρία πρώτα. Εμφάνιση πιο κοντά στο τους θα σας να διακρίνετε:

* **Premium τοπικά πλεονάζοντα χώρο αποθήκευσης (LRS)**: αποθήκευσης Premium Azure προσφέρει υποστήριξη υψηλών επιδόσεων, χαμηλής αδράνειας δισκέτα για εικονικές μηχανές εκτελούνται φόρτους εργασίας να/O-εντατική. Υπάρχουν 3 αντίγραφα των δεδομένων μέσα σε το ίδιο Azure κέντρο δεδομένων από μια περιοχή Azure. Τα αντίγραφα θα είναι σε διαφορετική σφαλμάτων και αναβάθμιση τομέων (για έννοιες ανατρέξτε στο θέμα [αυτό] [σχεδιασμού-Οδηγός-3,2] κεφάλαιο στο [σχεδιασμού Guide][planning-guide]). Σε περίπτωση ένα αντίγραφο των δεδομένων θα εκτός υπηρεσίας εξαιτίας μιας αποτυχίας κόμβου χώρου αποθήκευσης ή την αποτυχία δίσκου, δημιουργείται αυτόματα μια νέα ρεπλίκα.
* **Τοπικά πλεονάζοντα χώρο αποθήκευσης (LRS)**: σε αυτήν την περίπτωση, υπάρχουν 3 αντίγραφα των δεδομένων μέσα σε το ίδιο Azure κέντρο δεδομένων από μια περιοχή Azure. Τα αντίγραφα θα είναι σε διαφορετική σφαλμάτων και αναβάθμιση τομέων (για έννοιες ανατρέξτε στο θέμα [αυτό] [σχεδιασμού-Οδηγός-3,2] κεφάλαιο στο [σχεδιασμού Guide][planning-guide]). Σε περίπτωση ένα αντίγραφο των δεδομένων θα εκτός υπηρεσίας εξαιτίας μιας αποτυχίας κόμβου χώρου αποθήκευσης ή την αποτυχία δίσκου, δημιουργείται αυτόματα μια νέα ρεπλίκα. 
* **Παν πλεονάζοντα χώρο αποθήκευσης Εξοπλισμό**: σε αυτήν την περίπτωση, υπάρχει μια ασύγχρονης αναπαραγωγής που θα τροφοδοσίας μια επιπλέον 3 αντίγραφα των δεδομένων σε μια άλλη περιοχή Azure που είναι στις περισσότερες περιπτώσεις στην ίδια γεωγραφική περιοχή (όπως Βόρειας Ευρώπη και Δυτική Ευρώπη). Αυτό θα έχει ως αποτέλεσμα 3 επιπλέον αντίγραφα, ώστε να υπάρχουν 6 αντίγραφα στο άθροισμα. Μια παραλλαγή αυτή είναι μια προσθήκη όπου τα δεδομένα στην περιοχή από αναπαραγωγή Azure παν μπορεί να χρησιμοποιηθεί για ανάγνωση σκοπούς (πρόσβαση για ανάγνωση παν-πλεονάζοντα).
* **Ζώνη πλεονάζοντα χώρο αποθήκευσης (ZRS)**: σε αυτήν την περίπτωση το 3 αντίγραφα του τα δεδομένα παραμένουν στην ίδια περιοχή Azure. Όπως περιγράφεται σε [αυτό] [σχεδιασμού-Οδηγός-3.1] κεφάλαιο του [Οδηγού σχεδιασμού] [-Οδηγός σχεδιασμού] μια περιοχή Azure μπορεί να είναι ένας αριθμός κέντρα δεδομένων κοντά. Στην περίπτωση LRS τα αντίγραφα κατανέμεται πάνω από το διαφορετικά κέντρα δεδομένων που κάνουν μία περιοχή Azure.

Μπορείτε να βρείτε περισσότερες πληροφορίες [εδώ][storage-redundancy].
 
> [AZURE.NOTE] Για αναπτύξεις DBMS, δεν συνιστάται η χρήση της παν πλεονάζοντα χώρου αποθήκευσης
>
> Azure παν αναπαραγωγής χώρου αποθήκευσης είναι ασύγχρονη. Αναπαραγωγή των μεμονωμένων VHD τοποθετηθεί σε ένα μεμονωμένο Εικονική δεν συγχρονίζονται στο βήμα κλειδώματος. Επομένως, δεν είναι κατάλληλη για την αναπαραγωγή αρχείων DBMS που είναι κατανεμημένες σε ένα διαφορετικό VHD ή αναπτυχθεί σε σχέση με ένα λογισμικό RAID βάσει πολλών VHD. Λογισμικό DBMS απαιτεί ότι το χώρο αποθήκευσης στο δίσκο μόνιμη επακριβώς έχει συγχρονιστεί σε διαφορετική LUN και υποκείμενα δίσκων/VHD/κεφαλών. Λογισμικό DBMS χρησιμοποιεί διάφορες μηχανισμούς για την ακολουθία εισόδου/ΕΞΌΔΟΥ εγγραφής δραστηριότητες και ένα DBMS θα αναφέρει ότι το χώρο αποθήκευσης στο δίσκο στοχευμένες κατά την αναπαραγωγή έχει καταστραφεί εάν αυτά διαφέρουν ακόμη και από μερικά χιλιοστά του δευτερολέπτου. Ως εκ τούτου εάν μία θέλει πραγματικά μια ρύθμιση παραμέτρων βάσης δεδομένων με μια βάση δεδομένων επεκταθεί κατά μήκος πολλών VHD αναπαραχθούν παν, όπως μια αναπαραγωγή πρέπει να εκτελεστούν με βάση δεδομένων μέσες και λειτουργικότητα. Ένα δεν πρέπει να βασίζεστε στη Azure αποθήκευσης παν-αναπαραγωγής για να εκτελέσετε αυτήν την εργασία. 
>
> Το πρόβλημα είναι απλούστερο να εξηγήσετε με ένα σύστημα παράδειγμα. Ας υποθέσουμε ότι έχετε ένα σύστημα SAP που έχουν αποσταλεί στο Azure που έχει 8 VHD που περιέχει τα αρχεία δεδομένων από το DBMS συν μία VHD που περιέχει το αρχείο καταγραφής συναλλαγών. Κάθε μία από αυτές τις 9 VHD θα έχει δεδομένα που βρίσκονται σε μια συνεπή μέθοδο σύμφωνα με το DBMS, αν τα δεδομένα είναι που εγγράφονται στα αρχεία καταγραφής δεδομένων ή συναλλαγή.
>
> Σε προκειμένου να αντιγραφή παν σωστά τα δεδομένα και να διατηρήσετε μια εικόνα συνεπή βάσης δεδομένων, το περιεχόμενο του όλα εννέα VHD θα πρέπει να έχει παν αναπαραχθούν με ακρίβεια τη σειρά τις λειτουργίες εισόδου/εξόδου έχουν εκτελεστεί σε το εννέα διαφορετικές VHD. Ωστόσο, αποθήκευσης Azure παν-αναπαραγωγής δεν σας επιτρέπει να δηλώσετε εξαρτήσεων μεταξύ VHD. Αυτό σημαίνει ότι χώρος αποθήκευσης Microsoft Azure παν-αναπαραγωγής δεν γνωρίζει το γεγονός ότι το περιεχόμενο σε αυτές τις εννέα διαφορετικές VHD σχετίζονται μεταξύ τους και ότι οι αλλαγές στα δεδομένα είναι συνεπή μόνο όταν αναπαραγωγή στη σειρά τις λειτουργίες εισόδου/εξόδου συνέβη σε όλα τα 9 VHD.
>
> Εκτός από τις πιθανότητες να γίνεται υψηλή ότι οι εικόνες αναπαραχθούν παν στο σενάριο δεν παρέχουν μια εικόνα συνεπή βάσης δεδομένων, υπάρχει επίσης ένα μειονέκτημα στις επιδόσεις που δείχνει προς τα επάνω με παν πλεονάζοντα αποθήκευσης που μπορεί να σοβαρά επηρεάσουν την απόδοση. Στη σύνοψη μην χρησιμοποιείτε αυτόν τον τύπο του πλεονασμού χώρου αποθήκευσης για φόρτους εργασίας τύπος DBMS.
 
#### <a name="mapping-vhds-into-azure-virtual-machine-service-storage-accounts"></a>Αντιστοίχιση VHD στο Azure εικονική μηχανή υπηρεσία αποθήκευσης λογαριασμών
Ένας λογαριασμός χώρου αποθήκευσης Azure είναι όχι μόνο μια δομή διαχείρισης, αλλά επίσης ένα θέμα περιορισμοί. Ότι οι περιορισμοί ποικίλλουν ανάλογα εάν, αναφερθούμε ένα λογαριασμό Azure τυπική χώρου αποθήκευσης ή ένα άρτιο Azure χώρου αποθήκευσης. Τα ακριβή δυνατότητες και τους περιορισμούς, παρατίθενται [εδώ][storage-scalability-targets]
 
Έτσι για το χώρο αποθήκευσης τυπική Azure είναι σημαντικό να λάβετε υπόψη υπάρχει όριο στον το IOP Προέλευσης ανά λογαριασμού χώρου αποθήκευσης (γραμμή που περιέχει 'Συνολική αίτηση συχνότητα' στο [άρθρο][storage-scalability-targets]). Επιπλέον, υπάρχει μια αρχική όριο 100 λογαριασμών χώρου αποθήκευσης ανά Azure συνδρομή (από τον ΙΟΥΛΙΟΥ 2015). Επομένως, συνιστάται να εξισορροπήσετε IOP Προέλευσης του ΣΠΣ ανάμεσα σε πολλούς λογαριασμούς χώρου αποθήκευσης κατά τη χρήση του χώρου αποθήκευσης τυπική Azure. Ότι ένα μεμονωμένο Εικονική χρησιμοποιεί ιδανικά ένα λογαριασμό του χώρου αποθήκευσης εάν είναι δυνατό. Επομένως, εάν, αναφερθούμε DBMS αναπτύξεις όπου κάθε VHD που φιλοξενείται στον τυπικό αποθήκευσης Azure μπορεί να φτάσουν το όριο, θα πρέπει να αναπτύξετε μόνο 30-40 VHD ανά λογαριασμού χώρου αποθήκευσης Azure που χρησιμοποιεί το χώρο αποθήκευσης τυπική Azure. Από την άλλη πλευρά, εάν αξιοποιήσετε αποθήκευσης Premium Azure και θέλετε να αποθηκεύσετε όγκους μεγάλη βάση δεδομένων, ενδέχεται να είναι λεπτομερές όσον αφορά IOP Προέλευσης. Αλλά ένας λογαριασμός Azure Premium χώρου αποθήκευσης είναι τρόπο πιο περιοριστικό όγκου δεδομένων από ένα λογαριασμό Azure τυπική χώρου αποθήκευσης. Ως αποτέλεσμα, μπορείτε να αναπτύξετε μόνο περιορισμένο αριθμό VHD μέσα σε ένα λογαριασμό χώρου αποθήκευσης Premium Azure πριν να επιτύχετε το όριο όγκου δεδομένων. Κατά τη λήξη ομάδες ενός λογαριασμού χώρου αποθήκευσης Azure ως ένα "εικονικό ΣΑΝ" που έχει περιορισμένη δυνατοτήτων στο IOP Προέλευσης ή/και τη δυνατότητα. Ως αποτέλεσμα, η εργασία παραμένει, όπως αναπτύξεις εσωτερικής εγκατάστασης, για να ορίσετε τη διάταξη του VHD με τα διάφορα συστήματα SAP πάνω από το διαφορετικές 'φανταστικό ΣΑΝ συσκευές' ή λογαριασμοί Azure χώρου αποθήκευσης.
 
Για το χώρο αποθήκευσης τυπική Azure δεν συνιστάται να παρουσιάσετε χώρου αποθήκευσης από λογαριασμούς διαφορετικό χώρο αποθήκευσης για μια Εικονική μόνο εάν είναι δυνατό.

Ότι χρησιμοποιώντας την DS ή GS-σειρά ΣΠΣ Azure είναι δυνατό να VHD ενεργοποίησης από το Azure τυπική χώρου αποθήκευσης και Premium αποθήκευσης λογαριασμών. Περιπτώσεις χρήσης μοιάζει με τη σύνταξη αντίγραφα ασφαλείας σε τυπική αποθήκευσης αντίγραφα VHD ότι χρειάζεται DBMS δεδομένων και αρχεία καταγραφής στο χώρο αποθήκευσης Premium έρθει γνώμη όπου μπορεί να χρησιμοποιηθούν όπως ετερογενή χώρου αποθήκευσης. 

Βάση αναπτύξεις πελατών και έλεγχος περίπου 30 έως 40 VHD που περιέχει τα αρχεία δεδομένων βάσης δεδομένων και αρχεία καταγραφής μπορεί να αποδοθεί σε ένα Azure τυπική αποθήκευσης λογαριασμό με αποδεκτές επιδόσεις. Όπως προαναφέρθηκε, τον περιορισμό των ένα άρτιο Azure χώρου αποθήκευσης είναι πιθανό να την ικανότητα δεδομένων μπορεί να χωρέσει και δεν IOP Προέλευσης.

Ως με ΣΑΝ συσκευές εσωτερικής εγκατάστασης, κοινής χρήσης απαιτεί ορισμένες παρακολούθησης για να εμφανίσει εντοπίσετε συνωστισμούς σε ένα λογαριασμό του χώρου αποθήκευσης Azure. Η επέκταση Azure παρακολούθησης για SAP και της πύλης Azure είναι εργαλεία που μπορούν να χρησιμοποιηθούν για τον εντοπισμό απασχολημένοι Azure λογαριασμών χώρου αποθήκευσης που μπορεί να την εκτέλεση δευτερεύουσα προαιρετική απόδοση εισόδου/ΕΞΌΔΟΥ.  Εάν αυτή η κατάσταση εντοπίζεται συνιστάται να μετακινήσετε απασχολημένος ΣΠΣ σε άλλο λογαριασμό χώρου αποθήκευσης Azure. Ανατρέξτε [οδηγό ανάπτυξης] [Οδηγός ανάπτυξης] για λεπτομέρειες σχετικά με τον τρόπο για να ενεργοποιήσετε τον κεντρικό υπολογιστή του SAP παρακολούθηση δυνατότητες.

Ένα άλλο άρθρο που συνοψίζει βέλτιστες πρακτικές γύρω από χώρο αποθήκευσης τυπική Azure και Azure τυπικούς λογαριασμούς χώρου αποθήκευσης θα βρείτε εδώ <https://blogs.msdn.com/b/mast/archive/2014/10/14/configuring-azure-virtual-machines-for-optimal-storage-performance.aspx>
 
#### <a name="moving-deployed-dbms-vms-from-azure-standard-storage-to-azure-premium-storage"></a>Μετακίνηση αναπτυχθεί ΣΠΣ DBMS από το χώρο αποθήκευσης τυπική Azure με το χώρο αποθήκευσης Premium Azure
Θα σας συναντήσετε απολύτως ορισμένα σενάρια όπου ως πελάτη θέλετε να μετακινήσετε μια Εικονική ανεπτυγμένος από το χώρο αποθήκευσης τυπική Azure στο χώρο αποθήκευσης Premium Azure. Αυτό δεν είναι δυνατό χωρίς φυσική μετακίνηση των δεδομένων. Υπάρχουν πολλοί τρόποι για να επιτύχετε το στόχο:

* Απλώς θα μπορούσε να αντιγράψετε όλα VHD, βάσης VHD, καθώς και δεδομένα VHD σε νέο λογαριασμό του χώρου αποθήκευσης Premium Azure. Πολύ συχνά που επιλέξατε τον αριθμό των VHD στο χώρο αποθήκευσης τυπική Azure δεν λόγω το γεγονός ότι απαιτείται η ένταση δεδομένων. Αλλά είναι απαραίτητο ότι πολλοί VHD λόγω του IOP Προέλευσης. Τώρα που μετακινείτε στο χώρο αποθήκευσης Premium Azure που θα μπορούσε να πάει με τρόπο λιγότερο VHD για να επιτύχετε το ορισμένες μετάδοσης IOP Προέλευσης. Δεδομένου ότι στο χώρο αποθήκευσης τυπική Azure πληρώνετε για τα δεδομένα που χρησιμοποιήθηκαν και όχι το μέγεθος nominal δίσκου, τον αριθμό των VHD πραγματικά έχει σημασία όσον αφορά το κόστος. Ωστόσο, με το χώρο αποθήκευσης Premium Azure, να πληρώνετε για το μέγεθος του ονομαστικού δίσκου. Επομένως, περισσότερες από τον πελάτη προσπαθήστε να διατηρήσετε τον αριθμό των VHD Azure στο χώρο αποθήκευσης Premium στον αριθμό που απαιτείται για την επίτευξη τη μετάδοση IOP Προέλευσης είναι απαραίτητο. Επομένως, οι περισσότεροι πελάτες αποφασίσει σύμφωνα με τον τρόπο από ένα απλό 1:1 αντίγραφο.
* Εάν δεν έχουν ακόμα τοποθετηθεί, έχετε τοποθετήσετε έναν μεμονωμένο VHD που μπορεί να περιέχει ένα αντίγραφο ασφαλείας της βάσης δεδομένων της βάσης δεδομένων σας SAP. Μετά τη δημιουργία αντιγράφων ασφαλείας, μπορείτε να αποσύνδεση όλα VHD, συμπεριλαμβανομένου του VHD που περιέχει το αντίγραφο ασφαλείας και αντιγραφή βάσης VHD και VHD με το αντίγραφο ασφαλείας σε ένα λογαριασμό του χώρο αποθήκευσης Premium Azure. Θέλετε, στη συνέχεια, αναπτύξτε την εικονική Μηχανή με βάση τη βασική VHD και ενεργοποίησης του VHD με το αντίγραφο ασφαλείας. Τώρα μπορείτε να δημιουργήσετε πρόσθετες κενή δίσκων Premium χώρου αποθήκευσης για την εικονική Μηχανή που χρησιμοποιούνται για να επαναφέρετε τη βάση δεδομένων σε. Αυτό προϋποθέτει ότι το DBMS σάς επιτρέπει να αλλάξετε διαδρομές για τα αρχεία δεδομένων και καταγραφής ως μέρος της διαδικασίας επαναφοράς.
* Μια άλλη πιθανότητα είναι μια παραλλαγή πρώην και τη διαδικασία, όπου μπορείτε απλώς αντιγράψτε το αντίγραφο ασφαλείας VHD αποθήκευσης Premium Azure επισύναψή του σε σχέση με μια Εικονική που αναπτύσσεται και εγκαταστήσει πρόσφατα.
* Η τέταρτη δυνατότητα που μπορείτε να επιλέξετε όταν είστε σε ανάγκη για να αλλάξετε τον αριθμό των αρχείων δεδομένων της βάσης δεδομένων σας. Σε αυτήν την περίπτωση θα πρέπει να κάνετε ένα αντίγραφο ομοιογενές σύστημα SAP με εξαγωγή/εισαγωγή. Τοποθέτηση αυτές εξαγωγή αρχείων σε έναν VHD που αντιγράφεται στο λογαριασμό Azure Premium χώρου αποθήκευσης και επισύναψή του σε μια Εικονική που χρησιμοποιείτε για να εκτελέσετε τις διαδικασίες εισαγωγής. Οι πελάτες χρησιμοποιούν αυτή η δυνατότητα κυρίως όταν που θέλουν να μειώσετε τον αριθμό των αρχείων δεδομένων.

### <a name="deployment-of-vms-for-sap-in-azure"></a>Ανάπτυξη του ΣΠΣ για SAP στο Azure
Microsoft Azure προσφέρει πολλούς τρόπους για την ανάπτυξη του ΣΠΣ και σχετικές δίσκων. Με τον τρόπο αυτό είναι σημαντικό να κατανοήσετε τις διαφορές εφόσον προετοιμασίες μέρος του ΣΠΣ ενδέχεται να διαφέρουν ανάλογα με τον τρόπο ανάπτυξης. Σε γενικές γραμμές, θα σας ερευνήσει τα σενάρια που περιγράφονται σε τα ακόλουθα κεφάλαια.

#### <a name="deploying-a-vm-from-the-azure-marketplace"></a>Για την ανάπτυξη μια Εικονική από το Azure Marketplace
Θέλετε να κρατήσετε ένα της Microsoft ή άλλων κατασκευαστών που παρέχονται εικόνα από το Azure Marketplace για να αναπτύξετε το Εικονική. Μετά την ανάπτυξη του Εικονική στα Azure, μπορείτε να ακολουθήσετε τα ίδια γενικές οδηγίες και εργαλεία για να εγκαταστήσετε το λογισμικό SAP μέσα Εικονική σας όπως θα κάνατε σε ένα περιβάλλον εσωτερικής εγκατάστασης. Για την εγκατάσταση του λογισμικού SAP μέσα η Εικονική Azure, SAP και του Microsoft να για να αποστείλετε και να αποθηκεύσετε το μέσο εγκατάστασης SAP στο VHD Azure ή για να δημιουργήσετε μια Εικονική Azure λειτουργεί ως 'διακομιστής αρχείων' που περιέχει όλα τα απαραίτητα μέσα εγκατάστασης SAP.

#### <a name="deploying-a-vm-with-a-customer-specific-generalized-image"></a>Για την ανάπτυξη μια Εικονική με μια συγκεκριμένη εικόνα γενικευμένη πελατών
Λόγω κώδικα του συγκεκριμένες απαιτήσεις σε σχέση με την έκδοση του λειτουργικού Συστήματος ή DBMS, οι εικόνες που παρέχεται από το Azure Marketplace δεν μπορεί να ταιριάζουν στις ανάγκες σας. Επομένως, ίσως χρειαστεί να δημιουργήσετε μια Εικονική χρησιμοποιώντας τη δική σας 'private' εικόνα Εικονική OS/DBMS που μπορούν να αναπτυχθούν πολλές φορές αργότερα. Για να προετοιμάσετε όπως μια εικόνα 'private' για αναπαραγωγή, το λειτουργικό σύστημα πρέπει να είναι γενίκευση σε η Εικονική εσωτερικής εγκατάστασης. Ανατρέξτε [οδηγό ανάπτυξης] [Οδηγός ανάπτυξης] για λεπτομέρειες σχετικά με τον τρόπο γενίκευσης μια Εικονική.

Εάν έχετε ήδη εγκαταστήσει SAP περιεχόμενο σε σας Εικονική εσωτερικής εγκατάστασης (ειδικά για συστήματα επιπέδου 2), μπορείτε να προσαρμόσετε τις ρυθμίσεις του συστήματος SAP μετά την ανάπτυξη του η Εικονική Azure μέσω της παρουσίας μετονομάσετε τη διαδικασία που υποστηρίζονται από το SAP λογισμικό προμήθεια του διαχειριστή (Σημείωση SAP [1619720]). Διαφορετικά, μπορείτε να εγκαταστήσετε το λογισμικό SAP αργότερα μετά την ανάπτυξη του η Εικονική Azure.
 
Από τη βάση δεδομένων περιεχομένου χρησιμοποιούνται από την εφαρμογή SAP, μπορείτε να είτε για να δημιουργήσετε το περιεχόμενο πρόσφατα από μια εγκατάσταση SAP ή μπορείτε να εισαγάγετε το περιεχόμενό σας στο Azure χρησιμοποιώντας έναν VHD με ένα αντίγραφο ασφαλείας της βάσης δεδομένων DBMS ή αξιοποίηση δυνατότητες του το DBMS για άμεση δημιουργία αντιγράφων ασφαλείας στο Microsoft Azure αποθήκευσης. Σε αυτήν την περίπτωση, που θα μπορούσε να επίσης Προετοιμασία VHD με το DBMS δεδομένων και καταγραφής αρχείων εσωτερικής εγκατάστασης και, στη συνέχεια, εισαγάγετε αυτές ως δίσκων στο Azure. Αλλά θα λειτουργεί τη μεταφορά δεδομένων DBMS που φορτώνεται από εσωτερικής εγκατάστασης για να Azure μέσω δίσκων VHD που πρέπει να είναι έτοιμοι εσωτερικής εγκατάστασης.

#### <a name="moving-a-vm-from-on-premises-to-azure-with-a-non-generalized-disk"></a>Μετακίνηση μια Εικονική από εσωτερικής εγκατάστασης στο Azure με ένα δίσκο μη γενίκευση
Σχεδιασμός για να μετακινήσετε ένα συγκεκριμένο σύστημα SAP από εσωτερικής Azure (ανύψωσης και shift). Αυτό μπορεί να γίνει με την αποστολή του VHD που περιέχει το λειτουργικό σύστημα, τα δυαδικά αρχεία SAP και ενδεχόμενες δυαδικά δεδομένα DBMS συν το VHD με τα αρχεία δεδομένων και καταγραφής από το DBMS να Azure. Στο αντίθετη σενάριο #2 παραπάνω, διατηρείτε το όνομα κεντρικού υπολογιστή, SAP αναγνωριστικό ΑΣΦΑΛΕΊΑΣ και λογαριασμούς χρήστη SAP σε η Εικονική Azure όπως ήταν έχει ρυθμιστεί στο περιβάλλον εσωτερικής εγκατάστασης. Επομένως, γενίκευση η εικόνα δεν είναι απαραίτητο. Αυτήν την περίπτωση θα εφαρμοστούν κυρίως για σενάρια σταυρό εσωτερικής εγκατάστασης όπου ένα τμήμα του τοπίου SAP εκτελείται εσωτερικής εγκατάστασης και τμήματα στο Azure.

## <a name="871dfc27-e509-4222-9370-ab1de77021c3"></a>Υψηλή διαθεσιμότητα και αποκατάσταση ΣΠΣ Azure
Το παρακάτω λειτουργίες υψηλής διαθεσιμότητας (HA) και αποκατάστασης από καταστροφή (DR) που ισχύουν για διάφορα στοιχεία θα χρησιμοποιήσουμε για αναπτύξεις SAP και DBMS προσφέρει Azure

### <a name="vms-deployed-on-azure-nodes"></a>ΣΠΣ αναπτυχθεί σε κόμβους Azure
Η πλατφόρμα Azure δεν προσφέρει δυνατότητες όπως Live μετεγκατάστασης για ανάπτυξη ΣΠΣ. Αυτό σημαίνει ότι εάν συντήρησης είναι απαραίτητες σε ένα σύμπλεγμα διακομιστών στην οποία έχει αναπτυχθεί μια Εικονική, η Εικονική πρέπει να λάβετε διακοπή και επανεκκίνηση. Συντήρηση στο Azure πραγματοποιείται χρησιμοποιώντας επομένως, που ονομάζεται αναβάθμιση τομέων μέσα συμπλεγμάτων των διακομιστών. Μόνο ένα τομέα αναβάθμιση κάθε φορά που διατηρούνται. Κατά τη διάρκεια αυτών επανεκκίνηση θα υπάρξει ένα διακοπή της υπηρεσίας ενώ τερματίζεται η Εικονική, εκτελείται συντήρηση και επανεκκίνηση του Εικονική. Οι περισσότερες DBMS προμηθευτές παρέχουν ωστόσο υψηλή διαθεσιμότητα και αποκατάσταση λειτουργικότητα που θα γρήγορα επανεκκίνηση των υπηρεσιών DBMS σε έναν άλλο κόμβο εάν ο κύριος κόμβος δεν είναι διαθέσιμη. Η πλατφόρμα Azure παρέχει λειτουργικότητα για τη διανομή ΣΠΣ, αποθήκευσης και άλλες υπηρεσίες του Azure τομέων αναβάθμιση για να βεβαιωθείτε ότι αποτυχίες προγραμματισμένης συντήρησης ή υποδομή θα επηρεάσει μόνο ένα μικρό υποσύνολο των ΣΠΣ ή των υπηρεσιών.  Με το σχεδιασμό Προσέξτε είναι πιθανό να επιτύχετε διαθεσιμότητα επίπεδα συγκρίσιμη με υποδομών εσωτερικής εγκατάστασης.

Microsoft Azure διαθεσιμότητα συνόλων είναι μια λογική ομαδοποίηση ΣΠΣ ή τις υπηρεσίες που εξασφαλίζει ότι ΣΠΣ και άλλες υπηρεσίες διανέμονται σε διαφορετική σφαλμάτων και αναβάθμιση τομείς μέσα σε ένα σύμπλεγμα τέτοια ότι θα υπάρξει μόνο μία τερματισμού κόμβο σε οποιοδήποτε σημείο μία φορά (Διαβάστε [αυτό] [ virtual-machines-manage-availability] το άρθρο για περισσότερες λεπτομέρειες).

Χρειάζεται να ρυθμιστούν με σκοπό κατά την παράδοση ΣΠΣ όπως φαίνεται εδώ:

![Ορισμός του συνόλου διαθεσιμότητα για διαμορφώσεις DBMS HA][dbms-guide-figure-200]

Εάν θέλουμε να δημιουργήσετε ιδιαίτερα διαθέσιμες παραμέτρους της αναπτύξεις DBMS (ανεξάρτητα από τα μεμονωμένα DBMS HA λειτουργικότητα που χρησιμοποιείται), του ΣΠΣ DBMS θα πρέπει να:

* Προσθήκη του ΣΠΣ στο ίδιο Azure εικονικό δίκτυο (<https://azure.microsoft.com/documentation/services/virtual-network/>)
* Το ΣΠΣ της ρύθμισης παραμέτρων HA πρέπει επίσης να στο ίδιο υποδίκτυο. Επίλυση ονομάτων μεταξύ του διαφορετικά δευτερεύοντα δίκτυα δεν είναι δυνατή στο αναπτύξεις μόνο στο Cloud, θα λειτουργούν μόνο ανάλυση διευθύνσεων IP. Χρησιμοποιείτε--τοποθεσίας ή συνδεσιμότητας ExpressRoute για αναπτύξεις εσωτερικής εγκατάστασης σταυρό, σε δίκτυο με τουλάχιστον ένα υποδίκτυο θα είναι ήδη σε εξέλιξη. Επίλυση ονόματος που πρέπει να γίνει σύμφωνα με την εσωτερική AD πολιτικές και υποδομή δικτύου. 
[Σχόλιο]: <>  (Δοκιμή TODO MSSedusch Εάν εξακολουθεί να ισχύει στο ARM)

#### <a name="ip-addresses"></a>Διευθύνσεις IP
Συνιστάται ιδιαίτερα να ρυθμίσετε το ΣΠΣ για διαμορφώσεις HA με είναι ανθεκτικά τρόπο. Βασίζεται στις διευθύνσεις IP για την αντιμετώπιση των συνεργατών του HA μέσα στη ρύθμιση παραμέτρων HA δεν είναι αξιόπιστο στο Azure, εκτός εάν χρησιμοποιούνται στατικές διευθύνσεις IP. Υπάρχουν δύο έννοιες "Τερματισμός" στο Azure:

* Τερματισμός μέσω πύλη Azure ή Azure PowerShell cmdlet διακοπή AzureRmVM: σε αυτήν την περίπτωση η εικονική μηχανή λαμβάνει τερματισμού και καταργήστε την έχει εκχωρηθεί. Azure το λογαριασμό σας δεν είναι πλέον θα χρεωθεί για αυτό Εικονική, ώστε να είναι οι μόνες χρεώσεις που θα προκύψουν για το χώρο αποθήκευσης που χρησιμοποιείται. Ωστόσο, εάν δεν ήταν στατικό στην ιδιωτική διεύθυνση IP του περιβάλλοντος εργασίας δικτύου, τη διεύθυνση IP έχει κυκλοφορήσει και δεν υπάρχει εγγύηση ότι η διασύνδεση δικτύου λαμβάνει την παλιά διεύθυνση IP που έχουν εκχωρηθεί ξανά μετά την επανεκκίνηση του η Εικονική. Εκτέλεση του Τερματισμός λειτουργίας προς τα κάτω, μέσω της πύλης Azure ή καλώντας διακοπή AzureRmVM αυτόματα θα προκαλέσει άρσεως εκχώρησης. Εάν δεν θέλετε να deallocat υπολογιστή Χρησιμοποιήστε διακοπή AzureRmVM - StayProvisioned 
* Εάν τερματίζεται η Εικονική από ένα επίπεδο λειτουργικό σύστημα, η Εικονική λαμβάνει τερματίστε και δεν καταργήστε την έχει εκχωρηθεί. Ωστόσο, σε αυτήν την περίπτωση, Azure το λογαριασμό σας θα εξακολουθούν να χρεωθεί για την εικονική Μηχανή, παρά το γεγονός ότι είναι τερματισμού. Σε αυτήν την περίπτωση, η ανάθεση της διεύθυνσης IP για μια Εικονική σταμάτησε να θα παραμένουν ανέπαφες. Τερματισμός η Εικονική από μέσα δεν θα αυτόματα επιβάλετε άρσεως εκχώρησης.

Ακόμη και για σενάρια σταυρό εσωτερικής εγκατάστασης, από προεπιλογή μια τερματισμού και άρσεως εκχώρησης θα σημαίνει ότι άρσεως ανάθεσης από τις διευθύνσεις IP από την εικονική Μηχανή, ακόμα και αν είναι διαφορετικές πολιτικές εσωτερικής εγκατάστασης στο DHCP ρυθμίσεις. 

* Η εξαίρεση είναι εάν ένα αναθέτει μια στατική διεύθυνση IP σε ένα περιβάλλον εργασίας δικτύου όπως περιγράφεται [εδώ][virtual-networks-reserved-private-ip].
* Σε αυτήν την περίπτωση τη διεύθυνση IP παραμένει σταθερή με την προϋπόθεση ότι η διασύνδεση δικτύου δεν διαγράφεται.

> [AZURE.IMPORTANT] Για να διατηρήσετε την ανάπτυξη του ολόκληρη απλές και διαχειρίσιμα, η απαλοιφή σύσταση είναι εγκατάστασης του ΣΠΣ συνεργασία σε ένα DBMS HA ή ρύθμισης παραμέτρων DR εντός Azure με τον τρόπο που υπάρχει ένα λειτουργικό επίλυση ονομάτων μεταξύ του διαφορετικές ΣΠΣ που περιλαμβάνονται.
 
## <a name="deployment-of-host-monitoring"></a>Ανάπτυξη του κεντρικού υπολογιστή παρακολούθησης
Για τη χρήση των εφαρμογών SAP σε εικονικές μηχανές Windows Azure παραγωγικοί, SAP απαιτεί τη δυνατότητα να λάβετε δεδομένα από το φυσικό κεντρικούς υπολογιστές εκτελεί εικονικές μηχανές Windows Azure παρακολούθησης κεντρικού υπολογιστή. Θα χρειαστεί ένα συγκεκριμένο επίπεδο κώδικα του SAP HostAgent που σας επιτρέπει αυτήν τη δυνατότητα στο SAPOSCOL και SAP HostAgent. Το επίπεδο ακριβή κώδικα που τεκμηριώνονται σε σημείωση SAP [1409604].

Για τις λεπτομέρειες σχετικά με την ανάπτυξη των στοιχείων που παραδίδουν δεδομένα κεντρικού υπολογιστή για να SAPOSCOL και SAPHostAgent και τη Διαχείριση κύκλου ζωής αυτά τα στοιχεία, ανατρέξτε στο [οδηγό ανάπτυξης] [Οδηγός ανάπτυξης]

## <a name="3264829e-075e-4d25-966e-a49dad878737"></a>Λεπτομέρειες σχετικά με τον Microsoft SQL Server

### <a name="sql-server-iaas"></a>SQL Server IaaS
Ξεκινώντας με το Windows Azure, μπορείτε εύκολα να μετεγκαταστήσετε τις υπάρχουσες εφαρμογές SQL Server ενσωματωμένη σε πλατφόρμα Windows Server σε εικονικές μηχανές Windows Azure. SQL Server σε μια εικονική μηχανή σας δίνει τη δυνατότητα να μειώσετε το συνολικό κόστος της όσον αφορά την κατοχή ανάπτυξης, διαχείριση και συντήρηση των εφαρμογών πλάτους για μεγάλες επιχειρήσεις, μετεγκατάσταση εύκολα αυτές τις εφαρμογές στο Microsoft Azure. Με τον SQL Server σε ένα Azure εικονική μηχανή, διαχειριστές και τους προγραμματιστές να χρησιμοποιήσετε την ίδια ανάπτυξη και τα εργαλεία διαχείρισης που είναι διαθέσιμες στην εσωτερική εγκατάσταση. 

> [AZURE.IMPORTANT] Έχετε υπόψη σας δεν συζητάτε Azure βάση δεδομένων Microsoft SQL που είναι μια πλατφόρμα ως μια προσφορά υπηρεσίας της πλατφόρμας Microsoft Azure. Τη συζήτηση σε αυτό το έγγραφο είναι σχετικά με την εκτέλεση το προϊόν του SQL Server που είναι γνωστό για αναπτύξεις εσωτερικής εγκατάστασης σε εικονικές μηχανές Windows Azure, αξιοποίηση της υποδομής ως μια λειτουργία υπηρεσίας του Azure. Βάση δεδομένων δυνατότητες και λειτουργίες μεταξύ των δύο αυτές τις προσφορές είναι διαφορετικές και δεν θα πρέπει να είναι μεικτή προς τα επάνω μεταξύ τους. Δείτε επίσης: <https://azure.microsoft.com/services/sql-database/>
 
Σας συνιστούμε να εξετάσετε [αυτό] [ virtual-machines-sql-server-infrastructure-services] τεκμηρίωση πριν να συνεχίσετε.

Στις ενότητες που ακολουθούν τμήματα των τμημάτων της τεκμηρίωσης κάτω από την παραπάνω σύνδεση θα συναθροιστεί και που αναφέρονται. Συγκεκριμένες απαιτήσεις γύρω από SAP αναφέρονται επίσης και ορισμένες έννοιες περιγράφονται με περισσότερες λεπτομέρειες. Ωστόσο, συνιστάται ιδιαίτερα να εργάζεστε με την τεκμηρίωση επάνω από την πρώτη πριν από την ανάγνωση συγκεκριμένες τεκμηρίωση του SQL Server.

Υπάρχει κάποια SQL Server στο IaaS συγκεκριμένες πληροφορίες που πρέπει να γνωρίζετε πριν να συνεχίσετε:

* **Εικονική μηχανή SLA**: υπάρχει μια SLA για εικονικές μηχανές εκτελούνται στο Azure που θα βρείτε εδώ: <https://azure.microsoft.com/support/legal/sla/>  
* **Υποστήριξη έκδοση SQL**: για πελάτες του SAP, υποστηρίζουμε το SQL Server 2008 R2 και νεότερη έκδοση στο Microsoft Azure εικονική μηχανή. Παλαιότερες εκδόσεις δεν υποστηρίζονται. Διαβάστε αυτό γενικά [Δήλωση υποστήριξη](https://support.microsoft.com/kb/956893) για περισσότερες λεπτομέρειες. Έχετε υπόψη ότι γενικά SQL Server 2008 υποστηρίζεται από τη Microsoft καθώς και. Ωστόσο, λόγω σημαντική λειτουργικότητα για SAP που παρουσιάστηκε με SQL Server 2008 R2, SQL Server 2008 R2 είναι η ελάχιστη έκδοση για SAP. Έχετε υπόψη ότι έχετε εκτεταμένης SQL Server 2012 και 2014 με βαθύτερη ενσωμάτωση σε το σενάριο IaaS (όπως η δημιουργία αντιγράφων ασφαλείας απευθείας σε σχέση με αποθήκευσης Azure). Γι ' αυτό, θα σας περιορισμός αυτό το έγγραφο σε SQL Server 2012 και 2014 με την πιο πρόσφατη ενημέρωση κώδικα επίπεδο για Azure.
* **Υποστήριξη δυνατοτήτων SQL**: πιο SQL Server δυνατότητες που υποστηρίζονται στο Microsoft εικονικές μηχανές Windows Azure με ορισμένες εξαιρέσεις. **SQL Server ανακατεύθυνσης συμπλέγματος χρήση κοινόχρηστων δίσκων δεν υποστηρίζεται**.  Κατανεμημένο τεχνολογίες, όπως η δημιουργία ειδώλου βάσης δεδομένων, ομάδες διαθεσιμότητας AlwaysOn, αναπαραγωγής, Αποστολή αρχείου καταγραφής και Service Broker υποστηρίζονται μέσα σε μία μόνο περιοχή Azure. SQL Server AlwaysOn επίσης υποστηρίζεται ανάμεσα σε διαφορετικές περιοχές Azure όπως τεκμηριώνονται εδώ: <https://blogs.technet.com/b/dataplatforminsider/archive/2014/06/19/sql-server-alwayson-availability-groups-supported-between-microsoft-azure-regions.aspx>.  Διαβάστε τη [Δήλωση υποστήριξη](https://support.microsoft.com/kb/956893) για περισσότερες λεπτομέρειες. Ένα παράδειγμα σχετικά με τον τρόπο για να αναπτύξετε μια ρύθμιση παραμέτρων AlwaysOn εμφανίζονται στο [αυτό] [ virtual-machines-workload-template-sql-alwayson] το άρθρο. Επίσης, δείτε το βέλτιστες πρακτικές τεκμηρίωση [εδώ][virtual-machines-sql-server-infrastructure-services] 
* **Απόδοσης SQL**: είμαστε σίγουροι ότι μπορεί να διαφέρει Microsoft Azure φιλοξενούμενη εικονικές μηχανές θα εκτελέσει πολύ καλά σε σύγκριση με άλλα δημόσια cloud virtualization προσφορές, αλλά επιμέρους αποτελέσματα. Ανατρέξτε στο θέμα [αυτό] [ virtual-machines-sql-server-performance-best-practices] το άρθρο.
* **Χρήση εικόνων από το Azure Marketplace**: είναι ο πιο γρήγορος τρόπος για να αναπτύξετε μια νέα εικονική Μηχανή Azure της Microsoft για να χρησιμοποιήσετε μια εικόνα από το Azure Marketplace. Υπάρχουν εικόνες από το Azure Marketplace που περιέχουν SQL Server. Οι εικόνες όπου SQL Server ήδη είναι εγκατεστημένο δεν μπορεί να χρησιμοποιηθεί αμέσως για εφαρμογές SAP NetWeaver. Η αιτία είναι η προεπιλεγμένη ταξινόμηση SQL Server είναι εγκατεστημένη μέσα σε αυτές τις εικόνες και δεν η συρραφή που απαιτούνται από συστήματα SAP NetWeaver. Για να χρησιμοποιήσετε όπως εικόνες, ελέγξτε τα βήματα που τεκμηριώνονται σε κεφάλαιο [χρησιμοποιώντας μια SQL Server εικόνες από το Microsoft Azure Marketplace] [dbms-Οδηγός-5.6]. 
* Δείτε [Τις τιμές λεπτομερειών](https://azure.microsoft.com/pricing/) για περισσότερες πληροφορίες. Το [SQL Server 2012 παραχώρησης πολλαπλών αδειών χρήσης](https://download.microsoft.com/download/7/3/C/73CAD4E0-D0B5-4BE5-AB49-D5B886A5AE00/SQL_Server_2012_Licensing_Reference_Guide.pdf) και [SQL Server 2014 παραχώρησης πολλαπλών αδειών χρήσης Οδηγός](https://download.microsoft.com/download/B/4/E/B4E604D9-9D38-4BBA-A927-56E4C872E41C/SQL_Server_2014_Licensing_Guide.pdf) είναι επίσης ένα σημαντικό πόρων.
 
### <a name="sql-server-configuration-guidelines-for-sap-related-sql-server-installations-in-azure-vms"></a>Οδηγίες ρύθμισης παραμέτρων του SQL Server για SAP που σχετίζονται με εγκαταστάσεις του SQL Server στο ΣΠΣ Azure

#### <a name="recommendations-on-vmvhd-structure-for-sap-related-sql-server-deployments"></a>Συστάσεις σχετικά με τη δομή Εικονική/VHD για SAP που σχετίζονται με αναπτύξεις του SQL Server
Σύμφωνα με τη γενική περιγραφή, εκτελέσιμα SQL Server θα πρέπει να βρίσκεται ή να εγκατασταθούν στη μονάδα δίσκου συστήματος του VHD βάσης την εικονική Μηχανή (μονάδα δίσκου C:\).  Συνήθως, οι περισσότερες από τις βάσεις δεδομένων SQL Server συστήματος δεν εφαρμόζονται σε υψηλό επίπεδο από φόρτο εργασίας SAP NetWeaver. Επομένως το σύστημα βάσεις δεδομένων του SQL Server (υπόδειγμα msdb και μοντέλο) μπορεί να παραμείνει καθώς και της μονάδας δίσκου C:\. Εξαίρεση μπορεί να είναι tempdb, τα οποία στην περίπτωση ορισμένες ERP SAP και όλους τους φόρτους εργασίας εύρους ζώνης που ΈΧΕΙ, ενδέχεται να απαιτείται η ένταση λειτουργίες εισόδου/εξόδου που δεν χωράει σε το αρχικό Εικονική είτε μεγαλύτερου όγκου δεδομένων. Για τα συστήματα, θα πρέπει να εκτελεστούν τα ακόλουθα βήματα:

* Μετακίνηση των αρχείων δεδομένων πρωτεύοντος tempdb στην ίδια λογική μονάδα δίσκου ως κύρια δεδομένα αρχείων της βάσης δεδομένων SAP.
* Προσθέστε τυχόν πρόσθετες tempdb αρχεία δεδομένων σε κάθε μία από τις άλλες λογικές μονάδες δίσκου που περιέχει ένα αρχείο δεδομένων της βάσης δεδομένων χρήστη SAP.
* Προσθέστε το αρχείο καταγραφής tempdb τη λογική μονάδα δίσκου που περιέχει το αρχείο καταγραφής της βάσης δεδομένων χρήστη.
* **Αποκλειστικά για τους τύπους Εικονική που χρησιμοποιεί το τοπικό SSD** σε του υπολογισμού κόμβο tempdb δεδομένων και τα αρχεία καταγραφής μπορεί να τοποθετηθεί στη μονάδα δίσκου D:\. Ωστόσο, ενδέχεται να είναι καλό να χρησιμοποιήσετε πολλά αρχεία δεδομένων tempdb. Πρέπει να γνωρίζετε D:\ μονάδα δίσκου όγκους διαφέρουν ανάλογα με τον τύπο Εικονική.
 
Αυτές οι ρυθμίσεις παραμέτρων Ενεργοποίηση tempdb για την εκμετάλλευση περισσότερο χώρο από τη μονάδα δίσκου συστήματος είναι σε θέση να παρέχουν. Για να προσδιορίσετε το μέγεθος proper tempdb, μία να ελέγξετε τα μεγέθη tempdb στην υπάρχουσα συστήματα που εκτελούν εσωτερικής εγκατάστασης. Επιπλέον, αυτή η ρύθμιση παραμέτρων θα επιτρέπουν IOP Προέλευσης αριθμών σε σχέση με tempdb που δεν είναι δυνατό να παρέχονται με τη μονάδα δίσκου συστήματος. Ξανά, συστήματα που εκτελούν εσωτερικής μπορεί να χρησιμοποιηθεί για την παρακολούθηση της εισόδου/εξόδου φόρτο εργασίας έναντι tempdb, έτσι ώστε να προέρχονται οι αριθμοί IOP Προέλευσης που αναμένετε να δείτε στην tempdb σας.

Μια ρύθμιση παραμέτρων Εικονική που εκτελεί τον SQL Server με μια βάση δεδομένων SAP και όπου tempdb δεδομένων και αρχείο καταγραφής tempdb τοποθετούνται στη μονάδα δίσκου D:\ έχει τη μορφή:
 
![Ρύθμιση παραμέτρων αναφοράς του Azure Εικονική IaaS για SAP][dbms-guide-figure-300]

Έχετε υπόψη ότι η μονάδα δίσκου D:\ έχει διαφορετικά μεγέθη εξαρτάται από τον τύπο Εικονική. Εξαρτάται από την απαίτηση μέγεθος του tempdb μπορεί να είναι υποχρεωμένοι να ζεύγος tempdb δεδομένων και καταγραφής αρχείων με τα SAP βάσης δεδομένων δεδομένων και καταγραφής αρχεία στις περιπτώσεις όπου είναι πολύ μικρό D:\ μονάδα δίσκου.

#### <a name="formatting-the-vhds"></a>Μορφοποίηση του VHD
Για τον SQL Server NTFS μέγεθος μπλοκ για VHD που περιέχει δεδομένα του SQL Server και αρχεία καταγραφής πρέπει να είναι 64K. Δεν χρειάζεται να διαμορφώσετε τη μονάδα D:\. Αυτή η μονάδα δίσκου διατίθεται προ-διαμορφωμένα.

Για να βεβαιωθείτε ότι η επαναφορά ή η δημιουργία βάσεων δεδομένων είναι δεν την προετοιμασία τα αρχεία δεδομένων, εστιάζοντας το περιεχόμενο των αρχείων, ένα θα πρέπει να βεβαιωθείτε ότι εκτελείται η υπηρεσία SQL Server στο περιβάλλον χρήστη έχει ένα συγκεκριμένο δικαίωμα. Συνήθως οι χρήστες στην ομάδα διαχειριστών Windows έχουν αυτά τα δικαιώματα. Εάν εκτελείται η υπηρεσία SQL Server στο περιβάλλον χρήστη του χρήστη δεν είναι Windows διαχειριστής, πρέπει να εκχωρήσετε αυτόν το χρήστη το δικαίωμα χρήστη 'Εκτέλεση εργασιών συντήρησης τόμου'.  Δείτε τις λεπτομέρειες σε αυτό το άρθρο της Γνωσιακής Βάσης της Microsoft: <https://support.microsoft.com/kb/2574695>
 
#### <a name="impact-of-database-compression"></a>Επίδραση συμπίεσης βάσης δεδομένων
Στις ρυθμίσεις παραμέτρων όπου το εύρος ζώνης εισόδου/εξόδου μπορεί να γίνει περιοριστικό παράγοντα, κάθε μέτρο το οποίο μειώνει IOP Προέλευσης μπορεί να σας βοηθήσουν να απομακρύνετε τις άκρες το φόρτο εργασίας μία είναι δυνατό να εκτελεστεί σε ένα σενάριο IaaS όπως Azure. Επομένως, εάν δεν έχουν ακόμα, εφαρμόζοντας συμπίεσης SQL ΣΕΛΊΔΑ διακομιστή συνιστάται SAP και Microsoft πριν από την αποστολή ενός υπάρχοντος SAP βάσεις δεδομένων σε Azure.
 
Τις προτάσεις για να εκτελέσετε τη συμπίεση της βάσης δεδομένων πριν από την αποστολή σε Azure δίνεται από δύο λόγους:

* Το ποσό των δεδομένων που θα αποσταλεί είναι κάτω.
* Η διάρκεια της εκτέλεσης συμπίεσης είναι μικρότερη εάν υποθέσουμε ότι κάποιος να χρησιμοποιήσει ισχυρότερο υλικού με περισσότερες CPU ή νεότερη έκδοση εύρος ζώνης εισόδου/εξόδου ή λιγότερο εισόδου/εξόδου λανθάνων χρόνος εσωτερικής εγκατάστασης.
* Μικρότερα μεγέθη βάσης δεδομένων μπορεί να οδηγήσει σε λιγότερο κόστος για εκχώρηση στο δίσκο

Συμπίεση βάσης δεδομένων καθώς και λειτουργεί σε μια εικονικές μηχανές Windows Azure, όπως συμβαίνει εσωτερικής εγκατάστασης. Για περισσότερες λεπτομέρειες σχετικά με τον τρόπο για να συμπιέσετε μια υπάρχουσα SQL Server SAP βάσης δεδομένων Ελέγξτε εδώ: <https://blogs.msdn.com/b/saponsqlserver/archive/2010/10/08/compressing-an-sap-database-using-report-msscompress.aspx>
  
### <a name="sql-server-2014--storing-database-files-directly-on-azure-blog-storage"></a>SQL Server 2014 – Αποθήκευση βάσης δεδομένων αρχείων απευθείας στην Azure ιστολογίου χώρος αποθήκευσης
SQL Server 2014 ανοίγει τη δυνατότητα να αποθηκεύσετε αρχεία βάσεων δεδομένων απευθείας στο χώρο αποθήκευσης αντικειμένων Blob Azure χωρίς 'περιτυλίγματος' VHD γύρω από αυτές. Ειδικά με τη χρήση τυπική αποθήκευσης Azure ή μικρότερο τύποι Εικονική αυτή η δυνατότητα επιτρέπει σενάρια όπου μπορείτε να παρακάμψετε τα όρια της IOP Προέλευσης που θα επιβληθούν με περιορισμένο αριθμό VHD που μπορεί να ενεργοποιηθεί σε κάποιους τύπους Εικονική μικρότερο μέγεθος. Αυτό λειτουργεί για τις βάσεις δεδομένων χρήστη, ωστόσο δεν για βάσεις δεδομένων συστήματος του SQL Server. Αυτό ισχύει επίσης για δεδομένα και τα αρχεία καταγραφής του SQL Server. Εάν θέλετε να αναπτύξετε μια βάση δεδομένων SAP SQL Server αυτόν τον τρόπο αντί 'αναδίπλωση' στο VHD, φροντίστε τα ακόλουθα υπόψη:

* Ο λογαριασμός χώρου αποθήκευσης χρησιμοποιείται ανάγκες να είναι στην ίδια περιοχή Azure όπως αυτό που χρησιμοποιείται για την ανάπτυξη του SQL Server Εικονική εκτελείται στο.
* Ζητήματα που αναφέρονται παραπάνω σε σχέση με τη διανομή VHD μέσω διαφορετικούς λογαριασμούς χώρου αποθήκευσης Azure ισχύουν για αυτήν τη μέθοδο του καθώς και αναπτύξεις. Σημαίνει ότι το πλήθος λειτουργιών εισόδου/εξόδου σε σχέση με τα όρια του Azure λογαριασμού χώρου αποθήκευσης.
[Σχόλιο]: <>  (MSSedusch TODO, αλλά αυτό θα χρησιμοποιήσει εύρος ζώνης δικτύου και δεν εύρος αποθήκευσης ζώνης, αυτό δεν;)

Λεπτομέρειες σχετικά με αυτόν τον τύπο ανάπτυξης παρατίθενται εδώ: <https://msdn.microsoft.com/library/dn385720.aspx>
 
Για να αποθηκεύσετε αρχεία δεδομένων του SQL Server απευθείας στο χώρο αποθήκευσης Premium Azure, πρέπει να έχετε μια ελάχιστη έκδοση κώδικα SQL Server 2014 που τεκμηριώνονται εδώ: <https://support.microsoft.com/kb/3063054>. Αποθήκευση αρχείων δεδομένων του SQL Server στην τυπική αποθήκευσης Azure λειτουργεί με την τελική έκδοση του SQL Server 2014. Ωστόσο, το ίδιο πολύ προσθήκες περιέχουν μια άλλη σειρά διορθωτικές ενέργειες που κάνετε πιο αξιόπιστη την άμεση χρήση της αποθήκευσης αντικειμένων Blob του Azure για αρχεία δεδομένων του SQL Server και δημιουργία αντιγράφων ασφαλείας. Επομένως, συνιστάται να χρησιμοποιήσετε αυτές τις ενημερωμένες εκδόσεις κώδικα σε γενικές γραμμές.

### <a name="sql-server-2014-buffer-pool-extension"></a>Επέκταση χώρου συγκέντρωσης Buffer διακομιστή 2014 SQL
SQL Server 2014 εισαχθεί μια νέα δυνατότητα που ονομάζεται επέκταση χώρου συγκέντρωσης Buffer. Αυτή η λειτουργία επεκτείνει το χώρο συγκέντρωσης buffer του SQL Server που διατηρείται στη μνήμη με ένα δεύτερο επιπέδου cache που υποστηρίζεται από την τοπική SSD ενός διακομιστή ή Εικονική. Αυτό σας επιτρέπει να διατηρήσετε ένα μεγαλύτερο σύνολο εργασίας δεδομένων 'στη μνήμη'. Σε σύγκριση με την πρόσβαση σε τυπική αποθήκευσης Azure την πρόσβαση στην επέκταση του χώρου συγκέντρωσης buffer που είναι αποθηκευμένη στην τοπική SSD από μια Εικονική Azure είναι πολλοί παράγοντες πιο γρήγορα.  Γι ' αυτό, αξιοποίηση στην τοπική μονάδα δίσκου D:\ από τους τύπους Εικονική που έχουν εξαιρετική IOP Προέλευσης και απόδοση μπορεί να είναι πολύ λογικό τρόπο για μείωση της φόρτωσης IOP Προέλευσης σε σχέση με το χώρο αποθήκευσης Azure και τη βελτίωση εντυπωσιακά χρόνους απόκρισης ερωτημάτων. Αυτό ισχύει ιδίως όταν δεν χρησιμοποιείτε το χώρο αποθήκευσης Premium. Σε περίπτωση Premium χώρου αποθήκευσης και η χρήση της μνήμης Cache Premium Azure ανάγνωση στον κόμβο του υπολογισμού, όπως προτείνεται για τα αρχεία δεδομένων, υπάρχουν διαφορές μεγάλο είναι αναμενόμενο. Αιτία είναι ότι και οι δύο μνήμης cache (επέκταση χώρου συγκέντρωσης Buffer του SQL Server και Premium αποθήκευσης ανάγνωση Cache) χρησιμοποιούν το τοπικό δίσκων κόμβους υπολογιστικών.
Για περισσότερες λεπτομέρειες σχετικά με αυτήν τη λειτουργικότητα, ελέγξτε παρούσα τεκμηρίωση: <https://msdn.microsoft.com/library/dn133176.aspx> 

### <a name="backuprecovery-considerations-for-sql-server"></a>Δημιουργία αντιγράφων ασφαλείας/ανάκτησης ζητήματα για τον SQL Server
Κατά την ανάπτυξη του SQL Server σε Azure μεθοδολογία αντιγράφου ασφαλείας σας, πρέπει να αναθεωρηθούν. Ακόμα και αν το σύστημα δεν είναι παραγωγικοί σύστημα, η βάση δεδομένων SAP που φιλοξενούνται από τον SQL Server πρέπει να δημιουργηθούν αντίγραφα περιοδικά. Επειδή το χώρο αποθήκευσης Azure διατηρεί τρεις εικόνες, ένα αντίγραφο ασφαλείας τώρα είναι λιγότερο σημαντικά σε σχέση με παράγωγων σφάλμα χώρου αποθήκευσης. Ο λόγος προτεραιότητα για τη διατήρηση ένα κατάλληλο πρόγραμμα αντίγραφα ασφαλείας και επαναφορά είναι περισσότερα που μπορεί να αποζημιώσει για σφάλματα λογική/μη αυτόματα, παρέχοντας σημείο στο χρόνο αποκατάστασης δυνατότητες. Επομένως, ο στόχος είναι να είτε χρήση δημιουργίας αντιγράφων ασφαλείας για να επαναφέρετε τη βάση δεδομένων σε ένα συγκεκριμένο σημείο σε χρόνο ή για να χρησιμοποιήσετε τη δημιουργία αντιγράφων ασφαλείας στο Azure να σπείρεται άλλο σύστημα, αντιγράφοντας μια υπάρχουσα βάση δεδομένων. Για παράδειγμα, μπορεί να μεταφέρετε από μια ρύθμιση παραμέτρων SAP 2 επιπέδων σε μια ρύθμιση σύστημα 3 επιπέδων του ίδιου συστήματος με την επαναφορά ενός αντιγράφου ασφαλείας.

Υπάρχουν τρεις διαφορετικοί τρόποι δημιουργίας αντιγράφων ασφαλείας SQL Server με τον χώρο αποθήκευσης Azure:
 
1. SQL Server 2012 CU4 και νεότερη έκδοση μπορούν να των βάσεων δεδομένων εγγενώς αντιγράφου ασφαλείας σε μια διεύθυνση URL. Αυτό είναι λεπτομερή στο ημερολόγιο Web [νέες λειτουργίες του SQL Server 2014 – μέρος 5 – Δημιουργία αντιγράφων ασφαλείας/Επαναφορά βελτιώσεις](https://blogs.msdn.com/b/saponsqlserver/archive/2014/02/15/new-functionality-in-sql-server-2014-part-5-backup-restore-enhancements.aspx). Ανατρέξτε στο θέμα κεφάλαιο [SQL Server 2012 SP1 CU4 και νεότερες εκδόσεις] [dbms-Οδηγός-5.5.1].
1. Εκδόσεις του SQL Server πριν από την SQL 2012 CU4 να χρησιμοποιήσετε μια λειτουργία ανακατεύθυνσης δημιουργίας αντιγράφων ασφαλείας για να VHD και μετακίνηση στην ουσία η ροή εγγραφής προς μια θέση αποθήκευσης Azure που έχει ρυθμιστεί. Ανατρέξτε στο θέμα κεφάλαιο [SQL Server 2012 SP1 CU3 και προηγούμενες εκδόσεις] [dbms-Οδηγός-5.5.2].
1. Η τελική μέθοδος είναι η δημιουργία αντιγράφων ασφαλείας συμβατικός SQL Server σε δίσκο εντολή σε μια συσκευή δίσκου VHD.  Αυτό είναι ίδιο με το μοτίβο ανάπτυξης εσωτερικής εγκατάστασης και δεν αναλύεται με λεπτομέρειες σε αυτό το έγγραφο.

#### <a name="0fef0e79-d3fe-4ae2-85af-73666a6f7268"></a>SQL Server 2012 SP1 CU4 και νεότερες εκδόσεις
Αυτή η λειτουργία σάς επιτρέπει να απευθείας αντίγραφο ασφαλείας με το χώρο αποθήκευσης αντικειμένων BLOB του Azure. Χωρίς αυτήν τη μέθοδο, θα πρέπει να δημιουργήσετε αντίγραφο ασφαλείας σε άλλες VHD Azure που θα εκμετάλλευση χωρητικότητα VHD και IOP Προέλευσης. Η ιδέα στην ουσία είναι το εξής:
 
 ![Χρήση των αντιγράφων ασφαλείας SQL Server 2012 με το χώρο αποθήκευσης αντικειμένων BLOB Windows Azure][dbms-guide-figure-400]

Το πλεονέκτημα σε αυτήν την περίπτωση είναι ότι ένα δεν χρειάζεται να ξοδέψετε VHD για την αποθήκευση αντιγράφων ασφαλείας του SQL Server στο. Επομένως, έχετε λιγότερα VHD έχει εκχωρηθεί και το εύρος ζώνης ολόκληρου του VHD IOP Προέλευσης μπορεί να χρησιμοποιηθεί για τα αρχεία δεδομένων και καταγραφής. Έχετε υπόψη ότι το μέγιστο μέγεθος του αντιγράφου ασφαλείας είναι περιορισμένο σε έως 1 TB, όπως περιγράφεται στην ενότητα 'Περιορισμοί' σε αυτό το άρθρο: <https://msdn.microsoft.com/library/dn435916.aspx#limitations>. Εάν το μέγεθος του αντιγράφου ασφαλείας, παρά χρησιμοποιώντας τη συμπίεση αντιγράφων ασφαλείας SQL Server θα πρέπει να υπερβαίνει τις 1 TB στο μέγεθος, τη λειτουργικότητα που περιγράφεται στο κεφάλαιο [SQL Server 2012 SP1 CU3 και προηγούμενες εκδόσεις] [dbms-Οδηγός-5.5.2] σε αυτό έγγραφο πρέπει να χρησιμοποιηθεί.

[Σχετικές τεκμηρίωση](https://msdn.microsoft.com/library/dn449492.aspx) που περιγράφει την επαναφορά των βάσεων δεδομένων από αντίγραφα ασφαλείας χώρο αποθήκευσης αντικειμένων Blob του Azure συνιστούμε να μην για να επαναφέρετε απευθείας από το χώρο αποθήκευσης αντικειμένων BLOB του Azure εάν είναι η δημιουργία αντιγράφων ασφαλείας > 25 GB. Τις προτάσεις σε αυτό το άρθρο είναι απλώς που βασίζεται σε θέματα επιδόσεων και όχι λόγω λειτουργική περιορισμούς. Επομένως, διαφορετικές συνθήκες ενδέχεται να ισχύουν σε βάση κατά περίπτωση.

Μπορείτε να βρείτε τεκμηρίωση σχετικά με τον τρόπο αυτόν τον τύπο του αντιγράφου ασφαλείας έχει εγκατασταθεί και να χρησιμοποιηθούν σε [αυτό](https://msdn.microsoft.com/library/dn466438.aspx) το πρόγραμμα εκμάθησης
 
Παράδειγμα τη σειρά των βημάτων μπορεί να διαβαστεί [εδώ](https://msdn.microsoft.com/library/dn435916.aspx).

Αυτοματοποίηση της δημιουργίας αντιγράφων ασφαλείας, είναι μεγαλύτερη σημασία για να βεβαιωθείτε ότι τα αντικείμενα BLOB για κάθε αντίγραφο ασφαλείας είναι έχουν διαφορετικά ονόματα. Διαφορετικά θα αντικατασταθούν και έχει διακοπεί η αλυσίδα επαναφορά.
 
Προκειμένου να μην Δημιουργήστε ρυθμίσεις μεταξύ τους 3 διαφορετικούς τύπους δημιουργίας αντιγράφων ασφαλείας, συνιστάται να δημιουργήσετε διαφορετικό κοντέινερ κάτω από το λογαριασμό χώρου αποθήκευσης που χρησιμοποιείται για δημιουργία αντιγράφων ασφαλείας. Το κοντέινερ μπορεί να από Εικονική ή κατά τύπο Εικονική και δημιουργία αντιγράφων ασφαλείας, το μόνο. Το σχήμα μπορεί να μοιάζει με:
 
 ![Χρήση του SQL Server 2012 αντίγραφα ασφαλείας για Microsoft Azure χώρο αποθήκευσης αντικειμένων BLOB – διαφορετικό κοντέινερ στην περιοχή ξεχωριστό λογαριασμό χώρου αποθήκευσης][dbms-guide-figure-500]

Στο παραπάνω παράδειγμα, η δημιουργία αντιγράφων ασφαλείας θα δεν εκτελεστεί τον ίδιο λογαριασμό χώρου αποθήκευσης όπου αναπτύσσονται του ΣΠΣ. Υπάρχει ένα νέο λογαριασμό χώρου αποθήκευσης ειδικά για τη δημιουργία αντιγράφων ασφαλείας. Εντός των λογαριασμών χώρου αποθήκευσης, θα ήταν διαφορετικό κοντέινερ που δημιουργήθηκε με μια μήτρα του τύπου της δημιουργίας αντιγράφων ασφαλείας και το όνομα Εικονική. Όπως αγοράς θα διευκολύνουν να διαχειρίζονται τα αντίγραφα ασφαλείας των διαφορετικό του ΣΠΣ.

Τα αντικείμενα BLOB μία απευθείας συντάσσει τα αντίγραφα ασφαλείας σε, δεν πρόκειται να προσθέσετε για το πλήθος των το VHD από μια Εικονική. Ως εκ τούτου μία θα μπορούσε να μεγιστοποιήσετε τον μέγιστο αριθμό VHD ενεργοποιηθεί από το συγκεκριμένο SKU Εικονική για τα δεδομένα και συναλλαγής αρχείου καταγραφής και εξακολουθείτε να εκτελέσει ένα αντίγραφο ασφαλείας σε ένα κοντέινερ χώρου αποθήκευσης. 

#### <a name="f9071eff-9d72-4f47-9da4-1852d782087b"></a>SQL Server 2012 SP1 CU3 και προηγούμενες εκδόσεις
Το πρώτο βήμα που πρέπει να εκτελέσετε για να επιτύχετε ένα αντίγραφο ασφαλείας απευθείας σε σχέση με αποθήκευσης Azure θα ήταν για να κάνετε λήψη του msi που συνδέεται με [αυτό](https://www.microsoft.com/download/details.aspx?id=40740) το άρθρο KBA.
 
Λήψη x64 το αρχείο εγκατάστασης και την τεκμηρίωση. Το αρχείο θα εγκαταστήσετε ένα πρόγραμμα που ονομάζεται: 'Microsoft SQL Server αντίγραφα ασφαλείας για Microsoft Azure εργαλείο'. Διαβάστε προσεκτικά την τεκμηρίωση του προϊόντος.  Το εργαλείο στην ουσία λειτουργεί με τον εξής τρόπο:

* Από την πλευρά του SQL Server, ορίζεται μια θέση στο δίσκο για το αντίγραφο ασφαλείας του SQL Server (μην χρησιμοποιήσετε τη μονάδα D:\ για αυτό).
* Το εργαλείο θα σας επιτρέψει να ορίσετε κανόνες που μπορεί να χρησιμοποιηθεί για να κατευθύνετε διαφορετικούς τύπους αντίγραφα ασφαλείας σε διαφορετικό χώρο αποθήκευσης Azure κοντέινερ.
* Όταν οι κανόνες είναι στη θέση, το εργαλείο θα σας ανακατευθύνει στη ροή εγγραφής του αντιγράφου ασφαλείας σε μία των VHD/δίσκων στη θέση αποθήκευσης Azure που ορίστηκε νωρίτερα.
* Το εργαλείο θα αποχώρηση από ένα αρχείο μικρές στέλεχος μερικά KB μεγέθους του VHD/δίσκου στην οποία έχει οριστεί για τον SQL Server δημιουργίας αντιγράφων ασφαλείας. **Αυτό το αρχείο πρέπει να παραμένει στη θέση αποθήκευσης, εφόσον απαιτείται για την επαναφορά ξανά από χώρο αποθήκευσης Azure.**
    * Εάν έχετε χάσει το αρχείο στέλεχος (π.χ., με απώλεια των πολυμέσων χώρου αποθήκευσης που περιείχε το αρχείο στέλεχος) και έχετε επιλέξει την επιλογή αντίγραφα ασφαλείας σε ένα λογαριασμό Microsoft Azure αποθήκευσης, ενδέχεται να μπορείτε να ανακτήσετε το αρχείο στέλεχος μέσω χώρος αποθήκευσης Microsoft Azure πραγματοποιώντας λήψη του από το κοντέινερ χώρου αποθήκευσης στο οποίο έχει τοποθετηθεί. Στη συνέχεια, θα πρέπει να τοποθετήσετε το αρχείο στέλεχος σε ένα φάκελο στον τοπικό υπολογιστή όπου το εργαλείο έχει ρυθμιστεί ώστε να εντοπίσουν και να στείλετε στο ίδιο κοντέινερ με τον ίδιο κωδικό πρόσβασης κρυπτογράφησης εάν κρυπτογράφησης χρησιμοποιήθηκε με τον αρχικό κανόνα. 

Αυτό σημαίνει ότι το σχήμα όπως περιγράφεται παραπάνω για τις πιο πρόσφατες εκδόσεις του SQL Server μπορούν να δημιουργηθούν καθώς και σε εκδόσεις του SQL Server που δεν επιτρέπουν τη διεύθυνση απευθείας μια θέση αποθήκευσης Azure.
 
Αυτή η μέθοδος δεν μπορεί να χρησιμοποιηθεί με πιο πρόσφατες εκδόσεις του SQL Server που υποστηρίζουν τη δημιουργία αντιγράφων ασφαλείας εγγενώς σε σχέση με το χώρο αποθήκευσης Azure. Εξαιρούνται οι όπου περιορισμούς του εγγενούς αντιγράφου ασφαλείας σε Azure εμποδίζουν εγγενούς εκτέλεσης αντιγράφου ασφαλείας σε Azure.

#### <a name="other-possibilities-to-backup-sql-server-databases"></a>Άλλες δυνατότητες ασφαλείας βάσεις δεδομένων SQL Server
Άλλες δυνατότητες σε βάσεις δεδομένων αντιγράφων ασφαλείας είναι για να επισυνάψετε επιπλέον VHD σε μια Εικονική που χρησιμοποιείτε για την αποθήκευση αντιγράφων ασφαλείας στο. Σε αυτήν την περίπτωση που χρειάζεστε για να βεβαιωθείτε ότι το VHD δεν εκτελούνται πλήρους. Εάν πρόκειται για την περίπτωση, θα πρέπει να απενεργοποιήσετε τον VHD και ώστε να speak 'αρχειοθέτηση' αυτό και να το αντικαταστήσετε με μια νέα κενή VHD. Εάν μεταβείτε προς τα κάτω τη διαδρομή, που θέλετε να διατηρήσετε τα εξής VHD σε ξεχωριστή λογαριασμοί χώρου αποθήκευσης Azure από αυτά που το VHD με τα αρχεία βάσης δεδομένων.

Δεύτερο πιθανότητα είναι να χρησιμοποιήσετε μια μεγάλη Εικονική που μπορεί να έχει πολλές VHD συνημμένα. Π.χ. D14 με 32VHDs. Χρησιμοποιείτε κενά διαστήματα χώρου αποθήκευσης για να δημιουργήσετε ένα ευέλικτο περιβάλλον όπου μπορείτε να δημιουργήσετε κοινόχρηστους πόρους που χρησιμοποιούνται στη συνέχεια, ως αντίγραφο ασφαλείας των προορισμών για διαφορετικούς διακομιστές DBMS.
 
Ορισμένες βέλτιστες πρακτικές που έχετε τεκμηριώνονται [δείτε](https://blogs.msdn.com/b/sqlcat/archive/2015/02/26/large-sql-server-database-backup-on-an-azure-vm-and-archiving.aspx) επίσης. 

#### <a name="performance-considerations-for-backupsrestores"></a>Ζητήματα επιδόσεων για δημιουργία αντιγράφων ασφαλείας/επαναφέρει
Όπως στο αναπτύξεις χωρίς λειτουργικό σύστημα, δημιουργία αντιγράφων ασφαλείας/Επαναφορά επιδόσεων εξαρτάται από πόσες όγκους μπορεί να διαβαστεί παράλληλα και της ταχύτητας μετάδοσης των αυτές τις όγκους που μπορεί να είναι. Επιπλέον, την κατανάλωση CPU που χρησιμοποιείται από το αντίγραφο ασφαλείας συμπίεσης, μπορούν να συμβάλλουν σημαντική σε VM με μόνο έως 8 νήματα CPU. Επομένως, μπορεί να λάβει ένα:

* Τα λιγότερα τον αριθμό των VHD που χρησιμοποιείται για την αποθήκευση των δεδομένων αρχείων, όσο πιο μικρή τη συνολική μεταγωγή στην ανάγνωση.
* Το μικρότερο τον αριθμό των CPU νήματα σε Εικονική, τα πιο σοβαρά την επίδραση των αντιγράφων ασφαλείας συμπίεσης.
* Τα λιγότερα στόχοι (αντικείμενα BLOB ή VHD) για να γράψετε το αντίγραφο ασφαλείας, να το μικρότερο βαθμό τη μετάδοση.
* Όσο πιο μικρή η Εικονική μέγεθος, το μικρότερο του ορίου χώρου αποθήκευσης μετάδοσης εγγραφή και την ανάγνωση από το χώρο αποθήκευσης Azure. Ανεξάρτητα από το εάν τα αντίγραφα ασφαλείας είναι αποθηκευμένα απευθείας σε αντικειμένων Blob του Azure ή εάν είναι αποθηκευμένα στο VHD που είναι αποθηκευμένα ξανά σε αντικείμενα BLOB Azure.

Όταν χρησιμοποιείτε ένα BLOB Microsoft Azure αποθήκευσης ως προορισμό του αντιγράφου ασφαλείας στις πιο πρόσφατες εκδόσεις, μπορείτε περιορίζονται καθορίζοντας μόνο μία διεύθυνση URL προορισμού για κάθε συγκεκριμένο αντίγραφο ασφαλείας.
 
Αλλά, όταν χρησιμοποιείτε το 'Microsoft SQL Server αντίγραφο ασφαλείας στο Microsoft Azure εργαλείο' σε παλαιότερες εκδόσεις, μπορείτε να ορίσετε περισσότερα από ένα αρχείο προορισμού. Με περισσότερες από μία προορισμού, να περιορίσετε το μέγεθος της δημιουργίας αντιγράφων ασφαλείας και την απόδοση του αντιγράφου ασφαλείας είναι νεότερη έκδοση. Αυτό θα έχει ως αποτέλεσμα, στη συνέχεια, σε πολλά αρχεία, καθώς και στο χώρο αποθήκευσης Azure λογαριασμό. Στο μας δοκιμές, με χρήση πολλών αρχείων προορισμούς μία σίγουρα να επιτύχετε τη μετάδοση που μία θα μπορούσε να επιτύχετε με τις επεκτάσεις αντιγράφου ασφαλείας υλοποιηθεί σε από SQL Server 2012 SP1 CU4 σε. Μπορείτε επίσης δεν αποκλείονται από το όριο 1TB όπως το εγγενές αντίγραφο ασφαλείας σε Azure.

Ωστόσο, να θυμάστε ότι, η ταχύτητα μετάδοσης είναι επίσης εξαρτάται από τη θέση του λογαριασμού χώρου αποθήκευσης Azure που χρησιμοποιείτε για τη δημιουργία αντιγράφων ασφαλείας. Μπορεί να είναι μια ιδέα για να εντοπίσετε το λογαριασμό χώρου αποθήκευσης σε διαφορετική περιοχή από αυτήν του ΣΠΣ εκτελούνται. Π.χ. που θα εκτελέσετε τη ρύθμιση παραμέτρων Εικονική στην Ευρώπη Δυτική, αλλά τοποθέτηση το λογαριασμό χώρου αποθήκευσης που χρησιμοποιείτε για να δημιουργήσετε αντίγραφα ασφαλείας σε σχέση με Βόρειας Ευρώπης. Σίγουρα που θα έχουν επίδραση στις μεταγωγή των αντιγράφων ασφαλείας και δεν είναι ενδέχεται να δημιουργήσει μια μετάδοσης από 150MB/sec φαίνεται να είναι δυνατή η στις περιπτώσεις όπου το χώρο αποθήκευσης προορισμού και του ΣΠΣ εκτελούνται στο ίδιο τοπικού κέντρου δεδομένων.

#### <a name="managing-backup-blobs"></a>Διαχείριση αντικειμένων blob δημιουργίας αντιγράφων ασφαλείας
Υπάρχει απαίτηση για να διαχειριστείτε τα αντίγραφα ασφαλείας των δικών σας. Επειδή το προσδοκίες είναι ότι θα δημιουργηθούν πολλά αντικείμενα BLOB εκτελώντας αντίγραφα ασφαλείας καταγραφής συναλλαγών συχνές, Διαχείριση αυτά τα αντικείμενα BLOB εύκολα μπορεί να επιβαρύνει την πύλη Azure. Επομένως, είναι recommendable για να αξιοποιήσετε μια Azure Εξερεύνηση χώρου αποθήκευσης. Υπάρχουν αρκετά καλή αυτές που διατίθενται που μπορούν να σας βοηθήσουν να διαχειριστείτε ένα λογαριασμό Azure χώρου αποθήκευσης

* Microsoft Visual Studio με Azure SDK εγκατεστημένο (<https://azure.microsoft.com/downloads/>)
* Εξερεύνηση χώρου αποθήκευσης Microsoft Azure (<https://azure.microsoft.com/downloads/>)
* Εργαλεία τρίτου κατασκευαστή

[Σχόλιο]: <>  (Υποστηρίζονται ακόμη στο ARM)
[Σχόλιο]: <>  (### Εικονική azure δημιουργίας αντιγράφων ασφαλείας)
[Σχόλιο]: <>  (ΣΠΣ εντός του συστήματος SAP μπορεί να δημιουργηθεί αντίγραφο ασφαλείας χρησιμοποιεί τη λειτουργία δημιουργίας αντιγράφων ασφαλείας εικονική μηχανή Azure. Azure εικονική μηχανή δημιουργίας αντιγράφων ασφαλείας στη διάθεσή σας για πρώτη φορά το έτος 2015 στην αρχή και στο μεταξύ είναι μια τυπική μέθοδος δημιουργίας αντιγράφων ασφαλείας για μια πλήρη Εικονική στα Azure. Azure αντίγραφο ασφαλείας αποθηκεύει τα αντίγραφα ασφαλείας σε Azure και επιτρέπει την επαναφορά μια Εικονική ξανά.) 
[Σχόλιο]: <> (ΣΠΣ που εκτέλεση βάσεις δεδομένων μπορούν να δημιουργηθούν αντίγραφα ασφαλείας με συνεπή τρόπο, καθώς και εάν τα συστήματα DBMS υποστηρίζει το VSS Windows (ένταση σκιά αντίγραφο υπηρεσία - <https://msdn.microsoft.com/library/windows/desktop/bb968832.aspx>), όπως π.χ. SQL Server. Επομένως, χρήση των αντιγράφων ασφαλείας Εικονική Azure θα μπορούσε να είναι ένας τρόπος για να μεταβείτε σε μια δυνατότητα επαναφοράς αντίγραφο ασφαλείας της βάσης δεδομένων SAP. Ωστόσο, έχετε υπόψη που βασίζονται σε Εικονική Azure αντίγραφα ασφαλείας σε δεδομένη χρονική στιγμή επαναφέρει βάσεις δεδομένων δεν είναι δυνατό. Γι ' αυτό, η σύσταση είναι να εκτελέσετε αντίγραφα ασφαλείας των βάσεων δεδομένων με λειτουργίες DBMS αντί να βασίζεστε στο αντίγραφο ασφαλείας Εικονική Azure.) [Σχόλιο]: <> (για να εξοικειωθείτε με το αντίγραφο ασφαλείας εικονική μηχανή Azure Ξεκινήστε εδώ <https://azure.microsoft.com/documentation/services/backup/>)

### <a name="1b353e38-21b3-4310-aeb6-a77e7c8e81c8"></a>Χρήση μιας SQL Server εικόνων από το Microsoft Azure Marketplace
Η Microsoft προσφέρει ΣΠΣ στην αγορά Azure που περιέχει ήδη εκδόσεις του SQL Server. Για πελάτες SAP που απαιτούν άδειες χρήσης για SQL Server και των Windows, αυτό μπορεί να είναι μια ευκαιρία για να καλύψετε στην ουσία την ανάγκη για άδειες χρήσης κατά την περιστροφή του ΣΠΣ με τον SQL Server που είναι ήδη εγκατεστημένο. Για να χρησιμοποιήσετε τέτοιου είδους εικόνες για SAP, τα ακόλουθα θέματα που πρέπει να γίνουν:

* Οι εκδόσεις μη αξιολόγησης SQL Server απόκτηση υψηλότερη κόστους από απλώς ένα "Μόνο για τα Windows" Εικονική αναπτυχθεί από το Azure Marketplace. Ανατρέξτε στο θέμα αυτά τα άρθρα για να συγκρίνετε τις τιμές: <https://azure.microsoft.com/pricing/details/virtual-machines/> και <https://azure.microsoft.com/pricing/details/virtual-machines/#Sql>. 
* Μπορείτε μόνο να χρησιμοποιήσετε εκδόσεις του SQL Server που υποστηρίζονται από SAP, όπως το SQL Server 2012.
* Η συρραφή της παρουσίας του SQL Server στον οποίο είναι εγκατεστημένα τα ΣΠΣ που διέθετε το Azure Marketplace δεν είναι η συρραφή SAP NetWeaver απαιτεί την παρουσία του SQL Server για να εκτελέσετε. Μπορείτε να αλλάξετε τη συρραφή αν και με τις οδηγίες στην επόμενη ενότητα.

#### <a name="changing-the-sql-server-collation-of-a-microsoft-windowssql-server-vm"></a>Αλλάξετε τη συρραφή SQL Server Εικονική Microsoft Windows/SQL Server
Επειδή οι εικόνες SQL Server στο το Azure Marketplace δεν έχουν ρυθμιστεί για να χρησιμοποιήσετε τη συρραφή, το οποίο απαιτείται από εφαρμογές SAP NetWeaver, πρέπει να αλλάξουν αμέσως μετά την ανάπτυξη. Για τον SQL Server 2012, αυτό μπορεί να γίνει με τα παρακάτω βήματα μόλις η Εικονική έχει αναπτυχθεί και ο διαχειριστής έχει τη δυνατότητα να συνδεθείτε με την ανάπτυξη εικονική Μηχανή:

* Ανοίξτε ένα παράθυρο εντολών του Windows 'ως διαχειριστής".
* Αλλάξτε τον κατάλογο C:\Program Files\Microsoft SQL Server\110\Setup Bootstrap\SQLServer2012.
* Εκτέλεση της εντολής: Setup.exe /QUIET /ACTION = REBUILDDATABASE /INSTANCENAME = MSSQLSERVER /SQLSYSADMINACCOUNTS =`<local_admin_account_name`> /SQLCOLLATION = SQL_Latin1_General_Cp850_BIN2   
    * `<local_admin_account_name`> είναι το λογαριασμό που έχει οριστεί ως το λογαριασμό διαχειριστή κατά την ανάπτυξη του Εικονική για πρώτη φορά μέσω της συλλογής.

Η διαδικασία πρέπει να διαρκέσει μόνο μερικά λεπτά. Για να βεβαιωθείτε ότι εάν λήξει το βήμα με το σωστό αποτέλεσμα, εκτελέστε τα ακόλουθα βήματα:

* Ανοίξτε το SQL Server Management Studio.
* Ανοίξτε ένα παράθυρο του ερωτήματος.
* Εκτελέστε την εντολή sp_helpsort στην κύρια βάση δεδομένων SQL Server.

Πρέπει να μοιάζει το επιθυμητό αποτέλεσμα:

    Latin1-General, binary code point comparison sort for Unicode Data, SQL Server Sort Order 40 on Code Page 850 for non-Unicode Data

Εάν αυτό δεν είναι το αποτέλεσμα, ΔΙΑΚΌΨΤΕ την ανάπτυξη SAP και Διερεύνηση γιατί η εντολή εγκατάστασης δεν λειτούργησε όπως αναμένεται. Ανάπτυξη των εφαρμογών SAP NetWeaver στην παρουσία του SQL Server με διαφορετικές κωδικοσελίδες SQL Server από τον που αναφέρονται παραπάνω **δεν** υποστηρίζεται.

### <a name="sql-server-high-availability-for-sap-in-azure"></a>SQL Server υψηλής διαθεσιμότητας για SAP στο Azure
Όπως αναφέρθηκε προηγουμένως σε αυτό το έγγραφο, δεν υπάρχει δυνατότητα για τη δημιουργία κοινόχρηστου χώρου αποθήκευσης που απαιτείται για τη χρήση της λειτουργίας υψηλής διαθεσιμότητας παλιότερη SQL Server. Αυτή η λειτουργία θα εγκαταστήσετε δύο ή περισσότερες παρουσίες του SQL Server σε ένα Windows Server ανακατεύθυνσης συμπλέγματος (WSFC) χρησιμοποιώντας έναν κοινόχρηστο δίσκο για το χρήστη βάσεις δεδομένων (και τελικά tempdb). Αυτή είναι η μέθοδος τυπική υψηλή διαθεσιμότητα μεγάλο χρονικό διάστημα που υποστηρίζεται επίσης από SAP. Επειδή το Azure δεν υποστηρίζει κοινόχρηστο χώρο αποθήκευσης, δεν είναι δυνατό να πραγματοποιείται ρυθμίσεις παραμέτρων υψηλής διαθεσιμότητας SQL Server με μια ρύθμιση παραμέτρων του συμπλέγματος κοινόχρηστο δίσκο. Ωστόσο, πολλές άλλες μεθόδους υψηλή διαθεσιμότητα είναι ακόμη διαθέσιμες και περιγράφονται στις ενότητες που ακολουθούν.

[Σχόλιο]: <>  (Το άρθρο είναι εξακολουθεί να αναφέρεται σε ASM)
[Σχόλιο]: <>  (Πριν από την ανάγνωση τις τεχνολογίες διαφορετικές συγκεκριμένες υψηλή διαθεσιμότητα μπορεί να χρησιμοποιηθεί για τον SQL Server στο Azure, είναι πολύ καλό εγγράφου που παρέχει περισσότερες λεπτομέρειες και δείκτες [here][virtual-machines-sql-server-high-availability-and-disaster-recovery-solutions])

#### <a name="sql-server-log-shipping"></a>Αποστολή καταγραφής του SQL Server
Μία από τις μεθόδους υψηλής διαθεσιμότητας (HA) είναι Αποστολή αρχείου καταγραφής του SQL Server. Εάν το ΣΠΣ συμμετέχουν στη ρύθμιση παραμέτρων HA έχουν εργασία επίλυση ονομάτων, δεν υπάρχει κανένα πρόβλημα και τη ρύθμιση στο Azure δεν θα διαφέρει από οποιοδήποτε πρόγραμμα εγκατάστασης που έχει ολοκληρωθεί εσωτερικής εγκατάστασης. Δεν συνιστάται να βασίζεστε στην ανάλυση IP μόνο. Σε σχέση με τη ρύθμιση του αρχείου καταγραφής αποστολές και τις αρχές γύρω από την αποστολή αρχείου καταγραφής Ελέγξτε παρούσα τεκμηρίωση:

<https://TechNet.Microsoft.com/Library/ms187103.aspx>
 
Για να επιτύχετε πραγματικά οποιαδήποτε υψηλής διαθεσιμότητας, μία χρειάζεται για την ανάπτυξη του ΣΠΣ που βρίσκονται μέσα σε μια τέτοια αποστολή αρχείου καταγραφής ρύθμιση παραμέτρων για να είναι το ίδιο σύνολο διαθεσιμότητα Azure.

#### <a name="database-mirroring"></a>Δημιουργία ειδώλου βάσης δεδομένων
Mirroring βάσης δεδομένων ως υποστηρίζεται από SAP (ανατρέξτε στο θέμα Σημείωση SAP [965908]) εξαρτάται από τον ορισμό συνεργάτης ανακατεύθυνσης στη συμβολοσειρά σύνδεσης SAP. Για τις περιπτώσεις σταυρό εσωτερικής εγκατάστασης, θα σας λαμβάνεται ως δεδομένο ότι τα δύο ΣΠΣ είναι στον ίδιο τομέα και ότι το περιβάλλον χρήστη του SQL Server εκτελούνται δύο παρουσίες στην περιοχή είναι καθώς και οι χρήστες τομέα και έχετε τα απαραίτητα δικαιώματα σε δύο εμφανίσεις SQL Server που εμπλέκονται. Επομένως, τη ρύθμιση της βάσης δεδομένων αποτελούν πιστή αναπαράσταση στο Azure δεν διαφέρουν μεταξύ ένα τυπικό εσωτερικής/ρύθμιση παραμέτρων της εγκατάστασης.

Το αναπτύξεις μόνο στο Cloud, η ευκολότερη μέθοδος είναι να υπάρχει άλλη ρύθμιση του τομέα στο Azure να έχετε αυτές τις ΣΠΣ DBMS (και ιδανικά αποκλειστικό ΣΠΣ SAP) μέσα σε έναν τομέα.

Εάν ένας τομέας δεν είναι δυνατό, ένα επίσης να χρησιμοποιήσετε τα πιστοποιητικά για τη βάση δεδομένων που αποτελούν πιστή αναπαράσταση τελικά σημεία, όπως περιγράφεται εδώ: <https://technet.microsoft.com/library/ms191477.aspx>

Θα βρείτε εδώ ένα πρόγραμμα εκμάθησης για να γίνεται η ρύθμιση βάσης δεδομένων αποτελούν πιστή αναπαράσταση στο Azure: <https://technet.microsoft.com/library/ms189852.aspx> 

#### <a name="alwayson"></a>AlwaysOn
Υποστηρίζεται AlwaysOn για SAP εσωτερικής εγκατάστασης (ανατρέξτε στο θέμα Σημείωση SAP [1772688]), υποστηρίζεται για να χρησιμοποιηθεί σε συνδυασμό με το SAP στο Azure. Το γεγονός ότι δεν θα μπορείτε να δημιουργήσετε κοινόχρηστα δίσκων στο Azure αυτό δεν σημαίνει ότι ένα δεν είναι δυνατό να δημιουργήσετε μια ρύθμιση παραμέτρων AlwaysOn Windows Server ανακατεύθυνσης συμπλέγματος (WSFC) ανάμεσα σε διαφορετικές ΣΠΣ. Μόνο σημαίνει ότι δεν έχετε τη δυνατότητα να χρησιμοποιήσετε έναν κοινόχρηστο δίσκο ως απαρτίας στις ρυθμίσεις παραμέτρων του συμπλέγματος. Επομένως μπορείτε να δημιουργήσετε μια ρύθμιση παραμέτρων AlwaysOn WSFC στο Azure και απλώς δεν επιλέξτε τον τύπο απαρτίας που χρησιμοποιεί κοινόχρηστο δίσκο. Το Azure περιβάλλον αυτά τα ΣΠΣ έχουν αναπτυχθεί στο πρέπει να επίλυση του ΣΠΣ με βάση το όνομα και το ΣΠΣ θα πρέπει να είναι στον ίδιο τομέα. Αυτό ισχύει για το Azure μόνο και αναπτύξεις σταυρό εσωτερικής εγκατάστασης. Υπάρχουν ορισμένα ειδικά ζητήματα γύρω από την ανάπτυξη του SQL Server διαθεσιμότητα ομάδα ακρόασης (δεν είναι να συγχέονται με το σύνολο διαθεσιμότητα Azure) επειδή το Azure σε αυτό το σημείο στο φορά δεν σας επιτρέπει να δημιουργήσετε απλώς ένα αντικείμενο AD/DNS ως έχει πιθανές εσωτερικής εγκατάστασης. Επομένως, ορισμένα βήματα διαφορετική εγκατάσταση είναι απαραίτητα πρέπει να αντιμετωπίσετε τη συγκεκριμένη συμπεριφορά του Azure.

Ορισμένα ζητήματα χρησιμοποιώντας μια ακρόασης διαθεσιμότητα ομάδας είναι οι εξής:

* Χρησιμοποιώντας ένα ακρόασης ομάδα διαθεσιμότητα είναι δυνατή μόνο με Windows Server 2012 ή Windows Server 2012 R2 ως επισκέπτης λειτουργικό σύστημα του η Εικονική. Windows Server 2012 θα πρέπει να βεβαιωθείτε ότι έχει εφαρμοστεί αυτή η ενημέρωση κώδικα: <https://support.microsoft.com/kb/2854082> 
* Για Windows Server 2008 R2 δεν υπάρχει αυτήν την ενημέρωση κώδικα και AlwaysOn θα πρέπει να χρησιμοποιηθεί με τον ίδιο τρόπο όπως αποτελούν πιστή αναπαράσταση των βάσεων δεδομένων, καθορίζοντας συνεργάτης ανακατεύθυνσης στη συμβολοσειρά συνδέσεις (εργασία μέσω του SAP default.pfl παραμέτρου Demand dbs/mss/διακομιστή – δείτε σημείωση SAP [965908]).
* Όταν χρησιμοποιείτε ένα ακρόασης διαθεσιμότητα ομάδας, του ΣΠΣ βάσης δεδομένων πρέπει να είστε συνδεδεμένοι σε μια αποκλειστική εξισορρόπηση φόρτου. Επίλυση ονόματος στο μόνο στο Cloud αναπτύξεις είτε απαιτείται όλα ΣΠΣ ενός συστήματος SAP (διακομιστές εφαρμογών, DBMS διακομιστή και (Α) διακομιστή SCS) είναι στο ίδιο δίκτυο εικονικού ή απαιτείται από ένα επίπεδο εφαρμογών SAP τη διατήρηση του αρχείου etc\host για να λάβετε τα ονόματα Εικονική του ΣΠΣ διακομιστή SQL επιλυθεί. Για να αποφύγετε ότι Azure εκχωρεί νέες διευθύνσεις IP στις περιπτώσεις όπου δύο ΣΠΣ είναι παρεμπιπτόντως τερματισμού, μία πρέπει να εκχωρήσετε στατικές διευθύνσεις IP για τις διασυνδέσεις δικτύου αυτών ΣΠΣ στη ρύθμιση παραμέτρων AlwaysOn (καθορίζει μια στατική διεύθυνση IP περιγράφεται σε [αυτό] [ virtual-networks-reserved-private-ip] άρθρο) [Σχόλιο]: <> (παλιά ιστολόγια) [Σχόλιο]: <> (<https://blogs.msdn.com/b/alwaysonpro/archive/2014/08/29/recommendations-and-best-practices-when-deploying-sql-server-alwayson-availability-groups-in-windows-azure-iaas.aspx> <https://blogs.technet.com/b/rmilne/archive/2015/07/27/how-to-set-static-ip-on-azure-vm.aspx>) 
* Υπάρχουν ειδικές βήματα που απαιτούνται όταν δημιουργείτε τη ρύθμιση παραμέτρων του συμπλέγματος WSFC όπου το σύμπλεγμα χρειάζεται μια ειδική διεύθυνση IP έχει ανατεθεί, επειδή το Azure με την τρέχουσα λειτουργικότητα θα εκχωρήσετε, το όνομα του συμπλέγματος την ίδια διεύθυνση IP ανάλογα με τον κόμβο του συμπλέγματος δημιουργείται στο. Αυτό σημαίνει ότι ένα μη αυτόματη βήμα πρέπει να εκτελεστούν για να αντιστοιχίσετε μια διαφορετική διεύθυνση IP για το σύμπλεγμα.
* Η διαθεσιμότητα ακρόαση ομάδα πρόκειται να δημιουργηθούν σε Azure με τα τελικά σημεία TCP/IP που έχουν εκχωρηθεί σε του ΣΠΣ εκτελείται το κύριας και δευτερεύουσας αντίγραφα της ομάδας διαθεσιμότητας.
* Μπορεί να υπάρχει ανάγκη για την ασφάλιση αυτά τα τελικά σημεία με ACL.

[Σχόλιο]: <>  (TODO παλιό ιστολογίου)
[Σχόλιο]: <>  (Τα λεπτομερή βήματα και απαραίτητα μέσα για την εγκατάσταση μιας ρύθμισης παραμέτρων AlwaysOn στην Azure είναι η καλύτερη εμπειρία κατά την παρουσίαση μερικών μέσω του προγράμματος εκμάθησης διαθέσιμη [here][virtual-machines-windows-classic-ps-sql-alwayson-availability-groups])
[Σχόλιο]: <>  (Ρύθμιση προ-διαμορφωμένες AlwaysOn μέσω της συλλογής Azure < https://blogs.technet.com/b/dataplatforminsider/archive/2014/08/25/sql-server-alwayson-offering-in-microsoft-azure-portal-gallery.aspx>)
[Σχόλιο]: <>  (Δημιουργία μιας ακρόασης ομάδα διαθεσιμότητα είναι καλύτερα περιγράφεται σε [this][virtual-machines-windows-classic-ps-sql-int-listener] πρόγραμμα εκμάθησης)
[Σχόλιο]: <>  (Ασφαλή τα τελικά σημεία δικτύου με ACL εξηγούνται καλύτερα εδώ:)
[Σχόλιο]: <>  (* < https://michaelwasham.com/windows-azure-powershell-reference-guide/network-access-control-list-capability-in-windows-azure-powershell/>)
[Σχόλιο]: <>  (* < https://blogs.technet.com/b/heyscriptingguy/archive/2013/08/31/weekend-scripter-creating-acls-for-windows-azure-endpoints-part-1-of-2.aspx>)
[Σχόλιο]: <>  (* < https://blogs.technet.com/b/heyscriptingguy/archive/2013/09/01/weekend-scripter-creating-acls-for-windows-azure-endpoints-part-2-of-2.aspx>)  
[Σχόλιο]: <>  (* < https://blogs.technet.com/b/heyscriptingguy/archive/2013/09/18/creating-acls-for-windows-azure-endpoints.aspx>) 

Είναι δυνατή η ανάπτυξη μιας ομάδας διαθεσιμότητας SQL Server AlwaysOn πάνω από διαφορετικές περιοχές Azure καθώς και. Αυτή η λειτουργία θα αξιοποιήσετε τη συνδεσιμότητα Azure VNet-σε-Vnet ([περισσότερες λεπτομέρειες][virtual-networks-configure-vnet-to-vnet-connection]).
[Σχόλιο]: <> (TODO παλιά ιστολόγιο) [Σχόλιο]: <> (την εγκατάσταση του SQL Server AlwaysOn διαθεσιμότητα ομάδες σε ένα τέτοιο σενάριο περιγράφεται εδώ: <https://blogs.technet.com/b/dataplatforminsider/archive/2014/06/19/sql-server-alwayson-availability-groups-supported-between-microsoft-azure-regions.aspx>.) 

#### <a name="summary-on-sql-server-high-availability-in-azure"></a>Σύνοψη στον SQL Server υψηλή διαθεσιμότητα στο Azure
Δεδομένου ότι αποθήκευσης Azure προστατεύει το περιεχόμενο, υπάρχει ένας λιγότερο λόγος να υποστηρίζεται σε μια εικόνα ζεστού την αναμονή. Αυτό σημαίνει ότι η δική σας περίπτωση υψηλή διαθεσιμότητα πρέπει να προστατευτώ μόνο από τις παρακάτω περιπτώσεις:

* Μη διαθεσιμότητα του την εικονική Μηχανή στο σύνολό λόγω συντήρησης σε το σύμπλεγμα διακομιστών στο Azure ή άλλους λόγους
* Θέματα λογισμικού στο παρουσίας του SQL Server
* Προστασία από μη αυτόματη σφάλματος όπου λαμβάνει διαγράφονται δεδομένα και είναι απαραίτητη αποκατάστασης στη δεδομένη χρονική στιγμή

Κοιτώντας αντίστοιχα τεχνολογίες μία μπορεί να προβάλλει ότι τα πρώτα δύο περιπτώσεις μπορεί να καλύπτεται από βάσης δεδομένων αποτελούν πιστή αναπαράσταση ή AlwaysOn, ότι τρίτη πεζών και κεφαλαίων γραμμάτων μόνο μπορεί να είναι καλύπτονται από την αποστολή αρχείων καταγραφής.

Θα πρέπει να εξισορροπήσετε το πιο σύνθετη ρύθμιση του AlwaysOn, σε σχέση με τη δημιουργία ειδώλου βάσης δεδομένων, με τα πλεονεκτήματα της AlwaysOn. Αυτά τα πλεονεκτήματα μπορεί να εμφανίζεται ως:

*   Αναγνώσιμο δευτερεύοντα αντίγραφα.
*   Δημιουργία αντιγράφων ασφαλείας από το δευτερεύον αντίγραφα.
*   Καλύτερη κλιμάκωση.
*   Περισσότερα από ένα δευτερεύοντα αντίγραφα.

### <a name="9053f720-6f3b-4483-904d-15dc54141e30"></a>Γενικά SQL Server για SAP στη σύνοψη Azure
Υπάρχουν πολλά συστάσεις σε αυτόν τον οδηγό και σας συνιστούμε να διαβάσετε περισσότερες από μία φορές πριν σχεδιασμό του Azure ανάπτυξη. Σε γενικές γραμμές, όμως, θα πρέπει να ακολουθήσετε το επάνω δέκα DBMS γενικά σε Azure συγκεκριμένα σημεία:

[Σχόλιο]: <> μετάδοσης  (2.3 υψηλότερα από τι; Από ένα VHD;)
1. Χρησιμοποιήστε την πιο πρόσφατη έκδοση του DBMS, όπως το SQL Server 2014, η που διαθέτει τα περισσότερα πλεονεκτήματα στο Azure. Για τον SQL Server, αυτή είναι SQL Server 2012 SP1 CU4, η οποία να περιλαμβάνει τη δυνατότητα του τη δημιουργία αντιγράφων ασφαλείας με αποθήκευσης Azure. Ωστόσο, σε συνδυασμό με SAP θα συνιστούμε τουλάχιστον CU1 SP1 2014 του SQL Server ή SQL Server 2012 SP2 και την πιο πρόσφατη CU.
1. Σχεδιασμός προσεκτικά σας Οριζόντιος συστήματος SAP στο Azure να εξισορροπήσετε τη διάταξη αρχείου δεδομένων και Azure περιορισμούς:
    * Δεν έχουν πάρα πολλά VHD, αλλά πρέπει να βεβαιωθείτε ότι μπορείτε να επικοινωνήσετε μαζί σας απαιτείται IOP Προέλευσης.
    * Να θυμάστε ότι IOP Προέλευσης περιορίζονται επίσης ανά λογαριασμού χώρου αποθήκευσης Azure και ότι οι λογαριασμοί χώρου αποθήκευσης είναι περιορισμένη κάθε συνδρομής του Azure ([περισσότερες λεπτομέρειες][azure-subscription-service-limits]). 
    * Μόνο γραμμή κατά μήκος VHD Εάν χρειάζεστε για να επιτύχετε μια μεγαλύτερη ταχύτητα μετάδοσης.
1. Ποτέ εγκαταστήσετε λογισμικό ή να τοποθετήσετε τα αρχεία που απαιτούν διατήρησης στη μονάδα δίσκου D:\ όπως είναι μη μόνιμο και όλα τα στοιχεία σε αυτήν τη μονάδα θα χαθούν στο επανεκκίνηση των Windows.
1. Μην χρησιμοποιείτε Azure VHD σε cache για το χώρο αποθήκευσης τυπική Azure.
1. Μην χρησιμοποιείτε Azure αναπαραχθούν παν λογαριασμούς χώρου αποθήκευσης.  Χρησιμοποιήστε τοπικά πλεονάζοντα για DBMS φόρτους εργασίας.
1. Χρήση του προμηθευτή σας DBMS HA/DR λύσης για την αναπαραγωγή βάσης δεδομένων.
1. Πάντα Χρησιμοποιήστε επίλυση ονομάτων, δεν βασίζονται στις διευθύνσεις IP.
1. Χρησιμοποιήστε την υψηλότερη πιθανές συμπίεση βάσης δεδομένων. Για τον SQL Server αυτή είναι η συμπίεση σελίδας.
1. Να είστε προσεκτικοί όταν χρησιμοποιείτε εικόνες SQL Server από το Azure Marketplace. Εάν χρησιμοποιείτε τον SQL Server μία, πρέπει να αλλάξετε τη συρραφή παρουσία πριν από την εγκατάσταση τυχόν συστήματος SAP NetWeaver σε αυτό.
1. Εγκατάσταση και ρύθμιση παραμέτρων της παρακολούθησης Host SAP για Azure όπως περιγράφεται στο [Οδηγός ανάπτυξης του] [Οδηγός ανάπτυξης].

## <a name="specifics-to-sap-ase-on-windows"></a>Συγκεκριμένα στοιχεία SAP ASE στα Windows
Ξεκινώντας με το Windows Azure, μπορείτε εύκολα να μετεγκαταστήσετε τις υπάρχουσες εφαρμογές SAP ASE σε εικονικές μηχανές Windows Azure. ASE SAP σε μια εικονική μηχανή σας δίνει τη δυνατότητα να μειώσετε το συνολικό κόστος της όσον αφορά την κατοχή ανάπτυξης, διαχείριση και συντήρηση των εφαρμογών πλάτους για μεγάλες επιχειρήσεις, μετεγκατάσταση εύκολα αυτές τις εφαρμογές στο Microsoft Azure. Με το SAP ASE σε έναν υπολογιστή εικονικές Azure, διαχειριστές και τους προγραμματιστές να χρησιμοποιήσετε την ίδια ανάπτυξη και τα εργαλεία διαχείρισης που είναι διαθέσιμες στην εσωτερική εγκατάσταση.

Υπάρχει μια SLA για το εικονικές μηχανές Windows Azure που θα βρείτε εδώ: <https://azure.microsoft.com/support/legal/sla>

Συνεχίζουμε να είστε βέβαιοι ότι μπορεί να διαφέρει Microsoft Azure φιλοξενούμενη εικονικές μηχανές θα εκτελέσει πολύ καλά σε σύγκριση με άλλα δημόσια cloud virtualization προσφορές, αλλά επιμέρους αποτελέσματα. SAP αλλαγή μεγέθους ΧΥΜΟΊ αριθμών από τα διαφορετικά SAP πιστοποιηθεί Εικονική SKU θα παρέχονται σε ένα ξεχωριστό Σημείωση SAP [1928533].

Προτάσεις και συστάσεις όσον αφορά τη χρήση του χώρου αποθήκευσης Azure, ανάπτυξης του SAP ΣΠΣ ή παρακολούθηση SAP ισχύουν για υλοποιήσεις του SAP ASE σε συνδυασμό με των εφαρμογών SAP όπως αναφέρεται σε όλο το τα πρώτα τέσσερα κεφάλαια αυτού του εγγράφου.

### <a name="sap-ase-version-support"></a>Υποστήριξη ASE έκδοση SAP 
Προς το παρόν SAP υποστηρίζει SAP ASE έκδοση 16.0 για χρήση με τα προϊόντα της οικογένειας προγραμμάτων επιχειρήσεις SAP. Όλες τις ενημερώσεις για το SAP ASE server ή προγράμματα οδήγησης JDBC και ODBC για χρήση με τα προϊόντα της οικογένειας προγραμμάτων επιχειρήσεις SAP παρέχονται αποκλειστικά μέσω αγορά υπηρεσιών του SAP στο: <https://support.sap.com/swdc>.

Όπως για εγκαταστάσεις εσωτερικής εγκατάστασης, δεν γίνεται λήψη ενημερώσεων για το διακομιστή SAP ASE ή για τα προγράμματα οδήγησης JDBC και ODBC απευθείας από τοποθεσίες Web Sybase. Για λεπτομερείς πληροφορίες σχετικά με ενημερώσεις κώδικα που υποστηρίζονται για χρήση με την οικογένεια προγραμμάτων επιχειρήσεις SAP προϊόντα εσωτερικής εγκατάστασης και σε εικονικές μηχανές Windows Azure, ανατρέξτε στις παρακάτω σημειώσεις SAP:

* [1590719]
* [1973241]
 
Γενικές πληροφορίες σχετικά με την εκτέλεση οικογένεια επιχειρήσεις SAP σε SAP ASE μπορεί να περιλαμβάνει τη [ΣΆΡΩΣΗ](https://scn.sap.com/community/ase)

### <a name="sap-ase-configuration-guidelines-for-sap-related-sap-ase-installations-in-azure-vms"></a>Οδηγίες ρύθμισης παραμέτρων ASE SAP για SAP που σχετίζονται με εγκαταστάσεις ASE SAP στο ΣΠΣ Azure

#### <a name="structure-of-the-sap-ase-deployment"></a>Δομή της ανάπτυξης ASE SAP
Σύμφωνα με τη γενική περιγραφή, εκτελέσιμα SAP ASE θα πρέπει να βρίσκεται ή να εγκατασταθούν στη μονάδα δίσκου συστήματος της βάσης του Εικονική VHD (μονάδα δίσκου c:\). Συνήθως, οι περισσότερες από τις βάσεις δεδομένων συστήματος και εργαλεία SAP ASE είναι δεν πραγματικά χρησιμοποιηθούν οριστική από φόρτο εργασίας SAP NetWeaver. Ως εκ τούτου του συστήματος και εργαλεία βάσεις δεδομένων (υποδείγματος, μοντέλου, saptools, sybmgmtdb, sybsystemdb) μπορεί να παραμείνει στο καθώς και το C:\drive. 

Εξαίρεση θα μπορούσε να είναι η προσωρινή βάση δεδομένων που περιέχει όλους τους πίνακες εργασίας και προσωρινό πίνακες που δημιουργήθηκαν από ASE SAP, το οποίο σε περίπτωση ορισμένες ERP SAP και όλους τους φόρτους εργασίας εύρους ζώνης που ΈΧΕΙ ενδέχεται να απαιτείται μεγαλύτερου όγκου δεδομένων ή ένταση λειτουργίες εισόδου/εξόδου που δεν χωράει σε την αρχική εικονική Μηχανή βάσης VHD (μονάδα δίσκου c:\).
 
Ανάλογα με την έκδοση του SAPInst/SWPM χρησιμοποιείται για την εγκατάσταση του συστήματος, ενδέχεται να περιέχουν τη βάση δεδομένων:

* Ένα μεμονωμένο tempdb SAP ASE που δημιουργείται κατά την εγκατάσταση του SAP ASE
* Μια tempdb SAP ASE που δημιουργήθηκε από την εγκατάσταση του SAP ASE και μια επιπλέον saptempdb που δημιουργήθηκε από το ρουτίνας εγκατάστασης SAP
* Μια tempdb SAP ASE που δημιουργήθηκε από την εγκατάσταση του SAP ASE και μια επιπλέον tempdb που έχει δημιουργηθεί με μη αυτόματο τρόπο (π.χ. παρακάτω σημείωση SAP [1752266]) ώστε να πληρούνται οι απαιτήσεις εύρους ζώνης που ERP/ΈΧΕΙ συγκεκριμένες tempdb

Σε περίπτωση συγκεκριμένες ERP ή όλους τους φόρτους εργασίας εύρους ζώνης που ΈΧΕΙ νόημα, όσον αφορά την απόδοση, για να διατηρήσετε τις συσκευές tempdb από το επιπλέον που έχουν δημιουργηθεί tempdb (με SWPM ή με μη αυτόματο τρόπο) σε μια μονάδα δίσκου εκτός από το C:\. εάν υπάρχει κανένα επιπλέον tempdb αυτό συνιστάται να δημιουργήσετε ένα (Σημείωση SAP [1752266]).

Για τα συστήματα τα παρακάτω βήματα πρέπει να εκτελεστούν για την επιπλέον που έχουν δημιουργηθεί tempdb:

* Μετακινήστε την πρώτη συσκευή tempdb στην πρώτη συσκευή της βάσης δεδομένων SAP
* Προσθήκη tempdb συσκευές σε κάθε ένα από τα VHD που περιέχει μια συσκευές της βάσης δεδομένων SAP

Αυτή η ρύθμιση παραμέτρων επιτρέπει tempdb για την εκμετάλλευση είτε περισσότερο χώρο από τη μονάδα δίσκου συστήματος είναι σε θέση να παρέχουν. Ως αναφορά μία να ελέγξετε τα μεγέθη συσκευή tempdb στην υπάρχουσα συστήματα που εκτελούν εσωτερικής εγκατάστασης. Ή αυτή η ρύθμιση παραμέτρων θα Ενεργοποίηση IOP Προέλευσης αριθμών σε σχέση με tempdb που δεν είναι δυνατό να παρέχονται με τη μονάδα δίσκου συστήματος. Ξανά, συστήματα που εκτελούν εσωτερικής μπορεί να χρησιμοποιηθεί για την παρακολούθηση της εισόδου/εξόδου φόρτο εργασίας έναντι tempdb.

Τοποθέτηση ποτέ όλες τις συσκευές SAP ASE στον δίσκο D:\ από την εικονική Μηχανή. Αυτό ισχύει επίσης για το tempdb, ακόμα και αν τα αντικείμενα ενήμεροι το tempdb είναι προσωρινές.

#### <a name="impact-of-database-compression"></a>Επίδραση συμπίεσης βάσης δεδομένων
Στις ρυθμίσεις παραμέτρων όπου το εύρος ζώνης εισόδου/εξόδου μπορεί να γίνει περιοριστικό παράγοντα, κάθε μέτρο το οποίο μειώνει IOP Προέλευσης μπορεί να σας βοηθήσουν να απομακρύνετε τις άκρες το φόρτο εργασίας μία είναι δυνατό να εκτελεστεί σε ένα σενάριο IaaS όπως Azure. Επομένως, συνιστάται να βεβαιωθείτε ότι χρησιμοποιείται συμπίεση SAP ASE πριν από την αποστολή μιας υπάρχουσας βάσης δεδομένων SAP σε Azure.

Τις προτάσεις για να εκτελέσετε τη συμπίεση πριν από την αποστολή σε Azure, εάν δεν έχει ήδη υλοποιηθεί δίνεται από διάφορους λόγους:

* Το ποσό των δεδομένων που θα αποσταλεί Azure είναι κάτω
* Η διάρκεια της εκτέλεσης συμπίεσης είναι μικρότερη εάν υποθέσουμε ότι κάποιος να χρησιμοποιήσει ισχυρότερο υλικού με περισσότερες CPU ή νεότερη έκδοση εύρος ζώνης εισόδου/εξόδου ή λιγότερο εισόδου/εξόδου λανθάνων χρόνος εσωτερικής εγκατάστασης
* Μικρότερα μεγέθη βάσης δεδομένων μπορεί να οδηγήσει σε λιγότερο κόστος για εκχώρηση στο δίσκο

Συμπίεση δεδομένων και LOB εργαστείτε σε μια Εικονική φιλοξενούνται σε εικονικές μηχανές Windows Azure, όπως συμβαίνει εσωτερικής εγκατάστασης. Για περισσότερες λεπτομέρειες σχετικά με τον τρόπο για να ελέγξετε εάν συμπίεσης χρησιμοποιείται ήδη σε μια υπάρχουσα βάση δεδομένων SAP ASE Ελέγξτε Σημείωση SAP [1750510].

#### <a name="using-dbacockpit-to-monitor-database-instances"></a>Χρήση DBACockpit για την παρακολούθηση εμφανίσεις βάσης δεδομένων
Για συστήματα SAP που χρησιμοποιείτε SAP ASE ως πλατφόρμα βάσης δεδομένων, το DBACockpit είναι προσβάσιμα ως ενσωματωμένου προγράμματος περιήγησης των windows σε συναλλαγή DBACockpit ή ως Webdynpro. Ωστόσο την πλήρη λειτουργικότητα για την παρακολούθηση και τη διαχείριση της βάσης δεδομένων είναι διαθέσιμη στην εφαρμογή Webdynpro των μόνο το DBACockpit.

Ως με συστήματα εσωτερικής απαιτούνται ορισμένα βήματα για να ενεργοποιήσετε όλες τις λειτουργίες SAP NetWeaver χρησιμοποιούνται από την εφαρμογή Webdynpro από το DBACockpit. Ακολουθήστε Σημείωση SAP [1245200] για να ενεργοποιήσετε τη χρήση των webdynpros και Δημιουργία απαιτούμενων αυτά. Όταν ακολουθώντας τις οδηγίες στις σημειώσεις παραπάνω θα επίσης ρυθμίζετε τις παραμέτρους της διαχείρισης επικοινωνιών Internet (επιλογή) μαζί με τις θύρες που θα χρησιμοποιηθεί για συνδέσεις http και https. Η προεπιλεγμένη ρύθμιση για http μοιάζει κάπως έτσι:

> επιλογή/server_port_0 = PROT = HTTP, ΘΎΡΑ = 8000, PROCTIMEOUT = 600, χρονικού ΟΡΊΟΥ = 600
>
> επιλογή/server_port_1 = PROT = HTTPS, ΘΎΡΑ 443$, PROCTIMEOUT = = 600, χρονικού ΟΡΊΟΥ = 600

και οι συνδέσεις που δημιουργούνται σε συναλλαγή DBACockpit θα είναι παρόμοιο με αυτό:

> https://`<fullyqualifiedhostname`>: sap/44300/bc/sap/webdynpro/dba_cockpit
> 
> http://`<fullyqualifiedhostname`>: sap/8000/bc/sap/webdynpro/dba_cockpit

Ανάλογα με εάν και πώς η εικονική μηχανή Azure φιλοξενίας που βασίζεται στο σύστημα SAP είναι συνδεδεμένο μέσω τοποθεσίας σε τοποθεσία, πολλών τοποθεσίας ή ExpressRoute (ανάπτυξη σταυρό εσωτερικής εγκατάστασης), πρέπει να βεβαιωθείτε ότι αυτή η Επιλογή χρησιμοποιεί ένα πλήρως προσδιορισμένο όνομα κεντρικού υπολογιστή που μπορούν να επιλυθούν στον υπολογιστή όπου που προσπαθείτε να ανοίξετε το DBACockpit από. Ελέγξτε δείτε σημείωση SAP [773830] για να κατανοήσετε τον τρόπο Επιλογή Καθορίζει το όνομα κεντρικού υπολογιστή πλήρως προσδιορισμένη ανάλογα με τις παραμέτρους του προφίλ και ρύθμιση παραμέτρων επιλογή/host_name_full ρητά εάν απαιτείται.

Εάν αναπτύξει η Εικονική σε ένα μόνο στο Cloud σενάριο χωρίς σταυρό εσωτερικής εγκατάστασης συνδεσιμότητας μεταξύ εσωτερικής εγκατάστασης και του Azure, πρέπει να ορίσετε μια δημόσια διεύθυνση IP και μια domainlabel. Η μορφή για το δημόσιο όνομα DNS η Εικονική, στη συνέχεια, θα μοιάζει κάπως έτσι:

> `<custom domainlabel`>. `<azure region`>. cloudapp.azure.com

Μπορείτε να βρείτε περισσότερες λεπτομέρειες σχετικά με το όνομα DNS [εδώ][virtual-machines-azurerm-versus-azuresm].

Ορίζοντας την παράμετρο προφίλ SAP επιλογή/host_name_full ως το όνομα DNS από το Azure Εικονική τη σύνδεση ενδέχεται να μοιάζει με:

> sap/https://mydomainlabel.westeurope.cloudapp.NET:44300/bc/sap/webdynpro/dba_cockpit

> sap/http://mydomainlabel.westeurope.cloudapp.NET:8000/bc/sap/webdynpro/dba_cockpit

Σε αυτήν την περίπτωση, πρέπει να βεβαιωθείτε ότι:

* Προσθήκη κανόνων εισερχομένων στην ομάδα ασφαλείας δικτύου στην πύλη του Azure για τις θύρες TCP/IP που χρησιμοποιούνται για την επικοινωνία με Επιλογή
* Προσθήκη κανόνων εισερχομένων για τη ρύθμιση παραμέτρων του τείχους προστασίας των Windows για τις θύρες TCP/IP που χρησιμοποιούνται για την επικοινωνία με την Επιλογή

Για ένα αυτοματοποιημένο εισαχθούν όλες διορθώσεων διαθέσιμες, συνιστάται να εφαρμόσετε περιοδικά τη συλλογή διόρθωσης SAP σημείωση που ισχύουν για την έκδοση SAP:

* [1558958]
* [1619967]
* [1882376]

Μπορείτε να βρείτε περισσότερες πληροφορίες σχετικά με θέση DBA οδηγού για SAP ASE στις παρακάτω SAP σημειώσεις:

* [1605680]
* [1757924]
* [1757928]
* [1758182]
* [1758496] 
* [1814258]
* [1922555]
* [1956005]

#### <a name="backuprecovery-considerations-for-sap-ase"></a>Δημιουργία αντιγράφων ασφαλείας/ανάκτησης ζητήματα για SAP ASE
Κατά την ανάπτυξη SAP ASE σε Azure μεθοδολογία αντιγράφου ασφαλείας σας, πρέπει να αναθεωρηθούν. Ακόμα και αν το σύστημα δεν είναι παραγωγικοί σύστημα, η βάση δεδομένων SAP που φιλοξενούνται από SAP ASE πρέπει να δημιουργηθούν αντίγραφα περιοδικά. Επειδή το χώρο αποθήκευσης Azure διατηρεί τρεις εικόνες, ένα αντίγραφο ασφαλείας τώρα είναι λιγότερο σημαντικά σε σχέση με παράγωγων σφάλμα χώρου αποθήκευσης. Ο κύριος λόγος για τη διατήρηση ένα κατάλληλο πρόγραμμα δημιουργίας αντιγράφων ασφαλείας και επαναφορά είναι πιο που μπορεί να αποζημιώσει για σφάλματα λογική/μη αυτόματα, παρέχοντας σημείο στο χρόνο αποκατάστασης δυνατότητες. Επομένως, ο στόχος είναι να είτε χρήση δημιουργίας αντιγράφων ασφαλείας για να επαναφέρετε τη βάση δεδομένων σε ένα συγκεκριμένο σημείο σε χρόνο ή για να χρησιμοποιήσετε τη δημιουργία αντιγράφων ασφαλείας στο Azure να σπείρεται άλλο σύστημα, αντιγράφοντας μια υπάρχουσα βάση δεδομένων. Για παράδειγμα, μπορεί να μεταφέρετε από μια ρύθμιση παραμέτρων SAP 2 επιπέδων σε μια ρύθμιση σύστημα 3 επιπέδων του ίδιου συστήματος με την επαναφορά ενός αντιγράφου ασφαλείας.

Δημιουργία αντιγράφων ασφαλείας και επαναφορά μιας βάσης δεδομένων στο Azure λειτουργεί τον ίδιο τρόπο όπως και για εσωτερικής εγκατάστασης. Ανατρέξτε στο θέμα SAP σημειώσεις:

* [1588316]
* [1585981]

Για λεπτομέρειες σχετικά με τη δημιουργία αποτύπωση ρυθμίσεων παραμέτρων και προγραμματισμού δημιουργίας αντιγράφων ασφαλείας. Ανάλογα με τη στρατηγική και μπορείτε να ρυθμίσετε τις ανάγκες βάσης δεδομένων και καταγραφής Αποτυπώνει να δίσκο πάνω σε ένα από τα υπάρχοντα VHD ή να προσθέσετε μια επιπλέον VHD για το αντίγραφο ασφαλείας.  Για να μειώσετε τον κίνδυνο απώλειας δεδομένων σε περίπτωση σφάλμα καλό είναι να χρησιμοποιήσετε έναν VHD όπου βρίσκεται χωρίς συσκευή βάσης δεδομένων.

Εκτός από τα δεδομένα και LOB συμπίεσης SAP ASE προσφέρει επίσης συμπίεσης δημιουργίας αντιγράφων ασφαλείας. Για να καταλάβει λιγότερο χώρο με τη βάση δεδομένων και καταγραφής ενδείξεις συνιστάται να χρησιμοποιήσετε τη συμπίεση δημιουργίας αντιγράφων ασφαλείας. Ανατρέξτε στο θέμα Σημείωση SAP [1588316] για περισσότερες πληροφορίες. Συμπίεση της δημιουργίας αντιγράφων ασφαλείας επίσης είναι κρίσιμης σημασίας για να μειώσετε την ποσότητα των δεδομένων που είναι δυνατό να μεταφερθούν εάν σκοπεύετε να κάνετε λήψη αντιγράφων ασφαλείας ή VHD που περιέχει ενδείξεις δημιουργίας αντιγράφων ασφαλείας από το Azure εικονική μηχανή στην εσωτερική εγκατάσταση.

Μην χρησιμοποιείτε μονάδα δίσκου D:\ ως προορισμό ένδειξης βάσης δεδομένων ή αρχείου καταγραφής.

#### <a name="performance-considerations-for-backupsrestores"></a>Ζητήματα επιδόσεων για δημιουργία αντιγράφων ασφαλείας/επαναφέρει
Όπως στο αναπτύξεις χωρίς λειτουργικό σύστημα, δημιουργία αντιγράφων ασφαλείας/Επαναφορά επιδόσεων εξαρτάται από πόσες όγκους μπορεί να διαβαστεί παράλληλα και της ταχύτητας μετάδοσης των αυτές τις όγκους που μπορεί να είναι. Επιπλέον, την κατανάλωση CPU που χρησιμοποιείται από το αντίγραφο ασφαλείας συμπίεσης, μπορούν να συμβάλλουν σημαντική σε VM με μόνο έως 8 νήματα CPU. Επομένως, μπορεί να λάβει ένα:

* Τα λιγότερα τον αριθμό των VHD χρησιμοποιείται για την αποθήκευση τις συσκευές βάσης δεδομένων, όσο μικρότερο τη συνολική μεταγωγή στην ανάγνωση
* Το μικρότερο τον αριθμό των CPU νήματα σε Εικονική, τα πιο σοβαρά την επίδραση των αντιγράφων ασφαλείας συμπίεσης
* Τους στόχους λιγότερες (γραμμή σε καταλόγους, VHD) για να γράψετε το αντίγραφο ασφαλείας, να το μικρότερο βαθμό τη μετάδοση

Για να αυξήσετε τον αριθμό των προορισμών για να γράψετε να υπάρχουν δύο επιλογές που μπορεί να είναι χρησιμοποιούνται/συνδυασμένες ανάλογα με τις ανάγκες σας:

* Δίσκους της έντασης αντίγραφο ασφαλείας προορισμού μέσω πολλών μονταρισμένο VHD για να βελτιώσετε την ταχύτητα μεταγωγής IOP Προέλευσης που ραβδωτό τόμου
* Δημιουργία ένδειξης ρύθμισης παραμέτρων σε επίπεδο SAP ASE που χρησιμοποιεί περισσότερους από έναν κατάλογο προορισμού για να γράψετε την ένδειξη για να

Δίσκους ενός τόμου μέσω πολλών μονταρισμένο VHD έχει περιγράφονται προηγουμένως σε αυτόν τον οδηγό. Για περισσότερες πληροφορίες σχετικά με τη χρήση πολλών καταλόγων στη ρύθμιση παραμέτρων ένδειξης SAP ASE, ανατρέξτε στην τεκμηρίωση sp_config_dump αποθηκευμένη διαδικασία που χρησιμοποιείται για να δημιουργήσετε τη ρύθμιση των παραμέτρων ένδειξης στην το [Κέντρο πληροφοριών Sybase](http://infocenter.sybase.com/help/index.jsp).

### <a name="disaster-recovery-with-azure-vms"></a>Αποκατάσταση ΣΠΣ Azure

#### <a name="data-replication-with-sap-sybase-replication-server"></a>Αντιγραφή δεδομένων με το διακομιστή αναπαραγωγής Sybase SAP
Με το ASE SAP Server αναπαραγωγής Sybase SAP (SRS) παρέχει μια λύση θερμές αναμονής για να μεταφέρετε συναλλαγές της βάσης δεδομένων σε μια θέση απομακρυσμένοι ασύγχρονα. 

Την εγκατάσταση και τη λειτουργία του SRS λειτουργεί επίσης λειτουργικά σε μια Εικονική φιλοξενείται στις υπηρεσίες εικονική μηχανή Azure όπως συμβαίνει εσωτερικής εγκατάστασης.

HADR ASE μέσω διακομιστή αναπαραγωγής SAP είναι προγραμματισμένη με μελλοντική έκδοση. Θα ελεγχθεί με και θα κυκλοφορήσει για τις πλατφόρμες Windows Azure μόλις είναι διαθέσιμες.

## <a name="specifics-to-sap-ase-on-linux"></a>Συγκεκριμένα στοιχεία ASE SAP σε Linux

Ξεκινώντας με το Windows Azure, μπορείτε εύκολα να μετεγκαταστήσετε τις υπάρχουσες εφαρμογές SAP ASE σε εικονικές μηχανές Windows Azure. ASE SAP σε μια εικονική μηχανή σας δίνει τη δυνατότητα να μειώσετε το συνολικό κόστος της όσον αφορά την κατοχή ανάπτυξης, διαχείριση και συντήρηση των εφαρμογών πλάτους για μεγάλες επιχειρήσεις, μετεγκατάσταση εύκολα αυτές τις εφαρμογές στο Microsoft Azure. Με το SAP ASE σε έναν υπολογιστή εικονικές Azure, διαχειριστές και τους προγραμματιστές να χρησιμοποιήσετε την ίδια ανάπτυξη και τα εργαλεία διαχείρισης που είναι διαθέσιμες στην εσωτερική εγκατάσταση.

Για την ανάπτυξη ΣΠΣ Azure είναι σημαντικό να γνωρίζετε τα επίσημη SLA που θα βρείτε εδώ: <https://azure.microsoft.com/support/legal/sla>

Πληροφορίες SAP αλλαγής μεγέθους και λίστα SAP πιστοποιηθεί Εικονική SKU θα παρέχονται στο SAP Σημείωση [1928533]. Επιπλέον SAP αλλαγής μεγέθους έγγραφα για Azure εικονικές μηχανές θα βρείτε εδώ <http://blogs.msdn.com/b/saponsqlserver/archive/2015/06/19/how-to-size-sap-systems-running-on-azure-vms.aspx> και δείτε <http://blogs.msdn.com/b/saponsqlserver/archive/2015/12/01/new-white-paper-on-sizing-sap-solutions-on-azure-public-cloud.aspx>

Προτάσεις και συστάσεις όσον αφορά τη χρήση του χώρου αποθήκευσης Azure, ανάπτυξης του SAP ΣΠΣ ή παρακολούθηση SAP ισχύουν για υλοποιήσεις του SAP ASE σε συνδυασμό με των εφαρμογών SAP όπως αναφέρεται σε όλο το τα πρώτα τέσσερα κεφάλαια αυτού του εγγράφου.

Οι παρακάτω δύο σημειώσεις SAP περιλαμβάνουν γενικές πληροφορίες σχετικά με ASE Linux και ASE στο Cloud:

* [2134316]
* [1941500]

### <a name="sap-ase-version-support"></a>Υποστήριξη ASE έκδοση SAP 
Προς το παρόν SAP υποστηρίζει SAP ASE έκδοση 16.0 για χρήση με τα προϊόντα της οικογένειας προγραμμάτων επιχειρήσεις SAP. Όλες τις ενημερώσεις για το SAP ASE server ή προγράμματα οδήγησης JDBC και ODBC για χρήση με τα προϊόντα της οικογένειας προγραμμάτων επιχειρήσεις SAP παρέχονται αποκλειστικά μέσω αγορά υπηρεσιών του SAP στο: <https://support.sap.com/swdc>.

Όπως για εγκαταστάσεις εσωτερικής εγκατάστασης, δεν γίνεται λήψη ενημερώσεων για το διακομιστή SAP ASE ή για τα προγράμματα οδήγησης JDBC και ODBC απευθείας από τοποθεσίες Web Sybase. Για λεπτομερείς πληροφορίες σχετικά με ενημερώσεις κώδικα που υποστηρίζονται για χρήση με την οικογένεια προγραμμάτων επιχειρήσεις SAP προϊόντα εσωτερικής εγκατάστασης και σε εικονικές μηχανές Windows Azure, ανατρέξτε στις παρακάτω σημειώσεις SAP:

* [1590719]
* [1973241]
 
Γενικές πληροφορίες σχετικά με την εκτέλεση οικογένεια επιχειρήσεις SAP σε SAP ASE μπορεί να περιλαμβάνει τη [ΣΆΡΩΣΗ](https://scn.sap.com/community/ase)

### <a name="sap-ase-configuration-guidelines-for-sap-related-sap-ase-installations-in-azure-vms"></a>Οδηγίες ρύθμισης παραμέτρων ASE SAP για SAP που σχετίζονται με εγκαταστάσεις ASE SAP στο ΣΠΣ Azure

#### <a name="structure-of-the-sap-ase-deployment"></a>Δομή της ανάπτυξης ASE SAP
Σύμφωνα με τη γενική περιγραφή, εκτελέσιμα SAP ASE πρέπει να είναι βρίσκεται ή να εγκατασταθεί στο σύστημα αρχείων ριζικό κατάλογο της την εικονική Μηχανή (/sybase). Συνήθως, οι περισσότερες από τις βάσεις δεδομένων συστήματος και εργαλεία SAP ASE είναι δεν πραγματικά χρησιμοποιηθούν οριστική από φόρτο εργασίας SAP NetWeaver. Ως εκ τούτου του συστήματος και εργαλεία βάσεις δεδομένων (υποδείγματος, μοντέλου, saptools, sybmgmtdb, sybsystemdb) μπορεί να παραμείνει στο σύστημα αρχείων ριζικό καθώς και. 

Εξαίρεση μπορεί να είναι η προσωρινή βάση δεδομένων που περιέχει όλους τους πίνακες εργασίας και προσωρινό πίνακες που δημιουργήθηκαν από ASE SAP, το οποίο ενδέχεται να απαιτείται η υψηλότερη ένταση δεδομένων ή ένταση λειτουργίες εισόδου/εξόδου που δεν χωράει σε δίσκο OS την αρχική εικονική Μηχανή σε περίπτωση ορισμένες ERP SAP και όλους τους φόρτους εργασίας εύρους ζώνης που ΈΧΕΙ.
 
Ανάλογα με την έκδοση του SAPInst/SWPM χρησιμοποιείται για την εγκατάσταση του συστήματος, ενδέχεται να περιέχουν τη βάση δεδομένων:

* Ένα μεμονωμένο tempdb SAP ASE που δημιουργείται κατά την εγκατάσταση του SAP ASE
* Μια tempdb SAP ASE που δημιουργήθηκε από την εγκατάσταση του SAP ASE και μια επιπλέον saptempdb που δημιουργήθηκε από το ρουτίνας εγκατάστασης SAP
* Μια tempdb SAP ASE που δημιουργήθηκε από την εγκατάσταση του SAP ASE και μια επιπλέον tempdb που έχει δημιουργηθεί με μη αυτόματο τρόπο (π.χ. παρακάτω σημείωση SAP [1752266]) ώστε να πληρούνται οι απαιτήσεις εύρους ζώνης που ERP/ΈΧΕΙ συγκεκριμένες tempdb

Σε περίπτωση συγκεκριμένες ERP ή όλους τους φόρτους εργασίας εύρους ζώνης που ΈΧΕΙ νόημα, όσον αφορά την απόδοση, για να διατηρήσετε τις συσκευές tempdb από το επιπλέον που έχουν δημιουργηθεί tempdb (με SWPM ή με μη αυτόματο τρόπο) σε ένα ξεχωριστό αρχείο στο σύστημα που μπορεί να αντιπροσωπεύεται από ένα δίσκο δεδομένων μονής ακρίβειας Azure ή μια RAID Linux που εκτείνονται σε πολλές δίσκων Azure δεδομένων. Εάν υπάρχει κανένα επιπλέον tempdb το συνιστάται να δημιουργήσετε ένα (Σημείωση SAP [1752266]).

Για τα συστήματα τα παρακάτω βήματα πρέπει να εκτελεστούν για την επιπλέον που έχουν δημιουργηθεί tempdb:

* Μετακίνηση στον πρώτο κατάλογο tempdb στο πρώτο σύστημα αρχείων της βάσης δεδομένων SAP
* Προσθήκη σε καταλόγους tempdb σε κάθε ένα από τα VHD που περιέχει ένα σύστημα αρχείων της βάσης δεδομένων SAP

Αυτή η ρύθμιση παραμέτρων επιτρέπει tempdb για την εκμετάλλευση είτε περισσότερο χώρο από τη μονάδα δίσκου συστήματος είναι σε θέση να παρέχουν. Ως αναφορά μία να ελέγξετε τα μεγέθη καταλόγου tempdb στην υπάρχουσα συστήματα που εκτελούν εσωτερικής εγκατάστασης. Ή αυτή η ρύθμιση παραμέτρων θα Ενεργοποίηση IOP Προέλευσης αριθμών σε σχέση με tempdb που δεν είναι δυνατό να παρέχονται με τη μονάδα δίσκου συστήματος. Ξανά, συστήματα που εκτελούν εσωτερικής μπορεί να χρησιμοποιηθεί για την παρακολούθηση της εισόδου/εξόδου φόρτο εργασίας έναντι tempdb.

Τοποθετήστε ποτέ τους καταλόγους SAP ASE στη /mnt ή /mnt/resource από την εικονική Μηχανή. Αυτό ισχύει επίσης για το tempdb, ακόμα και αν τα αντικείμενα ενήμεροι το tempdb μόνο είναι προσωρινά, επειδή /mnt ή /mnt/resource είναι προεπιλεγμένη Εικονική Azure temp χώρο που δεν είναι μόνιμη. Μπορείτε να βρείτε περισσότερες λεπτομέρειες σχετικά με το χώρο temp Εικονική Azure σε [αυτό το άρθρο][virtual-machines-linux-how-to-attach-disk]

#### <a name="impact-of-database-compression"></a>Επίδραση συμπίεσης βάσης δεδομένων
Στις ρυθμίσεις παραμέτρων όπου το εύρος ζώνης εισόδου/εξόδου μπορεί να γίνει περιοριστικό παράγοντα, κάθε μέτρο το οποίο μειώνει IOP Προέλευσης μπορεί να σας βοηθήσουν να απομακρύνετε τις άκρες το φόρτο εργασίας μία είναι δυνατό να εκτελεστεί σε ένα σενάριο IaaS όπως Azure. Επομένως, συνιστάται να βεβαιωθείτε ότι χρησιμοποιείται συμπίεση SAP ASE πριν από την αποστολή μιας υπάρχουσας βάσης δεδομένων SAP σε Azure.

Τις προτάσεις για να εκτελέσετε τη συμπίεση πριν από την αποστολή σε Azure, εάν δεν έχει ήδη υλοποιηθεί δίνεται από διάφορους λόγους:

* Το ποσό των δεδομένων που θα αποσταλεί Azure είναι κάτω
* Η διάρκεια της εκτέλεσης συμπίεσης είναι μικρότερη εάν υποθέσουμε ότι κάποιος να χρησιμοποιήσει ισχυρότερο υλικού με περισσότερες CPU ή νεότερη έκδοση εύρος ζώνης εισόδου/εξόδου ή λιγότερο εισόδου/εξόδου λανθάνων χρόνος εσωτερικής εγκατάστασης
* Μικρότερα μεγέθη βάσης δεδομένων μπορεί να οδηγήσει σε λιγότερο κόστος για εκχώρηση στο δίσκο

Συμπίεση δεδομένων και LOB εργαστείτε σε μια Εικονική φιλοξενούνται σε εικονικές μηχανές Windows Azure, όπως συμβαίνει εσωτερικής εγκατάστασης. Για περισσότερες λεπτομέρειες σχετικά με τον τρόπο για να ελέγξετε εάν συμπίεσης χρησιμοποιείται ήδη σε μια υπάρχουσα βάση δεδομένων SAP ASE Ελέγξτε Σημείωση SAP [1750510]. Δείτε επίσης Σημείωση SAP [2121797] για πρόσθετες πληροφορίες σχετικά με τη συμπίεση της βάσης δεδομένων.

#### <a name="using-dbacockpit-to-monitor-database-instances"></a>Χρήση DBACockpit για την παρακολούθηση εμφανίσεις βάσης δεδομένων
Για συστήματα SAP που χρησιμοποιείτε SAP ASE ως πλατφόρμα βάσης δεδομένων, το DBACockpit είναι προσβάσιμα ως ενσωματωμένου προγράμματος περιήγησης των windows σε συναλλαγή DBACockpit ή ως Webdynpro. Ωστόσο την πλήρη λειτουργικότητα για την παρακολούθηση και τη διαχείριση της βάσης δεδομένων είναι διαθέσιμη στην εφαρμογή Webdynpro των μόνο το DBACockpit.

Ως με συστήματα εσωτερικής απαιτούνται ορισμένα βήματα για να ενεργοποιήσετε όλες τις λειτουργίες SAP NetWeaver χρησιμοποιούνται από την εφαρμογή Webdynpro από το DBACockpit. Ακολουθήστε Σημείωση SAP [1245200] για να ενεργοποιήσετε τη χρήση των webdynpros και Δημιουργία απαιτούμενων αυτά. Όταν ακολουθώντας τις οδηγίες στις σημειώσεις παραπάνω θα επίσης ρυθμίζετε τις παραμέτρους της διαχείρισης επικοινωνιών Internet (επιλογή) μαζί με τις θύρες που θα χρησιμοποιηθεί για συνδέσεις http και https. Η προεπιλεγμένη ρύθμιση για http μοιάζει κάπως έτσι:

> επιλογή/server_port_0 = PROT = HTTP, ΘΎΡΑ = 8000, PROCTIMEOUT = 600, χρονικού ΟΡΊΟΥ = 600
>
> επιλογή/server_port_1 = PROT = HTTPS, ΘΎΡΑ 443$, PROCTIMEOUT = = 600, χρονικού ΟΡΊΟΥ = 600

και οι συνδέσεις που δημιουργούνται σε συναλλαγή DBACockpit θα είναι παρόμοιο με αυτό:

> https://`<fullyqualifiedhostname`>: sap/44300/bc/sap/webdynpro/dba_cockpit
> 
> http://`<fullyqualifiedhostname`>: sap/8000/bc/sap/webdynpro/dba_cockpit

Ανάλογα με εάν και πώς η εικονική μηχανή Azure φιλοξενίας που βασίζεται στο σύστημα SAP είναι συνδεδεμένο μέσω τοποθεσίας σε τοποθεσία, πολλών τοποθεσίας ή ExpressRoute (ανάπτυξη σταυρό εσωτερικής εγκατάστασης), πρέπει να βεβαιωθείτε ότι αυτή η Επιλογή χρησιμοποιεί ένα πλήρως προσδιορισμένο όνομα κεντρικού υπολογιστή που μπορούν να επιλυθούν στον υπολογιστή όπου που προσπαθείτε να ανοίξετε το DBACockpit από. Ελέγξτε δείτε σημείωση SAP [773830] για να κατανοήσετε τον τρόπο Επιλογή Καθορίζει το όνομα κεντρικού υπολογιστή πλήρως προσδιορισμένη ανάλογα με τις παραμέτρους του προφίλ και ρύθμιση παραμέτρων επιλογή/host_name_full ρητά εάν απαιτείται.

Εάν αναπτύξει η Εικονική σε ένα μόνο στο Cloud σενάριο χωρίς σταυρό εσωτερικής εγκατάστασης συνδεσιμότητας μεταξύ εσωτερικής εγκατάστασης και του Azure, πρέπει να ορίσετε μια δημόσια διεύθυνση IP και μια domainlabel. Η μορφή για το δημόσιο όνομα DNS η Εικονική, στη συνέχεια, θα μοιάζει κάπως έτσι:

> `<custom domainlabel`>. `<azure region`>. cloudapp.azure.com

Μπορείτε να βρείτε περισσότερες λεπτομέρειες σχετικά με το όνομα DNS [εδώ][virtual-machines-azurerm-versus-azuresm].

Ορίζοντας την παράμετρο προφίλ SAP επιλογή/host_name_full ως το όνομα DNS από το Azure Εικονική τη σύνδεση ενδέχεται να μοιάζει με:

> sap/https://mydomainlabel.westeurope.cloudapp.NET:44300/bc/sap/webdynpro/dba_cockpit
> 
> sap/http://mydomainlabel.westeurope.cloudapp.NET:8000/bc/sap/webdynpro/dba_cockpit

Σε αυτήν την περίπτωση, πρέπει να βεβαιωθείτε ότι:

* Προσθήκη κανόνων εισερχομένων στην ομάδα ασφαλείας δικτύου στην πύλη του Azure για τις θύρες TCP/IP που χρησιμοποιούνται για την επικοινωνία με Επιλογή
* Προσθήκη κανόνων εισερχομένων για τη ρύθμιση παραμέτρων του τείχους προστασίας των Windows για τις θύρες TCP/IP που χρησιμοποιούνται για την επικοινωνία με την Επιλογή

Για ένα αυτοματοποιημένο εισαχθούν όλες διορθώσεων διαθέσιμες, συνιστάται να εφαρμόσετε περιοδικά τη συλλογή διόρθωσης SAP σημείωση που ισχύουν για την έκδοση SAP:

* [1558958]
* [1619967]
* [1882376]

Μπορείτε να βρείτε περισσότερες πληροφορίες σχετικά με θέση DBA οδηγού για SAP ASE στις παρακάτω SAP σημειώσεις:

* [1605680]
* [1757924]
* [1757928]
* [1758182]
* [1758496] 
* [1814258]
* [1922555]
* [1956005]

#### <a name="backuprecovery-considerations-for-sap-ase"></a>Δημιουργία αντιγράφων ασφαλείας/ανάκτησης ζητήματα για SAP ASE
Κατά την ανάπτυξη SAP ASE σε Azure μεθοδολογία αντιγράφου ασφαλείας σας, πρέπει να αναθεωρηθούν. Ακόμα και αν το σύστημα δεν είναι παραγωγικοί σύστημα, η βάση δεδομένων SAP που φιλοξενούνται από SAP ASE πρέπει να δημιουργηθούν αντίγραφα περιοδικά. Επειδή το χώρο αποθήκευσης Azure διατηρεί τρεις εικόνες, ένα αντίγραφο ασφαλείας τώρα είναι λιγότερο σημαντικά σε σχέση με παράγωγων σφάλμα χώρου αποθήκευσης. Ο κύριος λόγος για τη διατήρηση ένα κατάλληλο πρόγραμμα δημιουργίας αντιγράφων ασφαλείας και επαναφορά είναι πιο που μπορεί να αποζημιώσει για σφάλματα λογική/μη αυτόματα, παρέχοντας σημείο στο χρόνο αποκατάστασης δυνατότητες. Επομένως, ο στόχος είναι να είτε χρήση δημιουργίας αντιγράφων ασφαλείας για να επαναφέρετε τη βάση δεδομένων σε ένα συγκεκριμένο σημείο σε χρόνο ή για να χρησιμοποιήσετε τη δημιουργία αντιγράφων ασφαλείας στο Azure να σπείρεται άλλο σύστημα, αντιγράφοντας μια υπάρχουσα βάση δεδομένων. Για παράδειγμα, μπορεί να μεταφέρετε από μια ρύθμιση παραμέτρων SAP 2 επιπέδων σε μια ρύθμιση σύστημα 3 επιπέδων του ίδιου συστήματος με την επαναφορά ενός αντιγράφου ασφαλείας.

Δημιουργία αντιγράφων ασφαλείας και επαναφορά μιας βάσης δεδομένων στο Azure λειτουργεί τον ίδιο τρόπο όπως και για εσωτερικής εγκατάστασης. Ανατρέξτε στο θέμα SAP σημειώσεις:

* [1588316]
* [1585981]

Για λεπτομέρειες σχετικά με τη δημιουργία αποτύπωση ρυθμίσεων παραμέτρων και προγραμματισμού δημιουργίας αντιγράφων ασφαλείας. Ανάλογα με τη στρατηγική και μπορείτε να ρυθμίσετε τις ανάγκες βάσης δεδομένων και καταγραφής Αποτυπώνει να δίσκο πάνω σε ένα από τα υπάρχοντα VHD ή να προσθέσετε μια επιπλέον VHD για το αντίγραφο ασφαλείας.  Για να μειώσετε τον κίνδυνο απώλειας δεδομένων σε περίπτωση σφάλμα καλό είναι να χρησιμοποιήσετε έναν VHD όπου βρίσκεται το χωρίς καταλόγου/αρχείου βάσης δεδομένων.

Εκτός από τα δεδομένα και LOB συμπίεσης SAP ASE προσφέρει επίσης συμπίεσης δημιουργίας αντιγράφων ασφαλείας. Για να καταλάβει λιγότερο χώρο με τη βάση δεδομένων και καταγραφής ενδείξεις συνιστάται να χρησιμοποιήσετε τη συμπίεση δημιουργίας αντιγράφων ασφαλείας. Ανατρέξτε στο θέμα Σημείωση SAP [1588316] για περισσότερες πληροφορίες. Συμπίεση της δημιουργίας αντιγράφων ασφαλείας επίσης είναι κρίσιμης σημασίας για να μειώσετε την ποσότητα των δεδομένων που είναι δυνατό να μεταφερθούν εάν σκοπεύετε να κάνετε λήψη αντιγράφων ασφαλείας ή VHD που περιέχει ενδείξεις δημιουργίας αντιγράφων ασφαλείας από το Azure εικονική μηχανή στην εσωτερική εγκατάσταση.

Δεν χρησιμοποιείτε το Azure Εικονική temp χώρο /mnt ή /mnt/resource ως προορισμό ένδειξης βάσης δεδομένων ή αρχείου καταγραφής.

#### <a name="performance-considerations-for-backupsrestores"></a>Ζητήματα επιδόσεων για δημιουργία αντιγράφων ασφαλείας/επαναφέρει
Όπως στο αναπτύξεις χωρίς λειτουργικό σύστημα, δημιουργία αντιγράφων ασφαλείας/Επαναφορά επιδόσεων εξαρτάται από πόσες όγκους μπορεί να διαβαστεί παράλληλα και της ταχύτητας μετάδοσης των αυτές τις όγκους που μπορεί να είναι. Επιπλέον, την κατανάλωση CPU που χρησιμοποιείται από το αντίγραφο ασφαλείας συμπίεσης, μπορούν να συμβάλλουν σημαντική σε VM με μόνο έως 8 νήματα CPU. Επομένως, μπορεί να λάβει ένα:

* Τα λιγότερα τον αριθμό των VHD χρησιμοποιείται για την αποθήκευση τις συσκευές βάσης δεδομένων, όσο μικρότερο τη συνολική μεταγωγή στην ανάγνωση
* Το μικρότερο τον αριθμό των CPU νήματα σε Εικονική, τα πιο σοβαρά την επίδραση των αντιγράφων ασφαλείας συμπίεσης
* Λιγότερες προορισμών (Linux λογισμικό RAID, VHD) για να γράψετε το αντίγραφο ασφαλείας, να το μικρότερο βαθμό τη μετάδοση

Για να αυξήσετε τον αριθμό των προορισμών για να γράψετε να υπάρχουν δύο επιλογές που μπορεί να είναι χρησιμοποιούνται/συνδυασμένες ανάλογα με τις ανάγκες σας:

* Δίσκους της έντασης αντίγραφο ασφαλείας προορισμού μέσω πολλών μονταρισμένο VHD για να βελτιώσετε την ταχύτητα μεταγωγής IOP Προέλευσης που ραβδωτό τόμου
* Δημιουργία ένδειξης ρύθμισης παραμέτρων σε επίπεδο SAP ASE που χρησιμοποιεί περισσότερους από έναν κατάλογο προορισμού για να γράψετε την ένδειξη για να

Δίσκους ενός τόμου μέσω πολλών μονταρισμένο VHD έχει περιγράφονται προηγουμένως σε αυτόν τον οδηγό. Για περισσότερες πληροφορίες σχετικά με τη χρήση πολλών καταλόγων στη ρύθμιση παραμέτρων ένδειξης SAP ASE, ανατρέξτε στην τεκμηρίωση sp_config_dump αποθηκευμένη διαδικασία που χρησιμοποιείται για να δημιουργήσετε τη ρύθμιση των παραμέτρων ένδειξης στην το [Κέντρο πληροφοριών Sybase](http://infocenter.sybase.com/help/index.jsp).

### <a name="disaster-recovery-with-azure-vms"></a>Αποκατάσταση ΣΠΣ Azure

#### <a name="data-replication-with-sap-sybase-replication-server"></a>Αντιγραφή δεδομένων με το διακομιστή αναπαραγωγής Sybase SAP
Με το ASE SAP Server αναπαραγωγής Sybase SAP (SRS) παρέχει μια λύση θερμές αναμονής για να μεταφέρετε συναλλαγές της βάσης δεδομένων σε μια θέση απομακρυσμένοι ασύγχρονα. 

Την εγκατάσταση και τη λειτουργία του SRS λειτουργεί επίσης λειτουργικά σε μια Εικονική φιλοξενείται στις υπηρεσίες εικονική μηχανή Azure όπως συμβαίνει εσωτερικής εγκατάστασης.

HADR ASE μέσω SAP Server αναπαραγωγή δεν υποστηρίζεται σε αυτό το σημείο στο φορά. Μπορεί να ελεγχθεί με και κυκλοφορήσει για τις πλατφόρμες Windows Azure στο μέλλον.

## <a name="specifics-to-oracle-database-on-windows"></a>Λεπτομέρειες σχετικά με τη βάση δεδομένων Oracle στα Windows
Από την έναρξη της midyear 2013, λογισμικό Oracle υποστηρίζεται Oracle για να εκτελέσετε στο Microsoft Windows Hyper-V και Azure. Διαβάστε αυτό το άρθρο για να δείτε περισσότερες λεπτομέρειες σχετικά με τα γενικά υποστήριξη των Windows Hyper-V και Azure από Oracle: <https://blogs.oracle.com/cloud/entry/oracle_and_microsoft_join_forces> 

Μετά τη γενική υποστήριξη, υποστηρίζεται καθώς και το συγκεκριμένο σενάριο των εφαρμογών SAP αξιοποίηση βάσεις δεδομένων της Oracle. Λεπτομέρειες ονομάζονται σε αυτό το τμήμα του εγγράφου.

### <a name="oracle-version-support"></a>Υποστήριξη έκδοση Oracle
Όλες τις λεπτομέρειες σχετικά με τις εκδόσεις Oracle και αντίστοιχες εκδόσεις του λειτουργικού Συστήματος που υποστηρίζονται για την εκτέλεση SAP σε Oracle σε εικονικές μηχανές Windows Azure μπορεί να περιλαμβάνει τα παρακάτω σημείωση SAP [2039619]

Γενικές πληροφορίες σχετικά με την εκτέλεση οικογένεια επιχειρήσεις SAP στην Oracle μπορεί να βρεθεί στην ΣΆΡΩΣΗ: <https://scn.sap.com/community/oracle>

### <a name="oracle-configuration-guidelines-for-sap-installations-in-azure-vms"></a>Οδηγίες ρύθμισης παραμέτρων Oracle για τις εγκαταστάσεις SAP στο ΣΠΣ Azure

#### <a name="storage-configuration"></a>Ρύθμιση παραμέτρων αποθήκευσης
Μόνο μία παρουσία υποστηρίζεται Oracle χρήση δίσκων διαμόρφωση NTFS. Όλα τα αρχεία βάσης δεδομένων πρέπει να αποθηκεύονται στο σύστημα αρχείων NTFS βάση δίσκων VHD. Αυτές οι VHD είναι ενεργοποιημένες για το Εικονική Azure και βασίζονται σε χώρος αποθήκευσης αντικειμένων BLOB του Azure σελίδας (<https://msdn.microsoft.com/library/azure/ee691964.aspx>). Οποιοδήποτε είδος μονάδες δίσκου δικτύου ή απομακρυσμένα κοινόχρηστα στοιχεία όπως υπηρεσιών Azure αρχείων:
 
* <https://blogs.MSDN.com/b/windowsazurestorage/Archive/2014/05/12/introducing-Microsoft-Azure-File-Service.aspx> 
* <https://blogs.MSDN.com/b/windowsazurestorage/Archive/2014/05/27/persisting-Connections-To-Microsoft-Azure-Files.aspx>
 
**δεν** υποστηρίζονται για αρχεία βάσης δεδομένων της Oracle!

Χρήση των VHD Azure που βασίζονται σε χώρο αποθήκευσης BLOB του Azure σελίδας, οι δηλώσεις που γίνονται σε αυτό το έγγραφο στο κεφάλαιο [προσωρινή αποθήκευση για ΣΠΣ και VHD] [dbms-Οδηγός-2.1] και [χώρος αποθήκευσης Microsoft Azure] [dbms-Οδηγός-2.3] ισχύουν για αναπτύξεις με τη βάση δεδομένων Oracle καθώς και.

Όπως περιγράφεται παραπάνω σε το γενικό μέρος του εγγράφου, υπάρχει ορίων στις μετάδοσης IOP Προέλευσης για VHD Azure. Τα ακριβή ορίων ανάλογα με τον τύπο Εικονική χρησιμοποιούνται. Μπορείτε να βρείτε μια λίστα των τύπων Εικονική με τους στόχους πωλήσεων [εδώ][virtual-machines-sizes]

Για να προσδιορίσετε το υποστηριζόμενοι τύποι Εικονική Azure, ανατρέξτε στην σημείωση SAP [1928533]

Με την προϋπόθεση ότι το τρέχον όριο IOP Προέλευσης ανά δίσκο πληροί τις απαιτήσεις, είναι δυνατή η αποθήκευση όλων των αρχείων DB σε μία μεμονωμένη VHD μονταρισμένο Azure. 

Εάν απαιτούνται περισσότερες IOP Προέλευσης, συνιστάται να χρησιμοποιήσετε το παράθυρο χώρους συγκέντρωσης χώρου αποθήκευσης (μόνο διαθέσιμες στο Windows Server 2012 και νεότερη έκδοση) ή επιμερισμό των Windows για Windows 2008 R2 για να δημιουργήσετε ένα μεγάλο λογική συσκευή μέσω πολλών μονταρισμένο δίσκων VHD. Δείτε επίσης το κεφάλαιο [λογισμικό RAID] [dbms-Οδηγός-2.2] αυτού του εγγράφου. Αυτή η προσέγγιση απλοποιεί την επιβάρυνσης διαχείρισης για να διαχειριστείτε το χώρο στο δίσκο και αποφεύγεται η προσπάθεια για τη διανομή με μη αυτόματο τρόπο αρχείων σε πολλές μονταρισμένο VHD.

#### <a name="backup--restore"></a>Δημιουργία αντιγράφων ασφαλείας / Επαναφορά
Για δημιουργία αντιγράφων ασφαλείας / επαναφέρετε τη λειτουργικότητα, το BR SAP * υποστηρίζονται εργαλεία για Oracle με τον ίδιο τρόπο όπως σε τυπική λειτουργικά συστήματα Windows Server και το Hyper-V. Διαχείριση αποκατάστασης Oracle (RMAN) υποστηρίζεται επίσης για δημιουργία αντιγράφων ασφαλείας στο δίσκο και επαναφορά από δίσκο.

#### <a name="high-availability"></a>Υψηλή διαθεσιμότητα
[Σχόλιο]: <>  (σύνδεση αναφέρεται σε ASM)
Προστασία δεδομένων Oracle υποστηρίζεται για σκοπούς αποκατάστασης από καταστροφή και υψηλή διαθεσιμότητα. Μπορείτε να βρείτε λεπτομέρειες στο [αυτό] [ virtual-machines-windows-classic-configure-oracle-data-guard] τεκμηρίωση.

#### <a name="other"></a>Άλλες
Άλλες γενικά θέματα όπως σύνολα διαθεσιμότητα Azure ή SAP παρακολούθηση εφαρμογή όπως περιγράφεται στο τα πρώτα τρία κεφάλαια αυτού του εγγράφου για υλοποιήσεις του ΣΠΣ με καθώς και τη βάση δεδομένων Oracle.

## <a name="specifics-for-the-sap-maxdb-database-on-windows"></a>Συγκεκριμένες απαιτήσεις για τη βάση δεδομένων MaxDB SAP στα Windows

### <a name="sap-maxdb-version-support"></a>Υποστήριξη MaxDB έκδοση SAP
Προς το παρόν SAP υποστηρίζει SAP MaxDB έκδοση 7.9 για χρήση με προϊόντα στο Azure που βασίζονται σε SAP NetWeaver. Όλες τις ενημερώσεις για το SAP MaxDB server ή προγράμματα οδήγησης JDBC και ODBC για χρήση με προϊόντα που βασίζονται σε SAP NetWeaver παρέχονται αποκλειστικά μέσω το Marketplace υπηρεσίας SAP στο <https://support.sap.com/swdc>.
Γενικές πληροφορίες σχετικά με την εκτέλεση SAP NetWeaver σε SAP MaxDB μπορείτε να βρείτε στο <https://scn.sap.com/community/maxdb>.

### <a name="supported-microsoft-windows-versions-and-azure-vm-types-for-sap-maxdb-dbms"></a>Υποστηριζόμενοι τύποι Microsoft Windows εκδόσεις και Εικονική Azure για SAP MaxDB DBMS
Για να βρείτε την υποστηριζόμενη έκδοση των Microsoft Windows για DBMS MaxDB SAP στο Azure, ανατρέξτε στα θέματα:

* [Πίνακας Διαθεσιμότητα προϊόντος SAP (PAM)][sap-pam]
* Σημείωση SAP [1928533]

Συνιστάται ιδιαίτερα να χρησιμοποιείτε την πιο πρόσφατη έκδοση του λειτουργικού συστήματος των Microsoft Windows, η οποία είναι Microsoft Windows 2012 R2.

### <a name="available-sap-maxdb-documentation"></a>Τεκμηρίωση MaxDB διαθέσιμη SAP
Μπορείτε να βρείτε την ενημερωμένη λίστα τεκμηρίωση SAP MaxDB σε την παρακάτω σημείωση SAP [767598]
    
### <a name="sap-maxdb-configuration-guidelines-for-sap-installations-in-azure-vms"></a>Οδηγίες ρύθμισης παραμέτρων MaxDB SAP για τις εγκαταστάσεις SAP στο ΣΠΣ Azure

#### <a name="b48cfe3b-48e9-4f5b-a783-1d29155bd573"></a>Ρύθμιση παραμέτρων αποθήκευσης
Βέλτιστες πρακτικές Azure χώρου αποθήκευσης για SAP MaxDB ακολουθήσετε τις γενικές συστάσεις που αναφέρεται στο κεφάλαιο [δομή μιας ανάπτυξης RDBMS] [dbms-Οδηγός-2].

> [AZURE.IMPORTANT] Άλλες βάσεις δεδομένων, όπως το SAP MaxDB έχει επίσης δεδομένων και τα αρχεία καταγραφής. Ωστόσο, στο SAP MaxDB ορολογία το σωστό όρο είναι "Ένταση" (δεν "αρχείο"). Για παράδειγμα, υπάρχουν SAP MaxDB όγκους δεδομένων και όγκους καταγραφής. Μην συγχέετε αυτά με το λειτουργικό σύστημα δίσκου όγκους. 

Με λίγα λόγια πρέπει:

* Ορίστε το λογαριασμό Azure χώρου αποθήκευσης που περιέχει το SAP MaxDB δεδομένων και καταγραφής όγκους (δηλαδή αρχεία) σε **Τοπική πλεονάζοντα χώρο αποθήκευσης (LRS)** ως καθορισμένο στο κεφάλαιο [χώρος αποθήκευσης Microsoft Azure] [dbms-Οδηγός-2.3].
* Διαχωρίστε τη διαδρομή εισόδου/ΕΞΌΔΟΥ για SAP MaxDB όγκους δεδομένων (δηλαδή αρχεία) από τη διαδρομή εισόδου/ΕΞΌΔΟΥ για το αρχείο καταγραφής όγκους (δηλαδή αρχεία). Αυτό σημαίνει ότι SAP MaxDB όγκους δεδομένων (δηλαδή αρχεία) πρέπει να έχει εγκατασταθεί σε μία λογική μονάδα δίσκου και όγκους καταγραφής SAP MaxDB (δηλαδή αρχεία) πρέπει να έχει εγκατασταθεί σε μια άλλη λογική μονάδα δίσκου.
* Ορίστε το σωστό αρχείο σε cache για κάθε αντικειμένων blob του Azure, ανάλογα με το εάν χρησιμοποιείτε για SAP MaxDB δεδομένων ή το αρχείο καταγραφής όγκους (δηλαδή αρχεία) και, εάν χρησιμοποιείτε το τυπικό Azure ή Azure Premium χώρο αποθήκευσης, όπως περιγράφεται στο κεφάλαιο [προσωρινή αποθήκευση για ΣΠΣ] [dbms-Οδηγός-2.1].
* Με την προϋπόθεση ότι το τρέχον όριο IOP Προέλευσης ανά δίσκο πληροί τις απαιτήσεις, είναι δυνατή η αποθήκευση όλα τα όγκους δεδομένων σε ένα μεμονωμένο μονταρισμένο VHD Azure και επίσης αποθήκευση όλων όγκους καταγραφής βάσης δεδομένων σε μια άλλη μία VHD μονταρισμένο Azure.
* Εάν απαιτούνται περισσότερες IOP Προέλευσης ή/και το διάστημα, συνιστάται να χρησιμοποιήσετε το Microsoft το παράθυρο του χώρου αποθήκευσης χώρους συγκέντρωσης (μόνο διαθέσιμη στο Microsoft Windows Server 2012 και νεότερη έκδοση) ή επιμερισμό των Microsoft Windows για το Microsoft Windows 2008 R2 για τη δημιουργία ενός μεγάλου λογική συσκευή πολλών μονταρισμένο δίσκων VHD. Δείτε επίσης το κεφάλαιο [λογισμικό RAID] [dbms-Οδηγός-2.2] αυτού του εγγράφου. Αυτή η προσέγγιση απλοποιεί την επιβάρυνσης διαχείρισης για να διαχειριστείτε το χώρο στο δίσκο και αποφεύγεται η προσπάθεια των μη αυτόματη διανομή των αρχείων σε πολλές μονταρισμένο VHD.
* Για τις υψηλότερες απαιτήσεις IOP Προέλευσης, μπορείτε να χρησιμοποιήσετε Azure Premium χώρο αποθήκευσης, που είναι διαθέσιμο σε σειράς DS και ΣΠΣ GS σειρά.

![Ρύθμιση παραμέτρων αναφοράς του Azure Εικονική IaaS για SAP MaxDB DBMS][dbms-guide-figure-600]

#### <a name="23c78d3b-ca5a-4e72-8a24-645d141a3f5d"></a>Δημιουργία αντιγράφων ασφαλείας και επαναφοράς
Κατά την ανάπτυξη SAP MaxDB σε Azure, πρέπει να εξετάσετε μεθοδολογία αντιγράφου ασφαλείας σας. Ακόμα και αν το σύστημα δεν είναι παραγωγικοί σύστημα, η βάση δεδομένων SAP που φιλοξενούνται από SAP MaxDB πρέπει να δημιουργηθούν αντίγραφα περιοδικά. Επειδή το χώρο αποθήκευσης Azure διατηρεί τρεις εικόνες, ένα αντίγραφο ασφαλείας τώρα είναι λιγότερο σημαντικά όσον αφορά την προστασία του συστήματος έναντι αποτυχία χώρου αποθήκευσης και πιο σημαντικά αποτυχίες λειτουργικές ή διαχείρισης. Ο κύριος λόγος για τη διατήρηση μια σωστής δημιουργίας αντιγράφων ασφαλείας και επαναφορά πρόγραμμα είναι έτσι ώστε να μπορείτε να αποζημιώσει για τυχόν σφάλματα λογική ή μη αυτόματη, παρέχοντας δυνατότητες αποκατάστασης σε δεδομένη χρονική στιγμή. Επομένως, ο στόχος είναι να είτε να χρησιμοποιήσετε αντίγραφα ασφαλείας για να επαναφέρετε τη βάση δεδομένων σε ένα συγκεκριμένο σημείο σε χρόνο ή για να χρησιμοποιήσετε τη δημιουργία αντιγράφων ασφαλείας στο Azure να σπείρεται άλλο σύστημα, αντιγράφοντας μια υπάρχουσα βάση δεδομένων. Για παράδειγμα, μπορεί να μεταφέρετε από μια ρύθμιση παραμέτρων SAP 2 επιπέδων σε μια ρύθμιση σύστημα 3 επιπέδων του ίδιου συστήματος με την επαναφορά ενός αντιγράφου ασφαλείας.

Δημιουργία αντιγράφων ασφαλείας και επαναφορά μιας βάσης δεδομένων στο Azure λειτουργεί τον ίδιο τρόπο όπως και για συστήματα εσωτερικής εγκατάστασης, ώστε να μπορείτε να χρησιμοποιήσετε τις τυπικές εργαλεία δημιουργίας αντιγράφων ασφαλείας/επαναφοράς SAP MaxDB, που περιγράφονται σε ένα από τα έγγραφα τεκμηρίωση SAP MaxDB που αναφέρονται σε σημείωση SAP [767598]. 

#### <a name="77cd2fbb-307e-4cbf-a65f-745553f72d2c"></a>Ζητήματα επιδόσεων για δημιουργία αντιγράφων ασφαλείας και επαναφοράς
Όπως αναπτύξεις χωρίς λειτουργικό σύστημα, δημιουργία αντιγράφων ασφαλείας και επαναφορά επιδόσεων εξαρτάται από πόσες όγκους μπορεί να διαβαστεί σε παράλληλα και της ταχύτητας μετάδοσης των αυτές τις όγκους. Επιπλέον, την κατανάλωση CPU που χρησιμοποιείται από το αντίγραφο ασφαλείας συμπίεσης να αναπαραγάγετε μια σημαντική ρόλο σε ΣΠΣ με έως 8 νήματα CPU. Επομένως, μπορεί να λάβει ένα:

* Τα λιγότερα τον αριθμό των VHD χρησιμοποιείται για την αποθήκευση τις συσκευές βάσης δεδομένων, την κατώτερη τη συνολική μεταγωγή ανάγνωσης
* Το μικρότερο τον αριθμό των CPU νήματα σε Εικονική, τα πιο σοβαρά την επίδραση των αντιγράφων ασφαλείας συμπίεσης
* Τα λιγότερα στόχους (γραμμή σε καταλόγους, VHD) για να γράψετε το αντίγραφο ασφαλείας προς τα κάτω τη μετάδοση

Για να αυξήσετε τον αριθμό των στόχων για την εγγραφή, υπάρχουν δύο επιλογές τις οποίες μπορείτε να χρησιμοποιήσετε, πιθανώς σε συνδυασμό, ανάλογα με τις ανάγκες σας:

* Αποκλειστικό ξεχωριστή όγκους για δημιουργία αντιγράφων ασφαλείας
* Δίσκους της έντασης αντίγραφο ασφαλείας προορισμού μέσω πολλών μονταρισμένο VHD για να βελτιώσετε την ταχύτητα μεταγωγής IOP Προέλευσης στη συγκεκριμένη ραβδωτό σκληρό δίσκο
* Αντιμετωπίζετε ξεχωριστή αποκλειστική λογικού δίσκου συσκευές για:
    * SAP MaxDB όγκους δημιουργίας αντιγράφων ασφαλείας (π.χ., αρχεία)
    * SAP MaxDB όγκους δεδομένων (δηλαδή αρχεία)
    * SAP όγκους καταγραφής MaxDB (δηλαδή αρχεία)

Δίσκους ενός τόμου μέσω πολλών μονταρισμένο VHD έχει περιγράφεται παραπάνω σε κεφάλαιο [λογισμικό RAID] [dbms-Οδηγός-2.2] αυτού του εγγράφου. 

#### <a name="f77c1436-9ad8-44fb-a331-8671342de818"></a>Άλλες
Επίσης, όλα τα άλλα γενικά θέματα όπως σύνολα διαθεσιμότητα Azure ή SAP παρακολούθηση εφαρμόσετε όπως περιγράφεται στο τα πρώτα τρία κεφάλαια αυτού του εγγράφου για αναπτύξεις ΣΠΣ με τη βάση δεδομένων SAP MaxDB.
Άλλες ρυθμίσεις για συγκεκριμένο MaxDB SAP είναι διαφανή ΣΠΣ Azure και περιγράφονται σε διαφορετικά έγγραφα που αναφέρονται σε σημείωση SAP [767598] και σε αυτές τις σημειώσεις SAP:

* [826037] 
* [1139904]
* [1173395]

## <a name="specifics-for-sap-livecache-on-windows"></a>Συγκεκριμένες απαιτήσεις για liveCache SAP στα Windows

### <a name="sap-livecache-version-support"></a>SAP liveCache υποστήριξη έκδοση
Ελάχιστη έκδοση του SAP liveCache υποστηρίζεται σε εικονικές μηχανές Windows Azure είναι **SAP LC/LCAPPS 10.0 SP 25** , συμπεριλαμβανομένων των **liveCache 7.9.08.31** και **25 LCA Δόμηση**, κυκλοφορήσει για **EhP 2 για SAP SCM 7.0** και νεότερες εκδόσεις.

### <a name="supported-microsoft-windows-versions-and-azure-vm-types-for-sap-livecache-dbms"></a>Υποστηριζόμενοι τύποι Microsoft Windows εκδόσεις και Εικονική Azure για SAP liveCache DBMS
Για να βρείτε την υποστηριζόμενη έκδοση των Microsoft Windows για liveCache SAP στο Azure, ανατρέξτε στα θέματα:

* [Πίνακας Διαθεσιμότητα προϊόντος SAP (PAM)][sap-pam]
* Σημείωση SAP [1928533]

Συνιστάται ιδιαίτερα να χρησιμοποιείτε την πιο πρόσφατη έκδοση του λειτουργικού συστήματος των Microsoft Windows, η οποία είναι Microsoft Windows 2012 R2. 

### <a name="sap-livecache-configuration-guidelines-for-sap-installations-in-azure-vms"></a>SAP liveCache οδηγίες ρύθμισης παραμέτρων για τις εγκαταστάσεις SAP στο ΣΠΣ Azure

#### <a name="recommended-azure-vm-types"></a>Προτεινόμενοι τύποι Εικονική Azure
Καθώς SAP liveCache είναι μια εφαρμογή που εκτελεί υπολογισμούς τεράστιες, το ποσό ταχύτητας και του RAM και της CPU έχει μια κύρια επίδραση στις επιδόσεις liveCache SAP. 

Για τους τύπους Εικονική Azure που υποστηρίζονται από SAP (Σημείωση SAP [1928533]), όλων των εικονικού πόρων CPU που διατίθενται για την εικονική Μηχανή δημιουργούνται αντίγραφα από αποκλειστικό φυσικής πόρους CPU από το υπερεπόπτη. Δεν υπάρχει overprovisioning (και επομένως δεν υπάρχει ανταγωνισμό για τους πόρους CPU) τίθεται σε ισχύ.

Ομοίως, για όλους τύπους παρουσία Εικονική Azure υποστηρίζονται από SAP, η Εικονική μνήμη είναι 100% αντιστοιχιστεί η φυσική μνήμη – overprovisioning (υπερβολικής δέσμευσης), για παράδειγμα, δεν χρησιμοποιείται.

Από αυτό προοπτική, συνιστάται να χρησιμοποιήσετε τον νέο τύπο σειράς D ή Εικονική Azure σειράς DS (σε συνδυασμό με το χώρο αποθήκευσης Premium Azure), που έχουν 60% ταχύτερη επεξεργαστές από τη σειρά A. Για την υψηλότερη RAM και της CPU φόρτωση, μπορείτε να χρησιμοποιήσετε G σειρά και ΣΠΣ GS-σειρά (σε συνδυασμό με το χώρο αποθήκευσης Premium Azure) με την πιο πρόσφατη Intel® Xeon® επεξεργαστής E5 v3 οικογένεια, που έχουν δύο φορές τη μνήμη και τέσσερις φορές το συμπαγές κατάσταση μονάδα δίσκου χώρο αποθήκευσης (SSD) D/DS-σειράς.

#### <a name="storage-configuration"></a>Ρύθμιση παραμέτρων αποθήκευσης
Καθώς SAP liveCache βασίζεται στην τεχνολογία SAP MaxDB, όλα τα Azure αποθήκευσης βέλτιστη πρακτική εξάσκηση συστάσεις που αναφέρονται για SAP MaxDB στο κεφάλαιο [ρύθμιση παραμέτρων αποθήκευσης] [dbms-Οδηγός-8.4.1] ισχύουν για τις SAP liveCache. 

#### <a name="dedicated-azure-vm-for-livecache"></a>Αποκλειστική Εικονική Azure για liveCache
Καθώς SAP liveCache χρησιμοποιεί μεγάλο υπολογιστική ισχύ, για χρήση παραγωγικοί συνιστάται ιδιαίτερα να αναπτύξετε το σε μια αποκλειστική Azure εικονική μηχανή. 
 
![Αφοσιωμένη Εικονική Azure για liveCache για παραγωγικοί χρήση πεζών-κεφαλαίων][dbms-guide-figure-700]

#### <a name="backup-and-restore"></a>Δημιουργία αντιγράφων ασφαλείας και επαναφοράς
Δημιουργία αντιγράφων ασφαλείας και επαναφορά, συμπεριλαμβανομένων των ζητήματα επιδόσεων, περιγράφονται ήδη στο στα σχετικά κεφάλαια SAP MaxDB [δημιουργίας αντιγράφων ασφαλείας και επαναφορά] [dbms-Οδηγός-8.4.2] και [ζητήματα επιδόσεων για δημιουργία αντιγράφων ασφαλείας και επαναφορά] [dbms-Οδηγός-8.4.3]. 

#### <a name="other"></a>Άλλες
Όλα τα άλλα θέματα γενικά περιγράφονται ήδη στο σχετικό κεφάλαιο MaxDB SAP [αυτό] [dbms-Οδηγός-8.4.4]. 

## <a name="specifics-for-the-sap-content-server-on-windows"></a>Συγκεκριμένες απαιτήσεις για το διακομιστή περιεχομένου SAP στα Windows
Ο διακομιστής περιεχόμενο SAP είναι ένα στοιχείο ξεχωριστά, με βάση το διακομιστή για την αποθήκευση περιεχομένου όπως ηλεκτρονικά έγγραφα σε διαφορετικές μορφές. Ο διακομιστής περιεχομένου SAP παρέχονται από την ανάπτυξη της τεχνολογίας και θα πρέπει να χρησιμοποιούνται μεταξύ εφαρμογών για τις εφαρμογές του SAP. Είναι εγκατεστημένο στη ξεχωριστό σύστημα. Τυπικές περιεχόμενο είναι εκπαιδευτικό υλικό και την τεκμηρίωση από αποθήκη γνώσεων ή τεχνικά σχέδια που προέρχονται από το σύστημα διαχείρισης εγγράφων PLM mySAP. 

### <a name="sap-content-server-version-support"></a>Υποστήριξη έκδοση περιεχομένου διακομιστή SAP
SAP υποστηρίζει τη συγκεκριμένη στιγμή:

* **SAP Server περιεχομένου** με την έκδοση **6.50 (και νεότερες εκδόσεις)**
* **SAP MaxDB έκδοση 7.9**
* **Microsoft των υπηρεσιών IIS (Internet Information Server) έκδοση 8.0 (και νεότερες εκδόσεις)**

Συνιστάται ιδιαίτερα να χρησιμοποιείτε την πιο πρόσφατη έκδοση του περιεχομένου διακομιστή SAP, ο οποίος είναι **6,50 SP4**, τη στιγμή της σύνταξη αυτού του εγγράφου και νεότερη έκδοση του **Microsoft των υπηρεσιών IIS 8.5**. 

Επιλέξτε τις πιο πρόσφατες υποστηριζόμενες εκδόσεις του περιεχομένου SAP Server και των υπηρεσιών IIS της Microsoft στο η [Μήτρα διαθεσιμότητα προϊόντος SAP (PAM)][sap-pam].

### <a name="supported-microsoft-windows-and-azure-vm-types-for-sap-content-server"></a>Υποστηριζόμενοι τύποι των Microsoft Windows και Εικονική Azure για διακομιστή περιεχομένου SAP
Για να βρείτε υποστηριζόμενη έκδοση των Windows για το διακομιστή περιεχομένου SAP στο Azure, ανατρέξτε στα θέματα:

* [Πίνακας Διαθεσιμότητα προϊόντος SAP (PAM)][sap-pam]
* Σημείωση SAP [1928533]

Συνιστάται ιδιαίτερα να χρησιμοποιείτε την πιο πρόσφατη έκδοση του Microsoft Windows, το οποίο τη στιγμή της γράφετε αυτό το έγγραφο είναι **Windows Server 2012 R2**.

### <a name="sap-content-server-configuration-guidelines-for-sap-installations-in-azure-vms"></a>Οδηγίες ρύθμισης παραμέτρων διακομιστή περιεχομένου SAP για τις εγκαταστάσεις SAP στο ΣΠΣ Azure

#### <a name="storage-configuration"></a>Ρύθμιση παραμέτρων αποθήκευσης 
Εάν ρυθμίζετε SAP Server περιεχομένου για να αποθηκεύσετε αρχεία στη βάση δεδομένων SAP MaxDB, όλα Azure αποθήκευσης βέλτιστες πρακτικές σύσταση που αναφέρεται για SAP MaxDB στο κεφάλαιο [ρύθμιση παραμέτρων αποθήκευσης] [dbms-Οδηγός-8.4.1] ισχύουν για το σενάριο SAP Server περιεχομένου. 

Εάν ρυθμίζετε SAP Server περιεχομένου για να αποθηκεύσετε αρχεία στο σύστημα αρχείων, συνιστάται να χρησιμοποιήσετε μια αποκλειστική λογική μονάδα δίσκου. Χρήση του χώρου αποθήκευσης κενά διαστήματα σας επιτρέπει να επίσης αύξηση μεγέθους λογικού δίσκου και απόδοση IOP Προέλευσης, όπως περιγράφεται στο στο κεφάλαιο [λογισμικό RAID] [dbms-Οδηγός-2.2]. 

#### <a name="sap-content-server-location"></a>Θέση περιεχομένου διακομιστή SAP
SAP Server περιεχομένου έχει να αναπτυχθούν στην ίδια περιοχή Azure και VNET Azure όπου έχει αναπτυχθεί το σύστημα SAP. Που είναι δωρεάν για να αποφασίσετε εάν θέλετε να αναπτύξετε το περιεχόμενο SAP Server στοιχεία σε μια αποκλειστική Εικονική Azure ή στο το ίδιο Εικονική όπου εκτελείται το σύστημα SAP. 
 
![Αποκλειστική Azure Εικονική περιεχομένου διακομιστής SAP][dbms-guide-figure-800]

#### <a name="sap-cache-server-location"></a>Θέση του διακομιστή Cache SAP
Ο διακομιστής Cache SAP είναι ένα πρόσθετο στοιχείο με βάση το διακομιστή για να παρέχουν πρόσβαση σε έγγραφα (στο cache) τοπικά. Ο διακομιστής Cache SAP αποθηκεύει προσωρινά στα έγγραφα ενός διακομιστή περιεχομένου SAP. Πρόκειται για να βελτιστοποιήσετε την κίνηση του δικτύου, εάν έχετε έγγραφα που θα ανακτηθούν περισσότερες από μία φορές από διαφορετικές θέσεις. Ο γενικός κανόνας είναι ότι ο διακομιστής Cache SAP πρέπει να να βρίσκεστε κοντά το πρόγραμμα-πελάτη που έχει πρόσβαση σε διακομιστή Cache SAP. 

Εδώ, έχετε δύο επιλογές:

1. **Πρόγραμμα-πελάτη είναι ένα σύστημα SAP παρασκηνίου** Εάν ένα σύστημα SAP παρασκηνίου έχει ρυθμιστεί για να αποκτήσετε πρόσβαση σε διακομιστή περιεχομένου SAP, το σύστημα αυτό SAP είναι ένα πρόγραμμα-πελάτη. Καθώς και SAP σύστημα και SAP Server περιεχομένου αναπτύσσονται στην ίδια περιοχή Azure – στο ίδιο κέντρο δεδομένων Azure – έχουν φυσική κοντά μεταξύ τους. Γι ' αυτό, δεν χρειάζεται να έχει ένα αποκλειστικό διακομιστή SAP στο Cache. Προγράμματα-πελάτες του περιβάλλοντος εργασίας Χρήστη SAP (SAP Γραφικών ή web πρόγραμμα περιήγησης) απευθείας πρόσβαση στο σύστημα SAP και του συστήματος SAP ανακτά έγγραφα από το διακομιστή περιεχομένου SAP.
1. **Πρόγραμμα-πελάτης είναι ένα πρόγραμμα περιήγησης web εσωτερικής εγκατάστασης** Ο διακομιστής SAP περιεχομένου μπορεί να ρυθμιστεί για να είναι δυνατή η πρόσβαση απευθείας από το πρόγραμμα περιήγησης web. Σε αυτήν την περίπτωση, ένα πρόγραμμα περιήγησης web που εκτελεί – εσωτερικής είναι ένα πρόγραμμα-πελάτη του περιεχομένου διακομιστή SAP. Κέντρο δεδομένων εσωτερικής εγκατάστασης και κέντρου δεδομένων του Azure τοποθετούνται σε διαφορετικές θέσεις του φυσικού (ιδανικά κοντά μεταξύ τους). Το κέντρο δεδομένων εσωτερικής εγκατάστασης είναι συνδεδεμένο σε Azure μέσω Azure VPN-τοποθεσίας ή ExpressRoute. Παρόλο που και οι δύο επιλογές προσφέρει ασφαλή σύνδεση δικτύου VPN να Azure, σύνδεση δικτύου-τοποθεσίας δεν προσφέρει ένα δίκτυο SLA εύρους ζώνης και λανθάνοντος χρόνου μεταξύ του κέντρου δεδομένων εσωτερικής εγκατάστασης και το Azure κέντρο δεδομένων. Για να επιταχύνετε την πρόσβαση σε έγγραφα, μπορείτε να κάνετε ένα από τα εξής:
    1. Εγκατάσταση του Cache SAP Server εσωτερικής εγκατάστασης, Κλείσιμο για να την εσωτερική web περιήγησης (επιλογή σε σχήμα [this][dbms-guide-900-sap-cache-server-on-premises])
    1. Ρύθμιση παραμέτρων Azure ExpressRoute, το οποίο προσφέρει μια σύνδεση υψηλής ταχύτητας και χαμηλής αδράνειας αποκλειστικό δικτύου μεταξύ του κέντρου δεδομένων εσωτερικής εγκατάστασης και Azure κέντρου δεδομένων.
 
![Επιλογή για να εγκαταστήσετε το Cache SAP Server εσωτερικής εγκατάστασης][dbms-guide-figure-900]
<a name="642f746c-e4d4-489d-bf63-73e80177a0a8"></a>

#### <a name="backup--restore"></a>Δημιουργία αντιγράφων ασφαλείας / Επαναφορά
Εάν ρυθμίζετε τις παραμέτρους του SAP Server περιεχομένου για να αποθηκεύσετε αρχεία στη βάση δεδομένων SAP MaxDB, τα ζητήματα δημιουργίας αντιγράφων ασφαλείας/επαναφοράς διαδικασία και επιδόσεων περιγράφονται ήδη στο SAP MaxDB κεφάλαιο [δημιουργίας αντιγράφων ασφαλείας και επαναφορά] [dbms-Οδηγός-8.4.2] και κεφάλαιο [ζητήματα επιδόσεων για δημιουργία αντιγράφων ασφαλείας και επαναφορά] [dbms-Οδηγός-8.4.3]. 

Εάν ρυθμίζετε τις παραμέτρους του SAP Server περιεχομένου για να αποθηκεύσετε αρχεία στο σύστημα αρχείων, μία επιλογή είναι να εκτελέσει μη αυτόματη δημιουργία αντιγράφων ασφαλείας και επαναφορά της δομής ολόκληρου αρχείου όπου βρίσκονται τα έγγραφα. Παρόμοια με SAP MaxDB δημιουργία αντιγράφων ασφαλείας/επαναφορά, καλό είναι να έχετε μια αποκλειστική σκληρό δίσκο για το σκοπό δημιουργίας αντιγράφων ασφαλείας. 

#### <a name="other"></a>Άλλες
Άλλες ρυθμίσεις συγκεκριμένες SAP Server περιεχομένου είναι διαφανή ΣΠΣ Azure και περιγράφονται στα διάφορα έγγραφα και SAP σημειώσεις:

* <https://Service.SAP.com/contentserver> 
* Σημείωση SAP [1619726]  

## <a name="specifics-to-ibm-db2-for-luw-on-windows"></a>Συγκεκριμένα στοιχεία IBM DB2 για LUW στα Windows
Με το Microsoft Azure, μπορείτε εύκολα να μετεγκαταστήσετε υπάρχουσα εφαρμογή SAP εκτελείται σε IBM DB2 για Linux, UNIX και των Windows (LUW) για να Azure εικονικές μηχανές. Με το SAP σε IBM DB2 για LUW, διαχειριστές και τους προγραμματιστές να χρησιμοποιήσετε την ίδια ανάπτυξη και τα εργαλεία διαχείρισης που είναι διαθέσιμες στην εσωτερική εγκατάσταση.
Γενικές πληροφορίες σχετικά με την εκτέλεση οικογένεια επιχειρήσεις SAP σε IBM DB2 για LUW μπορείτε να βρείτε στο το δίκτυο Κοινότητας SAP (ΣΆΡΩΣΗ) στο <https://scn.sap.com/community/db2-for-linux-unix-windows>.

Για πρόσθετες πληροφορίες και ενημερώσεις σχετικά με SAP σε DB2 για LUW σε Azure, ανατρέξτε στο θέμα Σημείωση SAP [2233094]. 

### <a name="ibm-db2-for-linux-unix-and-windows-version-support"></a>IBM DB2 για Linux, UNIX και υποστήριξη έκδοση των Windows
SAP σε IBM DB2 για LUW σχετικά με τις υπηρεσίες του Microsoft Azure εικονική μηχανή υποστηρίζεται από την έκδοση 10.5 DB2.

Για πληροφορίες σχετικά με τις υποστηριζόμενες SAP προϊόντα και τύπους Εικονική Azure, ανατρέξτε στην σημείωση SAP [1928533].

### <a name="ibm-db2-for-linux-unix-and-windows-configuration-guidelines-for-sap-installations-in-azure-vms"></a>IBM DB2 για Linux, UNIX και οδηγίες ρύθμισης παραμέτρων των Windows για τις εγκαταστάσεις SAP στο ΣΠΣ Azure

#### <a name="storage-configuration"></a>Ρύθμιση παραμέτρων αποθήκευσης
Όλα τα αρχεία βάσης δεδομένων πρέπει να αποθηκεύονται στο σύστημα αρχείων NTFS βάση δίσκων VHD. Αυτές οι VHD είναι ενεργοποιημένες για το Εικονική Azure και βασίζονται στο χώρο αποθήκευσης αντικειμένων BLOB του Azure σελίδας (<https://msdn.microsoft.com/library/azure/ee691964.aspx>). Οποιοδήποτε είδος μονάδες δίσκου δικτύου ή απομακρυσμένα κοινόχρηστα στοιχεία, όπως τις ακόλουθες υπηρεσίες Azure αρχείων **δεν** υποστηρίζονται για αρχεία της βάσης δεδομένων είναι: 

* <https://blogs.MSDN.com/b/windowsazurestorage/Archive/2014/05/12/introducing-Microsoft-Azure-File-Service.aspx>
* <https://blogs.MSDN.com/b/windowsazurestorage/Archive/2014/05/27/persisting-Connections-To-Microsoft-Azure-Files.aspx>
 
Εάν χρησιμοποιείτε VHD Azure που βασίζονται σε χώρο αποθήκευσης BLOB του Azure σελίδας, των στοιχείων που αναφέρονται σε αυτό το έγγραφο στο κεφάλαιο [δομή μιας ανάπτυξης RDBMS] [dbms-Οδηγός-2] θα εφαρμοστούν επίσης στις αναπτύξεις με την IBM DB2 για LUW βάση δεδομένων. 

Όπως περιγράφεται παραπάνω σε το γενικό μέρος του εγγράφου, υπάρχει ορίων στις μετάδοσης IOP Προέλευσης για VHD Azure. Τα ακριβή ορίων εξαρτώνται από τον τύπο Εικονική που χρησιμοποιείται. Μπορείτε να βρείτε μια λίστα των τύπων Εικονική με τους στόχους πωλήσεων [εδώ][virtual-machines-sizes].

Με την προϋπόθεση ότι το τρέχον όριο IOP Προέλευσης ανά δίσκο είναι αρκετό, είναι δυνατή η αποθήκευση όλων των αρχείων βάσης δεδομένων σε ένα μεμονωμένο VHD μονταρισμένο Azure. 

Για τις επιδόσεις ζητήματα επίσης να ανατρέξετε κεφάλαιο "Δεδομένων ασφάλειας και επιδόσεων ζητήματα για βάση δεδομένων σε καταλόγους" σε οδηγούς εγκατάστασης SAP.

Εναλλακτικά, μπορείτε να χρησιμοποιήσετε σύνολα χώρου αποθήκευσης των Windows (μόνο διαθέσιμη στο Windows Server 2012 και νεότερη έκδοση) ή επιμερισμό των Windows για Windows 2008 R2 όπως περιγράφεται στο κεφάλαιο [λογισμικό RAID] [dbms-Οδηγός-2.2] αυτού του εγγράφου για να δημιουργήσετε ένα μεγάλο λογική συσκευή μέσω πολλών μονταρισμένο δίσκων VHD.
Για το δίσκων που περιέχει τις διαδρομές αποθήκευσης DB2 σας καταλόγων sapdata και saptmp, πρέπει να καθορίσετε ένα μέγεθος τομέα φυσικό δίσκο 512 KB. Όταν χρησιμοποιείτε σύνολα χώρου αποθήκευσης των Windows, πρέπει να δημιουργήσετε τα σύνολα χώρου αποθήκευσης με μη αυτόματο τρόπο μέσω περιβάλλον γραμμής εντολών χρησιμοποιώντας την παράμετρο "-LogicalSectorSizeDefault". Για λεπτομέρειες, ανατρέξτε <https://technet.microsoft.com/library/hh848689.aspx>.

#### <a name="backuprestore"></a>Δημιουργία αντιγράφων ασφαλείας/Επαναφορά
Η λειτουργία δημιουργίας αντιγράφων ασφαλείας/επαναφοράς για IBM DB2 για LUW υποστηρίζεται με τον ίδιο τρόπο όπως σε τυπική λειτουργικά συστήματα Windows Server και το Hyper-V.

Πρέπει να βεβαιωθείτε ότι έχετε μια έγκυρη βάση δεδομένων αντιγράφων ασφαλείας στρατηγική στη θέση. 

Όπως αναπτύξεις χωρίς λειτουργικό σύστημα, δημιουργία αντιγράφων ασφαλείας/Επαναφορά επιδόσεων εξαρτάται από πόσες όγκους μπορεί να διαβαστεί παράλληλα και της ταχύτητας μετάδοσης των αυτές τις όγκους που μπορεί να είναι. Επιπλέον, την κατανάλωση CPU που χρησιμοποιείται από το αντίγραφο ασφαλείας συμπίεσης, μπορούν να συμβάλλουν σημαντική σε VM με μόνο έως 8 νήματα CPU. Επομένως, μπορεί να λάβει ένα:

* Τα λιγότερα τον αριθμό των VHD χρησιμοποιείται για την αποθήκευση τις συσκευές βάσης δεδομένων, όσο μικρότερο τη συνολική μεταγωγή στην ανάγνωση
* Το μικρότερο τον αριθμό των CPU νήματα σε Εικονική, τα πιο σοβαρά την επίδραση των αντιγράφων ασφαλείας συμπίεσης
* Τα λιγότερα στόχους (γραμμή σε καταλόγους, VHD) για να γράψετε το αντίγραφο ασφαλείας προς τα κάτω τη μετάδοση

Για να αυξήσετε τον αριθμό των προορισμών για την εγγραφή, δύο επιλογές μπορεί να χρησιμοποιηθεί/συνδυασμένες ανάλογα με τις ανάγκες σας:

* Δίσκους της έντασης αντίγραφο ασφαλείας προορισμού μέσω πολλών μονταρισμένο VHD για να βελτιώσετε την ταχύτητα μεταγωγής IOP Προέλευσης που ραβδωτό τόμου
* Χρήση περισσότερες από μία καταλόγου προορισμού για να γράψετε το αντίγραφο ασφαλείας

#### <a name="high-availability-and-disaster-recovery"></a>Υψηλή διαθεσιμότητα και αποκατάσταση
Microsoft σύμπλεγμα Server (MSCS) δεν υποστηρίζεται.

Υποστηρίζεται DB2 υψηλή διαθεσιμότητα αποκατάσταση (HADR). Εάν το εικονικές μηχανές της ρύθμισης παραμέτρων HA έχετε εργασία επίλυση ονομάτων, τη διαμόρφωση στο Azure δεν θα διαφέρει από οποιοδήποτε πρόγραμμα εγκατάστασης που έχει ολοκληρωθεί εσωτερικής εγκατάστασης. Δεν συνιστάται να βασίζεστε στην ανάλυση IP μόνο.

Μην χρησιμοποιείτε Azure Store παν-αναπαραγωγής. Για περισσότερες πληροφορίες, ανατρέξτε κεφάλαιο [χώρος αποθήκευσης Microsoft Azure] [dbms-Οδηγός-2.3] και κεφάλαιο [υψηλή διαθεσιμότητα και αποκατάσταση ΣΠΣ Azure] [dbms-Οδηγός-3].

#### <a name="other"></a>Άλλες
Όλα τα άλλα γενικά θέματα όπως σύνολα διαθεσιμότητα Azure ή SAP παρακολούθηση εφαρμογή όπως περιγράφεται στο τα πρώτα τρία κεφάλαια αυτού του εγγράφου για αναπτύξεις καθώς και των ΣΠΣ με IBM DB2 για LUW. 

Ανατρέξτε επίσης κεφάλαιο [γενικά SQL Server για SAP στη σύνοψη Azure] [dbms-Οδηγός-5.8].

## <a name="specifics-to-ibm-db2-for-luw-on-linux"></a>Συγκεκριμένα στοιχεία IBM DB2 για LUW σε Linux
Με το Microsoft Azure, μπορείτε εύκολα να μετεγκαταστήσετε υπάρχουσα εφαρμογή SAP εκτελείται σε IBM DB2 για Linux, UNIX και των Windows (LUW) για να Azure εικονικές μηχανές. Με το SAP σε IBM DB2 για LUW, διαχειριστές και τους προγραμματιστές να χρησιμοποιήσετε την ίδια ανάπτυξη και τα εργαλεία διαχείρισης που είναι διαθέσιμες στην εσωτερική εγκατάσταση. Γενικές πληροφορίες σχετικά με την εκτέλεση οικογένεια επιχειρήσεις SAP σε IBM DB2 για LUW μπορείτε να βρείτε στο το δίκτυο Κοινότητας SAP (ΣΆΡΩΣΗ) στο <https://scn.sap.com/community/db2-for-linux-unix-windows>.

Για πρόσθετες πληροφορίες και ενημερώσεις σχετικά με SAP σε DB2 για LUW σε Azure, ανατρέξτε στο θέμα Σημείωση SAP [2233094].

### <a name="ibm-db2-for-linux-unix-and-windows-version-support"></a>IBM DB2 για Linux, UNIX και υποστήριξη έκδοση των Windows
SAP σε IBM DB2 για LUW σχετικά με τις υπηρεσίες του Microsoft Azure εικονική μηχανή υποστηρίζεται από την έκδοση 10.5 DB2.

Για πληροφορίες σχετικά με τις υποστηριζόμενες SAP προϊόντα και τύπους Εικονική Azure, ανατρέξτε στην σημείωση SAP [1928533].

### <a name="ibm-db2-for-linux-unix-and-windows-configuration-guidelines-for-sap-installations-in-azure-vms"></a>IBM DB2 για Linux, UNIX και οδηγίες ρύθμισης παραμέτρων των Windows για τις εγκαταστάσεις SAP στο ΣΠΣ Azure

#### <a name="storage-configuration"></a>Ρύθμιση παραμέτρων αποθήκευσης
Όλα τα αρχεία βάσης δεδομένων πρέπει να αποθηκεύονται σε ένα σύστημα αρχείων που βασίζεται σε δίσκων VHD. Αυτές οι VHD είναι ενεργοποιημένες για το Εικονική Azure και βασίζονται στο χώρο αποθήκευσης αντικειμένων BLOB του Azure σελίδας (<https://msdn.microsoft.com/library/azure/ee691964.aspx>).
Οποιοδήποτε είδος μονάδες δίσκου δικτύου ή απομακρυσμένα κοινόχρηστα στοιχεία, όπως τις ακόλουθες υπηρεσίες Azure αρχείων **δεν** υποστηρίζονται για αρχεία της βάσης δεδομένων είναι:

* <https://blogs.MSDN.com/b/windowsazurestorage/Archive/2014/05/12/introducing-Microsoft-Azure-File-Service.aspx>
* <https://blogs.MSDN.com/b/windowsazurestorage/Archive/2014/05/27/persisting-Connections-To-Microsoft-Azure-Files.aspx>

Εάν χρησιμοποιείτε VHD Azure που βασίζονται σε χώρο αποθήκευσης BLOB του Azure σελίδας, των στοιχείων που αναφέρονται σε αυτό το έγγραφο στο κεφάλαιο [δομή μιας ανάπτυξης RDBMS] [dbms-Οδηγός-2] θα εφαρμοστούν επίσης στις αναπτύξεις με την IBM DB2 για LUW βάση δεδομένων.

Όπως περιγράφεται παραπάνω σε το γενικό μέρος του εγγράφου, υπάρχει ορίων στις μετάδοσης IOP Προέλευσης για VHD Azure. Τα ακριβή ορίων εξαρτώνται από τον τύπο Εικονική που χρησιμοποιείται. Μπορείτε να βρείτε μια λίστα των τύπων Εικονική με τους στόχους πωλήσεων [εδώ][virtual-machines-sizes].

Με την προϋπόθεση ότι το τρέχον όριο IOP Προέλευσης ανά δίσκο είναι αρκετό, είναι δυνατή η αποθήκευση όλων των αρχείων βάσης δεδομένων σε ένα μεμονωμένο VHD μονταρισμένο Azure.

Για τις επιδόσεις ζητήματα επίσης να ανατρέξετε κεφάλαιο "Δεδομένων ασφάλειας και επιδόσεων ζητήματα για βάση δεδομένων σε καταλόγους" σε οδηγούς εγκατάστασης SAP.

Εναλλακτικά, μπορείτε να χρησιμοποιήσετε LVM (Διαχείριση λογικού τόμου) ή MDADM όπως περιγράφεται στο κεφάλαιο [λογισμικό RAID] [dbms-Οδηγός-2.2] αυτού του εγγράφου για να δημιουργήσετε ένα μεγάλο λογική συσκευή μέσω πολλών μονταρισμένο δίσκων VHD.
Για το δίσκων που περιέχει τις διαδρομές αποθήκευσης DB2 σας καταλόγων sapdata και saptmp, πρέπει να καθορίσετε ένα μέγεθος τομέα φυσικό δίσκο 512 KB.

#### <a name="backuprestore"></a>Δημιουργία αντιγράφων ασφαλείας/Επαναφορά
Η λειτουργία δημιουργίας αντιγράφων ασφαλείας/επαναφοράς για IBM DB2 για LUW υποστηρίζεται με τον ίδιο τρόπο όπως στην τυπική Linux εγκατάστασης εσωτερικής εγκατάστασης.

Πρέπει να βεβαιωθείτε ότι έχετε μια έγκυρη βάση δεδομένων αντιγράφων ασφαλείας στρατηγική στη θέση.

Όπως αναπτύξεις χωρίς λειτουργικό σύστημα, δημιουργία αντιγράφων ασφαλείας/Επαναφορά επιδόσεων εξαρτάται από πόσες όγκους μπορεί να διαβαστεί παράλληλα και της ταχύτητας μετάδοσης των αυτές τις όγκους που μπορεί να είναι. Επιπλέον, την κατανάλωση CPU που χρησιμοποιείται από το αντίγραφο ασφαλείας συμπίεσης, μπορούν να συμβάλλουν σημαντική σε VM με μόνο έως 8 νήματα CPU. Επομένως, μπορεί να λάβει ένα:

* Τα λιγότερα τον αριθμό των VHD χρησιμοποιείται για την αποθήκευση τις συσκευές βάσης δεδομένων, όσο μικρότερο τη συνολική μεταγωγή στην ανάγνωση
* Το μικρότερο τον αριθμό των CPU νήματα σε Εικονική, τα πιο σοβαρά την επίδραση των αντιγράφων ασφαλείας συμπίεσης
* Τα λιγότερα στόχους (γραμμή σε καταλόγους, VHD) για να γράψετε το αντίγραφο ασφαλείας προς τα κάτω τη μετάδοση

Για να αυξήσετε τον αριθμό των προορισμών για την εγγραφή, δύο επιλογές μπορεί να χρησιμοποιηθεί/συνδυασμένες ανάλογα με τις ανάγκες σας:

* Δίσκους της έντασης αντίγραφο ασφαλείας προορισμού μέσω πολλών μονταρισμένο VHD για να βελτιώσετε την ταχύτητα μεταγωγής IOP Προέλευσης που ραβδωτό τόμου
* Χρήση περισσότερες από μία καταλόγου προορισμού για να γράψετε το αντίγραφο ασφαλείας

#### <a name="high-availability-and-disaster-recovery"></a>Υψηλή διαθεσιμότητα και αποκατάσταση
Υποστηρίζεται DB2 υψηλή διαθεσιμότητα αποκατάσταση (HADR). Εάν το εικονικές μηχανές της ρύθμισης παραμέτρων HA έχετε εργασία επίλυση ονομάτων, τη διαμόρφωση στο Azure δεν θα διαφέρει από οποιοδήποτε πρόγραμμα εγκατάστασης που έχει ολοκληρωθεί εσωτερικής εγκατάστασης. Δεν συνιστάται να βασίζεστε στην ανάλυση IP μόνο.

Μην χρησιμοποιείτε Azure Store παν-αναπαραγωγής. Για περισσότερες πληροφορίες, ανατρέξτε κεφάλαιο [χώρος αποθήκευσης Microsoft Azure] [dbms-Οδηγός-2.3] και κεφάλαιο [υψηλή διαθεσιμότητα και αποκατάσταση ΣΠΣ Azure] [dbms-Οδηγός-3].

#### <a name="other"></a>Άλλες
Όλα τα άλλα γενικά θέματα όπως σύνολα διαθεσιμότητα Azure ή SAP παρακολούθηση εφαρμογή όπως περιγράφεται στο τα πρώτα τρία κεφάλαια αυτού του εγγράφου για αναπτύξεις καθώς και των ΣΠΣ με IBM DB2 για LUW.

Ανατρέξτε επίσης κεφάλαιο [γενικά SQL Server για SAP στη σύνοψη Azure] [dbms-Οδηγός-5.8].