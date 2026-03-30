# API Endpunkte Checkliste

Nutze diese Tabelle, um zu tracken, welche Use Cases und Endpunkte du bereits überprüft hast.

| Geprüft | Kategorie (Tag) | Methode | Pfad | Beschreibung |
|:---:|---|---|---|---|
| [x] | KundenPverwaltung | **POST** | `/auth/register` | Konto erstellen |
| [x] | Kundenverwaltung | **POST** | `/auth/login` | Anmelden |
| [x] | Kundenverwaltung | **POST** | `/auth/forgot-password` | Passwort zurücksetzen (E-Mail anfordern) |
| [x] | Kundenverwaltung | **POST** | `/auth/reset-password` | Neues Passwort setzen |
| [x] | Kundenverwaltung | **GET** | `/users/me` | Eigenes Profil abrufen |
| [x] | Kundenverwaltung | **PUT** | `/users/me` | Profildaten & Präferenzen aktualisieren |
| [x] | Kundenverwaltung | **DELETE** | `/users/me` | Konto löschen |
| [x] | Kundenverwaltung | **GET** | `/users/me/events` | Meine Eventliste anzeigen (Teilnehmer/Organisator/Interessiert) |
| [x] | Kundenverwaltung | **POST** | `/users/me/subscription` | Premium Abo abschließen / Zahlungsart angeben |
| [x] | Kundenverwaltung | **DELETE** | `/users/me/subscription` | Premium Abo kündigen |
| [x] | Eventverwaltung | **GET** | `/events` | Events suchen und filtern |
| [x] | Eventverwaltung | **POST** | `/events` | Event erstellen |
| [x] | Eventverwaltung | **GET** | `/events/{eventId}` | Details eines Events / Event Homepage |
| [x] | Eventverwaltung | **PUT** | `/events/{eventId}` | Event bearbeiten |
| [x] | Eventverwaltung | **DELETE** | `/events/{eventId}` | Event löschen / archivieren |
| [x] | Eventverwaltung | **POST** | `/events/{eventId}/interested` | Event zur eigenen Beobachtungsliste hinzufügen |
| [x] | Eventverwaltung | **GET** | `/events/{eventId}/comments` | Event kommentieren und bewerten (Medien abrufen) |
| [x] | Eventverwaltung | **POST** | `/events/{eventId}/comments` | Kommentare, Videos und Bilder für ein Event hochladen |
| [x] | Eventverwaltung | **GET** | `/events/{eventId}/resources` | Gesamtübersicht gebuchter Ressourcen (Personal & Material) |
| [x] | Eventverwaltung | **POST** | `/events/{eventId}/ticket-types` | Ticket-Typ erstellen (Veranstalter) |
| [x] | Eventverwaltung | **POST** | `/events/{eventId}/tickets` | Ticket kaufen (Ticketing Integration) |
| [x] | Gruppenverwaltung | **GET** | `/groups` | Meine Gruppen anzeigen (sortierbar) |
| [x] | Gruppenverwaltung | **POST** | `/groups` | Neue Gruppe erstellen |
| [x] | Gruppenverwaltung | **GET** | `/groups/{groupId}` | Gruppendetails |
| [x] | Gruppenverwaltung | **PUT** | `/groups/{groupId}` | Gruppe verwalten (Admin) |
| [x] | Gruppenverwaltung | **DELETE** | `/groups/{groupId}` | Gruppe löschen |
| [x] | Gruppenverwaltung | **POST** | `/groups/join` | Gruppe per Beitritts-Code beitreten |
| [x] | Gruppenverwaltung | **POST** | `/groups/{groupId}/leave` | Gruppe verlassen |
| [x] | Gruppenverwaltung | **GET** | `/groups/{groupId}/members` | Mitgliederliste anzeigen |
| [x] | Gruppenverwaltung | **PUT** | `/groups/{groupId}/members/{userId}/role` | Rolle/Rechte zuweisen (Admin) |
| [x] | Gruppenverwaltung | **DELETE** | `/groups/{groupId}/members/{userId}` | Mitglied entfernen / sperren (Admin) |
| [x] | Gruppenverwaltung | **GET** | `/groups/{groupId}/matchmaking` | Matchmaking - Andere suchende Personen finden |
| [x] | Gruppenverwaltung | **PUT** | `/groups/{groupId}/matchmaking` | Eigenen Matchmaking-Status setzen |
| [x] | Notizen & Kommunikation | **GET** | `/groups/{groupId}/messages` | Chat-Nachrichten abrufen |
| [x] | Notizen & Kommunikation | **POST** | `/groups/{groupId}/messages` | Nachricht senden |
| [x] | Notizen & Kommunikation | **GET** | `/groups/{groupId}/notes` | Notizen abrufen |
| [x] | Notizen & Kommunikation | **POST** | `/groups/{groupId}/notes` | Notiz (Anmerkung/Termin) erstellen |
| [x] | Notizen & Kommunikation | **PUT** | `/groups/{groupId}/notes/{noteId}` | Notiz ändern |
| [x] | Notizen & Kommunikation | **DELETE** | `/groups/{groupId}/notes/{noteId}` | Notiz löschen |
| [x] | Umfragen | **GET** | `/groups/{groupId}/polls` | Umfragen abrufen |
| [x] | Umfragen | **POST** | `/groups/{groupId}/polls` | Umfrage erstellen |
| [x] | Umfragen | **POST** | `/groups/{groupId}/polls/{pollId}/vote` | An Umfrage teilnehmen |
| [x] | Aufgaben | **PUT** | `/groups/{groupId}/tasks/consent` | Zustimmung für Aufgabenzuweisung umschalten |
| [x] | Aufgaben | **GET** | `/groups/{groupId}/tasks` | Zeitpläne und Aufgaben abrufen |
| [x] | Aufgaben | **POST** | `/groups/{groupId}/tasks` | Aufgabe erstellen und zuweisen |
| [x] | Aufgaben | **PUT** | `/groups/{groupId}/tasks/{taskId}` | Aufgabe bearbeiten |
| [x] | Aufgaben | **DELETE** | `/groups/{groupId}/tasks/{taskId}` | Aufgabe löschen |
| [x] | Aufgaben | **PUT** | `/groups/{groupId}/tasks/{taskId}/complete` | Aufgabe als fertig markieren |
| [x] | Finanzverwaltung (Gruppe) | **GET** | `/groups/{groupId}/finances/expenses` | Ausgabenliste anzeigen |
| [x] | Finanzverwaltung (Gruppe) | **POST** | `/groups/{groupId}/finances/expenses` | Ausgabe eintragen |
| [x] | Finanzverwaltung (Gruppe) | **GET** | `/groups/{groupId}/finances/payments` | Alle Einzahlungen anzeigen |
| [x] | Finanzverwaltung (Gruppe) | **POST** | `/groups/{groupId}/finances/payments` | Geldsumme einzahlen / Überweisen |
| [x] | Finanzverwaltung (Gruppe) | **GET** | `/groups/{groupId}/finances/balance` | Kassensturz |
| [x] | Finanzverwaltung (Gruppe) | **POST** | `/groups/{groupId}/finances/refund` | Rückzahlung veranlassen (Organisator) |
| [ ] | Dienstleister & Anfragen | **GET** | `/providers/{providerId}/customers` | Kunden des Dienstleisters einsehen |
| [x] | Dienstleister & Anfragen | **POST** | `/providers/{providerId}/requests` | Veranstaltungsanfrage senden (Kunde) |
| [x] | Dienstleister & Anfragen | **POST** | `/providers/offers` | Angebot generieren |
| [x] | Dienstleister & Anfragen | **POST** | `/providers/offers/{offerId}/convert` | Veranstaltungsanfrage zu Event konvertieren |
| [x] | Dienstleister & Anfragen | **GET** | `/providers/{providerId}/reviews` | Bewertungen des Dienstleisters einsehen |
| [x] | Dienstleister & Anfragen | **POST** | `/providers/{providerId}/reviews` | Dienstleister bewerten (Veranstalter) |
| [ ] | Materialverwaltung | **GET** | `/materials` | Material filtern / suchen |
| [x] | Materialverwaltung | **POST** | `/materials` | Material hinzufügen |
| [x] | Materialverwaltung | **PUT** | `/materials/{materialId}` | Material bearbeiten |
| [x] | Materialverwaltung | **DELETE** | `/materials/{materialId}` | Material löschen |
| [x] | Materialverwaltung | **GET** | `/materials/categories` | Material Kategorien anzeigen |
| [x] | Materialverwaltung | **POST** | `/materials/categories` | Material Kategorie hinzufügen |
| [x] | Materialverwaltung | **DELETE** | `/materials/categories/{categoryId}` | Material Kategorie löschen |
| [x] | Materialverwaltung | **PUT** | `/materials/categories/{categoryId}` | Material Kategorie bearbeiten |
| [x] | Materialverwaltung | **POST** | `/materials/{materialId}/book` | Material buchen |
| [x] | Materialverwaltung | **PUT** | `/materials/{materialId}/book/{bookingId}` | Materialbuchung nachträglich anpassen |
| [x] | Materialverwaltung | **GET** | `/materials/{materialId}/history` | Verleih-Historie anzeigen |
| [x] | Materialverwaltung | **GET** | `/materials/{materialId}/amortization` | Amortisierung anzeigen / Fortschritt |
| [x] | Personalverwaltung | **GET** | `/employees` | Mitarbeiter filtern / suchen |
| [x] | Personalverwaltung | **POST** | `/employees` | Mitarbeiter hinzufügen |
| [x] | Personalverwaltung | **PUT** | `/employees/{employeeId}` | Mitarbeiter bearbeiten |
| [x] | Personalverwaltung | **DELETE** | `/employees/{employeeId}` | Mitarbeiter löschen |
| [x] | Personalverwaltung | **POST** | `/employees/{employeeId}/assign` | Mitarbeiter einem Event zuordnen |
| [x] | Personalverwaltung | **GET** | `/employees/{employeeId}/schedule` | Zeit-/Arbeitsplan anzeigen |
| [x] | Rechnungsverwaltung | **GET** | `/invoices` | Rechnungen filtern |
| [x] | Rechnungsverwaltung | **POST** | `/invoices` | Rechnung generieren |
| [x] | Rechnungsverwaltung | **GET** | `/invoices/{invoiceId}` | Rechnungsdetails / PDF abrufen |
| [x] | Rechnungsverwaltung | **PUT** | `/invoices/{invoiceId}/pay` | Event/Rechnung als bezahlt markieren |
| [x] | Rechnungsverwaltung | **POST** | `/invoices/{invoiceId}/remind` | Mahnung/Erinnerung erstellen |
