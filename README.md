select * from invoice i
join invoice_line il ON il.invoice_id = i.invoice_id
where il.unit_price > 0.99;

select i.invoice_date, c.first_name, c.last_name, i.total
from invoice i
join customer c on i.customer_id = c.customer_id;

select c.first_name, c.last_name, e.first_name, e.last_name
from customer c
join employee e on c.support_rep_id = e.employee_id;

select al.title, ar.name
from album al
join artist ar on al.artist_id = ar.artist_id;

select pt.track_id
from playlist_track pt
join playlist p on p.playlist_id = pt.playlist_id
where p.name = 'Music'

select t.name
from track t
join playlist_track pt on pt.track_id = t.track_id
where pt.playlist_id = 5;

select t.name, p.name
from track t
join playlist_track pt on t.track_id = pt.track_id
join playlist p on pt.playlist_id = p.playlist_id;

select t.name, a.title
from track t 
join album a on t.album_id = a.album_id
join genre g on g.genre_id = t.genre_id
where g.name = 'Alternative & Punk';

**************************************************************

select *
from invoice 
where invoice_id in (select invoice_id from invoice_line where unit_price > 0.99);

select *
from playlist_track
where playlist_id in (select playlist_id from playlist where name = 'Music');

select name
from track
where track_id in (select track_id from playlist_track where playlist_id = 5)

select * from track
where genre_id in (select genre_id from genre where name = 'Comedy');

select * from track
where album_id in (select album_id from album where title = 'Fireball');

select * from track
where album_id in (
  select album_id from album where artist_id in(
    select artist_id from artist where name = 'Queen'
   )
  );

**********************************************

  update customer
set fax = null
where fax is not null;

update customer
set company = 'Self'
where company = null;

update customer 
set last_name = 'Thompson'
where first_name = 'Julia' and last_name = 'Barnett';

update customer 
set support_rep_id = 4
where email = 'luisrojas@yahoo.cl';

update track
set composer = 'the darkness around us'
where genre_id = (select genre_id from genre where name = 'metal')
and composer is null;

**********************************************

select count(*), g.name
from track t
join genre g on t.genre_id = g.genre_id
group by g.name;

select count(*), g.name
from track t
join genre g on g.genre_id = t.genre_id
where g.name = 'Pop' or g.name = 'Rock'
group by g.name;

select ar.name, count(*)
from album al
join artist ar on ar.artist_id = al.artist_id
group by ar.name;

************************************************

select distinct composer
from track;

select distinct billing_postal_code
from invoice;

select distinct company
from customer;

***********************************************

delete from practice_delete
where type = 'bronze';

delete from practice_delete 
where type = 'silver';

delete from practice_delete
where value = 150;

**********************************************

create table sql_users(
 id serial primary key,
 name varchar(100),
 email varchar(100)
);

insert into sql_users
(name, email)
values 
('Scott', 'scott@gmail.com'),
('Tim', 'tim@gmail.com'),
('Pete', 'pete@gmail.com');

create table sql_product(
 product_id serial primary key,
 product_name varchar(100),
 price integer
 
 );

 insert into sql_product
 (product_name, price)
 values
 ('glue', 1.50),
 ('paint', 2.99),
 ('bread', 3.05);

 create table sql_orders (
  order_id serial primary key,
  order_number integer, 
  user_id integer references sql_users(id),
  product_id integer references sql_product(product_id)
);

insert into sql_orders
 (order_number, user_id, product_id)
values
 (1, 1, 1),
 (1, 1, 2),
 (2, 2, 3),
 (3, 3, 2),
 (3, 3, 2),
 (5, 1, 2),
 (5, 1, 1),
 (5, 1, 3),
 (4, 3, 1);

select p.product_name from sql_orders o
join sql_product p on o.product_id = p.product_id
where o.order_number = 1;

select * from sql_orders;

select sum(p.price) from sql_orders o
join sql_product p on p.product_id = o.product_id
where order_number = 1;

select order_number from sql_orders
where user_id = 3;

select distinct (order_number) from sql_orders
where user_id > 0;