# Data exploration in SQL

- Description: In this project, a database containing the sales, products and  customers of a fictitious company, created and analyzed with the aim of developing queries that help find information and make decisions.
- Skill/technology: SQL, exploratory data analysis
--- 

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
- Output 

<body>
<table border="1">
<tbody><tr>
<td bgcolor="silver" class="medium">Fecha_minima</td>
</tr>

<tr>
<td class="normal" valign="top">2021-06-01</td>
</tr>
</tbody></table>

</body>

###  7. Día con mayores ingresos

```
SELECT 
    date,
    SUM(amount) as Amount
FROM sales
GROUP BY date
ORDER BY amount DESC limit 1;
```
- Output 

<body>
<table border="1">
<tbody><tr>
<td bgcolor="silver" class="medium">date</td>
<td bgcolor="silver" class="medium">Amount</td>
</tr>

<tr>
<td class="normal" valign="top">2021-09-18</td>
<td class="normal" valign="top">43047.5</td>
</tr>
</tbody></table>

</body>


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
- Output

<body>
<table border="1">
<tbody><tr>
<td bgcolor="silver" class="medium">product_id</td>
<td bgcolor="silver" class="medium">units</td>
<td bgcolor="silver" class="medium">name</td>
</tr>

<tr>
<td class="normal" valign="top">16</td>
<td class="normal" valign="top">562</td>
<td class="normal" valign="top">ASUS Chromebook Z1400CN</td>
</tr>

<tr>
<td class="normal" valign="top">89</td>
<td class="normal" valign="top">440</td>
<td class="normal" valign="top">tostadora</td>
</tr>

<tr>
<td class="normal" valign="top">41</td>
<td class="normal" valign="top">392</td>
<td class="normal" valign="top">zapatillas mujer</td>
</tr>

<tr>
<td class="normal" valign="top">80</td>
<td class="normal" valign="top">385</td>
<td class="normal" valign="top">harry potter Puzzle 3D</td>
</tr>

<tr>
<td class="normal" valign="top">73</td>
<td class="normal" valign="top">379</td>
<td class="normal" valign="top">kindle</td>
</tr>

<tr>
<td class="normal" valign="top">95</td>
<td class="normal" valign="top">371</td>
<td class="normal" valign="top">mancuernas</td>
</tr>

<tr>
<td class="normal" valign="top">90</td>
<td class="normal" valign="top">364</td>
<td class="normal" valign="top">juegos nintendo switch Mario + Rabbids Kingdom Battle</td>
</tr>

<tr>
<td class="normal" valign="top">63</td>
<td class="normal" valign="top">362</td>
<td class="normal" valign="top">calefactor</td>
</tr>

<tr>
<td class="normal" valign="top">67</td>
<td class="normal" valign="top">360</td>
<td class="normal" valign="top">proyector</td>
</tr>

<tr>
<td class="normal" valign="top">27</td>
<td class="normal" valign="top">355</td>
<td class="normal" valign="top">aspiradora</td>
</tr>

<tr>
<td class="normal" valign="top">28</td>
<td class="normal" valign="top">354</td>
<td class="normal" valign="top">garmin</td>
</tr>

<tr>
<td class="normal" valign="top">25</td>
<td class="normal" valign="top">352</td>
<td class="normal" valign="top">silla gaming</td>
</tr>

<tr>
<td class="normal" valign="top">57</td>
<td class="normal" valign="top">348</td>
<td class="normal" valign="top">vans</td>
</tr>

<tr>
<td class="normal" valign="top">77</td>
<td class="normal" valign="top">347</td>
<td class="normal" valign="top">echo</td>
</tr>

<tr>
<td class="normal" valign="top">46</td>
<td class="normal" valign="top">347</td>
<td class="normal" valign="top">freidora sin aceite</td>
</tr>

<tr>
<td class="normal" valign="top">3</td>
<td class="normal" valign="top">343</td>
<td class="normal" valign="top">xiaomi</td>
</tr>

<tr>
<td class="normal" valign="top">43</td>
<td class="normal" valign="top">343</td>
<td class="normal" valign="top">xiaomi redmi note 9</td>
</tr>

<tr>
<td class="normal" valign="top">21</td>
<td class="normal" valign="top">343</td>
<td class="normal" valign="top">disco duro externo</td>
</tr>

<tr>
<td class="normal" valign="top">39</td>
<td class="normal" valign="top">342</td>
<td class="normal" valign="top">zapatero</td>
</tr>

<tr>
<td class="normal" valign="top">87</td>
<td class="normal" valign="top">335</td>
<td class="normal" valign="top">botines mujer</td>
</tr>

<tr>
<td class="normal" valign="top">44</td>
<td class="normal" valign="top">335</td>
<td class="normal" valign="top">humidificador</td>
</tr>

<tr>
<td class="normal" valign="top">50</td>
<td class="normal" valign="top">335</td>
<td class="normal" valign="top">termómetro infrarrojos</td>
</tr>

<tr>
<td class="normal" valign="top">76</td>
<td class="normal" valign="top">334</td>
<td class="normal" valign="top">secador pelo</td>
</tr>

<tr>
<td class="normal" valign="top">56</td>
<td class="normal" valign="top">333</td>
<td class="normal" valign="top">ssd</td>
</tr>

<tr>
<td class="normal" valign="top">54</td>
<td class="normal" valign="top">332</td>
<td class="normal" valign="top">sartenes</td>
</tr>

<tr>
<td class="normal" valign="top">32</td>
<td class="normal" valign="top">330</td>
<td class="normal" valign="top">zapatillas casa hombre</td>
</tr>

<tr>
<td class="normal" valign="top">26</td>
<td class="normal" valign="top">328</td>
<td class="normal" valign="top">ipad</td>
</tr>

<tr>
<td class="normal" valign="top">20</td>
<td class="normal" valign="top">326</td>
<td class="normal" valign="top">playmobil</td>
</tr>

<tr>
<td class="normal" valign="top">96</td>
<td class="normal" valign="top">324</td>
<td class="normal" valign="top">under armour hombre</td>
</tr>

<tr>
<td class="normal" valign="top">1</td>
<td class="normal" valign="top">324</td>
<td class="normal" valign="top">auriculares inalámbricos</td>
</tr>

<tr>
<td class="normal" valign="top">48</td>
<td class="normal" valign="top">323</td>
<td class="normal" valign="top">ratón inalámbrico</td>
</tr>

<tr>
<td class="normal" valign="top">65</td>
<td class="normal" valign="top">321</td>
<td class="normal" valign="top">vatenick Cámara para Niños</td>
</tr>

<tr>
<td class="normal" valign="top">15</td>
<td class="normal" valign="top">321</td>
<td class="normal" valign="top">altavoz bluetooth</td>
</tr>

<tr>
<td class="normal" valign="top">11</td>
<td class="normal" valign="top">321</td>
<td class="normal" valign="top">mascarillas ffp2</td>
</tr>

<tr>
<td class="normal" valign="top">8</td>
<td class="normal" valign="top">320</td>
<td class="normal" valign="top">auriculares</td>
</tr>

<tr>
<td class="normal" valign="top">31</td>
<td class="normal" valign="top">317</td>
<td class="normal" valign="top">echo dot</td>
</tr>

<tr>
<td class="normal" valign="top">5</td>
<td class="normal" valign="top">317</td>
<td class="normal" valign="top">mascarillas</td>
</tr>

<tr>
<td class="normal" valign="top">30</td>
<td class="normal" valign="top">316</td>
<td class="normal" valign="top">zapatillas hombre</td>
</tr>

<tr>
<td class="normal" valign="top">51</td>
<td class="normal" valign="top">316</td>
<td class="normal" valign="top">monitor</td>
</tr>

<tr>
<td class="normal" valign="top">58</td>
<td class="normal" valign="top">315</td>
<td class="normal" valign="top">Samsung Galaxy M21</td>
</tr>

<tr>
<td class="normal" valign="top">59</td>
<td class="normal" valign="top">314</td>
<td class="normal" valign="top">lekue</td>
</tr>

<tr>
<td class="normal" valign="top">29</td>
<td class="normal" valign="top">313</td>
<td class="normal" valign="top">sudaderas hombres</td>
</tr>

<tr>
<td class="normal" valign="top">66</td>
<td class="normal" valign="top">313</td>
<td class="normal" valign="top">bolsos mujer</td>
</tr>

<tr>
<td class="normal" valign="top">72</td>
<td class="normal" valign="top">312</td>
<td class="normal" valign="top">calefactor baño</td>
</tr>

<tr>
<td class="normal" valign="top">94</td>
<td class="normal" valign="top">312</td>
<td class="normal" valign="top">Hisense HD TV 2020 32AE5500F</td>
</tr>

<tr>
<td class="normal" valign="top">6</td>
<td class="normal" valign="top">311</td>
<td class="normal" valign="top">nintendo switch</td>
</tr>

<tr>
<td class="normal" valign="top">2</td>
<td class="normal" valign="top">311</td>
<td class="normal" valign="top">alexa</td>
</tr>

<tr>
<td class="normal" valign="top">38</td>
<td class="normal" valign="top">310</td>
<td class="normal" valign="top">microondas</td>
</tr>

<tr>
<td class="normal" valign="top">53</td>
<td class="normal" valign="top">310</td>
<td class="normal" valign="top">sudaderas mujer</td>
</tr>

<tr>
<td class="normal" valign="top">10</td>
<td class="normal" valign="top">310</td>
<td class="normal" valign="top">lego</td>
</tr>

<tr>
<td class="normal" valign="top">71</td>
<td class="normal" valign="top">309</td>
<td class="normal" valign="top">cinta de correr</td>
</tr>

<tr>
<td class="normal" valign="top">37</td>
<td class="normal" valign="top">307</td>
<td class="normal" valign="top">cascos inalámbricos</td>
</tr>

<tr>
<td class="normal" valign="top">61</td>
<td class="normal" valign="top">307</td>
<td class="normal" valign="top">relojes inteligentes hombre</td>
</tr>

<tr>
<td class="normal" valign="top">22</td>
<td class="normal" valign="top">307</td>
<td class="normal" valign="top">airpods</td>
</tr>

<tr>
<td class="normal" valign="top">34</td>
<td class="normal" valign="top">307</td>
<td class="normal" valign="top">fire tv stick</td>
</tr>

<tr>
<td class="normal" valign="top">13</td>
<td class="normal" valign="top">306</td>
<td class="normal" valign="top">silla escritorio</td>
</tr>

<tr>
<td class="normal" valign="top">79</td>
<td class="normal" valign="top">305</td>
<td class="normal" valign="top">webcam</td>
</tr>

<tr>
<td class="normal" valign="top">23</td>
<td class="normal" valign="top">304</td>
<td class="normal" valign="top">juegos de mesa</td>
</tr>

<tr>
<td class="normal" valign="top">92</td>
<td class="normal" valign="top">303</td>
<td class="normal" valign="top">plancha ropa</td>
</tr>

<tr>
<td class="normal" valign="top">14</td>
<td class="normal" valign="top">302</td>
<td class="normal" valign="top">smartwatch</td>
</tr>

<tr>
<td class="normal" valign="top">36</td>
<td class="normal" valign="top">300</td>
<td class="normal" valign="top">iphone 12</td>
</tr>

<tr>
<td class="normal" valign="top">12</td>
<td class="normal" valign="top">299</td>
<td class="normal" valign="top">cafetera</td>
</tr>

<tr>
<td class="normal" valign="top">75</td>
<td class="normal" valign="top">299</td>
<td class="normal" valign="top">aspiradora escoba sin cable</td>
</tr>

<tr>
<td class="normal" valign="top">82</td>
<td class="normal" valign="top">298</td>
<td class="normal" valign="top">tous bolso</td>
</tr>

<tr>
<td class="normal" valign="top">78</td>
<td class="normal" valign="top">292</td>
<td class="normal" valign="top">mochila</td>
</tr>

<tr>
<td class="normal" valign="top">17</td>
<td class="normal" valign="top">291</td>
<td class="normal" valign="top">zapatillas casa mujer</td>
</tr>

<tr>
<td class="normal" valign="top">88</td>
<td class="normal" valign="top">290</td>
<td class="normal" valign="top">pijamas mujer</td>
</tr>

<tr>
<td class="normal" valign="top">45</td>
<td class="normal" valign="top">289</td>
<td class="normal" valign="top">Tenis nike mujer</td>
</tr>

<tr>
<td class="normal" valign="top">60</td>
<td class="normal" valign="top">288</td>
<td class="normal" valign="top">Chamarra mujer</td>
</tr>

<tr>
<td class="normal" valign="top">7</td>
<td class="normal" valign="top">287</td>
<td class="normal" valign="top">iphone XR</td>
</tr>

<tr>
<td class="normal" valign="top">19</td>
<td class="normal" valign="top">287</td>
<td class="normal" valign="top">mascarillas quirúrgicas</td>
</tr>

<tr>
<td class="normal" valign="top">74</td>
<td class="normal" valign="top">285</td>
<td class="normal" valign="top">mascarilla infantil</td>
</tr>

<tr>
<td class="normal" valign="top">68</td>
<td class="normal" valign="top">284</td>
<td class="normal" valign="top">funda iphone 11</td>
</tr>

<tr>
<td class="normal" valign="top">9</td>
<td class="normal" valign="top">284</td>
<td class="normal" valign="top">iphone 11</td>
</tr>

<tr>
<td class="normal" valign="top">81</td>
<td class="normal" valign="top">283</td>
<td class="normal" valign="top">HUAWEI P Smart S</td>
</tr>

<tr>
<td class="normal" valign="top">49</td>
<td class="normal" valign="top">283</td>
<td class="normal" valign="top">robot aspirador</td>
</tr>

<tr>
<td class="normal" valign="top">69</td>
<td class="normal" valign="top">281</td>
<td class="normal" valign="top">ps5</td>
</tr>

<tr>
<td class="normal" valign="top">91</td>
<td class="normal" valign="top">278</td>
<td class="normal" valign="top">tablet 10 pulgadas baratas y buenas</td>
</tr>

<tr>
<td class="normal" valign="top">24</td>
<td class="normal" valign="top">274</td>
<td class="normal" valign="top">satisfyer</td>
</tr>

<tr>
<td class="normal" valign="top">47</td>
<td class="normal" valign="top">274</td>
<td class="normal" valign="top">batidora</td>
</tr>

<tr>
<td class="normal" valign="top">64</td>
<td class="normal" valign="top">273</td>
<td class="normal" valign="top">aspiradora de mano</td>
</tr>

<tr>
<td class="normal" valign="top">52</td>
<td class="normal" valign="top">273</td>
<td class="normal" valign="top">impresora</td>
</tr>

<tr>
<td class="normal" valign="top">85</td>
<td class="normal" valign="top">269</td>
<td class="normal" valign="top">converse Sneaker Unisex-Adult</td>
</tr>

<tr>
<td class="normal" valign="top">42</td>
<td class="normal" valign="top">264</td>
<td class="normal" valign="top">ps4</td>
</tr>

<tr>
<td class="normal" valign="top">40</td>
<td class="normal" valign="top">262</td>
<td class="normal" valign="top">patinete eléctrico</td>
</tr>

<tr>
<td class="normal" valign="top">70</td>
<td class="normal" valign="top">262</td>
<td class="normal" valign="top">tostadoras pan</td>
</tr>

<tr>
<td class="normal" valign="top">33</td>
<td class="normal" valign="top">259</td>
<td class="normal" valign="top">relojes inteligentes mujer</td>
</tr>

<tr>
<td class="normal" valign="top">18</td>
<td class="normal" valign="top">256</td>
<td class="normal" valign="top">auriculares bluetooth</td>
</tr>

<tr>
<td class="normal" valign="top">93</td>
<td class="normal" valign="top">246</td>
<td class="normal" valign="top">mando ps4</td>
</tr>

<tr>
<td class="normal" valign="top">4</td>
<td class="normal" valign="top">239</td>
<td class="normal" valign="top">tablet</td>
</tr>

<tr>
<td class="normal" valign="top">86</td>
<td class="normal" valign="top">238</td>
<td class="normal" valign="top">escritorio</td>
</tr>

<tr>
<td class="normal" valign="top">62</td>
<td class="normal" valign="top">229</td>
<td class="normal" valign="top">apple watch Series&nbsp;3 (GPS)</td>
</tr>

<tr>
<td class="normal" valign="top">35</td>
<td class="normal" valign="top">229</td>
<td class="normal" valign="top">batidora de mano</td>
</tr>

<tr>
<td class="normal" valign="top">83</td>
<td class="normal" valign="top">228</td>
<td class="normal" valign="top">chándal hombre completo</td>
</tr>
</tbody></table>

</body>

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

- Output

<body>
<table border="1">
<tbody><tr>
<td bgcolor="silver" class="medium">user_id</td>
<td bgcolor="silver" class="medium">first_name</td>
<td bgcolor="silver" class="medium">last_name</td>
<td bgcolor="silver" class="medium">gender</td>
<td bgcolor="silver" class="medium">zip_code</td>
<td bgcolor="silver" class="medium">address</td>
<td bgcolor="silver" class="medium">state</td>
<td bgcolor="silver" class="medium">city</td>
<td bgcolor="silver" class="medium">phone</td>
<td bgcolor="silver" class="medium">amount</td>
</tr>

<tr>
<td class="normal" valign="top">31064</td>
<td class="normal" valign="top">DANIEL PASCUAL</td>
<td class="normal" valign="top">JIMÉNEZ RANGEL</td>
<td class="normal" valign="top">M</td>
<td class="normal" valign="top">38797</td>
<td class="normal" valign="top">Cerrito de Bermejo 1292</td>
<td class="normal" valign="top">Guanajuato</td>
<td class="normal" valign="top">Tarandacuao</td>
<td class="normal" valign="top">4292152123</td>
<td class="normal" valign="top">7545</td>
</tr>
</tbody></table>

</body>

