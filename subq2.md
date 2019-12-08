Geef voor elke speler die ooit een boete heeft betaald, de hoogste boete weer en hoelang het geleden is dat deze boete werd betaald. Sorteer van groot naar klein op bedrag en daarna omgekeerd op “leeftijd..” van de boete.
```sql
select spelersnr, bedrag, age(datum) as age
from boetes b
where bedrag >= all(select bedrag from boetes bb where b.spelersnr = bb.spelersnr) 
order by 2 desc, 3 asc, 1 asc
```
Geef alle spelers die geen wedstrijd voor team 1 heeft gespeeld. Sorteer op naam, daarna op nr.
```sql
select spelersnr, naam
from spelers s
where not exists(select * from wedstrijden w where s.spelersnr = w.spelersnr and  teamnr = 1 )
order by 2, 1
```
Geef alle spelers voor wie meer boetes zijn betaald dan dat ze wedstrijden hebben gespeeld. Zorg dat spelers zonder wedstrijd ook getoond worden.
Sorteer van voor naar achter, oplopend.
```sql
select naam, voorletters, geb_datum 
from spelers s inner join boetes b using(spelersnr)
group by s.spelersnr
having count(*) > (select count(*) from wedstrijden w where w.spelersnr = s.spelersnr)
order by 1,2,3 asc
```
Geef een lijst met het spelersnummer en de naam van de spelers die in Rijswijk wonen en die in 1980 een boete gekregen hebben van 25 euro (meerdere voorwaarden dus). Gebruik hiervoor geen exists operator maar wel zijn tegenhanger die meestal bij niet-gecorreleerde subquery's wordt gebruikt. Sorteer van voor naar achter, oplopend.
```sql
select s.spelersnr, s.naam
from spelers s inner join boetes b on s.spelersnr = b.spelersnr
where b.datum > '1980-01-01' and b.datum < '1981-01-01' and plaats like 'Rijswijk' and bedrag = 25
order by 1,2 asc
```
Geef het totaal aantal boetes, het totale boetebedrag, het minimum en het maximum boetebedrag dat door onze club betaald werd. Let er hierbij op dat er gehele getallen worden getoond (rond af indien nodig). Sorteer van voor naar achter, oplopend.
```sql
SELECT COUNT(*) as AANTAL_BOETES, ROUND(SUM(bedrag)) as TOTAAL_BEDRAG, ROUND(MIN(bedrag), 0 ) as MINIMUM, ROUND(MAX(bedrag)) as MAXIMUM FROM boetes h
order by 1, 2, 3, 4 asc
```
