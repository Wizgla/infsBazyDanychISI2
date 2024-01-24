```mysql
use __firma_zti;
#Zadanie 1
#Wyświetl imię i nazwisko każdego pracownika i jego rok urodzenia.
select pracownik.imie, pracownik.nazwisko, year(pracownik.data_urodzenia) from pracownik group by id_pracownika;
#Zadanie 2
#Wyświetl imię i nazwisko pracowników oraz ich wiek w latach (bez uwzględniania miesiąca i dnia urodzenia).
select pracownik.imie, pracownik.nazwisko, (year(now()) - year(pracownik.data_urodzenia)) as wiek from pracownik group by id_pracownika;
#Zadanie 3
#Wyświetl nazwę działu i liczbę pracowników przypisanych do każdego z nich.
select nazwa, count(id_pracownika) from dzial
inner join pracownik on id_dzialu = pracownik.dzial
group by nazwa;
#Zadanie 4
#Wyświetl nazwę kategorii oraz liczbę produktów w każdej z nich.
select kategoria.nazwa_kategori, count(towar.id_towaru) from kategoria
inner join towar on id_kategori = towar.kategoria
group by nazwa_kategori;
#Zadanie 5
#Wyświetl nazwę kategorii i w kolejnej kolumnie listę wszystkich produktów należącej do każdej z nich.
select kategoria.nazwa_kategori, group_concat(towar.nazwa_towaru) from kategoria
inner join towar on id_kategori = towar.kategoria
group by nazwa_kategori;
#Zadanie 6
#Wyświetl średnie zarobki pracowników za zaokrągleniem do 2 miejsc po przecinku.
select round(avg(pracownik.pensja),2) from pracownik;
#Zadanie 7
#Wyświetl średnie zarobki pracowników, którzy pracują co najmniej od 5 lat.
select round(avg(pracownik.pensja),2) from pracownik where (year(now())- year(data_zatrudnienia)>=5);
#Zadanie 8
#Wyświetl 10 najczęściej sprzedawanych produktów.
select towar.nazwa_towaru,count(nazwa_towaru) from towar
inner join pozycja_zamowienia on id_towaru = pozycja_zamowienia.towar
group by nazwa_towaru
order by count(id_towaru) desc limit 10;
#Zadanie 9
#Wyświetl numer zamówienia, jego wartość (suma wartości wszystkich jego pozycji) zarejestrowanych w pierwszym kwartale 2017 roku.
select zamowienie.numer_zamowienia, sum(pozycja_zamowienia.ilosc*pozycja_zamowienia.cena) as wartosc from zamowienie
inner join pozycja_zamowienia on zamowienie.id_zamowienia = pozycja_zamowienia.zamowienie
where data_zamowienia between '2017-01-01' and '2017-04-01'
group by numer_zamowienia;
#Zadanie 10
#Wyświetl imie, nazwisko i sumę wartości zamówień, które dany pracownik dodał. Posortuj malejąco po sumie.
select imie, pracownik.nazwisko,sum(pozycja_zamowienia.ilosc*pozycja_zamowienia.cena) as suma from pracownik
inner join zamowienie on id_pracownika = zamowienie.pracownik_id_pracownika
inner join pozycja_zamowienia on id_zamowienia = pozycja_zamowienia.zamowienie
group by id_pracownika
order by suma desc;
#Zadanie 11
#Wyświetl nazwę działu i minimalną, maksymalną i średnią wartość pensji w każdym dziale.
select dzial.nazwa, min(pensja), max(pracownik.pensja),round(avg(pracownik.pensja),2) from dzial
inner join pracownik on id_dzialu = pracownik.dzial
group by dzial.nazwa;
#Zadanie 12
#Wyświetl pełną nazwę klienta, wartość zamówienia dla 10 najwyższych wartości zamówienia.
select klient.pelna_nazwa,sum(pozycja_zamowienia.ilosc*pozycja_zamowienia.cena) as wartosc from klient
inner join zamowienie on id_klienta = zamowienie.klient
inner join pozycja_zamowienia on id_zamowienia = pozycja_zamowienia.zamowienie
group by id_klienta
order by wartosc desc limit 10;
#Zadanie 13
#Wyświetl wartość przychodu dla każdego roku. Dane posortuj malejąco według sumy wartości zamówień.
select year(zamowienie.data_zamowienia) as rok,sum(pozycja_zamowienia.ilosc*pozycja_zamowienia.cena) as wartosc from zamowienie
inner join pozycja_zamowienia on zamowienie.id_zamowienia = pozycja_zamowienia.zamowienie
group by rok
order by wartosc desc;
#Zadanie 14
#Wyświetl sumę wartości wszystkich anulowanych zamówień.
select sum(pozycja_zamowienia.ilosc*pozycja_zamowienia.cena) as wartosc from pozycja_zamowienia
inner join zamowienie on pozycja_zamowienia.zamowienie = zamowienie.id_zamowienia
inner join status_zamowienia on zamowienie.status_zamowienia = status_zamowienia.id_statusu_zamowienia
where nazwa_statusu_zamowienia = 'anulowane';
#Zadanie 15
#Wyświetl liczbę zamówień i sumę zamówień dla każdego miasta z podstawowego adresu klientów.
select adres_klienta.miejscowosc as miejscowosc,count(zamowienie.id_zamowienia) as liczba_zamowien,sum(pozycja_zamowienia.ilosc*pozycja_zamowienia.cena) as suma_zamowien from adres_klienta
inner join klient on id_klienta = adres_klienta.klient
inner join typ_adresu on typ_adresu = typ_adresu.id_typu
inner join zamowienie on id_klienta = zamowienie.klient
inner join pozycja_zamowienia on id_zamowienia = pozycja_zamowienia.zamowienie
where typ_adresu.nazwa = 'podstawowy'
group by miejscowosc
order by suma_zamowien desc;
#Zadanie 16
#Wyświetl dotychczasowy dochód firmy biorąc pod uwagę tylko zamówienia zrealizowane.
select sum(pozycja_zamowienia.cena*pozycja_zamowienia.ilosc)-sum(towar.cena_zakupu*pozycja_zamowienia.ilosc) as dochod from towar
inner join pozycja_zamowienia on id_towaru = pozycja_zamowienia.towar
inner join zamowienie on id_zamowienia = pozycja_zamowienia.zamowienie
inner join status_zamowienia on id_statusu_zamowienia = zamowienie.status_zamowienia;
#Zadanie 17
#Policz i wyświetl dochód (przychód z zamówień i cena zakupu towaru) w każdym roku działalności firmy.
select year(data_zamowienia) as rok, sum(pozycja_zamowienia.cena*pozycja_zamowienia.ilosc)-sum(towar.cena_zakupu*pozycja_zamowienia.ilosc) as dochod from zamowienie
inner join pozycja_zamowienia on id_zamowienia = pozycja_zamowienia.zamowienie
inner join towar on id_towaru = pozycja_zamowienia.towar
group by rok;
#Zadanie 18
#Wyświetl wartość aktualnego stanu magazynowego z podziałem na kategorię produktów.
select kategoria.nazwa_kategori as kategoria,sum(stan_magazynowy.ilosc*towar.cena_zakupu) as wartosc_zakupu,sum(stan_magazynowy.ilosc*pozycja_zamowienia.cena) as wartosc_sprzedazy from kategoria
inner join towar on id_kategori = towar.kategoria
inner join stan_magazynowy on id_towaru = stan_magazynowy.towar
inner join pozycja_zamowienia on id_towaru = pozycja_zamowienia.towar
group by kategoria;
#Zadanie 19
#Przygotuj zapytanie, które wyświetli dane w poniższej postaci (policz ilu pracowników urodziło się w danym miesiącu - uwaga na porządek sortowania).
```
