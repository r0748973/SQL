Geef de spelersnummers die minstens één keer bestuurslid zijn geweest.
Gebruik hiervoor geen JOIN, DISTINCT, GROUP BY, IN, ANY, ALL of EXISTS.
Sorteer op spelersnr
```sql
SELECT spelersnr
FROM spelers
INTERSECT
SELECT spelersnr
FROM bestuursleden
ORDER BY spelersnr
```
Geef een overzicht van de boetebedragen, aantal gewonnen en verloren sets en aantal verschillende functies. Bekijk de output voor de manier hoe het getoond moet worden.
Sorteer van links naar rechts.
Tip: Het lijkt onlogisch, maar zelfs NULL krijgt een datatype en kan niet impliciet wijzigen van datatype.
```sql
select cast(sum(bedrag)as char(6))as boetebedrag, null as aantalgewonnen, null as aantalverloren, null as aantalfuncties
from boetes
union
select null, cast(sum(gewonnen) as char(6)), null, null
from wedstrijden
Union
select null, null, cast(sum(verloren) as char(6)), null
from wedstrijden
union 
select null, null, null, cast(count(distinct functie) as char(6))
from bestuursleden
order by 1,2,3,4
```
Geef een lijst met alle spelersnrs, naam en het aantalwedstrijden ze gespeeld hebben en op een nieuwe lijn het aantal bestuursfuncties die ze hebben/hadden.
Spelers die zowel wedstrijden gespeeld hebben als bestuurslid zijn, komen dus twee keer voor in het resultaat.
Sorteer op spelersnr en aantal. Geen dubbels tonen.
```sql
select s.spelersnr as nr, s.naam, count(wedstrijdnr) as aantal
from spelers s inner join wedstrijden w on s.spelersnr = w.spelersnr
group by s.spelersnr
union
select s.spelersnr as nr, s.naam, count(functie) as aantal
from spelers s inner join bestuursleden b  on s.spelersnr = b.spelersnr
group by s.spelersnr
order by 1,3
```
Geef een overzicht van alle spelers, gevolgd door alle bestuursleden, gesorteerd op jaar van toetreding of beginjaar van hun functie en vervolgens op spelersnr.
Geen dubbels tonen.
```sql
select spelersnr as veld1, naam as veld2, jaartoe as veld3
from spelers
union
select spelersnr, functie, extract (year from begin_datum)
from bestuursleden
order by 3,1
```
Geef voor de spelers die bestuurslid en/of teamkapitein zijn hun naam en een oplijsting van hun functienamen (huidig of verleden) en hun divisies waarvoor ze kapitein zijn.
Sorteer op spelersnaam en naam.
Gebruik geen OUTER JOIN of WHERE.

```sql
select 'bestuursleden' as "label" , count(spelersnr) as aantal
from bestuursleden
union
select 'boetes' as "label" , count(spelersnr) as aantal
from boetes
union
select 'spelers' as "label" , count(spelersnr) as aantal
from spelers
union
select 'teams' as "label" , count(spelersnr) as aantal
from teams
union
select 'wedstrijden' as "label" , count(spelersnr) as aantal
from wedstrijden
order by 1
```
