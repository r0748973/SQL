We willen een statistiek van hoeveel wedstrijden de spelers winnen.
Geef een lijst met aantal gewonnen wedstrijden en het aantal spelers dat dit aantal wedstrijden gewonnen heeft.
Dus bv als er vier spelers zijn die elk drie wedstrijden hebben gewonnen, dan is de output: aantal_gewonnen: 3, aantal_spelers: 4. Dit graag voor alle aantallen gewonnen wedstrijden en alle spelers.
Sorteer op aantal gewonnen wedstrijden.
```sql
select aa.aantal_gewonnen, count(spelersnr) as aantal_spelers
from (select spelersnr, count( gewonnen)as aantal_gewonnen from wedstrijden  where gewonnen > verloren group by spelersnr) aa
group by aa.aantal_gewonnen
order by aa.aantal_gewonnen
```
Geef de spelersgegevens van de speler(s) met het hoogste bedrag (voor één boete, niet het totaalbedrag). Als twee spelers een even hoge boete gehad hebben, moeten beide spelers getoond worden (LIMIT is dus geen optie).
Sorteer alfabetisch op naam en voorletters.
```sql
SELECT spelersnr, voorletters, naam
FROM spelers s INNER JOIN boetes b
USING(spelersnr)
WHERE bedrag = (SELECT MAX(bedrag)
                FROM boetes)
ORDER BY naam, voorletters
			
```
Geef van elke speler het spelersnr, de naam en het verschil tussen zijn of haar jaar van toetreding en het gemiddeld jaar van toetreding van de spelers die in dezelfde plaats wonen. Sorteer op spelersnr. Zet het berekende verschil om naar het datatype numeric met precisie 5 en schaal 3.
```sql
select spelersnr, naam, voorletters, round(jaartoe - (select avg(jaartoe) from spelers ss where ss.plaats = s.plaats), 3) as numeric
from spelers s
order by 1
```
Geef voor elke aanvoerder het spelersnr, de naam en het aantal boetes dat voor hem of haar betaald is en het aantal teams dat hij of zij aanvoert. Aanvoerders zonder boetes mogen niet getoond worden. Sorteer, beginnend bij de eerste kolom, eindigend bij de laatste kolom.
```sql
select spelersnr, naam, voorletters, aantalboetes, aantalteams
from teams 
inner join spelers using(spelersnr)
inner join (
	select spelersnr, count(bedrag) as aantalboetes 
	from boetes
	group by spelersnr) as aboetes using(spelersnr)
inner join (
	select spelersnr, count(teams) as aantalteams
	from teams
	group by spelersnr
	) as ateams using(spelersnr)
group by spelersnr, naam, voorletters, aantalboetes, aantalteams
order by 1,2,3,4,5
```
Geef van alle spelers het verschil tussen het jaar van toetreding en het geboortejaar, maar geef alleen die spelers waarvan dat verschil groter is dan 20. Sorteer deze gegevens beginnend bij de eerste kolom, eindigend bij de laatste kolom.
```sql
select spelersnr, naam, voorletters, (jaartoe - extract(year from geb_datum)) as toetredingsleeftijd
from spelers
where (jaartoe - extract(year from geb_datum)) > 20
order by 1,2,3,4
```
Je kan per speler berekenen hoeveel boetes die speler heeft gehad en wat het totaalbedrag per speler is. Pas nu deze querie aan zodat per verschillend aantal boetes wordt getoond hoe vaak dit aantal boetes voorkwam.Sorteer eerst op de eerste kolom en daarna op de tweede kolom.
```sql
select bb.aantalboetes as a, count(spelersnr)
from (select spelersnr, count(*) as aantalboetes from boetes group by spelersnr ) bb
group by bb.aantalboetes
order by 1, 2
```
Geef van elke speler het spelersnr, de naam en het verschil tussen zijn of haar jaar van toetreding en het gemiddeld jaar van toetreding van de spelers die in dezelfde plaats wonen. Sorteer op spelersnr. Toon 3 getallen na de komma, zet het verschil om naar het numeric type met precisie van 5 en een schaal van 3.
```sql
select spelersnr, naam, voorletters, round(jaartoe - (select avg(jaartoe) from spelers ss where ss.plaats = s.plaats), 3) as numeric
from spelers s
order by 1
```
Geef voor alle spelers die geen penningmeester zijn of zijn geweest alle gewonnen wedstrijden, gesorteerd op wedstrijdnummer.
```sql
select w.spelersnr, wedstrijdnr
from wedstrijden w
where w.gewonnen > w.verloren and w.spelersnr NOT IN  (select spelersnr from bestuursleden where functie = 'Penningmeester')
order by wedstrijdnr
```
Geef voor elke speler die een wedstrijd heeft gespeeld het spelersnr en het totaal aantal boetes. Spelers die een wedstrijd gespeeld hebben, maar geen boetes hebben, moeten ook getoond worden.
Sorteer op het aantal boetes en op spelersnr;
```sql
select w.spelersnr, aantalboetes
from wedstrijden w
left outer join (select spelersnr, count(*) as aantalboetes from boetes group by spelersnr ) as boete on boete.spelersnr = w.spelersnr
group by w.spelersnr, aantalboetes
order by aantalboetes, spelersnr
```

