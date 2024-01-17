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
#Wyświetl nazwę działu i liczbę pracowników przypisanych do każdego z nich.
select dzial.nazwa as nazwa_dzialu, count(pracownik.id_pracownika) as ilosc_pracownikow from dzial
inner join pracownik on id_dzialu = pracownik.dzial
group by nazwa
```
4:
```mysql
#Wyświetl nazwę kategorii oraz liczbę produktów w każdej z nich.
select kategoria.nazwa_kategori, count(id_towaru) from kategoria
inner join towar on kategoria = id_kategori
group by nazwa_kategori
```
5:
```mysql
#Wyświetl nazwę kategorii i w kolejnej kolumnie listę wszystkich produktów należącej do każdej z nich.
select kategoria.nazwa_kategori, group_concat(nazwa_towaru) from kategoria
inner join towar on id_kategori = towar.kategoria
group by nazwa_kategori
```
6:
```mysql
select round(avg(pracownik.pensja),2) from pracownik;
```
7:
```mysql
select avg(pracownik.pensja) from pracownik where year(now())-year(data_zatrudnienia) > 5;
```
8:
```mysql
select towar.nazwa_towaru from pozycja_zamowienia
inner join towar on id_towaru = pozycja_zamowienia.towar
group by nazwa_towaru
order by count(towar) desc limit 10;
```
9:
```mysql
#Wyświetl numer zamówienia, jego wartość (suma wartości wszystkich jego pozycji) zarejestrowanych w pierwszym kwartale 2017 roku.
select zamowienie.numer_zamowienia,sum(ilosc * cena) from zamowienie
inner join pozycja_zamowienia on id_zamowienia = pozycja_zamowienia.zamowienie
where data_zamowienia between '2017-01-01' and '2017-03-31'
group by numer_zamowienia;
```
10:
```mysql
#Wyświetl imie, nazwisko i sumę wartości zamówień, które dany pracownik dodał. Posortuj malejąco po sumie.
select pracownik.imie,pracownik.nazwisko, sum(ilosc * cena) as suma from pracownik
inner join zamowienie on id_pracownika = zamowienie.pracownik_id_pracownika
inner join pozycja_zamowienia on id_zamowienia = pozycja_zamowienia.zamowienie
group by id_pracownika
order by suma desc;
```
11:
```mysql
#Wyświetl nazwę działu i minimalną, maksymalną i średnią wartość pensji w każdym dziale.
select dzial.nazwa, min(pracownik.pensja),max(pracownik.pensja),avg(pracownik.pensja) from pracownik
inner join dzial on id_dzialu = pracownik.dzial
group by nazwa;
```
12:
```mysql
#Wyświetl pełną nazwę klienta, wartość zamówienia dla 10 najwyższych wartości zamówienia.
select klient.pelna_nazwa, sum(ilosc * cena) as suma from klient
inner join zamowienie on id_klienta = zamowienie.klient
inner join pozycja_zamowienia on id_zamowienia = pozycja_zamowienia.zamowienie
group by id_klienta
order by suma desc limit 10;
```
13:
```mysql
#Wyświetl wartość przychodu dla każdego roku. Dane posortuj malejąco według sumy wartości zamówień.
select year(zamowienie.data_zamowienia)as rok,sum(ilosc * cena) as suma from zamowienie
inner join pozycja_zamowienia on id_zamowienia = pozycja_zamowienia.zamowienie
group by rok
order by suma desc;
```
14:
```mysql
#Wyświetl sumę wartości wszystkich anulowanych zamówień.
select sum(ilosc * cena) as suma from pozycja_zamowienia
inner join zamowienie on id_zamowienia = zamowienie
inner join status_zamowienia on id_statusu_zamowienia = status_zamowienia
where nazwa_statusu_zamowienia = 'anulowane'
group by nazwa_statusu_zamowienia;
```
15:
```mysql
#Wyświetl liczbę zamówień i sumę zamówień dla każdego miasta z podstawowego adresu klientów.
select  miejscowosc, count(zamowienie), sum(ilosc * cena) from zamowienie
inner join pozycja_zamowienia on id_zamowienia = pozycja_zamowienia.zamowienie
inner join klient on id_klienta = zamowienie.klient
inner join adres_klienta on id_klienta = adres_klienta.klient
where typ_adresu=1
group by miejscowosc;
```
16:
```mysql
#Wyświetl dotychczasowy dochód firmy biorąc pod uwagę tylko zamówienia zrealizowane.
select sum(pozycja_zamowienia.ilosc*pozycja_zamowienia.cena) as Przychod,sum(pozycja_zamowienia.ilosc*towar.cena_zakupu) as CenaZakupu,sum(pozycja_zamowienia.ilosc*pozycja_zamowienia.cena)-sum(pozycja_zamowienia.ilosc*towar.cena_zakupu) as Dochod from pozycja_zamowienia
inner join towar on towar = towar.id_towaru
inner join zamowienie on zamowienie = zamowienie.id_zamowienia
inner join status_zamowienia on id_statusu_zamowienia = status_zamowienia
where nazwa_statusu_zamowienia = 'zrealizowane'
group by nazwa_statusu_zamowienia;
```
17:
```mysql
#Wyświetl wartość aktualnego stanu magazynowego z podziałem na kategorię produktów.
select kategoria.nazwa_kategori,sum(stan_magazynowy.ilosc*towar.cena_zakupu) as wartosc_zakupu_magazynu,sum(stan_magazynowy.ilosc*pozycja_zamowienia.cena) as wartosc_przy_sprzedazy from kategoria
inner join towar on id_kategori = towar.kategoria
inner join stan_magazynowy on id_towaru = stan_magazynowy.towar
inner join pozycja_zamowienia on id_towaru = pozycja_zamowienia.towar
group by nazwa_kategori;
```
18:
```mysql
#Policz i wyświetl dochód (przychód z zamówień i cena zakupu towaru) w każdym roku działalności firmy.
select year(zamowienie.data_zamowienia)as rok,sum(pozycja_zamowienia.ilosc * cena)as przychod,sum(pozycja_zamowienia.ilosc*towar.cena_zakupu)as cena_zakupu,(sum(pozycja_zamowienia.ilosc * cena))-sum(pozycja_zamowienia.ilosc*towar.cena_zakupu) as dochod from zamowienie
inner join pozycja_zamowienia on id_zamowienia = pozycja_zamowienia.zamowienie
inner join towar on id_towaru = pozycja_zamowienia.towar
group by rok
```
19:
```mysql
#Przygotuj zapytanie, które wyświetli dane w poniższej postaci (policz ilu pracowników urodziło się w danym miesiącu - uwaga na porządek sortowania).
select monthname(pracownik.data_urodzenia) as miesiac, count(pracownik.id_pracownika) as liczba_pracownikow from pracownik
group by miesiac
order by FIELD(miesiac,'January', 'February', 'March', 'April', 'May', 'June', 
'July','August','September','October','November','December');
```
20:
```mysql
#Wyświetl imię i nazwisko pracownika i koszt jaki poniósł pracodawca od momentu jego zatrudnienia.
select pracownik.imie,pracownik.nazwisko,(TIMESTAMPDIFF(MONTH,data_zatrudnienia,date(now()))* pensja) as koszt from pracownik
where data_zatrudnienia  <= date(now())
group by id_pracownika;
```






