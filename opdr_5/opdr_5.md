# Sql basis

### Database
Maak gebruik van het sql-bestand flits.sql en voer hierop onderstaande opdrachten uit.
Theorie kun je vinden op: https://www.edutorial.nl/dbq/introductie/

### Queries flitspaal
* Welke cameras zijn er en waar staan ze (id, address, city, max_speed).
SELECT id, address, city, max_speed
FROM cameras;
* Overzicht van boetes op 50km wegen
SELECT id, speed_limit, speed_excess, fine, created_at
FROM fines
WHERE speed_limit = 50;
* Overzicht van overtredingen van 1 kenteken.


* N.a.w.-gegevens van de hardrijders die het meest in de fout zijn gegaan. (top 10)


* Welke camera’s (id, address, city) meten de meeste snelheidsovertredingen (top 10)


* Welke auto’s (kenteken, merk, type) zijn het meeste geflitst


* Welke flitspaal (=camera met id, address, city) flitst het meest (top 10)



* Kentekens van auto’s die het hoogste bedrag aan boetes hebben gekregen (top 10)


* Overzicht van auto’s (kenteken, merk, type) waarvan het kenteken overeenkomt met sitecode 10 (zoals X-999-XX) https://nl.wikipedia.org/wiki/Nederlands_kenteken.
