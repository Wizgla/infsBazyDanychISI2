```mysql
use infs_matuznyii;
#zadanie1
create table postac(
    id_postaci int auto_increment not null primary key,
    nazwa varchar(40),
    rodzaj enum('wiking','ptak','kobieta'),
    data_ur date,
    wiek int unsigned
);
insert into postac(nazwa, rodzaj, data_ur)
values ('Bjorn','wiking','1990-01-01'),
       ('Drozd','ptak','2010-01-01'),
       ('Tesciowa','kobieta','1900-01-01');
insert into postac(wiek)
values (year(now())-data_ur);
update postac set wiek = 88,data_ur = (date_sub(now(), interval 88 year)) where nazwa = 'tesciowa';
#zadanie2
create table walizka(
    id_walizki int auto_increment not null primary key,
    pojemnosc int unsigned,
    kolor enum('rozowy','teczowy','czerwony','zolty'),
    id_wlasciciela int,
    foreign key (id_wlasciciela) references postac(id_postaci)on delete cascade
);
alter table walizka alter column kolor set default 'rozowy';
insert into walizka(pojemnosc,id_wlasciciela)
values (25,(select postac.id_postaci from postac where nazwa = 'Bjorn')),
       (60,(select postac.id_postaci from postac where nazwa = 'Tesciowa'));
#zadanie3
create table izba(
    adres_budynku varchar(60),
    nazwa_izby varchar(60),
    metraz int unsigned not null,
    wlasciciel int,
    primary key (adres_budynku, nazwa_izby),
    foreign key (wlasciciel) references postac(id_postaci) on delete set null
);
alter table izba add column kolor varchar(20) default 'czarny' after metraz;
insert into izba(adres_budynku, nazwa_izby, metraz, wlasciciel)
values ('nisuganowo 1','bar',20,(select postac.id_postaci from postac where nazwa = 'Bjorn'));
update izba set nazwa_izby = 'spizarnia' where adres_budynku = 'nisuganowo 1';
#zadanie4
create table przetwory(
    id_przetworu int primary key,
    rok_produkcji int default 1654,
    id_wykonawcy int,
    zawartosc varchar(20),
    dodatek varchar(20) default 'papryczka chilli',
    id_konsumenta int,
    foreign key (id_konsumenta) references postac(id_postaci),
    foreign key (id_wykonawcy) references postac(id_postaci)
);
alter table przetwory modify column id_przetworu int auto_increment;
insert into przetwory(id_wykonawcy,zawartosc,id_konsumenta)
values ((select postac.id_postaci from postac where nazwa = 'Bjorn'),'bigos',(select postac.id_postaci from postac where nazwa = 'Bjorn'));
#zadanie5
insert into postac(nazwa, rodzaj, data_ur, wiek)
values ('wiking1','wiking','1990-01-01',35),
       ('wiking2','wiking','1990-01-01',35),
       ('wiking3','wiking','1990-01-01',35),
       ('wiking4','wiking','1990-01-01',35),
       ('wiking5','wiking','1990-01-01',35);
create table statek(
    nazwa_statku int primary key,
    rodzaj_statku enum('bojowy'),
    data_wodowanie date,
    max_ladowosc int unsigned
);
alter table statek modify column nazwa_statku varchar(30);
insert into statek(nazwa_statku, rodzaj_statku, data_wodowanie, max_ladowosc)
values            ('falkon','bojowy','2001-01-01',100),
                  ('falkon2','bojowy','2001-01-01',100);
alter table postac add column funkcja varchar(30);
alter table postac add column statek varchar(30);
alter table postac add foreign key(statek) references statek(nazwa_statku);
update postac set statek = 'falkon2' where rodzaj = 'wiking' or rodzaj = 'ptak';
delete from izba where nazwa_izby = 'spizarnia';
drop table izba;




#lab5
#zadanie1
#a) uśmierć dwóch najstarszych wikingów, ale to nie może być Bjorn
update postac set data_ur = '1969-01-02', wiek = (year(now())-'1969') where nazwa = 'wiking1' or nazwa = 'wiking3';
delete from postac where nazwa !='Bjorn' and rodzaj = 'wiking' order by data_ur asc limit 2;
#b) usuń klucz główny z tabeli postac
#alter table walizka drop foreign key walizka_ibfk_1;
#alter table przetwory drop foreign key przetwory_ibfk_2;
#alter table przetwory drop foreign key przetwory_ibfk_1;
alter table postac modify id_postaci int;
alter table postac drop primary key;
alter table postac drop column id_postaci;
#zadanie2
#a) do tabeli postać dodaj pole pesel składające się z 11 znaków i ustaw to pole na klucz główny
alter table postac add column pesel int(11) first;
update postac set pesel = CONCAT(FLOOR(RAND() * 9 + 1), FLOOR(RAND() * 100000000)) where nazwa is not null;
delete from postac where nazwa is null;
alter table postac add primary key postac(pesel);
#b) w tabeli postać zmień pole rodzaj, tak, aby możliwe było dodanie syreny
alter table postac modify column rodzaj enum('wiking','ptak','kobieta','syrena');
#c) wstaw do tabeli syrenę o nazwie Gertruda Nieszczera
alter table postac modify column pesel int(11) auto_increment;
insert into postac(nazwa, rodzaj, data_ur, wiek)
values ('Gertruda Nieszczera','syrena','1985-01-01',year(now())-'1985');
#zadanie3
#a) Wszystkie postacie, które mają w swojej nazwie 'a', wsadź na statek Bjorna
set @statekBjorna = (select statek from postac where nazwa = 'Bjorn');
update postac set statek = @statekBjorna where nazwa like '%a%';
#b) Zmniejsz ładowność wszystkim statkom o 30%, których data wodowania była w XX wieku
update statek set max_ladowosc = (max_ladowosc - (max_ladowosc*0.3)) where data_wodowanie between '1901-01-01' and '2001-01-01';
#c) ustaw warunek sprawdzający czy wiek postaci nie jest większy od 1000
alter table postac modify column wiek int(3); #nie wiem jak to zrobic prawidlowo ale tu bedzie ograniczenie do trech cyfr, czyli do 999
#zadanie4
#a) Do postaci dodaj węża Loko, tylko nie wsadzaj go na statek
alter table postac modify column rodzaj enum('wiking','ptak','kobieta','syrena','waz');
insert into postac(nazwa,rodzaj,data_ur,wiek)
values ('loko','waz','2010-01-01',year(now())-'2010');
#b) Stwórz nową tabelę na podstawie tabeli Postacie (dokładnie takie same pola), nazwij ją Marynarz -
#wrzuć do tej tabeli wszystkie postacie które mają zdefiniowany statek
create table marynaz like postac;
insert into marynaz select * from postac where postac.statek is not null;
#c) dostosuj odpowiednio klucze główne i obce
alter table marynaz add foreign key (statek) references statek(nazwa_statku);
#Zadanie5
#a) wysadź wszystkich ze statku
update postac set statek = null where nazwa is not null;
update marynaz set statek = null where nazwa is not null;
#b) uśmierć jednego wikinga
set @zabityWiking = (select postac.nazwa from postac where rodzaj = 'wiking' and nazwa != 'Bjorn' limit 1);
delete from postac where nazwa = @zabityWiking;
delete from marynaz where nazwa = @zabityWiking;
#c) zniszcz wszystkie statki
    delete from statek where nazwa_statku is not null;
    #alter table postac drop foreign key postac_ibfk_1;
    #alter table marynaz drop foreign key marynaz_ibfk_1;
    alter table postac drop column statek;
    alter table marynaz drop column statek;
#d) usuń tabelę statek
    drop table statek;
#e) stwórz tabelę zwierz z polami id - klucz główny samo zwiększający się, nazwa - ciąg znaków, wiek - liczba
    create table zwierz(
        id_zwierza int auto_increment primary key,
        nazwa varchar(20),
        wiek int
    );
#f) przekopiuj z tabeli postac wszystkie zwierzaki
insert into zwierz select pesel,nazwa,wiek from postac where rodzaj in ('ptak','waz','syrena');
delete from zwierz;

```
