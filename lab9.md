Zadanie pierwsze:
```mysql
DELIMITER //
create trigger kreatura_before_insert
before insert on kreatura
for each row
BEGIN
	IF new.waga <= 0
	then
	 set new.waga= 1;
	END IF;
END
//
```
Zadanie drugie:
```mysql
create table archiwum_wypraw(
	id_wyprawy int primary key auto_increment,
    nazwa varchar(30),
    data_rozpoczecia date,
    data_zakonczenia date,
    kierownik varchar(30)
)

delimiter //
create trigger archiwum_wypraw_after_delete 
after delete on wyprawa 
for each row
begin
	insert into archiwum_wypraw select w.id_wyprawy, w.nazwa, w.data_rozpoczecia, w.data_zakonczenia, k.nazwa from wyprawa w inner join kreatura k on w.kierownik = k.idKreatury
    where id_wyprawy = old.id_wyprawy;

end
//
```
Zadanie trzecie:
```mysql
delimiter $$
create procedure eliksir_sily(in id int)
begin
update kreatura set udzwig=udzwig*1.2 where idkreatury=id;
end
$$

delimiter //
create function podnies3(x varchar(30))
	returns varchar(30)
	
	begin 
	declare y varchar(30);
   
    set y = upper(x);
    return y;
   
end //

```
Zadanie czwarte:
```mysql
create table system_alarmowy(
id_alarmu int primary key auto_increment,
wiadomosc varchar(60)
)


delimiter ///
create trigger TesciowaNadchodziTrigger
on wyprawa
after insert
as 
begin
	if
    select k.nazwa, w.kierownik, s.nazwa from wyprawa w
    inner join kreatura k on w.kierownik=k.idkreatury
    inner join sektor s on 
```
