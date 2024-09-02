# Database Project for Visionary Care

The scope of this project is to use all the SQL knowledge gained throught the Software Testing course and apply them in practice.

Application under test: **Visionary Care**

Tools used: **MySQL Workbench**

## Database description 
Due to the growing need for ophthalmic data standardization I decided to create a database for an ophthalmology clinic.
The design of the database should capture all the relevant aspects of the clinic’s operations. This includes patient management, intervention management, medical records, medication inventory, and detailed records of diagnoses, treatments, and prescriptions. This helps in tracking patient progress over time and making informed medical decisions.

## Database Structure
You can find below the database schema that was generated through Reverse Engineer and which contains all the tables and the relationships between them.
![DesignDataBases](https://github.com/Andrei-Bursuc/Database-Project/blob/main/Reverse%20Engineer.png)

**The tables are connected in the following way:**

`Saloane`  is connected with `Interventii` through a 1:n relationship which was implemented through saloane.id_salon  as a primary key and interventii.id_salon as a foreign key.
  
`Pacienti`  is connected with `Interventii` through a 1:n relationship which was implemented through pacienti.id_pacient as a primary key and interventii.id_pacient as a foreign key.
  
`Pacienti`  is connected with `Retete` through a 1:n relationship which was implemented through pacienti.id_pacient as a primary key and retete.id_reteta as a foreign key.
  
`Medici`  is connected with `Interventii` through a 1:n relationship which was implemented through medici.id_medic as a primary key and interventii.id_medic as a foreign key.
 
`Medici`  is connected with `Retete` through a 1:n relationship which was implemented through medici.id_medic as a primary key and retete.id_medic as a foreign key.
  
`Retete`  is connected with `Registru_Retete` through a 1:n relationship which was implemented through retete.id_reteta as a primary key and registru_retete.id_retea as a foreign key.
  
`Medicamente`  is connected with `Registru_Retete` through a 1:n relationship which was implemented through medicamente.id_medicament as a primary key and registru_retete.id_medicament as a foreign key.

 Because the tables `Retete` and `Medicamente` are connected by a n:n relationship, the table `Registru_Retete` is used as a cross table in order to break this realationship and keep the database in a normalized state.

## Database Queries

### DDL (Data Definition Language)
The following instructions were written in the scope of CREATING the structure of the database (**CREATE** INSTRUCTIONS).
  
```sql
create database clinica_oftalmologica;
use clinica_oftalmologica;

create table saloane
(
id_salon int primary key auto_increment,
etaj int not null, 
capacitate int not null
); 

create table pacienti
(
id_pacient int primary key auto_increment ,
nume varchar(30)  not null,
prenume varchar(30) not null,
data_nasterii date not null,
telefon varchar(20) unique
);

create table medici
(
id_medic int primary key auto_increment,
nume varchar(30) not null,
prenume varchar(30) not null,
data_angajare date not null,
salar_anual decimal(9,4) not null,
specializare varchar(40) not null,
telefon varchar(20) unique
);

create table interventii
(
id_interventie int primary key auto_increment,
procedura varchar(50) not null,
data_interventie date not null,
id_salon int,
foreign key (id_salon) references saloane(id_salon),
id_pacient int,
foreign key (id_pacient) references pacienti(id_pacient),
id_medic int,
foreign key (id_medic) references medici(id_medic)
);

create table retete
(
id_reteta int primary key auto_increment,
data_eliberare date not null,
id_pacient int,
foreign key (id_pacient) references pacienti(id_pacient),
id_medic int,
foreign key (id_medic) references medici(id_medic)
);

create table medicamente
( 
id_medicament int primary key auto_increment,
denumire varchar(30) not null,
pret decimal (4,2) not null,
descriere varchar(100)
);

create table registru_retete
(
id_registru int primary key auto_increment,
cantitate int not null,
administrare varchar(40) not null,
id_reteta int,
foreign key(id_reteta) references retete(id_reteta),
id_medicament int,
foreign key(id_medicament) references medicamente(id_medicament)
);
```

After the database and the tables have been created, a few **ALTER** instructions were written in order to update the structure of the database, as described below:
```sql
alter table pacienti add column email varchar(30);
alter table interventii modify procedura varchar(80);
```
  
### DML (Data Manipulation Language)

In order to be able to use the database I populated the tables with various data necessary in order to perform queries and manipulate the data. 
In the testing process, this necessary data is identified in the **Test Design** phase and created in the **Test Implementation** phase. 

Below you can find all the **INSERT** instructions that were created in the scope of this project:

```sql
insert into pacienti(nume, prenume, data_nasterii, telefon, email) 
values
('Russindilar' , 'Raluca', '1993-03-31', '0724946413', 'raluca.bursuc@gmail.com'),
('Draghiceanu ' , 'Alin' , '1987-10-17' , '+0510725114678' , 'alyn_17dr@gmail.com'),
('Tonu' , 'Marius' , '1990-02-26' , '0762336541' , 'tonu_m90@yahoo.com' ),
('Grigoriu' , 'Alice' , '1993-11-26' , '0788910222' , 'aliss_gri@ggmail.com'),
('Sava' , 'Catalin-Ionut' , '2000-12-22' , '0722478989' , 'catalinI_sava@gmail.com'),
('Sotropa' , 'Diana' , '1970-01-18' , '0761333667' , 'sotropaDiana@yahoo.com');

#The data from the table can be interrogated by using the following script:
select* from pacienti;

insert into medici(nume, prenume, data_angajare, salar_anual, specializare, telefon)
values
('Preda', 'Ancuta' , '2010-07-11' , 98000 , 'Cataracta adultului' , '0722312447'),
('Oprea', 'Bianca' , '2015-01-11' , 92000 , 'Cataracta congenitala' , '0744223787'),
('Troi', 'Raluca' , '2013-04-15' , 94020.80 , 'Chirurgie refractiva grad I' , '0763558478'),
('Lisca', 'Monica' , '2019-11-20' , 85070, 'Oftalmologie pediatrica' , '0755365174'),
('Strelet', 'Mihai' , '2022-05-13' , 83600.30, 'Chirurgie refractiva grad II' , '0733121657');

insert into medici(nume, prenume, data_angajare, salar_anual, specializare, telefon)
values
('Apavaloaie', 'Cornel' , '2011-08-12' , 95000 , 'Cataracta congenitala' , '0724567543'),
('Grosu', 'Adrian' , '2017-02-23' , 99000 , 'Oftalmologie pediatrica' , '0742987565');
 
#The data from the table can be interrogated by using the following script:
select * from medici;

insert into saloane(etaj, capacitate)
values
(1 , 10),
(2 , 10),
(3 , 8),
(4 , 6);
        
#The data from the table can be interrogated by using the following script:
select * from saloane;
   
insert into interventii(procedura, data_interventie, id_salon, id_pacient, id_medic)
values
('Inlocuirea cristalinului artificial' , '2022-03-19', 1 , 1 , 1),
('Indepartarea cataractei juvenile' , '2022-04-16' , 3 , 2 , 2),
('Extragerea intracapsulara a cristalinului' , '2022-09-22' , 1 , 3 , 1),
('Excizia canalelor lacrimale', '2023-01-12', 2 , 4 , 4),
('Alte proceduri pe muschii sau tendoanele extraoculare' , '2023-03-17' , 4 , 5, 3),
('Transplant muscular pentru strabism' , '2024-02-09', 2, 6 , 5); 
        
insert into interventii(procedura, data_interventie, id_salon, id_pacient, id_medic)
values
('Corectia glaucomului congenital', '2024-10-25', 3, 2, null);
 
#The data from the table can be interrogated by using the following script:
select * from interventii;
        
insert into medicamente(denumire, pret, descriere)
values
('Nostamine' , 25.80 , 'Destinat atat utilizarii nazale cat si intraoculare'),
('Ialuvit' , 33.70 , 'Se distribuie uniform pe suprafata oculara formand un bandaj de protectie visco-elastic'),
('Tarosin'  , 45.80 , 'Destinat hemoragiilor retiniene'),
('Adrusen Mega' , 67.20 , 'Pentru preventia si tratamentul degenerescentei maculare legate varsta.'),
('Tobradex' , 56.8 , 'Pentru tratamentul inflamatiei si a unei posibile infectii oculare'); 

#The data from the table can be interrogated by using the following script:
select * from medicamente;
    
insert into retete( data_eliberare, id_pacient, id_medic)
values
('2022-03-20' , 1 , 1),
('2022-04-17' , 2 , 2),
('2022-09-23' , 3 , 1),
('2023-01-14' , 4 , 4),
('2023-03-17' , 5 , 3),
( '2024-02-10', 6 , 5);

#The data from the table can be interrogated by using the following script:
select * from retete;
  
insert into registru_retete(cantitate, administrare, id_reteta, id_medicament)
values
(30 , '5 zile-de 3 ori pe zi', 1, 2),
(20 , '3 zile-de 2 ori pe zi', 2, 3),
(100 , '10 zile-de 4 ori pe zi' , 3 , 5),
(60 , '7 zile-de 2 pe zi', 4, 1),
(3 , '3 zile-una pe zi', 5 , 4);
 
#The data from the table can be interrogated by using the following script:
select * from registru_retete; 
```

After the insert, in order to prepare the data to be better suited for the testing process, I **updated** some data in the following way:

```sql
#The expansion of specialization to include 'Strabism' is requested for Dr. Preda Ancuța:
update medici set specializare = 'Cataracta adultului/Strabism' where nume = 'Preda' and prenume = 'Ancuta';
 
#It is requested to increase by 5% the annual salary of doctors who have the specialization in 'Chirurgie refractiva grad II':
update medici set salar_anual = salar_anual * 1.05 where specializare = 'Chirurgie refractiva grad II';
```
 
After the testing process, I **deleted** the data that was no longer relevant in order to preserve the database clean: 

```sql
#The request would be to delete all records of interventions that occurred on a specific date:
delete from interventii where data_interventie = '2023-03-17';

#The deletion of the prescription that was issued for the patient with identifier 6 is requested:
delete from retete where id_pacient = 6;
```

### DQL (Data Query Language)
In order to simulate various scenarios that might happen in real life I created the following **queries** that would cover multiple potential real-life situations:

**WHERE**
```sql
#Display all the data of doctors employed starting from the year 2015:
select * from medici where year(data_angajare) >= 2015;
 
#Display the name and description of medications that have a price between 30 and 60 lei:
select denumire, descriere from medicamente **where** pret between 30 and 60;
```

**AND**
```sql
#Retrieve all the interventions made by Dr. Monica Lisca between January 22 2012, and June 30 2024:
select intrv.id_interventie, intrv.procedura, intrv.data_interventie, med.nume, med.prenume
from interventii intrv inner join medici med on intrv.id_medic = med.id_medic
where med.nume = 'Lisca' **and** med.prenume = 'Monica'
and intrv.data_interventie between '2022-01-12' **and** '2024-06-30';
```

**OR**
```sql
#Display all patients who were born before 1990 or after 2000:
select * from pacienti where year(data_nasterii) <= 1990 or year(data_nasterii) >= 2000;
``` 

**LIKE**
```sql
#Display the last name, first name, and date of birth for patients whose first name starts with the letter A:
select nume, prenume, data_nasterii from pacienti where upper(prenume) like 'A%';
```
 
**INNER JOIN**
```sql
#Display the name of the intervention, the name of the patient, and the name of the doctor who performed the intervention for each procedure in the clinic:
select intrv.procedura, pac.nume as nume_pacient, pac.prenume as prenume_pacient, med.nume as nume_medic, med.prenume as prenume_medic
from interventii intrv 
inner join pacienti pac on intrv.id_pacient =  pac.id_pacient
inner join medici med on intrv.id_medic =  med.id_medic;

#Display the prescribed medication names and the issue date for each prescription:
select ret.id_reteta, ret.data_eliberare, med.denumire from registru_retete reg 
inner join retete ret on ret.id_reteta = reg.id_reteta
inner join medicamente med on med.id_medicament = reg.id_medicament;
```

**LEFT JOIN**
```sql
#Display the names of the patients and the name of the intervention they underwent, if applicable, for both patients who have undergone interventions and those who have not:
select pac.nume, pac.prenume, intrv.procedura from pacienti pac left join interventii intrv on intrv.id_pacient = pac.id_pacient;
```

**RIGHT JOIN**
```sql
#Display the names of the doctors and the names of the interventions they performed, if applicable, for both doctors who have performed interventions and those who have not:
select med.nume, med.prenume, intrv.procedura from interventii intrv right join medici med on med.id_medic = intrv.id_medic;
```

**CROSS JOIN**
```sql
#Display all possible types of interventions that can be performed by each doctor:
select med.nume, med.prenume, intrv.procedura from medici med cross join interventii intrv;
```

**FUNCTII AGREGATE**
```sql
#Display the total anual salary, the min salary, the max salary, the average salary of the doctors working in the clinic and also the total number of doctors:
select sum(truncate(salar_anual, 2)) as salar_anual_total, truncate(max(salar_anual), 2) as salar_maxim,  truncate (min(salar_anual), 2) as salar_minim, truncate(avg(salar_anual), 2) as salar_mediu, count(id_medic) numar_medici from medici;
```

**GROUP BY**
```sql
#Display the number of doctors in each specialization if it exceeds one doctor per specialization:
select specializare, count(id_medic) from medici group by specializare having count(id_medic) > 1;
```

**LIMIT**
```sql
Display the doctors starting from 2, 3 and 4th positions, which have the annual salary greater or equal to 90000 and order them descending by salary:
select * from medici where salar_anual >= 90000 order by salar_anual desc limit 3 offset 1;
```

**SUBQUERIES**
```sql
#Select all doctors who have the same specialization as Dr. Apăvăloaie Cornel, except for him:
select * from medici where specializare = (select specializare from medici where nume = 'Apavaloaie' and prenume = 'Cornel') and nume!='Apavaloaie' and prenume != 'Cornel';
```

## Conclusions
Creating and managing a database for an ophthalmology clinic provides valuable insights and lessons that can improve future projects. Detailed requirements gathering is crucial. Involve stakeholders (doctors, nurses, administrative staff) early to understand their needs and expectations. Anticipate future growth in data volume and user numbers. Design the database schema to accommodate additional features or increased data without major overhauls. Using databases provides structured, efficient, and secure data management solutions that support business operations and decision-making. They offer scalability, reliability, and the capability to handle complex data relationships, making them a fundamental component in modern information systems.
