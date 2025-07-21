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

AIRPORT TABLE
 select * from airport;
+--------------+-------------------------+-------------+-----------------+
| airport_code | airport_name            | location    | facilities      |
+--------------+-------------------------+-------------+-----------------+
| AMD          | Sardar vallabh bhai IA  | Ahemmadabad | parking         |
| BLR          | Kempegowda IA           | Bangalore   | Parking,lounges |
| BOM          | Chathrapathi shivaji IA | Mumbai      | parking,lounges |
| COK          | Cochin IA               | Cochi       | parking         |
| DEL          | Indira Gandhi IA        | New Delhi   | parking,lounges |
| HYD          | Rajiv Gandhi IA         | Hyderabad   | Parking,lounges |
| MAA          | Chennai IA              | Chennai     | parking         |
| TRV          | Thiruvananthapuram IA   | Trivandrum  | lounges         |
+--------------+-------------------------+-------------+-----------------+

AIRLINE TABLE
 select * from airline;
+------------+-------------------+----------------+------------------+
| airline_id | airline_name      | contact_number | operation_region |
+------------+-------------------+----------------+------------------+
|          1 | Air India         | 2398760910     | New Delhi        |
|          2 | Indigo            | 5637098765     | Mumbai           |
|          3 | Vistara           | 9807980676     | New Delhi        |
|          4 | Spicejet          | 8769087632     | Hyderabad        |
|          5 | AkasaAir          | 2341234326     | Kolkatta         |
|          6 | Alliance Air      | 4500887721     | Bangalore        |
|          7 | Air india express | 7465763219     | Mumbai           |
|          8 | Emirates          | 9874356676     | Hyderabad        |
+------------+-------------------+----------------+------------------+

PASSENGER TABLE
select * from passenger;
+--------------+-------------+----------+-------------+----------------+
| passenger_id | firstname   | lastname | email       | passportnumber |
+--------------+-------------+----------+-------------+----------------+
|            1 | Pranav      | Krishna  | pranav23@23 | QA12312        |
|            2 | Mithali     | Lokesh   | mithu@123   | CV89021        |
|            3 | Akshatha    | Dev      | aksha@90    | LK23098        |
|            4 | Mohan       | Raj      | mohu@67     | GH97654        |
|            5 | Pratheeksha | Naresh   | pra@567     | OL78676        |
|            6 | Bhavanth    | Rag      | bhav@34     | MK66686        |
+--------------+-------------+----------+-------------+----------------+

FLIGHT TABLE
 select * from flight;
+-----------+---------------+---------------------+---------------------+----------------+----------------+-----------------+
| flight_id | flight_number | departure_datetime  | arrival_datetime    | origin_aircode | destin_aircode | available_seats |
+-----------+---------------+---------------------+---------------------+----------------+----------------+-----------------+
|         1 | DEL23         | 2025-07-14 18:54:19 | 2025-07-14 23:45:54 | DEL            | COK            |             250 |
|         2 | AIR21         | NULL                | NULL                | AMD            | BLR            |             150 |
|         3 | SPI24         | NULL                | NULL                | BOM            | COK            |             200 |
|         4 | EMI90         | NULL                | NULL                | HYD            | TRV            |             170 |
|         5 | MAL87         | NULL                | NULL                | TRV            | DEL            |             200 |
+-----------+---------------+---------------------+---------------------+----------------+----------------+-----------------+

BOOKING TABLE
 select * from booking;
+------------+-----------+--------------+----------------+
| booking_id | flight_id | passenger_id | payment_status |
+------------+-----------+--------------+----------------+
|          1 |         1 |            1 | paid           |
|          2 |         1 |            2 | paid           |
|          3 |         2 |            3 | paid           |
|          4 |         2 |            4 | unpaid         |
|          5 |         3 |            5 | paid           |
+------------+-----------+--------------+----------------+

PAYMENT TABLE
 select * from payment;
+------------+------------+----------------+---------+----------------------+
| payment_id | booking_id | payment_method | amount  | transaction_datetime |
+------------+------------+----------------+---------+----------------------+
|          1 |          1 | Gpay           | 1300.00 | 2025-07-14 18:58:28  |
|          2 |          2 | Phonepe        | 1450.00 | 2025-07-14 18:56:40  |
|          3 |          3 | Gpay           | 1000.00 | 2025-05-23 10:34:12  |
|          4 |          4 | Offline        | 2300.00 | 2025-06-13 09:12:45  |
|          5 |          5 | Offline        | 1800.00 | 2025-06-04 14:23:34  |
+------------+------------+----------------+---------+----------------------+
TRIGGER FOR BOOKING AND CANCELLATION

CREATE DEFINER=`root`@`localhost` TRIGGER `booking_AFTER_INSERT` AFTER INSERT ON `booking` FOR EACH ROW BEGIN
    IF new.payment_status='paid' then
            insert into booking_status(booking_id,booking_status) values(new.booking_id,"booked");
        ELSE
            insert into booking_status(booking_id,booking_status) values(new.booking_id,"cancelled");
         END IF;
END
> select * from booking_status;
+------------+----------------+
| booking_id | booking_status |
+------------+----------------+
|          7 | booked         |
+------------+----------------+
select * from booking_status;
+------------+----------------+
| booking_id | booking_status |
+------------+----------------+
|          7 | booked         |
|          8 | cancelled      |
+------------+----------------+

FLIGHT SEARCH

 create view flight_available as select *from flight where destin_aircode='COK';
  select * from flight_available;
+-----------+---------------+---------------------+---------------------+----------------+----------------+-----------------+
| flight_id | flight_number | departure_datetime  | arrival_datetime    | origin_aircode | destin_aircode | available_seats |
+-----------+---------------+---------------------+---------------------+----------------+----------------+-----------------+
|         1 | DEL23         | 2025-07-14 18:54:19 | 2025-07-14 23:45:54 | DEL            | COK            |             250 |
|         3 | SPI24         | 2025-07-18 12:55:22 | 2025-07-18 14:10:34 | BOM            | COK            |             200 |
+-----------+---------------+---------------------+---------------------+----------------+----------------+-----------------+

create view flight_search as select * from flight where origin_aircode='DEL';
select * from flight_search;
+-----------+---------------+---------------------+---------------------+----------------+----------------+-----------------+
| flight_id | flight_number | departure_datetime  | arrival_datetime    | origin_aircode | destin_aircode | available_seats |
+-----------+---------------+---------------------+---------------------+----------------+----------------+-----------------+
|         1 | DEL23         | 2025-07-14 18:54:19 | 2025-07-14 23:45:54 | DEL            | COK            |             250 |
|         2 | AIR21         | 2025-07-21 18:54:19 | 2025-07-21 23:30:02 | DEL            | BLR            |             150 |
+-----------+---------------+---------------------+---------------------+----------------+----------------+-----------------+

