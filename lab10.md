1:
```mysql
SELECT imie, nazwisko, data_urodzenia FROM __firma_zti.pracownik;
```
2:
```mysql
SELECT imie, nazwisko, year(now())-year(data_urodzenia) as wiek FROM __firma_zti.pracownik;
```
3:
```mysql
select d.nazwa, count(p.id_pracownika) as liczba_pracownikow from __firma_zti.dzial 
inner join __firma_zti.pracownik p on p.dzial=d.id_dzialu group by d.nazwa
```
4:
```mysql
select k.nazwa_kategori, count(t.id_towaru) as liczba_produktow from kategoria k 
inner join towar t on t.kategoria=k.id_kategori group by k.nazwa_kategori
```
5:
```mysql
select k.nazwa_kategori, group_concat(t.nazwa_towaru)  from kategoria k 
inner join towar t on t.kategoria=k.id_kategori group by k.nazwa_kategori
```
6:
```mysql
select Round(avg(pensja), 2) from pracownik

```
7:
```mysql
select Round(avg(pensja), 2) from pracownik where year(now()) - year(data_zatrudnienia)<=5
```
8:
```mysql
select t.nazwa_towaru from pozycja_zamowienia p 
inner join towar t on t.id_towaru = p.towar
group by t.nazwa_towaru  order by count(p.towar) desc limit 10

```
9:
```mysql
SELECT z.numer_zamowienia,sum(p.cena*p.ilosc) as wartosc FROM zamowienie z
inner join pozycja_zamowienia p on p.zamowienie=z.id_zamowienia where z.data_zamowienia between '2017-01-01' and '2017-03-31' 
group by z.numer_zamowienia
```
10:
```mysql
select p.imie, p.nazwisko, sum(pz.cena*pz.ilosc) as wartosc from pracownik p
inner join zamowienie z on p.id_pracownika=z.pracownik_id_pracownika
inner join pozycja_zamowienia pz  on z.id_zamowienia=pz.zamowienie
group by id_pracownika  order by sum(cena*ilosc) desc
```
11:
```mysql
select d.nazwa,  max(p.pensja), min(p.pensja), avg(pensja) from dzial d
inner join pracownik p on p.dzial=d.id_dzialu
group by p.dzial
```
12:
```mysql
select k.pelna_nazwa, sum(pz.ilosc*pz.cena) from klient k
inner join zamowienie z on z.klient = k.id_klienta
inner join pozycja_zamowienia pz on pz.zamowienie=z.id_zamowienia
group by pelna_nazwa order by sum(ilosc*cena) desc limit 10 
```
13:
```mysql
select year(z.data_zamowienia) as rok, sum(pz.ilosc*pz.cena) as przychod from zamowienie z
inner join pozycja_zamowienia pz on z.id_zamowienia=pz.zamowienie
inner join pracownik p on p.id_pracownika=z.pracownik_id_pracownika
group by year(z.data_zamowienia) order by sum(ilosc*cena) desc
```
14:
```mysql
select sz.nazwa_statusu_zamowienia, sum(pz.ilosc*pz.cena) as wartosc from pozycja_zamowienia pz
inner join zamowienie z on z.id_zamowienia=pz.zamowienie
inner join status_zamowienia sz on sz.id_statusu_zamowienia=z.status_zamowienia
where id_statusu_zamowienia=6 group by nazwa_statusu_zamowienia
```
15:
```mysql
select count(pz.zamowienie) as liczba_zamowien, sum(pz.ilosc*pz.cena) as suma_zamowien, ak.miejscowosc from pozycja_zamowienia pz
inner join zamowienie z on pz.zamowienie=z.id_zamowienia
inner join adres_klienta ak on ak.klient=z.klient
where typ_adresu=1
group by miejscowosc 

```
16:
```mysql
select sum(pz.ilosc*pz.cena)-sum(t.cena_zakupu) as dochod from pozycja_zamowienia pz
inner join zamowienie z on z.id_zamowienia= pz.zamowienie
inner join towar t on pz.towar=t.id_towaru
where status_zamowienia = 5
```
17:
```mysql
select sum(pz.ilosc*pz.cena)-sum(t.cena_zakupu) as dochod,year(data_zamowienia) as rok from pozycja_zamowienia pz
inner join zamowienie z on z.id_zamowienia= pz.zamowienie
inner join towar t on pz.towar=t.id_towaru
group by rok
```
18:
```mysql
select sum(sm.ilosc*pz.cena) as wartosc_stanu, k.nazwa_kategori from stan_magazynowy sm
inner join towar t on sm.towar=t.id_towaru
inner join kategoria k on t.kategoria=k.id_kategori
inner join pozycja_zamowienia pz on pz.towar=t.id_towaru
group by nazwa_kategori

```
19:
```mysql
select monthname(data_urodzenia) as miesiąc, count(id_pracownika) as Liczba_pracowników from pracownik 
group by miesiąc order by month(miesiąc) desc

```
20:
```mysql
select imie, nazwisko, sum(pensja) from pracownik
 where  data_zatrudnienia <= CURRENT_DATE group by id_pracownika

```
