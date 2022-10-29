# Data exploration on SQL

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

