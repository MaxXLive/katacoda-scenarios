# Möglichkeit 2: Native Integration in PostgresSQL mit JSON-Datentyp

Seit Version 9.2 wird JSON nativ von PostgreSQL unterstützt.
Das bedeutet, dass JSON ganz einfach als Wert in der Datenbank gespeichert werden kann und mithilfe von SQL gelesen werden kann.
Dabei beschränkt sich das Lesen nicht auf das Wurzel-Objekt, sondern auch auf alle Unter-Objekte.

Zunächst wird die Tabelle aus dem vorherigen Schritt entfernt:

`DROP TABLE person;`{{execute}}

Nun wird eine neue Tabelle für Personen erstellt.
Hierfür wird der Datentyp "JSON" verwendet.

`CREATE TABLE person (
id integer NOT NULL PRIMARY KEY,
data JSON
);`{{execute}}

Nun wird die Tabelle wieder mit den vorherigen Beispielen gefüllt:

`INSERT INTO person VALUES (0, '{"name": "Homer Simpson", "birthdate": "1951-05-12", "sex": "m"}');
INSERT INTO person VALUES (1, '{"name": "Marge Simpson", "birthdate": "1953-03-19", "sex": "f"}');
INSERT INTO person VALUES (2, '{"name": "Bart Simpson", "birthdate": "1980-04-01", "sex": "m"}');
INSERT INTO person VALUES (3, '{"name": "Lisa Simpson", "birthdate": "1982-05-09", "sex": "f"}');
INSERT INTO person VALUES (4, '{"name": "Maggie Simpson", "birthdate": "1989-01-12", "sex": "f"}');
`{{execute}}

Der Unterschied zum Speichern als "TEXT" ist, dass nun Objekte innerhalb des JSON aufgerufen werden können.
Wie in der "Data Warehouse"-Vorlesung bereits gelernt, kann dafür der ``->``-Operator, zum Abrufen per Schlüssel oder der `->>`-Operator, zum Abrufen als Text verwendet werden. 
Beispielsweise, wenn nur die Namen der Personen aus dem JSON abgerufen werden sollen:

`SELECT data ->> 'name' AS name FROM person;
`{{execute}}

Verschachtelte Abfragen können mit der Aneinanderreihung der Operatoren verwendet werden.

Ein weiterer Vorteil ist, dass auch nach Werten sortiert oder gefiltert werden kann.
Beispielsweise sortieren nach dem Geburtsdatum, absteigend:

`SELECT id, data ->> 'name' AS name, data ->> 'birthdate' AS birthdate, data ->> 'sex' AS sex FROM person ORDER BY data ->> 'birthdate' DESC;
`{{execute}}

Oder filtern anhand des Geschlechts, in diesem Fall nur alle Frauen:

`SELECT id, data ->> 'name' AS name, data ->> 'birthdate' AS birthdate, data ->> 'sex' AS sex FROM person WHERE data ->> 'sex' = 'f';
`{{execute}}

Außerdem gibt es viele weitere Funktionen, die in der PostgreSQL Dokumentation (https://www.postgresql.org/docs/9.5/functions-json.html) nachgelesen werden können, beispielsweise auch das Ändern von JSON-Werten.

Wie man erkennen kann, bietet die Verwendung des "JSON"-Datentyps in Postgres viele Vorteile im Gegensatz zum Abspeichern als "TEXT".



