Geef alle spelers die meer wedstrijden gespeeld hebben dan het aantal wedstrijden dat de huidige voorzitter heeft verloren. De huidige voorzitter komt zelf niet in de lijst voor. Gebruik geen subqueries.
Sorteer op spelersnr.

```sql
SELECT w1.spelersnr
FROM wedstrijden w1 INNER JOIN bestuursleden b1
ON ( functie = 'Voorzitter' AND eind_datum IS NULL)
INNER JOIN wedstrijden w2 ON ( b1.spelersnr = w2.spelersnr)
WHERE w1.spelersnr != w2.spelersnr
GROUP BY w1.spelersnr, w2.spelersnr
HAVING COUNT(w1.wedstrijdnr) > SUM(DISTINCT w2.verloren)
ORDER BY spelersnr
```


Geef per team het hoogste wedstrijdnummer van een wedstrijd, gespeeld door een bestuurslid (actief en niet meer actief) die geen boete heeft gekregen.
Sorteer op teamnr.
```sql
select teamnr, MAX(wedstrijdnr) as laatstewedstrijd
from spelers s inner join bestuursleden b on s.spelersnr = b.spelersnr
left outer join boetes bb on s.spelersnr = bb.spelersnr 
inner join wedstrijden w on s.spelersnr = w.spelersnr
where bedrag is null
group by teamnr
order by teamnr
```


Geef het gemiddeld boetebedrag per speler, afgerond op twee cijfers na de komma. spelers zonder boete moeten bovenaan staan met 'geen boetes'
Sorteer daarna op spelersnaam en vervolgens op gemiddeld boetebedrag.
Gebruik als datatype van de 2de kolom varchar(8) voor spelers die wel boetes hebben.

```sql
select naam,
case 
when sum(bedrag)/count(bedrag) is null then 'geen boetes' else
round(sum(bedrag)/count(bedrag),2)::varchar end as gemiddeld
from spelers left outer join boetes using(spelersnr)
group by spelers.naam, spelersnr
order by 
case when sum(bedrag)/count(bedrag) is null then 0 else 1 end
,naam,gemiddeld
```


Geef voor alle vrouwelijke spelers die in Den Haag, Zoetermeer of Leiden wonen
het spelersnummer, hun woonplaats en een lijst van de teams waarvoor ze ooit gespeeld hebben, als ze ooit een wedstrijd gespeeld hebben. sorteer volgens 1,2,3

```sql
select s.spelersnr, plaats, teamnr
from spelers s left outer join wedstrijden b using(spelersnr) 
where geslacht LIKE 'F' and plaats LIKE 'Den Haag' or plaats LIKE 'Zoetermeer' or plaats LIKE 'Leiden'
order by s.spelersnr, plaats, teamnr
```


Geef een lijst met het spelersnummer, de naam van de speler, de datum van de boete en het bedrag van de boete van al de spelers die een boete gekregen hebben met een bedrag groter dan 45,50 euro en in Rijswijk wonen. Geef expliciet aan welke join je gebruikt.

```sql
select s.spelersnr, naam, datum, bedrag
from spelers s inner join boetes b using(spelersnr)
where bedrag > 45.50 and plaats LIKE 'Rijswijk'
```
Geef voor alle vrouwelijke spelers die in Den Haag, Zoetermeer of Leiden wonen
het spelersnummer, hun woonplaats en een lijst van de teams waarvoor ze ooit gespeeld hebben, als ze ooit een wedstrijd gespeeld hebben. sorteer volgens 1,2,3

```sql
select s.spelersnr, plaats, teamnr
from spelers s left outer join wedstrijden b using(spelersnr) 
where geslacht LIKE 'F' and plaats LIKE 'Den Haag' or plaats LIKE 'Zoetermeer' or plaats LIKE 'Leiden'
order by s.spelersnr, plaats, teamnr
```

Geef een lijst met het spelersnummer, de naam van de speler, de datum van de boete en het bedrag van de boete van al de spelers die een boete gekregen hebben met een bedrag groter dan 45,50 euro en in Rijswijk wonen. Geef expliciet aan welke join je gebruikt.

```sql
select s.spelersnr, naam, datum, bedrag
from spelers s inner join boetes b using(spelersnr)
where bedrag > 45.50 and plaats LIKE 'Rijswijk'
```

Geef het spelersnummer en bondsnummer van alle spelers die jonger zijn dan de speler met bondsnummer 8467.
gebruik een INNER JOIN. Sorteer 1,2

```sql
Geef het spelersnummer en bondsnummer van alle spelers die jonger zijn dan de speler met bondsnummer 8467.
gebruik een INNER JOIN. Sorteer 1,2
```

Geef van elke speler (die wedstrijden gespeeld heeft en boetes gekregen heeft) wonend in Rijswijk het spelersnr, de naam, de lijst met boetebedragen en de lijst met teams waarvoor hij/zij een wedstrijd gespeeld heeft. Geef het resultaat volgens oplopend spelersnr en boetebedrag.

```sql
select s.spelersnr, naam, bedrag, w.teamnr
from spelers s inner join wedstrijden w on s.spelersnr = w.spelersnr
inner join boetes b on s.spelersnr = b.spelersnr
where plaats like 'Rijswijk'
order by s.spelersnr, bedrag
```

Geef de nummers van alle spelers (ook al hebben ze geen wedstrijd gespeeld), een lijst van de nummers van alle wedstrijden die ze gewonnen hebben, alsook het verschil in sets waarmee ze gewonnen hebben.
Toon je resultaten gesorteerd volgens dit verschil van groot naar klein en oplopend op spelersnr.

```sql
SELECT s.spelersnr, wedstrijdnr, (gewonnen - verloren) AS verschil
FROM spelers s
	LEFT OUTER JOIN wedstrijden w ON s.spelersnr = w.spelersnr AND gewonnen > verloren
ORDER BY verschil DESC, s.spelersnr ASC
```

Geef een alfabetisch gesorteerde lijst van de namen van alle leden van de tennisvereniging die nog geen wedstrijden gespeeld hebben

```sql
select naam 
from spelers s left outer join teams tt on tt is null
left outer join wedstrijden w on wedstrijdnr is null 
order by naam
```

Geef voor alle huidige bestuurleden hun functie en de lijst van boetes die voor hen werd betaald.
Omdat je dit wil vergelijken met de boetebedragen die betaald werden voor leden die niet in het bestuur zitten, wil je deze boetebedragen ook opnemen in de tweede kolom van je resultaat. Sorteer je antwoord eerst op functie en daarna op het boetebedrag.

```sql
select functie, bedrag
from bestuursleden be left outer join spelers s on s.spelersnr = be.spelersnr 
full outer join boetes b on s.spelersnr = b.spelersnr and datum is not null 
where eind_datum is null
order by functie, bedrag
```
