Zadanie pierwsze:
```mysql
create table infs_matuznyii.kreatura as select * from wikingowie.kreatura;
create table infs_matuznyii.ekwipunek as select * from wikingowie.ekwipunek;
create table infs_matuznyii.zasob as select * from wikingowie.zasob;
select * from zasob;
select * from zasob where rodzaj = 'jedzenie';
select idZasobu, ilosc from ekwipunek where idKreatury in (1,3,5);
```
Zadanie drugie:
```mysql
select * from kreatura where rodzaj !='wiedzma' and udzwig >= 50;
select * from zasob where waga between 2 and 5;
select * from kreatura where nazwa like '%or%' and udzwig between 30 and 70;
```
Zadanie trzecie:
```mysql
select * from zasob where month(dataPozyskania) in (7,8);
select * from zasob order by rodzaj ASC;
select * from kreatura where kreatura.dataUr is not null order by dataUr asc limit 5;
```
Zadanie czwarte:
```mysql
select distinct zasob.rodzaj from zasob;
select concat(nazwa,'-',rodzaj) as nazwaIrodzaj from kreatura where rodzaj like 'wi%';
select *, ilosc * waga as calkowitaWaga from zasob where year(dataPozyskania) between 2000 and 2007;
```
Zadanie pjate:
```mysql
select nazwa, waga * 0.7 as massaBrutto, waga * 0.3 as massaOdpadow from zasob where rodzaj = 'jedzenie';
select * from zasob where rodzaj is null;
select distinct rodzaj from zasob where nazwa like '%Ba' or 'os%' order by rodzaj asc;
```
