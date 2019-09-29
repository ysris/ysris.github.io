---
title: "SQL"
date: 2019-09-15T18:25:22+02:00
draft: false
---

Le langage SQL (structured query language) a été créé par IBM dans les années 80 pour la gestion des SGBDR (système de gestion de bases de données relationnelles). Il est depuis devenu un standard et a fait l’objet de nombreux travaux de normalisation par l’ANSI (American National Standard Institute). 

Chaque SGBD possède sa propre déclinaison de SQL (TRANSACT-SQL pour Sql Server, PL-SQL pour Oracle…). 
SQL est un langage basé sur les principes d’algèbre relationnelle (https://en.wikipedia.org/wiki/Relational_algebra) et de « relational calculus » (https://en.wikipedia.org/wiki/Tuple_relational_calculus).

SQL est composé de trois catégories d’instructions permettant :

* La manipulation de données (Data Manipulation Language, DML) en fournissant les instructions de création, mise à jour, suppression et extraction de données stockées.
* La définition de données (Data Description Language, DDL) en permettant la création, modification, et suppressions des objets SQL (tables, index, vues…).
* Le gestion des contrôles d’accès (Data Control Language, DCL) en permettant notamment de gérer les droits d’accès aux données.

---

## Le langage de manipulation, SELECT simple

### Sélection de colonnes (projection)
L’ordre SELECT le plus simple se présente ainsi, où table est le nom de la table cible : 
{{< highlight sql >}}
SELECT * FROM employe;
{{< /highlight >}}

'*' signifie que le résultat doit contenir toutes les colonnes de la table. On peut ne retenir que certaines colonnes en indiquant le nom de celles-ci séparées par des virgules à la place de l’astérisque :
{{< highlight sql >}}
SELECT Nom, Fonction FROM Employe;
{{< /highlight >}}
### Résultats distincts
La clause DISTINCT ajoutée derrière l’ordre SELECT permet de filtrer les doublons. Exemple, Liste des localités ou il y a des départements :
{{< highlight sql >}}
SELECT DISTINCT Localite FROM Departement;
{{< /highlight >}}

### Filtrer les résultats avec une ou des conditions (sélection)
La clause WHERE permet de spécifier les lignes à sélectionner. Celle-ci doit obligatoirement être suivie de la condition définissant les lignes sélectionnées. Exemple : Nom des départements situés à Paris : 
{{< highlight sql >}}
SELECT Nom FROM Departement WHERE Localite = ‘Paris’;
{{< /highlight >}}

### Condition simple
Une condition simple s’exprime comme la comparaison de deux expressions simples : 
    * =, !=, >, >=, <, <=
    * BETWEEN expr1 AND expr2 
    * OR
    * IN
    * LIKE

---
## Le langage de manipulation, jointures

---
## Le langage de manipulation, opérateurs ensemblistes

---
## Le langage de manipulation, sous-interrogation

---
## Le langage de manipulation, fonctions de groupe

---
## Le langage de manipulation, expressions et fonctions

---
## Le langage de manipulation, modifications de données


Insert multiple rows in a single statement
{{< highlight sql >}}
INSERT INTO TestTable
(
  [foo]
  , [bar]
)
VALUES 
(1,'AABBCC')
, (2,'FFDD')
, (3,'TTHHJJKKLL')
{{< /highlight >}}

Update
{{< highlight sql >}}
update thetable 
set customerId = (select id from dbo.customer where email = 'accountingAccountDefaultTemplate')
where customerId = 40
{{< /highlight >}}

Delete
{{< highlight sql >}}
Delete from…
{{< /highlight >}}

Delete avec une clause join
{{< highlight sql >}}
delete xxx
from xxx ducm
join [dbo].[conversationmessage] cm on cm.id = ducm.conversationMessageId
where destId IN (select id from [dbo].[customer] where rolesstring='CompanyClient')
{{< /highlight >}}

Truncate


---
## Le langage de définitions des données : création de tables 
With PK
CREATE TABLE [dbo].[filez](
	[id] [int] IDENTITY(1,1) NOT NULL,
	[filename] [varchar](max) NULL,
	[alias] [varchar](max) NULL,
	PRIMARY KEY CLUSTERED ([id] ASC)
)
W/o PK
CREATE TABLE customers (email NVARCHAR(255), passhash NVARCHAR(255))

Création de colonnes
{{< highlight sql >}}
ALTER TABLE dbo.paralel_a ADD fileId INT;
{{< /highlight >}}

### Supprimer une table
{{< highlight sql >}}
DROP TABLE customers
{{< /highlight >}}

## Le langage de définitions des données : index
https://hackernoon.com/clustered-vs-nonclustered-what-index-is-right-for-my-data-717b329d042c
{{< highlight sql >}}
CREATE NONCLUSTERED INDEX IX_email ON ' + @destTableName + '(email);
{{< /highlight >}}

## Le langage de définitions des données : vues
{{< highlight sql >}}
CREATE VIEW xxx AS SELECT * FROM uuu
{{< /highlight >}}


## Le langage de définitions des données : droits d’accès

Le langage de contrôle d’accès : privilèges
https://docs.microsoft.com/en-us/sql/relational-databases/indexes/create-nonclustered-indexes?view=sql-server-2017
{{< highlight sql >}}
CREATE LOGIN thelogin WITH PASSWORD = 'supersecurepassword';
CREATE USER theuser FOR LOGIN thelogin WITH DEFAULT_SCHEMA = dbo
GRANT CONTROL ON DATABASE::thedatabase TO theuser 
{{< /highlight >}}
	

Le langage de logique : variables, fonctions, curseurs(meh)

Procédure stockée
{{< highlight sql >}}
CREATE PROCEDURE #STEP2 
	@P_fiduName nvarchar(MAX)
AS BEGIN 
	PRINT('HELLO WORLD')
END;
GO
{{< /highlight >}}


Xp_cmdshell

{{< highlight sql >}}
--To allow advanced options to be changed.
EXEC sp_configure 'show advanced options', 1
GO
-- To update the currently configured value for advanced options.
RECONFIGURE
GO
-- To enable the feature.
EXEC sp_configure 'xp_cmdshell', 1
GO
-- To update the currently configured value for this feature.
RECONFIGURE
GO
EXEC xp_cmdshell 'dir *.exe';
GO  
{{< /highlight >}}

Remove duplicates

Case then when end
Spécificités, EXEC(), shrink database
Tables temporaires
Variables globales @@ : @@version, @@deleted…
Gestion des droits d’accès à une base
Utilisateur readonly
@@SERVERNAME

EXEC(‘’)

Cross server queries

Injection SQL

Nom de machines
SELECT SERVERPROPERTY('MACHINENAME')
SELECT HOST_NAME() 
Format dates
https://anubhavg.wordpress.com/2009/06/11/how-to-format-datetime-date-in-sql-server-2005/

Super fast count

SELECT OBJECT_NAME(i.id) [Table_Name], i.rowcnt [Row_Count]
FROM sys.sysindexes i WITH (NOLOCK)
WHERE i.indid in (0,1)
ORDER BY i.rowcnt desc



select @@servername
select DB_NAME()

change start auto increment id
DBCC checkident ('filez', reseed, 10000)


ALTER TABLE Purchasing.PurchaseOrderHeader  
NOCHECK CONSTRAINT FK_PurchaseOrderHeader_Employee_EmployeeID;  


DROP INDEX IX_ProductVendor_BusinessEntityID   
    ON Purchasing.ProductVendor;  
GO  





{{< highlight sql >}}

{{< /highlight >}}

## MySQL

### Exporter une base de données en script SQL
mysqldump -u root -p --databases __PROJECTNAME__ > importscript.sql

## Importer un script dans une base de données
mysql -u root -p
source importscript.sql;