Zadanie pierwsze:
```mysql
CREATE DATABASE lab;
use lab;
#zadanie pierwsze
create table postac (
    id_postaci INT NOT NULL auto_increment PRIMARY KEY,
    nazwa char(40),
    rodzaj ENUM('wiking','ptak','kobieta'),
    data_ur DATE,
    wiek INT unsigned);
insert into postac(nazwa, rodzaj, data_ur, wiek)
values ('Bjorn','wiking','2000-01-31',24),
       ('Drozd','ptak','2000-01-31',15),
       ('Tesciowa','kobieta','2000-01-31',124);
update postac
set wiek = 88
where nazwa = 'Tesciowa';
```
Zadanie drugie:
```mysql
create table walizka(
    id_walizki INT AUTO_INCREMENT primary key,
    pojemnosc INT UNSIGNED,
    kolor enum('rozowy', 'czerwony', 'teczowy', 'zolty'),
    id_wlasciciela INT,
    FOREIGN KEY (id_wlasciciela) REFERENCES postac(id_postaci) ON DELETE CASCADE
);
alter table walizka
alter column kolor set default 'rozowy';
insert into walizka(pojemnosc, id_wlasciciela)
values (25, (select id_postaci from postac where nazwa = 'Bjorn'));
insert into walizka(pojemnosc, id_wlasciciela)
values (30, (select id_postaci from postac where nazwa = 'Tesciowa'));
```
Zadanie trzecie:
```mysql
create table izba(
    adres_budynku VARCHAR(70),
    nazwa_izby varchar(70),
    metraz INT UNSIGNED,
    wlasciciel INT,
    PRIMARY KEY (adres_budynku, nazwa_izby),
    foreign key (wlasciciel) references postac(id_postaci) ON DELETE set null
);
alter table izba
add column kolor varchar(20) default 'czarny' after metraz;
insert into izba(adres_budynku, nazwa_izby, metraz,wlasciciel)
values ('nisuganowo 1','spizarnia',20,1);
```
Zadanie czwarte:
```mysql
create table przetwory(
    id_przetworu INT UNSIGNED PRIMARY KEY,
    rok_produkcji INT default 1654,
    id_wykonawcy INT,
    zawartosc varchar(100),
    dodatek varchar(100) default 'papryczka chilli',
    id_konsumenta INT,
    foreign key (id_konsumenta) references postac(id_postaci),
    foreign key (id_wykonawcy) references postac(id_postaci)
);
insert into przetwory(id_przetworu, id_wykonawcy, zawartosc, id_konsumenta)
values (1,1,'puszka Zubra',1);
```
Zadanie pjate:
```mysql
insert into postac(nazwa, rodzaj, data_ur, wiek)
values ('WikingNoName1','wiking','2009-01-01',15),
       ('WikingNoName2','wiking','2009-01-01',15),
       ('WikingNoName3','wiking','2009-01-01',15),
       ('WikingNoName4','wiking','2009-01-01',15),
       ('WikingNoName5','wiking','2009-01-01',15);
```
Zadanie szoste:
```mysql
create table statek(
    nazwa_statku varchar(30) primary key,
    rodzaj enum('bojowy','transportowy'),
    data_wodowania date,
    max_ladowosc INT unsigned
);

insert into statek(nazwa_statku, rodzaj, data_wodowania, max_ladowosc)
values ('falkon1','bojowy','2000-01-01',100),
       ('falkon2','bojowy','2000-01-01',100);
alter table postac add column funkcja varchar(200);
update postac set funkcja = 'capitan' where nazwa = 'Bjorn';
alter table postac add column id_statku VARCHAR(200);
alter table postac add foreign key (id_statku) references statek(nazwa_statku);
update postac set id_statku = 'falkon1' where rodzaj in ('wiking');
update postac set id_statku = 'falkon1' where rodzaj in ('ptak');
DELETE FROM izba WHERE nazwa_izby = 'spiżarnia';
DROP TABLE izba;
```
