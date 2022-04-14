# Möglichkeit 1: JSON als TEXT speichern

Unabhängig vom DBMS kann JSON als String serialisiert werden.

Dafür wird zunächst eine Tabelle für Personen erstellt:

`CREATE TABLE person (
    id integer NOT NULL PRIMARY KEY,
    data TEXT
);`{{execute}}

Und mit Werten gefüllt:

`INSERT INTO person VALUES (0, '{"name": "Homer Simpson", "birthdate": "1951-05-12", "sex": "m"}');
INSERT INTO person VALUES (1, '{"name": "Marge Simpson", "birthdate": "1953-03-19", "sex": "f"}');
INSERT INTO person VALUES (2, '{"name": "Bart Simpson", "birthdate": "1980-04-01", "sex": "m"}');
INSERT INTO person VALUES (3, '{"name": "Lisa Simpson", "birthdate": "1982-05-09", "sex": "f"}');
INSERT INTO person VALUES (4, '{"name": "Maggie Simpson", "birthdate": "1989-01-12", "sex": "f"}');
`{{execute}}

Nun sollten alle Personen angezeigt werden:

`SELECT * FROM person;
`{{execute}}

Bei dieser Art des Speicherns ist es nicht möglich, auf Unterobjekte innerhalb des JSON-Objekts zuzugreifen.
Das bedeutet, dass beispielsweise auch keine Sortierung nach dem Namen oder Geburtsdatum möglich ist.
Auch ein Filtern ist nicht möglich. Die einzige Möglichkeit einen Eintrag abzurufen, ist über den Primärschlüssel:

`SELECT data FROM person WHERE id=2;
`{{execute}}