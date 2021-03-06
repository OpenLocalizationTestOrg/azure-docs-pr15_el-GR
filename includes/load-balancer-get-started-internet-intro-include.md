Μια μονάδα εξισορρόπησης φόρτου Azure είναι μια μονάδα εξισορρόπησης φόρτου επιπέδου-4 (TCP, UDP). Η μονάδα εξισορρόπησης φόρτου παρέχει υψηλή διαθεσιμότητα από τη διανομή της εισερχόμενης κυκλοφορίας μεταξύ παρουσιών υπηρεσίας υγιή στις υπηρεσίες νέφους ή εικονικές μηχανές σε ένα σύνολο εξισορρόπησης φόρτου. Μονάδα εξισορρόπησης φόρτου Azure επίσης να παρουσιάσετε αυτές τις υπηρεσίες σε πολλαπλές θύρες, πολλές διευθύνσεις IP ή και τα δύο.

Μπορείτε να ρυθμίσετε μια μονάδα εξισορρόπησης φόρτου για να:

* Η φόρτωση του υπολοίπου εισερχόμενη κυκλοφορία Internet στα εικονικά μηχανήματα (ΣΠΣ). Αναφερόμαστε σε μια μονάδα εξισορρόπησης φόρτου σε αυτό το σενάριο ως μια [μονάδα εξισορρόπησης φόρτου βρίσκονται εκτεθειμένοι με το Internet](../articles/load-balancer/load-balancer-internet-overview.md).
* Φόρτωση κίνηση ισορροπία μεταξύ των VM σε ένα εικονικό δίκτυο (VNet), μεταξύ των VM σε υπηρεσίες νέφους ή ανάμεσα σε υπολογιστές εσωτερικής εγκατάστασης και VM σε ένα εικονικό δίκτυο μεταξύ εγκαταστάσεις. Αναφερόμαστε σε μια μονάδα εξισορρόπησης φόρτου σε αυτό το σενάριο ως ένα [εσωτερικό εξισορρόπηση φόρτου (ILB)](../articles/load-balancer/load-balancer-internal-overview.md).
* Εξωτερική κίνηση προς τα εμπρός σε μια συγκεκριμένη παρουσία VM.
