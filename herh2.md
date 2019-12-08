Geef per klant het totaal aantal reizen waaraan deze klant zal deelnemen en het langste bezoek dat deze klant zal maken aan een hemelobject, over alle reizen heen.
Klanten zonder reizen of zonder bezoeken moeten ook voorkomen in het overzicht.
Sorteer op klantnr.

```sql
select kk.klantnr, count(distinct r.reisnr) as aantal, max(verblijfsduur) as langstebezoek
from klanten kk left outer join deelnames d using(klantnr)
left outer join reizen r using(reisnr)
left outer join bezoeken b using (reisnr)
group by kk.klantnr
order by kk.klantnr
```

Geef de naam, diameter en het langste verblijf op van alle hemelobjecten met minder dan 5 hemelobjecten die rond dit object draaien.
Sorteer op objectnaam.

```sql
SELECT pp.objectnaam, pp.diameter, max(verblijfsduur) as maximale_verblijf
FROM hemelobjecten pp
left outer join hemelobjecten mm ON mm.satellietvan = pp.objectnaam
inner join bezoeken b on b.objectnaam = pp.objectnaam
group by pp.objectnaam
having count(distinct mm.objectnaam) < 5
order by pp.objectnaam
```

Geef de diameter van de grootste, niet bezochte maan (satelliet van een planeet).

```sql
SELECT MAX(m.diameter) AS grootstemaan
FROM hemelobjecten p
INNER JOIN hemelobjecten m ON m.satellietvan = p.objectnaam
LEFT OUTER JOIN bezoeken AS b ON b.objectnaam = m.objectnaam
WHERE p.satellietvan = 'Zon' AND b.objectnaam IS NULL
```

Geef de leeftijd van de jongste klant op moment van vertrek.

```sql
SELECT EXTRACT(year FROM min(age(r.vertrekdatum, k.geboortedatum))) AS jongsteleeftijd
FROM deelnames AS d
INNER JOIN reizen AS r USING (reisnr)
INNER JOIN klanten AS k USING (klantnr)
```

Welke planeten met meer dan 7 manen worden bezocht (met of zonder verblijf)?
Sorteer aflopend op basis van het aantal manen.
Let erop dat je planeten die meerdere keren bezocht worden, niet dubbel telt.

```sql
SELECT pp.objectnaam
FROM hemelobjecten pp
INNER JOIN hemelobjecten mm ON mm.satellietvan = pp.objectnaam
inner join bezoeken b on b.objectnaam = pp.objectnaam
WHERE pp.satellietvan = 'Zon'
group by pp.objectnaam
having count(distinct mm.objectnaam) > 7
order by count(distinct mm.objectnaam) DESC
```
Welke reizen hebben exact drie verschillende hemelobjecten als reisdoel?
Sorteer op reisnr.
```sql
select reisnr
from reizen inner join bezoeken using(reisnr)
group by reisnr
having count(distinct objectnaam)=3
order by reisnr
```
Geef een lijst van alle reizen met tenminste 1 bezoek of passage aan de Maan of aan Mars. De lijst moet stijgend gesorteerd zijn op basis van het vertrekdatum.

```sql
select reisnr, vertrekdatum
from reizen inner join bezoeken using(reisnr)
where objectnaam like 'Mars' or objectnaam like 'Maan'
group by reisnr, vertrekdatum
order by vertrekdatum
```

Bereken voor de klant wiens naam begint met G en eindigt met s hoeveel hij in totaal al besteed heeft aan reizen.
```sql
select naam, sum(prijs)
from klanten inner join deelnames using(klantnr)
inner join reizen using(reisnr)
where naam LIKE 'G%s'
group by naam
order by naam
```
Op welke planeten verblijft men gemiddeld langer dan 2 dagen?
Sorteer op objectnaam.
```sql
select b.objectnaam, avg(verblijfsduur)
from bezoeken b inner join hemelobjecten h on b.objectnaam = h.objectnaam
where satellietvan like 'Zon'
group by b.objectnaam 
having avg(verblijfsduur)>2
order by b.objectnaam
```
Welke reizigers hebben al meer dan 1 reis ondernomen waarvoor ze meer dan 2,5 miljoen euro moesten betalen?
Sorteer op naam
```sql
select naam, count(r.reisnr) as aantal_reizen
from klanten kk inner join deelnames d on kk.klantnr = d.klantnr
inner join reizen r on d.reisnr=r.reisnr
where prijs > 2.5
group by naam
having count(r.reisnr) > 1
order by naam
```



