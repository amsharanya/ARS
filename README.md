DATABASE CREATION
 create database if not exists ars;
Query OK, 1 row affected (0.147 sec)

USING DATABASE
 use  ars;
Database changed

PASSENGER TABLE CREATION
 create table passenger ( passenger_id int primary key, firstname varchar(50),lastname varchar(50), email varchar(100), passportnumber varchar(20));
Query OK, 0 rows affected (0.355 sec)

AIRPORT TABLE CREATION
create table airport(airport_code varchar(3) primary key, airport_name varchar(100), location varchar(255),facilities varchar(255));
Query OK, 0 rows affected (0.201 sec)

AIRLINE TABLE CREATION
 create table airline(airline_id int primary key,airline_name varchar(100), contact_number varchar(20), operation_region varchar(100));
Query OK, 0 rows affected (0.194 sec)

FLIGHT TABLE CREATION
 create table flight(flight_id int primary key, flight_number varchar(20) unique, departure_datetime datetime, arrival_datetime datetime, origin_aircode varchar(3), destin_aircode varchar(3), available_seats int, foreign key(origin_aircode) references airport(airport_code), foreign key (destin_aircode) references airport(airport_code));
Query OK, 0 rows affected (0.679 sec)

BOOKING TABLE CREATION
 create table booking(booking_id int primary key, flight_id int, passenger_id int, payment_status varchar(20), foreign key(flight_id) references flight(flight_id), foreign key(passenger_id) references passenger(passenger_id));
Query OK, 0 rows affected (0.579 sec)

PAYMENT TABLE CREATION
 create table payment(payment_id int primary key, booking_id int unique,payment_method varchar(50),amount decimal(10,2),transaction_datetime datetime, foreign key(booking_id) references booking(booking_id));
Query OK, 0 rows affected (0.374 sec)
