Zadanie pierwsze:
```mysql
select avg(waga) as medWaga from kreatura where rodzaj = 'wiking';
select rodzaj, avg(waga) as medWaga, count(*) as liczbaKreatur from kreatura group by rodzaj;
select rodzaj,round(avg(YEAR(now()) - year(dataUr))) as medWiek from kreatura group by rodzaj;
```
Zadanie drugie:
```mysql
select rodzaj, sum(waga) as sumWag from zasob where zasob.rodzaj is not null group by rodzaj;
select nazwa,avg(waga) as medWaga from zasob where ilosc >= 4 group by nazwa having sum(waga)>10;
select rodzaj, count(distinct nazwa) as liczbaRoznychNazw from zasob group by rodzaj having count(*) > 1;
```
Zadanie trzecie:
```mysql
select k.idKreatury, k.nazwa as nazwaKreatury, z.nazwa as nazwaZasobu, e.ilosc from ekwipunek e join kreatura k on e.idKreatury = k.idKreatury join zasob z on e.idZasobu = z.idZasobu;
select k.idKreatury, k.nazwa as nazwaKreatury, group_concat(z.nazwa separator ',') as nazwaZasobow from ekwipunek e join kreatura k on e.idKreatury = k.idKreatury join zasob z on e.idZasobu = z.idZasobu group by k.idKreatury, k.nazwa;
select k.idKreatury, k.nazwa as nazwaKreatury from kreatura k left join ekwipunek e on k.idKreatury = e.idKreatury  where e.idKreatury is null;
```
Zadanie czwarte:
```mysql
select k.nazwa as nazwaWikingow, z.nazwa as nazwaZasobu from kreatura k join ekwipunek e on k.idKreatury = e.idKreatury join zasob z on e.idZasobu = z.idZasobu where year(k.dataUr) between 1600 and 1699;
select k.nazwa as nazwaKreatury, e.ilosc as iloscJedzenia from kreatura k join ekwipunek e on k.idKreatury = e.idKreatury join zasob z on e.idZasobu = z.idZasobu where z.rodzaj = 'jedzenie' order by k.dataUr desc limit 5;
select k1.nazwa as nazwaPierwszejKreatury, k2.nazwa as nazwaDrugiejKreatury from kreatura k1 join kreatura k2 on k1.idKreatury = k2.idKreatury - 5;
```
Zadanie pjate:
```mysql
select k.rodzaj, avg(z.waga) as sredniaWagaZasobow from kreatura k join ekwipunek e on k.idKreatury = e.idKreatury join zasob z on e.idZasobu = z.idZasobu where k.rodzaj not in ('malpa','waz') and e.ilosc < 30 group by k.rodzaj;
SELECT
  rodzaj,
  AVG(year(dataUr)) AS sredni_rok_ur,
  (
    select rodzaj from kreatura where dataUr = (select min(dataUr) from kreatura where dataUr is not null limit 1)
    ) as najmlodszaKreatura,
    (
    select rodzaj from kreatura where dataUr = (select max(dataUr) from kreatura where dataUr is not null limit 1)
    ) as najstarszaKreatura
FROM kreatura
WHERE dataUr IS NOT NULL
GROUP BY rodzaj;
```
