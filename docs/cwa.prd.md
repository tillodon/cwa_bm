
### **Skizze eines PRD (Product Requirements Document) für die Corona-Warn-App**

**1. Einführung**
* **1.1 Projektüberblick:** Eine kurze, prägnante Zusammenfassung des Projekts, seiner Ziele und seines Zwecks.
    * *Ziel:* Die Ausbreitung von COVID-19 in Deutschland durch eine digitale Lösung zur Kontaktnachverfolgung zu verlangsamen.
    * *Problembeschreibung:* Die manuelle Kontaktnachverfolgung ist zeitaufwändig und skaliert nicht effizient bei einer hohen Anzahl von Infektionen.
    * *Lösung:* Eine dezentrale, datenschutzfreundliche App, die anonyme Risikobegegnungen erkennt und Nutzer benachrichtigt.

**2. Geschäftsziele & Erfolgskennzahlen**
* **2.1 Geschäftsziele:**
    * Reduzierung der Neuinfektionen.
    * Frühes Erkennen von Risikokontakten, um die Quarantäne zu beschleunigen.
    * Erhöhung der Transparenz und des Vertrauens in die staatlichen Maßnahmen.
* **2.2 Erfolgskennzahlen (KPIs):**
    * Anzahl der App-Downloads und aktiven Nutzer.
    * Anteil der Nutzer, die ein positives Testergebnis in der App teilen.
    * Anzahl der ausgelösten Risikowarnungen.
    * (Indirekt) Korrelation zwischen App-Nutzung und regionaler R-Rate.

**3. Nutzergruppen & Personas**
* **3.1 Hauptnutzer:**
    * Bürger, die die App zur persönlichen Risikoüberwachung nutzen möchten.
    * Personen, die positiv getestet wurden und andere anonym warnen wollen.
    * (Später) Reisende, die einen digitalen Nachweis benötigen.

**4. Funktionsanforderungen (Product Requirements)**
* **4.1 Core-Funktionen (MVP - Minimum Viable Product):**
    * **Risikoeinschätzung:**
        * Fähigkeit, anonyme Bluetooth-Begegnungen zu protokollieren.
        * Algorithmus zur Berechnung des Risikos basierend auf Dauer und Nähe der Begegnungen.
        * Benachrichtigung bei erhöhtem Risiko.
    * **Testergebnis-Meldung:**
        * Sichere und anonyme Übermittlung eines positiven Testergebnisses von einer autorisierten Stelle (Labor).
        * Möglichkeit für den Nutzer, die anonyme Warnung auszulösen.
    * **Datenschutz:**
        * Dezentrales System: Speicherung der Daten auf dem Endgerät des Nutzers.
        * Anonymität: Keine Speicherung von persönlichen Daten, IP-Adressen oder Standortdaten. Verwendung von sich ändernden, pseudonymen IDs.
        * Open-Source-Code zur Überprüfung durch die Öffentlichkeit.
* **4.2 Sekundäre Funktionen (spätere Releases):**
    * **Impfnachweis & Genesenenzertifikat:**
        * Funktion zum Einscannen und Speichern von digitalen Impfzertifikaten (QR-Codes).
        * Anzeige des Zertifikats zur Verifizierung.
    * **Check-in-Funktion:**
        * Anonymes Einchecken an Orten (z.B. Restaurants) über QR-Code-Scanning.
        * Warnung bei einem bekanntgewordenen Infektionsfall an diesem Ort.
    * **Tagebuch:**
        * Manuelle Eingabe von Kontakten und besuchten Orten.

**5. Nicht-funktionale Anforderungen**
* **5.1 Performance:**
    * Geringer Batterieverbrauch.
    * Geringer Datenverbrauch.
    * Hohe Zuverlässigkeit der Bluetooth-Erkennung.
* **5.2 Sicherheit:**
    * Einhaltung der DSGVO-Richtlinien.
    * Schutz der Nutzerdaten vor unbefugtem Zugriff.
    * Sichere Kommunikation mit dem Backend-Server zur Übermittlung der anonymen Schlüssel.
* **5.3 Plattformen:**
    * iOS (Apple)
    * Android (Google)
    * Zusammenarbeit mit den Schnittstellen "Apple/Google Exposure Notification API".

**6. Technische Spezifikationen**
* **6.1 Architektur:**
    * Client-Server-Architektur mit Schwerpunkt auf dem dezentralen, clientseitigen Datenmodell.
    * Backend zur Verteilung der anonymen, verifizierten Infektionsschlüssel.
* **6.2 APIs:**
    * Nutzung der offiziellen Apple/Google API zur Bluetooth-basierten Kontaktnachverfolgung.
    * Schnittstelle zum Labor-Informationssystem zur Übermittlung der Testergebnisse.

**7. Design & User Experience (UX) - Anforderungen**
* **7.1 User Interface (UI):**
    * Klare, einfache und intuitive Benutzeroberfläche.
    * Einfache Onboarding-Prozesse.
    * Verständliche Darstellung des Risikostatus (z.B. Ampelsystem).
* **7.2 Sprachliche Anforderungen:**
    * Die Sprache muss einfach, verständlich und angstfrei sein.
    * Klare Handlungsanweisungen bei Risikowarnungen.

**8. Offene Fragen & Annahmen**
* **8.1 Annahmen:**
    * Die Nutzerbasis wird groß genug sein, um eine relevante Datenmenge zu generieren.
    * Die Technologie (Bluetooth) ist zuverlässig genug für die Kontaktnachverfolgung.
* **8.2 Offene Fragen:**
    * Wie wird die Akzeptanz in der Bevölkerung maximiert?
    * Wie wird der Missbrauch der App verhindert (z.B. falsche Testergebnisse melden)?