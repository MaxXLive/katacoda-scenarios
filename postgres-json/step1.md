# Installation & Vorbereitung

Zunächst wird PostgreSQL auf der Ubuntu Umgebung installiert. Dies kann einige Sekunden in Anspruch nehmen:

`sudo apt install -y postgresql postgresql-contrib`{{execute}}

Nachdem dies erledigt ist, kann man sich mit der PostgreSQL Umgebung als "postgres" Benutzer verbinden:

`sudo -u postgres psql`{{execute}}

Zum Testen wird nun eine Datenbank erstellt:

`CREATE DATABASE testing;`{{execute}}

Wenn alles funktioniert hat, sollte diese nun angezeigt werden, wenn folgender Befehl aufgerufen wird:

`\l`{{execute}}

Das heißt, man kann sich nun mit der neuen Datenbank verbinden:

`\c testing`{{execute}}

In den nächsten Schritten werden nun die Möglichkeiten zur Nutzung von JSON erklärt...