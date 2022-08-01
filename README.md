# Super Market
Αρχικά για την ενεργοποίηση του service η διαδικασία που πρέπει να ολοκληρωθεί είναι η παρακάτω. Πρέπει να είναι εγκατεστημένο στον υπολογιστή  το Docker,το οποίο θα κάνει δυνατή την λειτουργία συγκεκριμέων containers και images,με χρήση των αρχείων docker_compose.yml και Dockerfile.Αφού βάλουμε σε έναν φάκελο τα αρχεία αυτά και το project.py ανούγουμε ένα terminal,μπαίνουμε στο directory του φακέλου και εκτελούμε την εντολή docker-compose up -d.
Πλέον το service είναι έτοιμο για να εκτελεστούν τα endpoints τα οποία βρίσκονται στο αρχείο project.py. Τέλος πρέπει να υπάρχει διαθέσιμο κάποιο πρόγραμμα που επιτρέπει την χρήση HTTP μεθόδων(κατά προτίμηση postman).Οι browser δέχονται μόνο get μεθόδους. Με την εντολή docker-compose down μπορούμε να σταματήσουμε το service.
# Αναλυτικά τα endpoints.
Αρχικά στις γραμμες 36-47 έχουμε  function που δημιουργεί μοναδικό κωδικό αυθεντικοποίησης του εκάστοτε χρήστη.

Στις γραμμές 51-79 βρίσκεται το πρώτο endpoint,όπου γίνεται η δημιουργία ενός απλού χρήστη.Πρώτα γίνεται έλεγχος ότι το json που βάζουμε είναι σωστό και πως υπάρχει "όνομα,email,κωδικός ssn(AMKA) και κατηγορία χρήστη".Γίνεται έλεγχος ώστε το ΑΜΚΑ να μην έχει ΗΗ πάνω απο 31 και Μήνα πάνω απο 12.Στην συνέχεια αν δεν υπάρχει ήδη χρήστης με το ιδίο email ολοκληρώνεται η εγγραφή.

Έπειτα στις γραμμές 81-107 είναι το δεύτερο endpoint όπου γίνεται το login ενός απλού χρήστη.Γίνεται ελεγχός για την εγκυρότητα των στοιχείων που εισάγουμε,και εφοόσον είναι σωστά δίνεται ενάς κωδικός(uuid) που αντιστοιχεί για το τωρινό session του χρήστη.

Στις γραμμές 112-153 είναι το τρίτο endpoint όπου γίνεται αναζήτηση προιόντος βάσει ονόματος,κατηγορίας ή κωδικού.Εφόσον το uuid είναι έγκυρο(συμβαίνει στα περισσότερα endpoint οπότε δεν θα ξαναναφερθώ σε αυτό),και βάσει του κριτηρίου αναζήτησης που βάζουμε,παρουσιάζονται τα αντίστοιχα προιόντα με την κατάλληλη ταξινόμηση.Σε περίπτωση που δεν εισαγάγουμε μόνο ένα κριτήριο υπάρχει αντίστοιχο μήνυμα.

Στις γραμμές 163-207 είναι το τέταρτο endpoint όπου γίνεται προσθήκη προιόντος στο καλάθι βάσει του κωδικού του.Μετά τους ελέγχους,έφοσον η ποσότητα του προιόντος που ζητάμε είναι μικρότερη απο το στοκ υπολογίζεται η τιμή και προστίθεται σε μια λίστα που αντιστοιχεί στο καλάθι.

Στις γραμμές 213-223 υπάρχει το endpoint όπου εμφανίζεται το καλάθι.

Στις γραμμές 227-258,υπάρχει το endpoint που αφορά την διαγραφή προιόντος απο το καλάθι.Εφόσον η λίστα του καλαθιού δεν είναι άδεια αφαιρούμε απο αυτήν τα προιόντα που έχουν ίδιο id με αυτό που βάλαμε.

Στις γραμμές 260-273 είναι το endpoint της αγοράς.Υπάρχει έλγεχος ώστε τα ψηφία της κάρτας να είναι 16.Δεν έχει υλοποιηθεί ο έλεγχος για την ηλικία του πελάτη.

Στις γραμμές 277-299 είναι το endpoint για την παρουσίαση του ιστορικού αγορών.Με δεδόμενο το email του χρήστη που χρησιμοποιεί το συγκεκριμένο session προσθέτουμε το orderhistory στον συγκεκριμένο user με χρήση του update με μέθοδο Patch.

Στις γραμμές 302-311 το endpoint κάνει διαγραφεί του χρήστη που είναι συνδεδεμένος,απλά με χρήση της εντολής delete_one.

Έπειτα στις γραμμες 319-328 υπάρχει η δημιουργία αντίστοιχου session για τον admin.

Στις γραμμές 332-355 γίνεται η δημιουργία λογαριασμόυ Admin. Αφού ελέγχουμε ότι έχουμε περάσει όλα τα απαραίτητα δεδομένα και εφόσον δεν υπάρχει άλλος admin με ίδιο email προστίθεται στο collection.

Στις γραμμές 360-385 γίνεται login του admin παρόμοια με του απλού user.Εδώ λοιπόν υπάρχει αντίστοιχα το create_admin_session όπου παίρνει uuid κωδικό ο admin.

Στις γραμμές 388-408 γίνεται δημιουργία ενός προιόντος. Μετά τους απαραίτητους ελέγχους,τσεκάρουμε ότι έχουμε προσθέσει ολα τα δεδομένα(όνομα,τιμή κλπ) και το προσθέτουμε απλά με το insert_one στο collection.

Στις γραμμές 412-426 παρόμοια με την διαγραφή προιόντους απο το καλάθι του χρήστη,αφού δούμε ότι υπάρχει το προιόν με το συγκεκριμένο id που δώσαμε το διαγράφουμε απλά απο το collection products με το delete_one

Στις γραμμές 429-459,το endpoint έχει να κάνει με ενημέρωση ενός προιόντος.Πάλι μετά τους ελέγχους,ελέγχουμε αν υπάρχει το προιόν με το συγκεκριμένο id που έχουμε δώσει,και εφόσον υπάρχει ενημερώνουμε το collection με τα ανανεωμένα στοιχεία του με το update_one.Αν δεν υπάρχει προιόν με το συγκεκριμένο id όπως σε όλα τα endpoint βγαίνει το ανάλογο μήνυμα σφάλματος.



