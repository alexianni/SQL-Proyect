# Data exploration in SQL

### - Creación de tablas

```
CREATE TABLE USERS (
user_id int auto_increment primary key,
first_name varchar(50),
last_name varchar(50),
gender varchar(2),
zip_code varchar(5) not null,
address varchar(200) not null,
state varchar(50),
city varchar(50),
phone varchar(10) unique
);
```
```
CREATE TABLE PRODUCTS (
product_id int auto_increment primary key,
name varchar(100) not null,
description varchar(300),
type varchar(20) not null,
price_usd float not null
);
```
```
CREATE TABLE SALES (
order_id int auto_increment primary key,
date date,
units int, 
amount float,
user_id_fk int,
product_id_fk int,
constraint user_fk foreign key (user_id_fk) references USERs(user_id) ON DELETE CASCADE,
constraint product_fk foreign key (product_id_fk) references PRODUCTS(product_id) ON DELETE CASCADE
);
```
### - Visualización de tablas
```
SHOW TABLES IN sales_db;
```
- Output
<table border="1">
<tbody><tr>
<td bgcolor="silver" class="medium">Tables_in_sales_db</td>
</tr>

<tr>
<td class="normal" valign="top">products</td>
</tr>

<tr>
<td class="normal" valign="top">sales</td>
</tr>

<tr>
<td class="normal" valign="top">users</td>
</tr>
</tbody></table>

### - Se agregan datos

- Products 
```
LOAD DATA INFILE 'products.txt' 
 INTO TABLE products;
```
- Users 
```
SELECT 
    *
FROM
    Users;

insert into Users
(user_id,
gender,
zip_code,
address,
state,
city,
phone,
first_name,
last_name)
```

```
LOAD DATA INFILE 'users.txt' 
 INTO TABLE users;
```

- Sales

```
select * from Sales;

insert into Sales (
order_id,
date,
units,
amount,
product_id_fk,
user_id_fk)

```
```
LOAD DATA INFILE 'sales.txt' 
 INTO TABLE sales;
```


### 1. Cantidad de productos del catalogo

```
select * from products; 

select 
count(name) as Cantidad_de_productos
from products;
```
- Output

<table border="1">
<tbody><tr>
<td bgcolor="silver" class="medium">Cantidad_de_productos</td>
</tr>

<tr>
<td class="normal" valign="top">95</td>
</tr>
</tbody></table>

</body></html>

###  2. Cantidad de artículos del catálogo por tipo de productos ordenándolos de  la mayor cifra a la menor
```

select 
count(distinct name) as Cantidad_de_productos,
type as Tipo_de_producto
from products
group by(type)
order by count(distinct name) desc;
```

- Output

<body>
<table border="1">
<tbody><tr>
<td bgcolor="silver" class="medium">Cantidad_de_productos</td>
<td bgcolor="silver" class="medium">Tipo_de_producto</td>
</tr>

<tr>
<td class="normal" valign="top">30</td>
<td class="normal" valign="top">Tech</td>
</tr>

<tr>
<td class="normal" valign="top">20</td>
<td class="normal" valign="top">Home</td>
</tr>

<tr>
<td class="normal" valign="top">17</td>
<td class="normal" valign="top">Clothes</td>
</tr>

<tr>
<td class="normal" valign="top">7</td>
<td class="normal" valign="top">Healt</td>
</tr>

<tr>
<td class="normal" valign="top">7</td>
<td class="normal" valign="top">Phones</td>
</tr>

<tr>
<td class="normal" valign="top">5</td>
<td class="normal" valign="top">Games</td>
</tr>

<tr>
<td class="normal" valign="top">5</td>
<td class="normal" valign="top">Videogames</td>
</tr>

<tr>
<td class="normal" valign="top">2</td>
<td class="normal" valign="top">Accessories</td>
</tr>

<tr>
<td class="normal" valign="top">1</td>
<td class="normal" valign="top">Sex Shop</td>
</tr>
</tbody></table>

</body>

### 3. Product id, product name y el price del artículo más caro y del  más barato  

```

select
price_usd as Precio,
product_id as id,
name as Nombre_producto
from products
order by (price_usd) limit 1;
```
- Output

<body>
<table border="1">
<tbody><tr>
<td bgcolor="silver" class="medium">Precio</td>
<td bgcolor="silver" class="medium">id</td>
<td bgcolor="silver" class="medium">Nombre_producto</td>
</tr>

<tr>
<td class="normal" valign="top">6.75</td>
<td class="normal" valign="top">74</td>
<td class="normal" valign="top">mascarilla infantil</td>
</tr>
</tbody></table>
 
 ```
select
price_usd as Precio,
product_id as id,
name as Nombre_producto
from products
order by (price_usd) desc limit 1;
```
 - Output

</body>

<body>
<table border="1">
<tbody><tr>
<td bgcolor="silver" class="medium">Precio</td>
<td bgcolor="silver" class="medium">id</td>
<td bgcolor="silver" class="medium">Nombre_producto</td>
</tr>

<tr>
<td class="normal" valign="top">787.5</td>
<td class="normal" valign="top">36</td>
<td class="normal" valign="top">iphone 12</td>
</tr>
</tbody></table>

</body>

### 4. Mayor registros dentro de la plataforma, por genero

```
select *from users;

select 
count(user_id) as Cantidad,
gender as Genero
from users
group by gender;
```

- Output

<body>
<table border="1">
<tbody><tr>
<td bgcolor="silver" class="medium">Cantidad</td>
<td bgcolor="silver" class="medium">Genero</td>
</tr>

<tr>
<td class="normal" valign="top">1924</td>
<td class="normal" valign="top">M</td>
</tr>

<tr>
<td class="normal" valign="top">1576</td>
<td class="normal" valign="top">F</td>
</tr>
</tbody></table>

</body>

### 5. El top 5 de estados es donde hay menos usuarios registrados

```
select 
state,
count(user_id) Cantidad_de_usuarios
from users
group by state
order by user_id  asc limit 5;
```

- Output 

<body>
<table border="1">
<tbody><tr>
<td bgcolor="silver" class="medium">state</td>
<td bgcolor="silver" class="medium">Cantidad_de_usuarios</td>
</tr>

<tr>
<td class="normal" valign="top">Michoacán de Ocampo</td>
<td class="normal" valign="top">237</td>
</tr>

<tr>
<td class="normal" valign="top">Chiapas</td>
<td class="normal" valign="top">180</td>
</tr>

<tr>
<td class="normal" valign="top">Morelos</td>
<td class="normal" valign="top">32</td>
</tr>

<tr>
<td class="normal" valign="top">Chihuahua</td>
<td class="normal" valign="top">216</td>
</tr>

<tr>
<td class="normal" valign="top">Jalisco</td>
<td class="normal" valign="top">145</td>
</tr>
</tbody></table>

</body>

### 6. Fecha mínima de la tabla SALES 
```
select * from sales;

select
min(date) as Fecha_minima
from sales;
```
###  7. Día con mayores ingresos

```
SELECT 
    date,
    SUM(amount) as Amount
FROM sales
GROUP BY date
ORDER BY amount DESC limit 1;
```

### 8. Listado con todos los nombres de los productos empezando por el que más  unidades ha vendido. 

```
select 
product_id,
sum(units) units,
name
from products P
left JOIN SALES S ON P.product_id = S.product_id_fk 
group by(p.name)
order by(units) desc;
```

### 9. Datos del usuario con mayor amount generado.

```
select 
user_id, 
first_name,
last_name,
gender,
zip_code,
address,
state,
city,
phone,
sum(amount) amount
 from users U 
 left join sales S on U.user_id = S. user_id_fk
 group by (user_id)
 order by(amount) desc limit 1; 
 ```


