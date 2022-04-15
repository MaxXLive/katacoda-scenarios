# JSON in relationalen Datenbanken

## Einleitung

JSON steht für JavaScript Object Notation und besteht ähnlich wie XML und YAML aus "Key-Value"-Paaren.
Es wird verwendet
- für den Datenaustausch zwischen REST-Schnittstellen
- als Konfigurationsdatei, beispielsweise bei Node.js oder Katacoda
- als Dokumentenformat in dokumentenorientierten Datenbanken wie MongoDB

Dies wäre ein Beispiel für ein JSON-Objekt: 
<pre>
{
    "name": "John",
    "age": 30,
    "car": {
        "color": "red",
        "doors": 4
    }
}
</pre>

Es gibt verschiedene Gründe für das Verwenden von JSON in relationalen Datenbanken.
Hier einige Beispiele:

- Einfaches Speichern von flexiblen Daten (Schema-on-read). Beispielsweise von externen Schnittstellen, auf die man kein Einfluss hat.
- Daten, die zusammengehören müssen nicht mit Joins aufwendig verbunden werden. Dies spart Zeit beim Ausführen.
- Vermeidung der Umwandlung von JSON für die Tabellen innerhalb der Datenbank. Bei komplizierten und verschachtelten JSON-Objekten kann dies viel Rechenzeit in Anspruch nehmen.

## Ausblick
In diesem Scenario werden Möglichkeiten zur Verwendung von JSON in relationalen Datenbanken gezeigt.
Als Datenbankmanagementsystem (DBMS) wird PostgreSQL verwendet, da es interessante Unterschiede beim Speichern von JSON-Daten bietet.
Zunächst wird PostgreSQL installiert und vorbereitet.
Sobald die Datenbank bereitsteht, wird gezeigt, wie JSON-Daten als String im "TEXT"-Datentyp gespeichert werden.
Danach wird auf die Unterschiede zwischen den Datentypen "JSON" und "JSONB" eingegangen und ein Geschwindigkeitsvergleich durchgeführt.



