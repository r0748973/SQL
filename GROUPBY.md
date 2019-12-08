We willen telkens het aantal reizen en de totale prijs van de volgende situaties. Voor elke maand van vertrek, voor elk tiental van de reisduur, voor de combinatie van maand van vertrek en tiental van de reisduur en het het totale plaatje.
Sorteer van voor naar achter.
```sql
select extract(month from vertrekdatum) as date_part,
round(reisduur/10,0), count(reisnr), sum(prijs)
from reizen
group by cube( 1,2)
order by 1,2,3,4
```
Geef voor elke geboortejaar van de klanten, het aantal klanten, het kleinste klantennummer en het grootste klantennummer. Geef ook het totaal aantal klanten en het kleinste en grootste klantennummer.
Sorteer van voor naar achter.
```sql
SELECT EXTRACT(YEAR FROM geboortedatum) as date_part, COUNT(klantnr), MIN(klantnr), MAX(klantnr)
FROM klanten
GROUP BY ROLLUP (date_part)
ORDER BY 1,2,3,4
```
