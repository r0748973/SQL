Geef alle spelers die geen enkele wedstrijd voor team 1 hebben gespeeld. Sorteer op naam, daarna op spelersnr.
```sql
select spelersnr, naam
from spelers s
where NOT EXISTS( select * from wedstrijden w where w.spelersnr = s.spelersnr and teamnr = 1)
order by 2, 1
```
Geef alle wedstrijden van het team waarvan speler 6 aanvoerder is. Sorteer
```sql
select wedstrijdnr 
from wedstrijden w
where teamnr = (select teamnr from teams where spelersnr = 6)
```
Maak een lijst met alle spelers die ooit een boete gekregen hebben die hoger is dan 50 euro. Geen dubbels. Sorteer van voor naar achter.
```sql
select distinct naam, voorletters, plaats
from spelers s inner join boetes b using(spelersnr)
group by s.naam, s.voorletters, s.plaats, b.bedrag
having bedrag > 50
```
