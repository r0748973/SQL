Geef een totaal overzicht van alle spelers, hun boetes en de functies die ooit vervuld hebben. Elke speler moet getoond worden, als ie een eventuele boete heeft gekregen en/of een eventuele functie als bestuurslid heeft gehad dan moet dit ook getoond worden. Toon de volledige naam, het bedrag en datum van de eventuele boete en de eventuele bestuursfuncties. Sorteer van voor naar achter.
```sql
select naam,voorletters,functie,bedrag,datum
from spelers left outer join bestuursleden using(spelersnr)
		left outer join boetes using(spelersnr)
order by 1,2,3,4,5
```
Hoeveel sets zijn er in totaal gewonnen, hoeveel sets werden in totaal verloren en welk is het uiteindelijke saldo?
```sql
select sum(gewonnen) as totaal_gewonnen, sum(verloren) as totaal_verloren,sum(gewonnen)-sum(verloren) as saldo
from wedstrijden
```
