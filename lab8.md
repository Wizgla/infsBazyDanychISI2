zadanie pierwsze:
```mysql
insert into infs_matuznyii.kreatura select * from wikingowie.kreatura;
create table infs_matuznyii.uczestnicy select * from wikingowie.uczestnicy;
create table infs_matuznyii.etapy_wyprawy select * from wikingowie.etapy_wyprawy;
create table infs_matuznyii.sektor select * from wikingowie.sektor;
create table infs_matuznyii.wyprawa select * from wikingowie.wyprawa;

select k.nazwa from kreatura k left join uczestnicy u on u.id_uczestnika=k.idkreatury where u.id_uczestnika is null;
SELECT w.nazwa AS nazwaWyprawy, SUM(e.ilosc) AS sumaIlosciEkwipunku FROM wyprawa w INNER JOIN uczestnicy u ON w.id_wyprawy = u.id_wyprawy INNER JOIN ekwipunek e ON w.id_wyprawy = e.idEkwipunku GROUP BY w.nazwa;
```
zadanie drugie:
```mysql
select w.nazwa as nazwaWyprawy, count(u.id_uczestnika) as liczbaUczestnikow, group_concat(k.nazwa) as nazwyUczestnikow from wyprawa w inner join uczestnicy u on w.id_wyprawy = u.id_wyprawy inner join kreatura k on u.id_uczestnika = k.idKreatury group by w.id_wyprawy,w.nazwa;

select w.nazwa as nazwaWyprawy,s.nazwa as nazwaSektoru, k.nazwa as kierownik from wyprawa w inner join etapy_wyprawy e on w.id_wyprawy = e.idWyprawy inner join kreatura k on w.kierownik = k.idKreatury inner join sektor s on e.sektor = s.id_sektora order by w.data_rozpoczecia asc, e.kolejnosc asc;
```
