# Zusammenfassung

Meiner Meinung nach sollte beim Speichern von JSON in relationalen Datenbanken (speziell bei PostgreSQL) darauf geachtet werden, dass die nativen Datentypen für JSON zu verwendet werden.
Auch, wenn keine SQL-basierten Abfragen von JSON-Objekten verwendet werden soll, bieten die nativen Datentypen viele Vorteile.
Einerseits wird die Ausführungszeit verbessert und der Speicherplatz minimiert.
Andererseits ist man für Optimierungen besser vorbereitet, falls beispielsweise auffällt, dass ein Abruf auf die JSON-Daten erforderlich ist.
Außerdem wird beim Speichern direkt auf ein gültiges JSON Format geprüft.

Allgemein kann man sagen, dass die Vorteile, die Nachteile stark überwiegen, weshalb ich bei der Verwendung von PostgreSQL den "Binary JSON (JSONB)" Datentyp bevorzugen würde.