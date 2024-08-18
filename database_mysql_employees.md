<h1>Database Project for Employee and Company Management System</h1>

The scope of this project is to use all the SQL knowledge gained throught the Software Testing course and apply them in practice.

Application under test: MySQL

Tools used: MySQL Workbench

Database description: The Employee and Company Management System database is designed to manage and store information related to employees, the companies they work for, products offered by these companies, and transactions involving these products.

The primary goal of this database is to centralize and streamline the management of data related to employees, companies, products, and transactions, providing a comprehensive view of business operations.


## Database Schema
<br>
You can find below the database schema that was generated through Reverse Engineer and which contains all the tables and the relationships between them.

The tables are connected in the following way:

<ul>
  <li> "Angajati" is connected with "Oficii"" through a **many-to-one** relationship which was implemented through "Oficii.Cod_oficiu" as a primary key and "Angajati.Cod_oficiu" as a foreign key.</li>
  <li> "Companii" is connected with "Angajati" through a **one-to-one** relationship which was implemented through "Companii.Cod_companie" as a primary key and "Companii.Reprezentant_companie" as a foreign key.</li>
  <li> "Tranzactii" is connected with "Companii" through a **many-to-one** relationship which was implemented through "Companii.Cod_companie" as a primary key and "Tranzactii.Cod_client" as a foreign key.</li>
  <li> "Tranzactii" is connected with "Angajati" through a **many-to-one** relationship which was implemented through "Angajati.Employee_Code" as a primary key and "Tranzactii.Cod_vanzator"* as a foreign key.</li>
  <li> "Tranzactii" is connected with "Produse" through a **many-to-one** relationship which was implemented through "Produse.Product_Code" as a primary key and "Tranzactii.Cod_produs" as a foreign key.</li>
  <li>"Angajati" is connected with "Oficii" through a **many-to-one** relationship which was implemented through "Oficii.Cod_oficiu" as a primary key and "Angajati.Cod_oficiu" as a foreign key.</li>
</ul>

## Database Queries


### DDL (Data Definition Language)

  The following instructions were written in the scope of CREATING the structure of the database (CREATE INSTRUCTIONS)

Instructions for creating the database Employees
    ```
    create database Employees;
    ```

    
Instructions for creating the table Angajati:
```
CREATE TABLE Angajati (
  Employee_Code VARCHAR(2),
  Surname VARCHAR(15),
  Cod_oficiu VARCHAR(2),
  NAME VARCHAR(15),
  Age NUMBER,
  ROLE VARCHAR(20),
  Hire_Date DATE,
  Payment NUMBER(10,1),
  PRIMARY KEY (Employee_Code)
);
```

Instructions for creating the table Companii:
```
CREATE TABLE Companii (
  Cod_companie VARCHAR(2) PRIMARY KEY,
  Compania VARCHAR(15),
  Reprezentant_companie VARCHAR(3),
  Datoria NUMBER(2),
  CONSTRAINT fk_reprezentant FOREIGN KEY (Reprezentant_companie) REFERENCES Angajati(Employee_Code)
);
```

Instructions for creating the table Produse:
```
CREATE TABLE Produse (
  Product_Code NUMBER(5) GENERATED ALWAYS AS IDENTITY (START WITH 1 INCREMENT BY 1),
  DESCRIPTION VARCHAR(20),
  Price NUMBER(10,2),
  Stock CHAR(1) CHECK (Stock IN ('Y','N')),
  PRIMARY KEY (Product_Code)
);
```

Instructions for creating the table Tranzactii:

```
CREATE TABLE Tranzactii (
  Data_comenzii DATE,
  Cod_client VARCHAR(2),
  Cod_vanzator VARCHAR(2),
  Cod_produs NUMBER(5),
  Cantitatea NUMBER(2),
  CONSTRAINT fk_client FOREIGN KEY (Cod_client) REFERENCES Companii(Cod_companie),
  CONSTRAINT fk_vanzator FOREIGN KEY (Cod_vanzator) REFERENCES Angajati(Employee_Code),
  CONSTRAINT fk_produs FOREIGN KEY (Cod_produs) REFERENCES Produse(Product_Code)
);
```
Instructions for creating the table Oficii:
```
CREATE TABLE Oficii (
  Cod_oficiu VARCHAR(2) PRIMARY KEY,
  Localitatea VARCHAR(15),
  Regiunea VARCHAR(6),
  Cod_manager VARCHAR(3),
  CONSTRAINT fk_manager FOREIGN KEY (Cod_manager) REFERENCES Angajati(Employee_Code)
);
```
  ## After the database and the tables have been created, a few ALTER instructions were written in order to update the structure of the database, as described below:

## Adding Primary or Foreign Keys:
-- Adding a foreign key constraint to the `Angajati` table
```
ALTER TABLE Angajati
ADD CONSTRAINT fk_oficiu FOREIGN KEY (Cod_oficiu) REFERENCES Oficii(Cod_oficiu);
```

## Updating Data with Foreign Key References:
-- Updating `Cod_oficiu` in the `Angajati` table to match new foreign key references
```
UPDATE Angajati
SET Cod_oficiu = '1'
WHERE Employee_Code = '01';

UPDATE Angajati
SET Cod_oficiu = '2'
WHERE Employee_Code = '02';

UPDATE Angajati
SET Cod_oficiu = '3'
WHERE Employee_Code = '03';
```
 
  
  <li>DML (Data Manipulation Language)</li>

  In order to be able to use the database I populated the tables with various data necessary in order to perform queries and manipulate the data. 
  In the testing process, this necessary data is identified in the Test Design phase and created in the Test Implementation phase. 

  Below you can find all the insert instructions that were created in the scope of this project:

Inserting data into the Angajati table:
```
INSERT INTO Angajati (Employee_Code, Surname, Cod_oficiu, NAME, Age, ROLE, Hire_Date, Payment) 
VALUES ('01', 'Popescu', '1', 'Ion', 34, 'Suport Analyst', TO_DATE('2021-05-04', 'YYYY-MM-DD'), 5986);

INSERT INTO Angajati (Employee_Code, Surname, Cod_oficiu, NAME, Age, ROLE, Hire_Date, Payment) 
VALUES ('02', 'Andreescu', '2', 'Maria', 36, 'Suport IT', TO_DATE('2020-08-01', 'YYYY-MM-DD'), 6000);

INSERT INTO Angajati (Employee_Code, Surname, Cod_oficiu, NAME, Age, ROLE, Hire_Date, Payment) 
VALUES ('03', 'Ionescu', '3', 'Gheorghe', 31, 'Suport Aplicatii', TO_DATE('2017-11-01', 'YYYY-MM-DD'), 6500);

INSERT INTO Angajati (Employee_Code, Surname, Cod_oficiu, NAME, Age, ROLE, Hire_Date, Payment) 
VALUES ('04', 'Manea', '4', 'Andrei', 24, 'HR Specialist', TO_DATE('2023-10-04', 'YYYY-MM-DD'), 4500);

INSERT INTO Angajati (Employee_Code, Surname, Cod_oficiu, NAME, Age, ROLE, Hire_Date, Payment) 
VALUES ('05', 'Bordea', '5', 'Mihai', 42, 'Manager', TO_DATE('2014-02-04', 'YYYY-MM-DD'), 7500);

INSERT INTO Angajati (Employee_Code, Surname, Cod_oficiu, NAME, Age, ROLE, Hire_Date, Payment) 
VALUES ('06', 'Mircescu', '6', 'Tudor', 34, 'Financial Analyst', TO_DATE('2022-03-05', 'YYYY-MM-DD'), 4500);

INSERT INTO Angajati (Employee_Code, Surname, Cod_oficiu, NAME, Age, ROLE, Hire_Date, Payment) 
VALUES ('07', 'Dobrescu', '1', 'Mara', 30, 'Application Suport', TO_DATE('2020-05-05', 'YYYY-MM-DD'), 5500);
```

Inserting data into the Companii table:

```
INSERT INTO Companii (Cod_companie, Compania, Reprezentant_Companie, Datoria) 
VALUES ('11', 'MedicHelp', '01', 67.6);

INSERT INTO Companii (Cod_companie, Compania, Reprezentant_Companie, Datoria) 
VALUES ('22', 'Medicover', '02', 65.03);

INSERT INTO Companii (Cod_companie, Compania, Reprezentant_Companie, Datoria) 
VALUES ('33', 'MedLife', '03', 67.46);

INSERT INTO Companii (Cod_companie, Compania, Reprezentant_Companie, Datoria) 
VALUES ('44', 'HelpMe', '04', 53.98);

INSERT INTO Companii (Cod_companie, Compania, Reprezentant_Companie, Datoria) 
VALUES ('55', 'Clinical Rsrch', '05', 45.93);

INSERT INTO Companii (Cod_companie, Compania, Reprezentant_Companie, Datoria) 
VALUES ('66', 'Parexel', '06', 54.38);

INSERT INTO Companii (Cod_companie, Compania, Reprezentant_Companie, Datoria) 
VALUES ('77', 'Syneos Corpate', '07', 10.34);

INSERT INTO Companii (Cod_companie, Compania, Reprezentant_Companie, Datoria) 
VALUES ('88', 'MediBun', '08', 5.45);

INSERT INTO Companii (Cod_companie, Compania, Reprezentant_Companie, Datoria) 
VALUES ('99', 'helpmenow', '09', 67.98);

INSERT INTO Companii (Cod_companie, Compania, Reprezentant_Companie, Datoria) 
VALUES ('10', 'Medie', '10', 77.98);
```

Inserting data into the Produse table:

```
INSERT INTO Produse (DESCRIPTION, Price, Stock) 
VALUES ('Laptop Mac', 1500.00, 'Y');

INSERT INTO Produse (DESCRIPTION, Price, Stock) 
VALUES ('Laptop', 1500.00, 'Y');

INSERT INTO Produse (DESCRIPTION, Price, Stock) 
VALUES ('TV', 1500.00, 'Y');

INSERT INTO Produse (DESCRIPTION, Price, Stock) 
VALUES ('Casti', 1500.00, 'Y');

INSERT INTO Produse (DESCRIPTION, Price, Stock) 
VALUES ('Mouse', 1500.00, 'Y');

INSERT INTO Produse (DESCRIPTION, Price, Stock) 
VALUES ('Cablu', 1500.00, 'Y');

INSERT INTO Produse (DESCRIPTION, Price, Stock) 
VALUES ('TabletaLG', 1500.00, 'Y');

INSERT INTO Produse (DESCRIPTION, Price, Stock) 
VALUES ('Laptop Samsung', 1500.00, 'Y');

INSERT INTO Produse (DESCRIPTION, Price, Stock) 
VALUES ('Telefon IPHONE', 1500.00, 'Y');

INSERT INTO Produse (DESCRIPTION, Price, Stock) 
VALUES ('TV LG', 1500.00, 'Y');
```

Inserting data into the Tranzactii table:
```
INSERT INTO Tranzactii (Data_comenzii, Cod_client, Cod_vanzator, Cod_produs, Cantitatea) 
VALUES (TO_DATE('2023-02-06', 'YYYY-MM-DD'), '11', '01', 1, 5);

INSERT INTO Tranzactii (Data_comenzii, Cod_client, Cod_vanzator, Cod_produs, Cantitatea) 
VALUES (TO_DATE('2022-04-06', 'YYYY-MM-DD'), '22', '02', 5, 7);

INSERT INTO Tranzactii (Data_comenzii, Cod_client, Cod_vanzator, Cod_produs, Cantitatea) 
VALUES (TO_DATE('2021-11-03', 'YYYY-MM-DD'), '33', '03', 3, 7);

INSERT INTO Tranzactii (Data_comenzii, Cod_client, Cod_vanzator, Cod_produs, Cantitatea) 
VALUES (TO_DATE('2023-02-06', 'YYYY-MM-DD'), '44', '04', 8, 4);

INSERT INTO Tranzactii (Data_comenzii, Cod_client, Cod_vanzator, Cod_produs, Cantitatea) 
VALUES (TO_DATE('2023-02-06', 'YYYY-MM-DD'), '55', '05', 2, 2);

INSERT INTO Tranzactii (Data_comenzii, Cod_client, Cod_vanzator, Cod_produs, Cantitatea) 
VALUES (TO_DATE('2023-02-06', 'YYYY-MM-DD'), '66', '06', 6, 6);
```

Inserting data into the Oficii table:

```
INSERT INTO Oficii (Cod_oficiu, Localitatea, Regiunea, Cod_manager) 
VALUES ('1', 'Bucuresti', 'Sud', '01');

INSERT INTO Oficii (Cod_oficiu, Localitatea, Regiunea, Cod_manager) 
VALUES ('2', 'Timisoara', 'Vest', '02');

INSERT INTO Oficii (Cod_oficiu, Localitatea, Regiunea, Cod_manager) 
VALUES ('3', 'Craiova', 'Sud', '03');

INSERT INTO Oficii (Cod_oficiu, Localitatea, Regiunea, Cod_manager) 
VALUES ('4', 'Iasi', 'Est', '04');

INSERT INTO Oficii (Cod_oficiu, Localitatea, Regiunea, Cod_manager) 
VALUES ('5', 'Ploiesti', 'Sud', '05');

INSERT INTO Oficii (Cod_oficiu, Localitatea, Regiunea, Cod_manager) 
VALUES ('6', 'Constanta', 'Sud', '06');
```


  ### After the insert, in order to prepare the data to be better suited for the testing process, I updated some data in the following way:
Instructions for updating data in tables:
  Updating Angajati table to set Cod_oficiu values:
```
UPDATE Angajati
SET Cod_oficiu = '1'
WHERE Employee_Code = '01';

UPDATE Angajati
SET Cod_oficiu = '2'
WHERE Employee_Code = '02';

UPDATE Angajati
SET Cod_oficiu = '3'
WHERE Employee_Code = '03';

UPDATE Angajati
SET Cod_oficiu = '4'
WHERE Employee_Code = '04';

UPDATE Angajati
SET Cod_oficiu = '5'
WHERE Employee_Code = '05';

UPDATE Angajati
SET Cod_oficiu = '6'
WHERE Employee_Code = '06';

```

  <li>DQL (Data Query Language)</li>

## After the testing process, I deleted the data that was no longer relevant in order to preserve the database clean: 

Deleting specific employees from Angajati table where payment is <5000:

```
DELETE FROM Angajati
WHERE Payment<5000:;
```
Deleting specific rows from the Angajati table:
```
DELETE FROM Angajati
WHERE Employee_Code = '07';  -- Delete employee Mara Dobrescu who no longer needs to be in the database

DELETE FROM Angajati
WHERE Employee_Code = '06';  -- Delete employee Tudor Mircescu if no longer needed for the tests

DELETE FROM Angajati
WHERE Employee_Code = '05';  -- Delete employee Mihai Bordea if no longer needed for the tests
```


## In order to simulate various scenarios that might happen in real life I created the following queries that would cover multiple potential real-life situations:

**Query with WHERE**:
```
SELECT * FROM Angajati
WHERE Age > 30;  -- Find employees older than 30
```
**Query with AND:**
```
SELECT * FROM Angajati
WHERE Age > 30 AND ROLE = 'Manager';  -- Find employees older than 30 who are Managers
```

**Query with OR:**
```
SELECT * FROM Companii
WHERE Cod_companie = '11' OR Cod_companie = '22';  -- Find companies with code '11' or '22'
```

**Query with LIKE:**
```
SELECT * FROM Produse
WHERE DESCRIPTION LIKE '%Laptop%';  -- Find products with 'Laptop' in their description
```
**Left Join and Order by:**
```

SELECT
	a.Employee_Code,
	a.Surname,
a.NAME,
a.Age,
a.ROLE,
a.Hire_Date,
a.Payment,
o.Localitatea AS Office_Location,
o.Regiunea AS Office_Region
FROM
	Angajati a
LEFT JOIN
	Oficii o
ON
	a.cod_oficiu=Cod.oficiu
ORDER BY
	a.Employee_Code
```

**Subquery**
```
SELECT NAME, Payment
FROM Angajati
WHERE Payment > (
  SELECT AVG(Payment)
  FROM Angajati
);  -- Find employees with a payment higher than the average payment
```


<li>Conclusions</li>
In this project, i created and managed a database involving employees, companies, products, transactions, and offices. Key achievements include:

-Table Creation: Defined tables with appropriate primary and foreign keys to maintain data integrity.
-Data Management: Inserted data and verified it with SELECT queries.
-Integrity Constraints: Implemented foreign key constraints to enforce relationships, such as between employees and offices or transactions and products.
-Querying: Developed complex queries using various filters and joins to extract and analyze data effectively.

Lessons Learned:

-Ensured data integrity through proper use of foreign keys.
-Enhanced skills in writing advanced SQL queries.
-Emphasized the importance of accurate documentation and testing.

This project provided hands-on experience in database design, management, and querying, highlighting the practical application of relational database concepts.


