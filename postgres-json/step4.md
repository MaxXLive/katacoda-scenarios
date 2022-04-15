# Möglichkeit 3: Native Integration in PostgresSQL mit JSONB-Datentyp

Neben dem "JSON"-Datentyp bietet PostgreSQL ebenfalls den "JSONB"-Datentyp.
"JSONB" steht hierbei für "Binary JSON" und weist hiermit schon den ersten Unterschied zum "JSON"-Datentyp hin.
"JSONB" speichert nämlich, im Gegensatz zu "JSON" die Daten as Binary-Code und entfernt somit unnötige Leerzeichen und weiteren Overhead, der mit der Verwendung von ASCII/UFT-8 mitkommt.
Dies benötigt zwar etwas mehr Zeit beim Speichern der Daten, ist aber beim Abruf um einiges Performanter.
Außerdem unterstützt "JSONB" die Verwendung von Indizes, wodurch der Abruf um ein vielfaches schneller werden kann, da beispielsweise beim Filtern nicht mehr jede Zeile in der Tabelle durchgegangen und geprüft werden muss.

Wie im vorherigen Schritt wird zunächst die alte Tabelle entfernt:

`DROP TABLE person;`{{execute}}

Und nun eine Tabelle mit dem "JSONB"-Datentyp erstellt:

`CREATE TABLE person (
id integer NOT NULL PRIMARY KEY,
data JSONB
);`{{execute}}

Nun könnte man ebenfalls die Beispielwerte aus dem vorherigen Schritt Einfügen und Abrufen, jedoch würde dies keinen offensichtlichen Vorteil aufzeigen.
Um den Vorteil von "JSONB" im Vergleich zu "JSON" zu zeigen, müssen etwas mehr Werte eingefügt werden.
Dafür wird ein Befehl verwendet, der 10.000 Werte in die Tabelle eingefügt.
Dieser inkrementiert die Id, welche auch als Name verwendet wird, nimmt ein zufälliges Datum zwischen 1900 und 2000 und wählt zufällig das Geschlecht:

`INSERT INTO person (id, data)
SELECT id,
(
    '{"name": "' || id || '",
    "birthdate": "' || DATE(timestamp '1900-01-01 00:00:00' + random() * (timestamp '2000-01-01 00:00:00' - timestamp '1900-01-01 00:00:00')) || '",
    "sex": "' || CASE WHEN random () > 0.5 THEN 'f' ELSE 'm' END || '"}'
)::json
AS FieldValue FROM generate_series(1, 10000) id;
`{{execute}}

Nachdem die Tabelle nun 10.000 Einträge hat, muss die Anzeige für die Ausführungszeit eingeschaltet werden:

`\timing on`{{execute}}

Nun wird zunächst ein Befehl ausgeführt, der die Anzahl an Frauen zählen soll:

`SELECT count(*) as women FROM person WHERE data ->> 'sex' = 'f';`{{execute}}

Die Dauer kann variieren, in meinem Fall dauerte die erste Ausführung (ohne Indizierung) ``Time: 6.317 ms``.
Um die Ausführungszeit zu verbessern, erstellen wir jetzt einen Index für das Geschlecht:

`CREATE INDEX idx_sex ON person ((data ->> 'sex'));`{{execute}}

Nun führen wir den vorherigen Befehl erneut aus:

`SELECT count(*) as women FROM person WHERE data ->> 'sex' = 'f';`{{execute}}

Man kann beobachten, dass sich die Ausführungszeit fast halbiert. In meinem Fall waren es nun mit Indizierung ``Time: 3.809 ms``, beim zweiten Mal Ausführen sogar nur ``Time: 2.619 ms``.

Vergleichen wir das nun mit dem "JSON"-Datentyp, indem wir wieder die Tabelle löschen und mit "JSON" erstellen:

`DROP TABLE person;
CREATE TABLE person (
id integer NOT NULL PRIMARY KEY,
data JSON
);`{{execute}}

Füllt man diese Tabelle nun mit 10.000 Einträgen:

`INSERT INTO person (id, data)
SELECT id,
(
'{"name": "' || id || '",
"birthdate": "' || DATE(timestamp '1900-01-01 00:00:00' + random() * (timestamp '2000-01-01 00:00:00' - timestamp '1900-01-01 00:00:00')) || '",
"sex": "' || CASE WHEN random () > 0.5 THEN 'f' ELSE 'm' END || '"}'
)::json
AS FieldValue FROM generate_series(1, 10000) id;
`{{execute}}

Und zählt wieder alle Frauen:

`SELECT count(*) as women FROM person WHERE data ->> 'sex' = 'f';`{{execute}}

EIn meinem Fall ergibt sich eine Ausführungszeit von ``Time: 15.118 ms``, die auch mit mehrfachem Ausführen nicht besser wird.
Daran sieht man gut, dass der "JSON"-Datentyp keine Indizierung bietet, wodurch beim Filtern jeder Eintrag zunächst aus ASCII/UFT-8 dekodiert werden muss.
Dies erhöht die Ausführungszeit, im Vergleich zur Indizierten "JSONB" Tabelle um den Faktor 5.