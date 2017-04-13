<properties 
   pageTitle="Υλικό για StorSimple 10 GbE διασυνδέσεις | Microsoft Azure"
   description="Περιγράφει τα υποστηριζόμενα μικρών διαστάσεων-περιβάλλον πομποδέκτες (SFP), καλώδια και διακόπτες για τις 10 διασυνδέσεις δικτύου GbE στη συσκευή σας StorSimple."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="09/21/2016"
   ms.author="alkohli" />

# <a name="supported-hardware-for-the-10-gbe-network-interfaces-on-your-storsimple-device"></a>Υποστηρίζονται υλικό για τις 10 διασυνδέσεις δικτύου GbE στη συσκευή σας StorSimple

## <a name="overview"></a>Επισκόπηση

Σε αυτό το άρθρο παρέχει πληροφορίες σχετικά με συμπληρωματικό υλικό που λειτουργεί με τη συσκευή σας Microsoft Azure StorSimple.

## <a name="list-of-devices-tested-by-microsoft"></a>Λίστα των συσκευών ελεγχθεί από τη Microsoft

Η Microsoft έχει ελέγξει τα ακόλουθα μικρών διαστάσεων με δυνατότητα σύνδεσης πομποδέκτες (SFP), καλώδια και διακόπτες για να βεβαιωθείτε ότι λειτουργούν καλύτερα με συσκευές. (Στους παρακάτω πίνακες θα ενημερωθούν ως νέο υλικό ελέγχεται.)

### <a name="sfp-transceivers"></a>Πομποδέκτες SFP +

|Πραγματοποίηση|Μοντέλο|
|---|---|
|Cisco|SFP-10G-SR|

### <a name="cables"></a>Καλώδια

|S. Όχι. |Πραγματοποίηση|Μοντέλο|
|---|---|---|
| 1.|Cisco|SFP-H10GB-CU1M|
| 2.|Cisco|SFP-H10GB-CU2M|
| 3.|Cisco|SFP-H10GB-CU3M|
| 4.|Tripp Lite|N820 - 05M (OM3)|

### <a name="switches"></a>Διακόπτες

|S. Όχι.|Πραγματοποίηση|Μοντέλο|
|---|---|---|
| 1. |Cisco|N3K-C3172PQ-10GE|
| 2. |Cisco|N3K-C3048-ZM-F|
| 3. |Cisco|N5K-C5596UP-ΠΑΓΊΩΝ|

## <a name="list-of-devices-tested-in-the-field"></a>Λίστα των συσκευών ελεγχθεί στο πεδίο

Αυτή η ενότητα περιέχει τη λίστα των συσκευών που έχουν με επιτυχία αναπτυχθεί στο πεδίο από τους πελάτες StorSimple. Αυτά δεν έχουν ελεγχθεί από τη Microsoft, αλλά είναι πιθανό να εργαστείτε με τη συσκευή σας StorSimple.
 
| Παράμετρος                         | Τιμή                                    |
|-----------------------------------|------------------------------------------|
| Κάντε εναλλαγή                     | Juniper                                  |
| Αλλαγή μοντέλου                    | ex4550 32F                               |
| Έκδοση λειτουργικού συστήματος αλλαγής | JunOS 12.3R9.4                           |
| Μοντέλο blade                     | Θύρες ενσωματωμένη PIC (0)                    |
| Ορισμός πομποδέκτη                  | Juniper                                  |
| Μοντέλο πομποδέκτη               | Κωδικός προϊόντος 740 021308 <br></br> Κωδικός προϊόντος 740 030658                   |
| Έκδοση υλικολογισμικού πομποδέκτη    | Αναθ 01 έκδοση 0,0 (αναφερθεί)            |
| Μοντέλο καλώδιο                     | Διπλής όψης τις LC/LC 50/125µ, OM3, LSZH |
| Μοντέλο StorSimple                | 8600                                     |
| Έκδοση λογισμικού StorSimple     | 6.3.9600.17491                           |


## <a name="list-of-devices-tested-by-oem-provider-mellanox"></a>Λίστα των συσκευών ελεγχθεί από την υπηρεσία παροχής OEM (Mellanox)  

Mellanox έχει ελέγξει τα ακόλουθα μικρών διαστάσεων με δυνατότητα σύνδεσης πομποδέκτες (SFP), καλώδια και διακόπτες για να βεβαιωθείτε ότι λειτουργούν καλύτερα με Mellanox διασυνδέσεις δικτύου όπως τις 10 διασυνδέσεις δικτύου GbE στη συσκευή σας StorSimple.

### <a name="cables-and-modules-supported-by-mellanox"></a>Καλώδια και οι λειτουργικές μονάδες που υποστηρίζονται από το Mellanox 

Ο παρακάτω πίνακας παραθέτει τα καλώδια και οι λειτουργικές μονάδες που υποστηρίζονται από το Mellanox. Αυτά δεν έχουν ελεγχθεί από τη Microsoft, αλλά είναι πιθανό να εργαστείτε με τη συσκευή σας StorSimple.

| S. Όχι. | Ταχύτητα | Μοντέλο                 | Περιγραφή                                            | Πραγματοποίηση                |
|--------|-------|-----------------------|--------------------------------------------------------|-----------------------|
| 1.     | 10 GbE| ΓΑΒ-SFP-SFP - 1 ΕΚΑΤΟΜΜΎΡΙΟ        | παθητικές χάλκινοι καλώδιο SFP + 10 Gb/s 1 εκατομμύριο                   | Arista                |
| 2.     | 10 GbE| ΓΑΒ-SFP-SFP - 2M        | παθητικές χάλκινοι καλώδιο SFP + 10 Gb/s 2m                   | Arista                |
| 3.     | 10 GbE| ΓΑΒ-SFP-SFP - 3M        | παθητικές χάλκινοι καλώδιο SFP + 10 Gb/s 3m                   | Arista                |
| 4.     | 10 GbE| ΓΑΒ-SFP-SFP - 5M        | παθητικές χάλκινοι καλώδιο SFP + 10 Gb/s 5m                   | Arista                |
| 5.     | 10 GbE| Cisco SFP-H10GBCU1M   | Cisco SFP + καλώδιο                                       | Cisco                 |
| 6.     | 10 GbE| Cisco SFP-H10GBCU3M   | Cisco SFP + καλώδιο                                       | Cisco                 |
| 7.     |10 GbE | Cisco SFP-H10GBCU5M   | Cisco SFP + καλώδιο                                       | Cisco                 |
| 8.     | 10 GbE| J9281B HP X242 10 G    | SFP + για να SFP + 1 εκατομμύριο κατευθύνετε επισύναψη χάλκινοι καλώδιο             | HP                    |
| 9.     | 10 GbE| BLc HP 455883 B21     | 10Gb SR SFP + επιλέξετε                                       | HP                    |
| 10.    | 10 GbE| BLc HP 455886 B21     | 10Gb LR SFP + επιλέξετε                                       | HP                    |
| 11.    |10 GbE | BLc HP 487649 B21     | 10GbE 0,5 m SFP + χάλκινοι καλώδιο                           | HP                    |
| 12.    |10 GbE | BLc HP 487652 B21     | SFP + 1 εκατομμύριο 10GbE χάλκινοι καλώδιο                             | HP                    |
| 13.    |10 GbE | BLc HP 487655 B21     | SFP + 3m 10GbE χάλκινοι καλώδιο                             | HP                    |
| 14.    |10 GbE | BLc HP 487658 B21     | SFP + 7m 10GbE χάλκινοι καλώδιο                             | HP                    |
| 15.    |10 GbE | BLc HP 537963 B21     | SFP + 5m 10GbE χάλκινοι καλώδιο                             | HP                    |
| 16.    |10 GbE | AP784A HP             | 3m σειρά C παθητικές χάλκινοι SFP + καλώδιο                  | HP                    |
| 17.    |10 GbE | AP785A HP             | 5m σειρά C παθητικές χάλκινοι SFP + καλώδιο                  | HP                    |
| 18.    |10 GbE | AP818A HP             | 1 εκατομμύριο B σειρά ενεργό χάλκινοι SFP + καλώδιο                   | HP                    |
| 19.    |10 GbE | AP819A HP             | 3m B σειρά ενεργό χάλκινοι SFP + καλώδιο                   | HP                    |
| 20.    |10 GbE | J9150A HP             | X132 10 G SFP + LC SR πομποδέκτη                        | HP                    |
| 21.    |10 GbE | J9151A HP             | X132 10 G SFP + LC LR πομποδέκτη                        | HP                    |
| 22.    |10 GbE | J9283B HP             | X242 10 G SFP + SFP + 3 m DAC καλώδιο                        | HP                    |
| 23.    |10 GbE | J9285B HP             | X242 10 G SFP + SFP + 7 m DAC καλώδιο                        | HP                    |
| 24.    | 10 GbE| JD095B HP             | X240 10 G SFP + SFP + 0,65 m DAC καλώδιο                     | HP                    |
| 25.    | 10 GbE| JD096B HP             | X240 10 G SFP + SFP + 1.2 m DAC καλώδιο                      | HP                    |
| 26.    | 10 GbE| JD097B HP             | X240 10 G SFP + SFP + 3 m DAD καλώδιο                        | HP                    |
| 27.    | 10 GbE| Mellanox MAM1Q00A QSA | QSFP SFP + προσαρμογέα                                   | Τεχνολογίες Mellanox |
| 28.    | 10 GbE| MC2309124-006 Mt      | X SFP+ παθητικές χάλκινοι καλώδιο 1 για να 24awg 10 Gb/s QSFP 7 m   | Τεχνολογίες Mellanox |
| 29.    | 10 GbE| MC2309124-007 Mt      | X SFP+ παθητικές χάλκινοι καλώδιο 1 για να 24awg 10 Gb/s QSFP 7 m   | Τεχνολογίες Mellanox |
| 30.    | 10 GbE| MC2309130-003 Mt      | X SFP+ παθητικές χάλκινοι καλώδιο 1 για να 30awg 10 Gb/s QSFP 3 m   | Τεχνολογίες Mellanox |
| 31.    | 10 GbE| MC2309130 00A Mt      | X SFP+ παθητικές χάλκινοι καλώδιο 1 για να 30awg 10 Gb/s QSFP 0,5 m | Τεχνολογίες Mellanox |
| 32.    | 10 GbE| MC3309124-005 Mt      | Χάλκινοι παθητικές καλώδιο 1 24awg 10 Gb/s x SFP+ 5 m           | Τεχνολογίες Mellanox |
| 33.    | 10 GbE| MC3309124-007 Mt      | Χάλκινοι παθητικές καλώδιο 1 24awg 10 Gb/s x SFP+ 7 m           | Τεχνολογίες Mellanox |
| 34.    | 10 GbE| MC3309130-003 Mt      | Χάλκινοι παθητικές καλώδιο 1 30awg 10 Gb/s x SFP+ 3 m           | Τεχνολογίες Mellanox |
| 35.    | 10 GbE| MC3309130 00A Mt      | Χάλκινοι παθητικές καλώδιο 1 30awg 10 Gb/s x SFP+ 0,5 m         | Τεχνολογίες Mellanox |

### <a name="switches-supported-by-mellanox"></a>Διακόπτες που υποστηρίζονται από το Mellanox 

Ο παρακάτω πίνακας παραθέτει τους διακόπτες που υποστηρίζονται από το Mellanox. Αυτά δεν έχουν ελεγχθεί από τη Microsoft, αλλά είναι πιθανό να εργαστείτε με τη συσκευή σας StorSimple.

| S. Όχι. | Ταχύτητα | Μοντέλο | Περιγραφή                                                         | Πραγματοποίηση              |
|--------|-------|-------------|---------------------------------------------------------------------|-------------|
| 1.     | 10GBe | 516733 B21  | Διακόπτης Blade Ethernet 10GbE ProCurve 6120XG HP                      | HP    |
| 2.     | 10GBe | 538113 B21  | Λειτουργική μονάδα διαβίβασης HP 10GbE (PTM)                                  | HP    |
| 3.     | 10GBe | EN4093      | IBM PureFlex λειτουργικής μονάδας μεταβλητού μεγέθους διακόπτη 10 Gigabit ύφασμα EN4093 συστήματος | IBM   |
| 4.     | 1GbE  | 3020        | Cisco Catalyst 3020 1GbE διακόπτη blade                               | Cisco |
| 5.     | 1GbE  | 3020 X       | Blade διακόπτη Cisco Catalyst 3020 X 1GbE                              | Cisco |
| 6.     | 1GbE  | 438030 B21  | HP 1GbE διακόπτη λειτουργική μονάδα - GbE2c επιπέδου 2/3 Ethernet Blade εναλλαγή       | HP    |
| 7.     | 1GbE  | 6120G       | HP ProCurve 6120G/XG 1GbE διακόπτη blade                              | HP    |

## <a name="next-steps"></a>Επόμενα βήματα

[Μάθετε περισσότερα σχετικά με τα στοιχεία του υλικού StorSimple και κατάστασης](storsimple-monitor-hardware-status.md).
