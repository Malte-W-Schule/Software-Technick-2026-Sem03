# STRIDE-Bedrohungsmodellierung – Evently Event-System

## Überblick

Die STRIDE-Methode ist ein von Microsoft entwickeltes Framework zur systematischen Identifikation von Sicherheitsbedrohungen in Softwaresystemen. Der Name ist ein Akronym aus sechs Bedrohungskategorien, die jeweils ein spezifisches Schutzziel verletzen.

| Kategorie | Bedrohung | Verletztes Schutzziel |
|-----------|-----------|----------------------|
| **S** – Spoofing | Identitätstäuschung | Authentizität |
| **T** – Tampering | Datenmanipulation | Integrität |
| **R** – Repudiation | Abstreitbarkeit von Aktionen | Nachvollziehbarkeit |
| **I** – Information Disclosure | Unberechtigte Datenweitergabe | Vertraulichkeit |
| **D** – Denial of Service | Dienstverweigerung | Verfügbarkeit |
| **E** – Elevation of Privilege | Rechteausweitung | Autorisierung |

---

## Systemkontext

Das Evently Event-System ist ein Client-Server-System mit einer dreistufigen Architektur:

- **Stufe 1 (Client):** Web-Browser und Mobile App
- **Stufe 2 (Server):** API Gateway, Auth-Service und domänenspezifische Microservices (Event, Gruppe, Finanzen, Dienstleister, Benachrichtigungen)
- **Stufe 3 (Daten):** PostgreSQL-Datenbank, S3-kompatibler Medienspeicher

Besonders schützenswerte Assets sind:
- Nutzerdaten (E-Mail, Telefon, IBAN, Passwort-Hashes)
- JWT-Authentifizierungstoken
- Finanzdaten (Einzahlungen, Ausgaben, Rückerstattungen)
- Ticket-PDFs mit QR-Codes

---

## S – Spoofing (Identitätstäuschung)

### S1 – Credential-basierter Identitätsbetrug
| Feld | Inhalt |
|------|--------|
| **Bedrohung** | Ein Angreifer meldet sich mit gestohlenen Zugangsdaten (E-Mail + Passwort) als legitimer Nutzer an. |
| **Betroffene Komponente** | `POST /auth/login`, Auth-Service |
| **Wahrscheinlichkeit** | Hoch (Credential-Stuffing-Angriffe sind weit verbreitet) |
| **Schutzmaßnahmen** | Zwei-Faktor-Authentifizierung (2FA via SMS, E-Mail oder App); Rate-Limiting (max. 5 Fehlversuche/Minute pro IP); Kontensperre nach Mehrfachfehlschlägen; bcrypt-Passwort-Hashing mit Salt |

### S2 – JWT-Token-Fälschung
| Feld | Inhalt |
|------|--------|
| **Bedrohung** | Ein Angreifer erstellt oder manipuliert ein JWT-Token, um sich als anderer Nutzer oder als Administrator auszugeben. |
| **Betroffene Komponente** | API Gateway, alle gesicherten Endpunkte |
| **Wahrscheinlichkeit** | Mittel |
| **Schutzmaßnahmen** | Starke Signierung der JWTs (RS256 oder HS256 mit sicherem Secret); kurze Token-Laufzeit (z.B. 15 min) + Refresh-Token-Mechanismus; Token-Blacklist bei Logout; Prüfung des `alg`-Headers gegen „none"-Angriff |

---

## T – Tampering (Datenmanipulation)

### T1 – SQL-Injection
| Feld | Inhalt |
|------|--------|
| **Bedrohung** | Angreifer injiziert SQL-Code in API-Parameter (z.B. Suchfelder, Filter), um Datenbankdaten zu lesen, zu ändern oder zu löschen. |
| **Betroffene Komponente** | Alle Endpunkte mit Query-Parametern (z.B. `GET /events?radius=...`) |
| **Wahrscheinlichkeit** | Mittel |
| **Schutzmaßnahmen** | Ausschließlicher Einsatz von Prepared Statements / ORM; serverseitige Eingabevalidierung aller Parameter; Web Application Firewall (WAF) |

### T2 – Manipulation von Ticketpreisen und Finanzdaten
| Feld | Inhalt |
|------|--------|
| **Bedrohung** | Ein Nutzer manipuliert den Request-Body beim Ticketkauf (z.B. `price: 0.01` statt des echten Preises), um Tickets zu unterbezahlen. |
| **Betroffene Komponente** | `POST /events/{eventId}/tickets`, Finanz-Service |
| **Wahrscheinlichkeit** | Hoch |
| **Schutzmaßnahmen** | Preis wird **ausschließlich serverseitig** anhand der Ticket-Typ-ID aus der Datenbank ermittelt; kein clientseitiger Preis wird akzeptiert; HTTPS schützt den Transportweg |

### T3 – Manipulation von IBAN- und Bankdaten
| Feld | Inhalt |
|------|--------|
| **Bedrohung** | Angreifer ändert gespeichertes Bankkonto (IBAN/BIC) für Gruppenfinanzierungen oder Auszahlungen, um Gelder umzuleiten. |
| **Betroffene Komponente** | `POST /events`, `POST /groups/{groupId}/finances`, Finanz-Service |
| **Wahrscheinlichkeit** | Niedrig (erfordert Authentifizierung) |
| **Schutzmaßnahmen** | IBAN-Validierung nach SEPA-Standard serverseitig; Änderungen an Bankdaten erzeugen einen E-Mail-Bestätigungslink; Audit-Logging aller Finanzänderungen |

---

## R – Repudiation (Abstreitbarkeit)

### R1 – Abstreiten von Finanztransaktionen
| Feld | Inhalt |
|------|--------|
| **Bedrohung** | Ein Gruppenleiter oder Mitglied bestreitet, eine Ausgabe getätigt oder eine Einzahlung bestätigt zu haben. |
| **Betroffene Komponente** | Finanz-Service, `POST /groups/{groupId}/finances/expenses` |
| **Wahrscheinlichkeit** | Mittel |
| **Schutzmaßnahmen** | Unveränderliches Audit-Log (Zeitstempel, User-ID, Aktion, IP-Adresse); E-Mail-Bestätigung bei größeren Transaktionen; digitale Quittungen für Ticketkäufe |

### R2 – Abstreiten von Angebots- oder Vertragsannahmen
| Feld | Inhalt |
|------|--------|
| **Bedrohung** | Ein Dienstleister bestreitet, ein Angebot erstellt oder akzeptiert zu haben. |
| **Betroffene Komponente** | `PUT /providers/offers/{offerId}`, Dienstleister-Service |
| **Wahrscheinlichkeit** | Mittel |
| **Schutzmaßnahmen** | Statusänderungen werden mit User-ID, Zeitstempel und IP im Audit-Log festgehalten; E-Mail-Bestätigung an beide Parteien (Veranstalter + Dienstleister) |

---

## I – Information Disclosure (Datenleck)

### I1 – Unbefugter Zugriff auf Nutzerdaten
| Feld | Inhalt |
|------|--------|
| **Bedrohung** | Ein authentifizierter Nutzer ruft persönliche Daten eines anderen Nutzers ab (z.B. E-Mail, Telefonnummer, IBAN). |
| **Betroffene Komponente** | `GET /users/me`, Gruppen-Endpunkte mit Mitgliederlisten |
| **Wahrscheinlichkeit** | Mittel |
| **Schutzmaßnahmen** | Rollenbasierte Zugriffssteuerung (RBAC); API-Responses geben nur die für die Rolle notwendigen Felder zurück (Datensparsamkeit); IBAN wird nie im Klartext an Clients zurückgegeben |

### I2 – XSS-Angriff und Token-Diebstahl
| Feld | Inhalt |
|------|--------|
| **Bedrohung** | Ein Angreifer injiziert JavaScript in Kommentarfelder (z.B. bei Event-Kommentaren). Das Script liest den JWT-Token aus dem `localStorage` und sendet ihn an den Angreifer. |
| **Betroffene Komponente** | Client (Web-Browser), `POST /events/{eventId}/comments` |
| **Wahrscheinlichkeit** | Hoch (ohne Schutzmaßnahmen) |
| **Schutzmaßnahmen** | Output-Encoding aller angezeigten Nutzerdaten; Content-Security-Policy (CSP) im HTTP-Header; JWT in `HttpOnly`-Cookies statt `localStorage`; serverseitige Eingabebereinigung (Sanitization) |

### I3 – Datenleck durch ausführliche Fehlermeldungen
| Feld | Inhalt |
|------|--------|
| **Bedrohung** | Server gibt bei Fehlern interne Informationen preis (Stack Traces, SQL-Fehlermeldungen, Datenbankstruktur), die einem Angreifer bei weiteren Angriffen helfen. |
| **Betroffene Komponente** | API Gateway, alle Microservices |
| **Wahrscheinlichkeit** | Hoch (Standard-Verhalten vieler Frameworks) |
| **Schutzmaßnahmen** | Generische, benutzerfreundliche Fehlermeldungen nach außen; detaillierte Logs ausschließlich in internen Log-Systemen (kein Log-Exposure über HTTP) |

---

## D – Denial of Service (Dienstverweigerung)

### D1 – Brute-Force-Angriff auf Login/Registrierung
| Feld | Inhalt |
|------|--------|
| **Bedrohung** | Massenhafte automatisierte Anfragen an den Login- oder Registrierungsendpunkt überlasten den Auth-Service. |
| **Betroffene Komponente** | `POST /auth/login`, `POST /auth/register` |
| **Wahrscheinlichkeit** | Hoch |
| **Schutzmaßnahmen** | Rate-Limiting (z.B. 10 Requests/Minute pro IP); CAPTCHA bei Registrierung; automatische IP-Sperrung bei Auffälligkeiten; horizontale Skalierung des Auth-Service |

### D2 – Upload-Flood über Medien-Endpunkt
| Feld | Inhalt |
|------|--------|
| **Bedrohung** | Angreifer lädt massenhaft große Bild- und Videodateien hoch, um Speicherplatz und Bandbreite zu erschöpfen. |
| **Betroffene Komponente** | `POST /events/{eventId}/comments` (multipart/form-data) |
| **Wahrscheinlichkeit** | Mittel |
| **Schutzmaßnahmen** | Maximale Dateigröße pro Upload (z.B. 10 MB); MIME-Type-Validierung (nur erlaubte Formate); Rate-Limiting für Upload-Endpunkte; asynchrone Verarbeitung über CDN/Blob-Storage |

### D3 – Ressourcenerschöpfung durch komplexe Suchanfragen
| Feld | Inhalt |
|------|--------|
| **Bedrohung** | Angreifer sendet rechenintensive Suchanfragen (z.B. sehr großer Radius, viele Filter), die den Event-Service überlasten. |
| **Betroffene Komponente** | `GET /events`, Event-Service, Datenbank |
| **Wahrscheinlichkeit** | Mittel |
| **Schutzmaßnahmen** | Maximale Werte für Query-Parameter (z.B. max. Radius = 500 km); Datenbankindizes auf häufig gefilterten Feldern; Query-Timeout-Limits |

---

## E – Elevation of Privilege (Rechteausweitung)

### E1 – Zugriff auf Admin-/Gruppenleiter-Funktionen
| Feld | Inhalt |
|------|--------|
| **Bedrohung** | Ein normaler Nutzer ruft Endpunkte auf, die nur Gruppenleitern oder Veranstaltern zugänglich sein sollen (z.B. Gruppe löschen, Event bearbeiten). |
| **Betroffene Komponente** | `PUT /groups/{groupId}`, `DELETE /events/{eventId}`, Gruppenverwaltung |
| **Wahrscheinlichkeit** | Mittel |
| **Schutzmaßnahmen** | Serverseitige Rollenprüfung bei jedem Request (RBAC); Rollen werden im JWT-Token hinterlegt und vom API Gateway verifiziert; „Principle of Least Privilege" |

### E2 – IDOR (Insecure Direct Object Reference)
| Feld | Inhalt |
|------|--------|
| **Bedrohung** | Ein Nutzer greift durch simples Durchnummerieren von IDs auf Events, Gruppen oder Finanzdaten anderer Nutzer zu, zu denen er keine Berechtigung hat (z.B. `GET /groups/42/finances`). |
| **Betroffene Komponente** | Alle Endpunkte mit Pfadparametern (`{eventId}`, `{groupId}`, etc.) |
| **Wahrscheinlichkeit** | Hoch |
| **Schutzmaßnahmen** | Ressourcenbasierte Autorisierungsprüfung auf dem Server (z.B. Ist der anfragende Nutzer Mitglied der Gruppe mit ID 42?); Verwendung von nicht-erratbaren UUIDs statt sequenzieller Integer-IDs |

### E3 – JWT-Claim-Manipulation
| Feld | Inhalt |
|------|--------|
| **Bedrohung** | Angreifer modifiziert den JWT-Payload (z.B. `"role": "admin"`) und sendet ihn ohne gültige Signatur, in der Hoffnung, dass die Signatur nicht validiert wird. |
| **Betroffene Komponente** | API Gateway, Auth-Service |
| **Wahrscheinlichkeit** | Niedrig (bei korrekter Implementierung) |
| **Schutzmaßnahmen** | Strenge Signaturvalidierung des JWTs bei jedem Request; Ablehnung von Tokens mit `"alg": "none"`; regelmäßige Rotation der Signing-Keys |

---

## Übergreifende Schutzmaßnahmen

Die folgenden Maßnahmen gelten systemweit und reduzieren mehrere Bedrohungskategorien gleichzeitig:

| Maßnahme | Adressiert |
|----------|-----------|
| **HTTPS/TLS** auf allen Verbindungen (Client↔API, API↔DB) | T, I |
| **DSGVO-Konformität**: Datensparsamkeit, Recht auf Löschung (`DELETE /users/me`) | I |
| **Secrets Management**: Keine Passwörter/Keys im Quellcode, Verwendung von Umgebungsvariablen / Secret Stores | T, I, E |
| **Dependency Scanning**: Regelmäßige Prüfung auf bekannte CVEs in verwendeten Bibliotheken | T, D, E |
| **Logging & Monitoring**: Zentrale, manipulationssichere Log-Infrastruktur mit Alerting bei Anomalien | R, D |
| **Regelmäßige Sicherheits-Audits und Penetrationstests** | S, T, I, E |
| **Datenbankzugriff nur über Service-Accounts** mit minimalen Rechten (kein direkter DB-Zugriff von außen) | T, I, E |
