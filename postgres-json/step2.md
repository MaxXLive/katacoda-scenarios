# Möglichkeit 1: JSON als TEXT speichern

Unabhängig vom DBMS kann JSON als String serialisiert werden.

Dafür wird zunächst eine Tabelle für Personen erstellt

`CREATE TABLE person (
    id integer NOT NULL,
    data TEXT
);`{{execute}}

Und mit Werten gefüllt

`INSERT INTO person VALUES (0, '{"name": "Homer Simpson", "birthdate": "1951-05-12", "sex": "m"}');
INSERT INTO person VALUES (1, '{"name": "Marge Simpson", "birthdate": "1953-03-19", "sex": "f"}');
INSERT INTO person VALUES (2, '{"name": "Bart Simpson", "birthdate": "1980-04-01", "sex": "m"}');
INSERT INTO person VALUES (3, '{"name": "Lisa Simpson", "birthdate": "1982-05-09", "sex": "f"}');
INSERT INTO person VALUES (0, '{"name": "Maggie Simpson", "birthdate": "1989-01-12", "sex": "f"}');
`{{execute}}

Nun sollten alle Personen angezeigt werden

`SELECT * FROM person;
`{{execute}}