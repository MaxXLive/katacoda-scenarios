# Möglichkeit 2: Native Integration in PostgresSQL mit JSON-Datentyp

Seit Version 9.2 wird JSON nativ von PostgreSQL unterstützt.
Das bedeutet, dass JSON ganz einfach als Wert in der Datenbank gespeichert werden kann und mithilfe von SQL gelesen werden kann.
Dabei beschränkt sich das Lesen nicht auf das Wurzel-Objekt, sondern auch auf alle Unter-Objekte.

Zunächst wird eine Tabelle für Personen erstellt.
Hierfür wird der Datentyp "Binary JSON bzw. JSONB" verwendet.
Dieser entfernt unnötige Leerzeichen und bietet

`CREATE TABLE cards (
id integer NOT NULL,
data jsonb
);
`{{execute}}