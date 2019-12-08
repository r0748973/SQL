Geef de planeet (draait dus rond de zon) met de meeste satellieten.
Sorteer op objectnaam.
```sql
SELECT objectnaam 
FROM (
    SELECT h.satellietvan AS objectnaam, COUNT(h.objectnaam) AS aantal_manen
    FROM hemelobjecten AS h
    WHERE h.satellietvan <> 'Zon'
    GROUP BY h.satellietvan
        ) AS aantal_manen
WHERE aantal_manen = (
    SELECT MAX(aantal_manen) 
    FROM    (
        SELECT h.satellietvan AS objectnaam, COUNT(h.objectnaam) AS aantal_manen
        FROM hemelobjecten AS h
        WHERE h.satellietvan <> 'Zon'
        GROUP BY h.satellietvan
        ) AS aantal_manen
    )
ORDER BY objectnaam
```
Geef het hemellichaam dat het laatst bezocht is.
Gebruik hiervoor de laatste vertrekdatum van de reis en laatste volgnummer van bezoek. Tip: gebruik hiervoor een rij-subquery.
Gebruik geen limit of top.
```sql
select objectnaam
from bezoeken
where (reisnr, volgnr) = ( select reisnr, MAX(volgnr)
		from bezoeken
		where reisnr = (select reisnr
	  from reizen where vertrekdatum = (select max(vertrekdatum) from reizen))
		group by reisnr)
```
Geef een lijst van alle hemelobjecten die meer keer bezocht gaan worden dan Jupiter. (onafhankelijk van het aantal deelnames)
Sorteer op objectnaam.
```sql
select h.objectnaam, diameter
from hemelobjecten h inner join bezoeken using(objectnaam)
group by h.objectnaam
having count(reisnr) > (select count(reisnr) from bezoeken where objectnaam LIKE 'Jupiter')
order by h.objectnaam
```
