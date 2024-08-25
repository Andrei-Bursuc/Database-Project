<h1>Database Project for Visionary Care</h1>

The scope of this project is to use all the SQL knowledge gained throught the Software Testing course and apply them in practice.

Application under test: **Visionary Care**

Tools used: MySQL Workbench

Database description: **The Growing Need for Ophthalmic Data Standardization.
Data availability has catalyzed medical research with notable examples in ophthalmology, focused largely on common conditions, including glaucoma, diabetic retinopathy, cataracts, and age-related macular degeneration. 
Health care data are derived from a variety of sources and may be prospectively collected for specific research studies, collected as part of routine clinical practice, or generated as a by-product of data from industries external but related to health care.**

<ol>
<li>Database Schema </li>
<br>
You can find below the database schema that was generated through Reverse Engineer and which contains all the tables and the relationships between them.
https://github.com/Andrei-Bursuc/Database-Project/blob/main/Reverse%20Engineer.png

The tables are connected in the following way:

<ul>
  <li> **Saloane**  is connected with **Interventii** through a **1:n** relationship which was implemented through **saloane.id_salon ** as a primary key and **interventii.id_salon** as a foreign key</li>
  
  <li> **Pacienti**  is connected with **Interventii** through a **1:n** relationship which was implemented through **pacienti.id_pacient** as a primary key and **interventii.id_pacient** as a foreign key</li>
  
  <li> **Pacienti**  is connected with **Retete** through a **1:n** relationship which was implemented through **pacienti.id_pacient** as a primary key and **retete.id_reteta** as a foreign key</li>
  
 <li> **Medici**  is connected with **Interventii** through a **1:n** relationship which was implemented through **medici.id_medic** as a primary key and **interventii.id_medic** as a foreign key</li>
 
  <li> **Medici**  is connected with **Retete** through a **1:n** relationship which was implemented through **medici.id_medic** as a primary key and **retete.id_medic** as a foreign key</li>
  
  <li> **Retete**  is connected with **Registru_Retete** through a **1:n** relationship which was implemented through **retete.id_reteta** as a primary key and **registru_retete.id_retea** as a foreign key</li>
  
  <li> **Medicamente**  is connected with **Registru_Retete** through a **1:n** relationship which was implemented through **medicamente.id_medicament** as a primary key and **registru_retete.id_medicament** as a foreign key</li>

  <li> Because the tables **Retete** and **Medicamente** are connected by a **n:n** relationship, the table ** Registru_Retete** is used as a cross table in order to break this realationship and keep the database in a normalized state </li>
</ul><br>

<li>Database Queries</li><br>

<ol type="a">
  <li>DDL (Data Definition Language)</li>

  The following instructions were written in the scope of CREATING the structure of the database (CREATE INSTRUCTIONS)

<br />create table **saloane**
<br />(
<br />id_salon int primary key auto_increment,
<br />etaj int not null, 
<br />capacitate int not null
<br />); 

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




  After the database and the tables have been created, a few ALTER instructions were written in order to update the structure of the database, as described below:

 <br /> alter table **pacienti** add column email varchar(30);
<br />alter table **interventii** modify procedura varchar(80);
  
  <li>DML (Data Manipulation Language)</li>

  In order to be able to use the database I populated the tables with various data necessary in order to perform queries and manipulate the data. 
  In the testing process, this necessary data is identified in the Test Design phase and created in the Test Implementation phase. 

  Below you can find all the insert instructions that were created in the scope of this project:

   <br /> insert into pacienti(nume, prenume, data_nasterii, telefon, email) 
   <br />  values ('Russindilar' , 'Raluca', '1993-03-31', '0724946413', 'raluca.bursuc@gmail.com'),
        <br />    ('Draghiceanu ' , 'Alin' , '1987-10-17' , '+0510725114678' , 'alyn_17dr@gmail.com'),
        <br />    ('Tonu' , 'Marius' , '1990-02-26' , '0762336541' , 'tonu_m90@yahoo.com' ),
         <br />   ('Grigoriu' , 'Alice' , '1993-11-26' , '0788910222' , 'aliss_gri@ggmail.com'),
         <br />   ('Sava' , 'Catalin-Ionut' , '2000-12-22' , '0722478989' , 'catalinI_sava@gmail.com'),
         <br />   ('Sotropa' , 'Diana' , '1970-01-18' , '0761333667' , 'sotropaDiana@yahoo.com');

  After the insert, in order to prepare the data to be better suited for the testing process, I updated some data in the following way:

  **Inserati aici toate instructiunile de UPDATE pe care le-ati scris folosind filtrarile necesare astfel incat sa actualizati doar datele de care aveti nevoie**


  <li>DQL (Data Query Language)</li>

After the testing process, I deleted the data that was no longer relevant in order to preserve the database clean: 

**Inserati aici toate instructiunile de DELETE pe care le-ati scris folosind filtrarile necesare astfel incat sa stergeti doar datele de care aveti nevoie**

In order to simulate various scenarios that might happen in real life I created the following queries that would cover multiple potential real-life situations:

**Inserati aici toate instructiunile de SELECT pe care le-ati scris folosind filtrarile necesare astfel incat sa extrageti doar datele de care aveti nevoie**
**Incercati sa acoperiti urmatoarele:**<br>
**- where**<br>
**- AND**<br>
**- OR**<br>
**- NOT**<br>
**- like**<br>
**- inner join**<br>
**- left join**<br>
**- OPTIONAL: right join**<br>
**- OPTIONAL: cross join**<br>
**- functii agregate**<br>
**- group by**<br>
**- having**<br>
**- OPTIONAL DAR RECOMANDAT: Subqueries - nu au fost in scopul cursului. Puteti sa consultati tutorialul [asta](https://www.techonthenet.com/mysql/subqueries.php) si daca nu intelegeti ceva contactati fie trainerul, fie coordonatorul de grupa**<br>

</ol>

<li>Conclusions</li>

**Inserati aici o concluzie cu privire la ceea ce ati lucrat, gen lucrurile pe care le-ati invatat, lessons learned, un rezumat asupra a ceea ce ati facut si orice alta informatie care vi se pare relevanta pentru o concluzie finala asupra a ceea ce ati lucrat**

</ol>
