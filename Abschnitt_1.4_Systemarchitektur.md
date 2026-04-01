# 1.4 Schnittstellen und Systemarchitektur

---

## 1.4.1 REST-API (OpenAPI-Spezifikation)

Die vollständige REST-API-Beschreibung des Evently-Servers ist als separate Datei beigefügt:

📄 **`Swagger-REST_02.yaml`** – OpenAPI 3.0 Spezifikation

Die API umfasst folgende Themenbereiche (Tags):

| Tag | Beschreibung |
|-----|-------------|
| Kundenverwaltung | Registrierung, Login (inkl. 2FA), Profilverwaltung |
| Eventverwaltung | Events erstellen, suchen, Tickets kaufen |
| Gruppenverwaltung | Gruppen, Beitrittscodes, Matchmaking |
| Notizen & Kommunikation | Notizen, Chat, Terminabgleich |
| Umfragen | Abstimmungen und Umfragen |
| Aufgaben | Aufgaben und Zuweisungen |
| Finanzverwaltung (Gruppe) | Einzahlungen, Ausgaben, Kassensturz |
| Dienstleister & Anfragen | Anfragen, Angebote, Bewertungen |
| Materialverwaltung | Equipment, Buchungen |
| Personalverwaltung | Mitarbeiter und Arbeitspläne |
| Rechnungsverwaltung | Rechnungen und Zahlungsstatus |

Alle gesicherten Endpunkte erfordern eine Authentifizierung via **JWT Bearer Token**.

---

## 1.4.2 UML-Sequenzdiagramme

Die folgenden Sequenzdiagramme stellen die logisch-zeitlichen Abläufe der relevanten User Stories dar. Sie modellieren die Interaktion zwischen Nutzer, Client, API Gateway, Microservices und Datenbank.

| Datei | User Story |
|-------|-----------|
| `SD_US1_Registrierung_Login.puml` | US1 – Registrierung, Login (2FA) & Passwort zurücksetzen |
| `SD_US2_Event_erstellen.puml` | US2 – Event erstellen und verwalten |
| `SD_US3_Ticket_kaufen.puml` | US3 – Ticket kaufen (Ticketing-Integration) |
| `SD_US4_Gruppe_erstellen_beitreten.puml` | US4 – Gruppe erstellen & per Code beitreten |
| `SD_US5_Dienstleister_Anfrage_Angebot.puml` | US5 – Dienstleister: Anfrage & Angebot |
| `SD_US6_Gruppenfinanzen.puml` | US6 – Gruppenfinanzen & Kassensturz |

---

## 1.4.3 Drei-Stufen-Systemarchitektur (UML-Verteilungsdiagramm)

Das System folgt einer klassischen **Drei-Stufen-Architektur (3-Tier Architecture)**:

```
Stufe 1: Präsentationsschicht  →  Web-Browser / Mobile App (Client)
Stufe 2: Logikschicht          →  API Gateway + Microservices (Anwendungsserver)
Stufe 3: Datenschicht          →  PostgreSQL + S3-Storage (Datenbankserver)
```

Das vollständige UML-Verteilungsdiagramm ist in der Datei **`Verteilungsdiagramm.puml`** definiert und zeigt:

- Alle beteiligten Hardware-Knoten (Clients, Server, externe Dienste)
- Die Kommunikationsverbindungen und verwendeten Protokolle (HTTPS, HTTP intern, SQL, S3 API)
- Den Load Balancer (nginx) mit TLS-Terminierung als Eingangspunkt
- Die Microservice-Struktur auf dem Anwendungsserver
- Externe Abhängigkeiten (Zahlungsanbieter, SMS-Gateway, E-Mail-Provider)

### Kommunikationsprotokoll-Übersicht

| Verbindung | Protokoll | Verschlüsselung |
|-----------|-----------|----------------|
| Client ↔ Load Balancer | HTTPS | TLS 1.3 |
| Load Balancer ↔ API Gateway | HTTP (intern) | Netzwerkisolierung |
| API Gateway ↔ Microservices | REST / HTTP | Internes Netz |
| Microservices ↔ Datenbank | SQL (Port 5432) | TLS verschlüsselt |
| Microservices ↔ Dateispeicher | S3 API | HTTPS |
| Notification-Service ↔ Extern | SMTP / SMS API | HTTPS |
| Finanz-Service ↔ Zahlung | Payment API | HTTPS / PCI-DSS |

---

## 1.4.4 Sicherheitsrisiken und STRIDE-Bedrohungsmodellierung

Die vollständige STRIDE-Analyse ist in der Datei **`STRIDE_Risikoanalyse.md`** dokumentiert. Nachfolgend eine Zusammenfassung.

### Was ist STRIDE?

STRIDE ist ein Bedrohungsmodellierungsframework von Microsoft mit sechs Kategorien:

| Kategorie | Bedrohungstyp | Verletztes Schutzziel |
|-----------|--------------|----------------------|
| **S** – Spoofing | Identitätstäuschung | Authentizität |
| **T** – Tampering | Datenmanipulation | Integrität |
| **R** – Repudiation | Abstreitbarkeit von Aktionen | Nachvollziehbarkeit |
| **I** – Information Disclosure | Unberechtigte Datenweitergabe | Vertraulichkeit |
| **D** – Denial of Service | Dienstverweigerung | Verfügbarkeit |
| **E** – Elevation of Privilege | Rechteausweitung | Autorisierung |

### Identifizierte Bedrohungen (Übersicht)

| ID | Kategorie | Bedrohung | Betroffene Komponente | Schutzmaßnahme |
|----|-----------|-----------|----------------------|----------------|
| S1 | Spoofing | Login mit gestohlenen Zugangsdaten | `POST /auth/login` | 2FA, Rate-Limiting, bcrypt |
| S2 | Spoofing | Gefälschter / manipulierter JWT-Token | API Gateway | RS256-Signatur, kurze Laufzeit, Blacklist |
| T1 | Tampering | SQL-Injection in Suchparameter | Alle Query-Endpunkte | Prepared Statements, Input-Validierung |
| T2 | Tampering | Manipulation von Ticketpreisen im Request | `POST /events/{id}/tickets` | Serverseitiger Preis aus DB, kein Client-Preis |
| T3 | Tampering | Änderung von IBAN/Bankdaten | Finanz-Endpunkte | IBAN-Validierung, E-Mail-Bestätigung, Audit-Log |
| R1 | Repudiation | Abstreiten von Finanztransaktionen | Finanz-Service | Unveränderliches Audit-Log, E-Mail-Quittungen |
| R2 | Repudiation | Abstreiten von Angebots-Akzeptanz | Dienstleister-Service | Zeitgestempeltes Log, beidseitige E-Mail-Bestätigung |
| I1 | Info. Disclosure | Zugriff auf fremde Nutzerdaten / IBAN | Alle Nutzer-Endpunkte | RBAC, Datensparsamkeit, keine IBAN im Response |
| I2 | Info. Disclosure | XSS → JWT-Token-Diebstahl aus localStorage | Browser-Client | HttpOnly-Cookie, CSP-Header, Sanitization |
| I3 | Info. Disclosure | Stack Traces in Fehlerantworten | API Gateway | Generische externe Fehlermeldungen |
| D1 | Denial of Service | Brute-Force auf Login/Registrierung | Auth-Service | Rate-Limiting, CAPTCHA, IP-Sperrung |
| D2 | Denial of Service | Upload-Flood über Medien-Endpunkt | `POST /events/{id}/comments` | Dateigröße-Limit, MIME-Validierung, CDN |
| D3 | Denial of Service | Rechenintensive Suchanfragen | Event-Service, DB | Param-Limits, DB-Indizes, Query-Timeout |
| E1 | Elev. of Privilege | Normaler Nutzer nutzt Admin-Funktionen | `PUT /groups/{id}`, `DELETE /events/{id}` | RBAC, JWT-Claims, Least Privilege |
| E2 | Elev. of Privilege | IDOR – Durchnummerieren von Ressourcen-IDs | Alle `{id}`-Endpunkte | Mitgliedschaftsprüfung, UUIDs statt Integer-IDs |
| E3 | Elev. of Privilege | JWT-Claim-Manipulation (`"role": "admin"`) | API Gateway | Strict Signature Validation, kein `alg: none` |

### Übergreifende Schutzmaßnahmen

| Maßnahme | Adressierte Kategorien |
|----------|----------------------|
| **HTTPS/TLS** für alle Verbindungen | T, I |
| **DSGVO-Konformität** (Datensparsamkeit, `DELETE /users/me`) | I |
| **Secrets Management** (keine Keys im Code, Env-Variablen) | T, I, E |
| **Dependency Scanning** (CVE-Prüfung aller Libraries) | T, D, E |
| **Zentrales Logging & Monitoring** mit Anomalie-Alerting | R, D |
| **Regelmäßige Penetrationstests** | S, T, I, E |
| **DB-Zugriff nur über Service-Accounts** mit minimalen Rechten | T, I, E |
