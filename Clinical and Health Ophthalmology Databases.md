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

`Saloane`  is connected with Interventii through a 1:n relationship which was implemented through saloane.id_salon  as a primary key and interventii.id_salon as a foreign key.
  
`Pacienti`  is connected with Interventii through a 1:n relationship which was implemented through pacienti.id_pacient as a primary key and interventii.id_pacient as a foreign key.
  
`Pacienti`  is connected with Retete through a 1:n relationship which was implemented through pacienti.id_pacient as a primary key and retete.id_reteta as a foreign key.
  
`Medici`  is connected with Interventii through a 1:n relationship which was implemented through medici.id_medic as a primary key and interventii.id_medic as a foreign key.
 
`Medici`  is connected with Retete through a 1:n relationship which was implemented through medici.id_medic as a primary key and retete.id_medic as a foreign key.
  
`Retete`  is connected with Registru_Retete through a 1:n relationship which was implemented through retete.id_reteta as a primary key and registru_retete.id_retea as a foreign key.
  
`Medicamente`  is connected with Registru_Retete through a 1:n relationship which was implemented through medicamente.id_medicament as a primary key and registru_retete.id_medicament as a foreign key.

 Because the tables Retete and Medicamente are connected by a n:n relationship, the table  Registru_Retete is used as a cross table in order to break this realationship and keep the database in a normalized state.

## Database Queries

<ol type="a">
  <li>DDL (Data Definition Language)</li>

  The following instructions were written in the scope of CREATING the structure of the database (CREATE INSTRUCTIONS)
  
```sql
create database clinica_oftalmologica;
use clinica_oftalmologica;

create table saloane
(
id_salon int primary key auto_increment,
etaj int not null, 
capacitate int not null
); 

<br />create table **pacienti**
<br />(
<br />id_pacient int primary key auto_increment ,
<br />nume varchar(30)  not null,
<br />prenume varchar(30) not null,
<br />data_nasterii date not null,
<br />telefon varchar(20) unique
<br />);

<br />create table **medici**
<br />(
<br />id_medic int primary key auto_increment,
<br />nume varchar(30) not null,
<br />prenume varchar(30) not null,
<br />data_angajare date not null,
<br />salar_anual decimal(9,4) not null,
<br />specializare varchar(40) not null,
<br />telefon varchar(20) unique
<br />);

<br />create table **interventii**
<br />(
<br />id_interventie int primary key auto_increment,
<br />procedura varchar(50) not null,
<br />data_interventie date not null,
<br />id_salon int,
<br />foreign key (id_salon) references saloane(id_salon),
<br />id_pacient int,
<br />foreign key (id_pacient) references pacienti(id_pacient),
<br />id_medic int,
<br />foreign key (id_medic) references medici(id_medic)
<br />);

<br />create table **retete**
<br />(
<br />id_reteta int primary key auto_increment,
<br />data_eliberare date not null,
<br />id_pacient int,
<br />foreign key (id_pacient) references pacienti(id_pacient),
<br />id_medic int,
<br />foreign key (id_medic) references medici(id_medic)
<br />);

<br />create table **medicamente**
<br />( 
<br />id_medicament int primary key auto_increment,
<br />denumire varchar(30) not null,
<br />pret decimal (4,2) not null,
<br />descriere varchar(100)
<br />);


<br />create table **registru_retete**
<br />(
<br />id_registru int primary key auto_increment,
<br />cantitate int not null,
<br />administrare varchar(40) not null,
<br />id_reteta int,
<br />foreign key(id_reteta) references retete(id_reteta),
<br />id_medicament int,
<br />foreign key(id_medicament) references medicamente(id_medicament)
<br />);
```

  After the database and the tables have been created, a few ALTER instructions were written in order to update the structure of the database, as described below:

 <br /> alter table **pacienti** add column email varchar(30);
<br />alter table **interventii** modify procedura varchar(80);
  
  <li>DML (Data Manipulation Language)</li>

  In order to be able to use the database I populated the tables with various data necessary in order to perform queries and manipulate the data. 
  In the testing process, this necessary data is identified in the Test Design phase and created in the Test Implementation phase. 

  Below you can find all the insert instructions that were created in the scope of this project:

   <br /> insert into **pacienti**(nume, prenume, data_nasterii, telefon, email) 
   <br /> values  &nbsp;('Russindilar' , 'Raluca', '1993-03-31', '0724946413', 'raluca.bursuc@gmail.com'),
          <br />  &nbsp;&nbsp; &nbsp;&nbsp;   ('Draghiceanu ' , 'Alin' , '1987-10-17' , '+0510725114678' , 'alyn_17dr@gmail.com'),
          <br />  &nbsp;&nbsp; &nbsp;&nbsp;   ('Tonu' , 'Marius' , '1990-02-26' , '0762336541' , 'tonu_m90@yahoo.com' ),
          <br />  &nbsp;&nbsp;&nbsp;&nbsp;   ('Grigoriu' , 'Alice' , '1993-11-26' , '0788910222' , 'aliss_gri@ggmail.com'),
          <br />  &nbsp;&nbsp;&nbsp;&nbsp;   ('Sava' , 'Catalin-Ionut' , '2000-12-22' , '0722478989' , 'catalinI_sava@gmail.com'),
          <br /> &nbsp;&nbsp; &nbsp;&nbsp;   ('Sotropa' , 'Diana' , '1970-01-18' , '0761333667' , 'sotropaDiana@yahoo.com');

The data from the table can be interrogated by using the following script 
 <br /> select* from **pacienti**;
  <br />
  <br />  insert into **medici** (nume, prenume, data_angajare, salar_anual, specializare, telefon)
  <br />    values&nbsp;('Preda', 'Ancuta' , '2010-07-11' , 98000 , 'Cataracta adultului' , '0722312447'),
          <br />  &nbsp;    ('Oprea', 'Bianca' , '2015-01-11' , 92000 , 'Cataracta congenitala' , '0744223787'),
          <br /> &nbsp;    ('Troi', 'Raluca' , '2013-04-15' , 94020.80 , 'Chirurgie refractiva grad I' , '0763558478'),
          <br />  &nbsp;    ('Lisca', 'Monica' , '2019-11-20' , 85070, 'Oftalmologie pediatrica' , '0755365174'),
          <br /> &nbsp;   ('Strelet', 'Mihai' , '2022-05-13' , 83600.30, 'Chirurgie refractiva grad II' , '0733121657'); <br /> <br />
          <br />    insert into **medici** (nume, prenume, data_angajare, salar_anual, specializare, telefon)
          <br />   values&nbsp;('Apavaloaie', 'Cornel' , '2011-08-12' , 95000 , 'Cataracta congenitala' , '0724567543'),
          <br />    &nbsp;   ('Grosu', 'Adrian' , '2017-02-23' , 99000 , 'Oftalmologie pediatrica' , '0742987565');
 
The data from the table can be interrogated by using the following script<br /> 
 select* from **medici**;
  <br />
 <br /> insert into **saloane**(etaj, capacitate)
 <br />   values(1 , 10),
        <br /> &nbsp;     (2 , 10),
        <br />  &nbsp;   (3 , 8),
        <br /> &nbsp;  (4 , 6);
        
The data from the table can be interrogated by using the following script<br />
       select * from **saloane**; 
    <br />     
 <br /> insert into **interventii**(procedura, data_interventie, id_salon, id_pacient, id_medic)
 <br /> values('Inlocuirea cristalinului artificial' , '2022-03-19', 1 , 1 , 1),
       <br /> &nbsp;('Indepartarea cataractei juvenile' , '2022-04-16' , 3 , 2 , 2),
       <br /> &nbsp;('Extragerea intracapsulara a cristalinului' , '2022-09-22' , 1 , 3 , 1),
       <br /> &nbsp;('Excizia canalelor lacrimale', '2023-01-12', 2 , 4 , 4),
       <br /> &nbsp;('Alte proceduri pe muschii sau tendoanele extraoculare' , '2023-03-17' , 4 , 5, 3),
       <br /> ('Transplant muscular pentru strabism' , '2024-02-09', 2, 6 , 5); 
        
 insert into **interventii**(procedura, data_interventie, id_salon, id_pacient, id_medic)<br />
values ('Corectia glaucomului congenital', '2024-10-25', 3, 2, null);<br />
 
  The data from the table can be interrogated by using the following script<br />
        select * from **interventii**; <br />
   <br />        
 insert into **medicamente**(denumire, pret, descriere)
    <br /> values('Nostamine' , 25.80 , 'Destinat atat utilizarii nazale cat si intraoculare'),
         <br />  ('Ialuvit' , 33.70 , 'Se distribuie uniform pe suprafata oculara formand un bandaj de protectie visco-elastic'),
          <br /> ('Tarosin'  , 45.80 , 'Destinat hemoragiilor retiniene'),
          <br /> ('Adrusen Mega' , 67.20 , 'Pentru preventia si tratamentul degenerescentei maculare legate varsta.'),
		  <br /> ('Tobradex' , 56.8 , 'Pentru tratamentul inflamatiei si a unei posibile infectii oculare'); <br />
    <br />  The data from the table can be interrogated by using the following script<br />
       select * from **medicamente**; <br />
    <br />     
insert into **retete**( data_eliberare, id_pacient, id_medic)
   <br />  values('2022-03-20' , 1 , 1),
       <br />    ('2022-04-17' , 2 , 2),
        <br />   ('2022-09-23' , 3 , 1),
         <br />  ('2023-01-14' , 4 , 4),
          <br /> ('2023-03-17' , 5 , 3),
          <br /> ( '2024-02-10', 6 , 5);

<br /> The data from the table can be interrogated by using the following script<br />
       select * from **retete**; <br />
  <br />
<br />insert into **registru_retete**(cantitate, administrare, id_reteta, id_medicament)
  <br /> values(30 , '5 zile-de 3 ori pe zi', 1, 2),
        <br /> (20 , '3 zile-de 2 ori pe zi', 2, 3),
         <br />(100 , '10 zile-de 4 ori pe zi' , 3 , 5),
         <br />(60 , '7 zile-de 2 pe zi', 4, 1),
       <br /> (3 , '3 zile-una pe zi', 5 , 4);
 <br /> The data from the table can be interrogated by using the following script<br />
       select * from **registru_retete**; <br /> 
         

       
 
  After the insert, in order to prepare the data to be better suited for the testing process, I updated some data in the following way:

  1.The expansion of specialization to include 'Strabism' is requested for Dr. Preda Ancuța  <br />
**update** medici set specializare = 'Cataracta adultului/Strabism' where nume = 'Preda' and prenume = 'Ancuta'; <br /> 
<br />  2.It is requested to increase by 5% the annual salary of doctors who have the specialization in 'Chirurgie refractiva grad II'."<br /> 
**update** medici set salar_anual = salar_anual * 1.05 where specializare = 'Chirurgie refractiva grad II';
 
After the testing process, I deleted the data that was no longer relevant in order to preserve the database clean: 

1.The request would be to delete all records of interventions that occurred on a specific date.  <br />
 **delete** from interventii where data_interventie = '2023-03-17';  <br />
<br /> 2. The deletion of the prescription that was issued for the patient with identifier 6 is requested<br />
 **delete** from retete where id_pacient = 6;

  <li>DQL (Data Query Language)</li>

In order to simulate various scenarios that might happen in real life I created the following queries that would cover multiple potential real-life situations:



**where**<br /> Display all the data of doctors employed starting from the year 2015." <br />
 select * from medici **where** year(data_angajare) >= 2015;<br />
 
Display the name and description of medications that have a price between 30 and 60 lei."<br />
 select denumire, descriere from medicamente **where** pret between 30 and 60;
 
**- AND**<br> Present all the interventions made by Dr. Monica Lisca between January 22 2012, and June 30 2024..<br />
<br />select intrv.id_interventie, intrv.procedura, intrv.data_interventie, med.nume, med.prenume
<br />from interventii intrv inner join medici med on intrv.id_medic = med.id_medic
<br />where med.nume = 'Lisca' **and** med.prenume = 'Monica'
<br />**and** intrv.data_interventie between '2022-01-12' **and** '2024-06-30';';

**- OR**<br>Display all patients who were born before 1990 or after 2000<br>
 <br>select * from pacienti where year(data_nasterii) <= 1990 **or** year(data_nasterii) >= 2000;
 

**- like**<br> Display the last name, first name, and date of birth for patients whose first name starts with the letter A.<br>
<br> select nume, prenume, data_nasterii from pacienti where upper(prenume) **like** 'A%';
 
**- inner join**<br>  Display the name of the intervention, the name of the patient, and the name of the doctor who performed the intervention for each procedure in the clinic<br>
<br>select intrv.procedura, pac.nume as nume_pacient, pac.prenume as prenume_pacient, med.nume as nume_medic, med.prenume as prenume_medic from interventii intrv 
**inner join** pacienti pac on intrv.id_pacient =  pac.id_pacient
**inner join** medici med on intrv.id_medic =  med.id_medic;

--Display the prescribed medication names and the issue date for each prescription <br>
<br>select ret.id_reteta, ret.data_eliberare, med.denumire from registru_retete reg 
**inner join** retete ret on ret.id_reteta = reg.id_reteta
**inner join** medicamente med on med.id_medicament = reg.id_medicament;

**- left join**<br>   Display the names of the patients and the name of the intervention they underwent, if applicable, for both patients who have undergone interventions and those who have not<br>
<br>select pac.nume, pac.prenume, intrv.procedura from pacienti pac **left join** interventii intrv on intrv.id_pacient = pac.id_pacient;

**- right join**<br>   Display the names of the doctors and the names of the interventions they performed, if applicable, for both doctors who have performed interventions and those who have not<br>
<br>select med.nume, med.prenume, intrv.procedura from interventii intrv **right join** medici med on med.id_medic = intrv.id_medic;

**- cross join**<br>  Display all possible types of interventions that can be performed by each doctor<br>
<br>select med.nume, med.prenume, intrv.procedura from medici med **cross join** interventii intrv;

**- functii agregate**<br> Display the total anual salary, the min salary, the max salary, the average salary of the doctors working in the clinic and also the total number of doctors.<br>
<br>select **sum**(truncate(salar_anual, 2)) as salar_anual_total, truncate(**max**(salar_anual), 2) as salar_maxim,  truncate (**min**(salar_anual), 2) as salar_minim, truncate(**avg**(salar_anual), 2) as salar_mediu, **count**(id_medic) numar_medici from medici;<br>

**- group by**<br>  Display the number of doctors in each specialization if it exceeds one doctor per specialization<br>
<br>select specializare, count(id_medic) from medici **group by** specializare having count(id_medic) > 1;

**- limit**<br>  Display the doctors starting from 2, 3 and 4th positions, which have the annual salary greater or equal to 90000 and order them descending by salary.<br>
<br>select * from medici where salar_anual >= 90000 order by salar_anual desc limit 3 offset 1;<br>

**- Subqueries** -<br>
Select all doctors who have the same specialization as Dr. Apăvăloaie Cornel, except for him.<br>
<br>select * from medici where specializare = (select specializare from medici where nume = 'Apavaloaie' and prenume = 'Cornel') and nume!='Apavaloaie' and prenume != 'Cornel';

</ol>

## Conclusions
Creating and managing a database for an ophthalmology clinic provides valuable insights and lessons that can improve future projects. Detailed requirements gathering is crucial. Involve stakeholders (doctors, nurses, administrative staff) early to understand their needs and expectations. Anticipate future growth in data volume and user numbers. Design the database schema to accommodate additional features or increased data without major overhauls. Using databases provides structured, efficient, and secure data management solutions that support business operations and decision-making. They offer scalability, reliability, and the capability to handle complex data relationships, making them a fundamental component in modern information systems.
</ol>
