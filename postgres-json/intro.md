# JSON in relationalen Datenbanken

In diesem Scenario werden Möglichkeiten zur Verwendung von JSON in relationalen Datenbanken gezeigt.

JSON steht für JavaScript Object Notation und besteht ähnlich wie XML und YAML aus "Key-Value"-Paaren.

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

- Einfaches Speichern von flexiblen Daten. Beispielsweise von externen Schnittstellen, auf die man kein Einfluss hat.
- Daten, die zusammengehören müssen nicht mit Joins aufwendig verbunden werden. Dies spart Zeit beim Ausführen.
- Vermeidung der Umwandlung von JSON für die Tabellen innerhalb der Datenbank. Bei komplizierten und verschachtelten JSON-Objekten kann dies viel Rechenzeit in Anspruch nehmen.

In diesem Scenario wird Postgres als relationale Datenbank verwendet.



