Zadanie pierwsze:
```mysql
DELETE FROM postac
WHERE rodzaj = 'wiking' and nazwa != 'Bjorn'
ORDER BY wiek DESC
LIMIT 2;
#alter table walizka DROP foreign key walizka_ibfk_1;
#alter table przetwory DROP foreign key przetwory_ibfk_1;
#alter table przetwory DROP foreign key przetwory_ibfk_2;
ALTER TABLE postac MODIFY COLUMN id_postaci INT NOT NULL;
ALTER TABLE postac DROP PRIMARY KEY;
```
Zadanie drugie:
```mysql
alter table postac add column pesel char(11) not null;
update postac set pesel = '12345678901' where nazwa = 'Bjorn';
update postac set pesel = '12345678902' where nazwa = 'Tesciowa';
update postac set pesel = '12345678903' where nazwa = 'Drozd';
update postac set pesel = '12345678904' where nazwa = 'WikingNoName3';
update postac set pesel = '12345678905' where nazwa = 'WikingNoName4';
update postac set pesel = '12345678906' where nazwa = 'WikingNoName5';
alter table postac ADD PRIMARY KEY (pesel);
alter table postac modify column rodzaj enum('wiking', 'ptak', 'kobieta', 'syrena');
insert into postac(id_postaci, nazwa, rodzaj, data_ur, wiek, funkcja, id_statku, pesel)
values (20,'Gertruda Nieszczera', 'syrena', '2000-01-01',24,null,'falkon2','12345678907');
```
Zadanie trzecie:
```mysql
update postac set id_statku = 'falkon1' where nazwa like '%a%';
update statek set max_ladowosc = max_ladowosc * 0.7 where year(data_wodowania) between 1900 and 1999;
delete from postac where wiek > 1000;
```
Zadanie czwarte:
```mysql
alter table postac modify column rodzaj enum('wiking', 'ptak', 'kobieta', 'syrena','waz');
insert into postac (id_postaci, nazwa, rodzaj, data_ur, wiek, funkcja, id_statku, pesel)
values (21,'Loko','waz','2000-01-01',24,null,null,'12345678908');
CREATE TABLE Marynarz AS SELECT * FROM postac WHERE id_statku IS NOT NULL;
show create table postac;
ALTER TABLE Marynarz ADD PRIMARY KEY (pesel);
ALTER TABLE Marynarz ADD FOREIGN KEY (id_statku) REFERENCES statek(nazwa_statku);
```
Zadanie pjate:
```mysql
UPDATE postac SET id_statku = NULL where 1;
UPDATE marynarz SET id_statku = NULL where 1;
delete from postac where rodzaj = 'wiking' and nazwa != 'Bjorn' LIMIT 1;
delete from marynarz where rodzaj = 'wiking' and nazwa != 'Bjorn' LIMIT 1;
delete from statek where 1;
ALTER TABLE postac DROP FOREIGN KEY postac_ibfk_1;
ALTER TABLE marynarz DROP FOREIGN KEY marynarz_ibfk_1;
drop table statek;
create table zwierz(
    id INT NOT NULL AUTO_INCREMENT PRIMARY KEY,
    nazwa varchar(40),
    wiek int
);
insert into zwierz(nazwa, wiek) select nazwa,wiek from postac where rodzaj = 'ptak';

```
