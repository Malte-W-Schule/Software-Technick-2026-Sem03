# API Endpunkte Checkliste

Nutze diese Tabelle, um zu tracken, welche Use Cases und Endpunkte du bereits überprüft hast.

| Geprüft | Kategorie (Tag) | Methode | Pfad | Beschreibung |
|:---:|---|---|---|---|
| [ ] | KundenPverwaltung | **POST** | `/auth/register` | Konto erstellen |
| [ ] | Kundenverwaltung | **POST** | `/auth/login` | Anmelden |
| [ ] | Kundenverwaltung | **POST** | `/auth/forgot-password` | Passwort zurücksetzen (E-Mail anfordern) |
| [ ] | Kundenverwaltung | **POST** | `/auth/reset-password` | Neues Passwort setzen |
| [ ] | Kundenverwaltung | **GET** | `/users/me` | Eigenes Profil abrufen |
| [ ] | Kundenverwaltung | **PUT** | `/users/me` | Profildaten & Präferenzen aktualisieren |
| [ ] | Kundenverwaltung | **DELETE** | `/users/me` | Konto löschen |
| [ ] | Kundenverwaltung | **GET** | `/users/me/events` | Meine Eventliste anzeigen (Teilnehmer/Organisator/Interessiert) |
| [ ] | Kundenverwaltung | **POST** | `/users/me/subscription` | Premium Abo abschließen / Zahlungsart angeben |
| [ ] | Kundenverwaltung | **DELETE** | `/users/me/subscription` | Premium Abo kündigen |
| [ ] | Eventverwaltung | **GET** | `/events` | Events suchen und filtern |
| [ ] | Eventverwaltung | **POST** | `/events` | Event erstellen |
| [ ] | Eventverwaltung | **GET** | `/events/{eventId}` | Details eines Events / Event Homepage |
| [ ] | Eventverwaltung | **PUT** | `/events/{eventId}` | Event bearbeiten |
| [ ] | Eventverwaltung | **DELETE** | `/events/{eventId}` | Event löschen / archivieren |
| [ ] | Eventverwaltung | **POST** | `/events/{eventId}/interested` | Event zur eigenen Beobachtungsliste hinzufügen |
| [ ] | Eventverwaltung | **GET** | `/events/{eventId}/comments` | Event kommentieren und bewerten (Medien abrufen) |
| [ ] | Eventverwaltung | **POST** | `/events/{eventId}/comments` | Kommentare, Videos und Bilder für ein Event hochladen |
| [ ] | Eventverwaltung | **GET** | `/events/{eventId}/resources` | Gesamtübersicht gebuchter Ressourcen (Personal & Material) |
| [ ] | Eventverwaltung | **POST** | `/events/{eventId}/ticket-types` | Ticket-Typ erstellen (Veranstalter) |
| [ ] | Eventverwaltung | **POST** | `/events/{eventId}/tickets` | Ticket kaufen (Ticketing Integration) |
| [ ] | Gruppenverwaltung | **GET** | `/groups` | Meine Gruppen anzeigen (sortierbar) |
| [ ] | Gruppenverwaltung | **POST** | `/groups` | Neue Gruppe erstellen |
| [ ] | Gruppenverwaltung | **GET** | `/groups/{groupId}` | Gruppendetails |
| [ ] | Gruppenverwaltung | **PUT** | `/groups/{groupId}` | Gruppe verwalten (Admin) |
| [ ] | Gruppenverwaltung | **DELETE** | `/groups/{groupId}` | Gruppe löschen |
| [ ] | Gruppenverwaltung | **POST** | `/groups/join` | Gruppe per Beitritts-Code beitreten |
| [ ] | Gruppenverwaltung | **POST** | `/groups/{groupId}/leave` | Gruppe verlassen |
| [ ] | Gruppenverwaltung | **GET** | `/groups/{groupId}/members` | Mitgliederliste anzeigen |
| [ ] | Gruppenverwaltung | **PUT** | `/groups/{groupId}/members/{userId}/role` | Rolle/Rechte zuweisen (Admin) |
| [ ] | Gruppenverwaltung | **DELETE** | `/groups/{groupId}/members/{userId}` | Mitglied entfernen / sperren (Admin) |
| [ ] | Gruppenverwaltung | **GET** | `/groups/{groupId}/matchmaking` | Matchmaking - Andere suchende Personen finden |
| [ ] | Gruppenverwaltung | **PUT** | `/groups/{groupId}/matchmaking` | Eigenen Matchmaking-Status setzen |
| [ ] | Notizen & Kommunikation | **GET** | `/groups/{groupId}/messages` | Chat-Nachrichten abrufen |
| [ ] | Notizen & Kommunikation | **POST** | `/groups/{groupId}/messages` | Nachricht senden |
| [ ] | Notizen & Kommunikation | **GET** | `/groups/{groupId}/notes` | Notizen abrufen |
| [ ] | Notizen & Kommunikation | **POST** | `/groups/{groupId}/notes` | Notiz (Anmerkung/Termin) erstellen |
| [ ] | Notizen & Kommunikation | **PUT** | `/groups/{groupId}/notes/{noteId}` | Notiz ändern |
| [ ] | Notizen & Kommunikation | **DELETE** | `/groups/{groupId}/notes/{noteId}` | Notiz löschen |
| [ ] | Umfragen | **GET** | `/groups/{groupId}/polls` | Umfragen abrufen |
| [ ] | Umfragen | **POST** | `/groups/{groupId}/polls` | Umfrage erstellen |
| [ ] | Umfragen | **POST** | `/groups/{groupId}/polls/{pollId}/vote` | An Umfrage teilnehmen |
| [ ] | Aufgaben | **PUT** | `/groups/{groupId}/tasks/consent` | Zustimmung für Aufgabenzuweisung umschalten |
| [ ] | Aufgaben | **GET** | `/groups/{groupId}/tasks` | Zeitpläne und Aufgaben abrufen |
| [ ] | Aufgaben | **POST** | `/groups/{groupId}/tasks` | Aufgabe erstellen und zuweisen |
| [ ] | Aufgaben | **PUT** | `/groups/{groupId}/tasks/{taskId}` | Aufgabe bearbeiten |
| [ ] | Aufgaben | **DELETE** | `/groups/{groupId}/tasks/{taskId}` | Aufgabe löschen |
| [ ] | Aufgaben | **PUT** | `/groups/{groupId}/tasks/{taskId}/complete` | Aufgabe als fertig markieren |
| [ ] | Finanzverwaltung (Gruppe) | **GET** | `/groups/{groupId}/finances/expenses` | Ausgabenliste anzeigen |
| [ ] | Finanzverwaltung (Gruppe) | **POST** | `/groups/{groupId}/finances/expenses` | Ausgabe eintragen |
| [ ] | Finanzverwaltung (Gruppe) | **GET** | `/groups/{groupId}/finances/payments` | Alle Einzahlungen anzeigen |
| [ ] | Finanzverwaltung (Gruppe) | **POST** | `/groups/{groupId}/finances/payments` | Geldsumme einzahlen / Überweisen |
| [ ] | Finanzverwaltung (Gruppe) | **GET** | `/groups/{groupId}/finances/balance` | Kassensturz |
| [ ] | Finanzverwaltung (Gruppe) | **POST** | `/groups/{groupId}/finances/refund` | Rückzahlung veranlassen (Organisator) |
| [ ] | Dienstleister & Anfragen | **GET** | `/providers/{providerId}/customers` | Kunden des Dienstleisters einsehen |
| [ ] | Dienstleister & Anfragen | **POST** | `/providers/{providerId}/requests` | Veranstaltungsanfrage senden (Kunde) |
| [ ] | Dienstleister & Anfragen | **POST** | `/providers/offers` | Angebot generieren |
| [ ] | Dienstleister & Anfragen | **POST** | `/providers/offers/{offerId}/convert` | Veranstaltungsanfrage zu Event konvertieren |
| [ ] | Dienstleister & Anfragen | **GET** | `/providers/{providerId}/reviews` | Bewertungen des Dienstleisters einsehen |
| [ ] | Dienstleister & Anfragen | **POST** | `/providers/{providerId}/reviews` | Dienstleister bewerten (Veranstalter) |
| [ ] | Materialverwaltung | **GET** | `/materials` | Material filtern / suchen |
| [ ] | Materialverwaltung | **POST** | `/materials` | Material hinzufügen |
| [ ] | Materialverwaltung | **PUT** | `/materials/{materialId}` | Material bearbeiten |
| [ ] | Materialverwaltung | **DELETE** | `/materials/{materialId}` | Material löschen |
| [ ] | Materialverwaltung | **GET** | `/materials/categories` | Material Kategorien anzeigen |
| [ ] | Materialverwaltung | **POST** | `/materials/categories` | Material Kategorie hinzufügen |
| [ ] | Materialverwaltung | **DELETE** | `/materials/categories/{categoryId}` | Material Kategorie löschen |
| [ ] | Materialverwaltung | **PUT** | `/materials/categories/{categoryId}` | Material Kategorie bearbeiten |
| [ ] | Materialverwaltung | **POST** | `/materials/{materialId}/book` | Material buchen |
| [ ] | Materialverwaltung | **GET** | `/materials/{materialId}/history` | Verleih-Historie anzeigen |
| [ ] | Materialverwaltung | **GET** | `/materials/{materialId}/amortization` | Amortisierung anzeigen / Fortschritt |
| [ ] | Personalverwaltung | **GET** | `/employees` | Mitarbeiter filtern / suchen |
| [ ] | Personalverwaltung | **POST** | `/employees` | Mitarbeiter hinzufügen |
| [ ] | Personalverwaltung | **PUT** | `/employees/{employeeId}` | Mitarbeiter bearbeiten |
| [ ] | Personalverwaltung | **DELETE** | `/employees/{employeeId}` | Mitarbeiter löschen |
| [ ] | Personalverwaltung | **POST** | `/employees/{employeeId}/assign` | Mitarbeiter einem Event zuordnen |
| [ ] | Personalverwaltung | **GET** | `/employees/{employeeId}/schedule` | Zeit-/Arbeitsplan anzeigen |
| [ ] | Rechnungsverwaltung | **GET** | `/invoices` | Rechnungen filtern |
| [ ] | Rechnungsverwaltung | **POST** | `/invoices` | Rechnung generieren |
| [ ] | Rechnungsverwaltung | **GET** | `/invoices/{invoiceId}` | Rechnungsdetails / PDF abrufen |
| [ ] | Rechnungsverwaltung | **PUT** | `/invoices/{invoiceId}/pay` | Event/Rechnung als bezahlt markieren |
| [ ] | Rechnungsverwaltung | **POST** | `/invoices/{invoiceId}/remind` | Mahnung/Erinnerung erstellen |
