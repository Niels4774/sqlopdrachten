# Sql basis

### Database gegevens aanpassen
Maak gebruik van het sql-bestand idscan.sql en voer hierop onderstaande opdrachten uit.
Theorie kun je vinden op: https://www.edutorial.nl/dbq/introductie/

### Queries winkelketen
* Welke medewerkers (id, voornaam, achternaam)zijn er in dienst van de winkelketen.
SELECT id, firstname, lastname
FROM persons;

* Hoeveel medewerkers hebben dezelfde functie (jobtitle)
SELECT jobtitle, COUNT(*) AS aantal_medewerkers
FROM persons
GROUP BY jobtitle;

* Hoeveel medewerkers zijn professor of ingenieur (title = prof, ir of ing)
SELECT COUNT(*) AS aantal_medewerkers
FROM persons
WHERE title IN ('prof', 'ir', 'ing');

* Overzicht van medewerkers (id, voornaam, tussenvoegsel, achternaam) per gebouw (buildingname, street en buildingnumber)
SELECT p.id, p.firstname, p.lastname, b.buildingname, b.street, b.buildingnumber
FROM persons p
JOIN scans s ON p.id = s.person_id
JOIN buildings b ON s.building_id = b.id;

* Overzicht van medewerkers (id, voornaam, tussenvoegsel, achternaam) die op een *  * bepaalde datum in gebouw met id 1 waren (buildingname, 
SELECT p.id, p.firstname, p.lastname, p.title, b.buildingname, b.street, b.buildingnumber
FROM persons p
JOIN scans s ON p.id = s.person_id
JOIN buildings b ON s.building_id = b.id
WHERE b.id = 1
AND s.scandate = '2025-05-09'; 
* Overzicht van medewerkers die op diezelfde datum vergeten zijn om uit te checken
SELECT p.id, p.firstname, p.lastname, p.title, b.buildingname, b.street, b.buildingnumber
FROM persons p
JOIN scans s ON p.id = s.person_id
JOIN buildings b ON s.building_id = b.id
WHERE b.id = 1
AND s.scandate = '2025-05-09'  
AND s.in_out = 'in'
AND NOT EXISTS (
    SELECT 1
    FROM scans s2
    WHERE s2.person_id = s.person_id
    AND s2.building_id = s.building_id
    AND s2.scandate = s.scandate
    AND s2.in_out = 'out'
);
* Overzicht van het aantal medewerker per gebouw op 13 september 2023.
SELECT b.buildingname, b.street, b.buildingnumber, COUNT(DISTINCT s.person_id) AS aantal_medewerkers
FROM scans s
JOIN buildings b ON s.building_id = b.id
WHERE s.scandate = '2023-09-13'
GROUP BY b.id;

* Overzicht van medewerkers en het aantal uur dat ze op 15 september 2023 hebben gewerkt.
SELECT 
    p.id, 
    p.firstname, 
    p.lastname, 
    SUM(TIMESTAMPDIFF(HOUR, s1.scantime, s2.scantime)) AS aantal_uren
FROM persons p
JOIN scans s1 ON p.id = s1.person_id
JOIN scans s2 ON p.id = s2.person_id
WHERE s1.scandate = '2023-09-15' 
AND s2.scandate = '2023-09-15'
AND s1.in_out = 'in' 
AND s2.in_out = 'out'
AND s1.scantime < s2.scantime
GROUP BY p.id;

* Medewerker van de maand! (De medewerker die het meeste uren heeft gemaakt van iedereen in de maand september)
SELECT 
    p.id, 
    p.firstname, 
    p.lastname, 
    SUM(TIMESTAMPDIFF(HOUR, s1.scantime, s2.scantime)) AS totaal_uren
FROM persons p
JOIN scans s1 ON p.id = s1.person_id
JOIN scans s2 ON p.id = s2.person_id
WHERE s1.scandate BETWEEN '2023-09-01' AND '2023-09-30'
AND s2.scandate BETWEEN '2023-09-01' AND '2023-09-30'
AND s1.in_out = 'in' 
AND s2.in_out = 'out'
AND s1.scantime < s2.scantime
GROUP BY p.id
ORDER BY totaal_uren DESC
LIMIT 1;