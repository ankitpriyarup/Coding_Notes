# DBMS

Data: Raw and isolated facts about an entity\(recorded\) eg - text, audio, video, image, etc  
Information: Processed, meaningful, usable data is Information  
Database: Collection of similar data.

Database Management System is a collection of interrelated data representing information relevant to an enterprise and a set of programs to store and retrieve those data in the most efficient and convenient manner and the collection of data is usually refereed as a database  
Where is database needed? How was data stored before computers

Disadvantage Of File System:

1. Data Redundancy
2. Data Inconsistency
3. Difficulty in accessing data
4. Data isolation
5. Security problem
6. Atomicity problem
7. Concurrent access anomalies
8. Integrity problem

In a database it can happen that 90% of data is historic 10% is new which is used daily. So we separately store them in order to reduce space ultimately reducing the memory access time. OLAP \(Online Analytical Processing\) - Historic, OLTP \(Online Transaction Processing\)

Query Language - Procedural \(Relational Algebra\), Non Procedural \(Relational Calculus\)  
Structured Query Language \(SQL\)

* In practical implementation we use RDBMS, SQL is to write query on it
* So relational model is a conceptual/theoretical framework and RDBMS is it's implementation
* Relational algebra and calculus are mathematical system of query language used on relational model.

## Data Models

* **Relational Model:** Data is stored in tables called relational. Attributes \(Columns\) Tuple \(Rows\). Degree = no. of attributes. Cardinality = no. of tuples. Advantages: Simplicity, Structural Independance \(only concerned about data and not with structure\), Easy to use, Query capability, Scalable. Disadvantages: Structured limit \(field length limit, type, etc\), Cost \(of softwares and implementing\).
* **ER \(Entity Relationship\) Model:** It defines the conceptual view of a database. It works around real world entities and the assosiation among them. At view level it is considered a good option for designing database. An entity is a person or a thing which has logical or physical existence in the real world. For example, in a school database, students, teachers, classes, and courses. An entity set is a collection of similar types of entities. Entities are represented by means of their properties, called attributes. For example, a student entity may have name, class, and age as attributes. Advantages: Better understanding \(graphical\), Easy conversion to other models, easy and straightforward. Disadvantages: Popular for high-level design, No industry standard for notation.
* **Object Oriented Model:** This data model is another method of representing real world objects. It considers each object in the world as objects and isolates it from each other. It groups its related functionalities together and allows inheriting its functionality to other related sub-groups. Advantages: Codes are reused because of inheritance, More flexible to change, More organized. Disadvantage: Complex to implement, Not widely used.

> Both ER & Object Oriented Model are Object based model since they use real world objects

* **Semi-Structured Model:** The data is modelled as a tree or rooted graph where the nodes and edges are labelled with names and/or have attributes associated with them. The data can exist without there being a schema, although it is possible that there is one. \(XML, JSON\) Advantages - It can represent the information of some data sources that cannot be constrained by schema, Data transfer is portable, flexible, schema can be easily changed, useful for browsers. Disadvantages - Queries are less efficient then in strucuted models.
* **Hierarchial Model:** In hierarchical model, data is organized into a tree like structure with each record is having one parent record and many children. The main drawback of this model is that, it can have only one to many relationships between nodes. Rarely used.
* **Network Model:** The network model was created to represent complex data relationships more effectively than the hierarchical model, to improve database performance. Advantages - Conceptual simplicity, handles more relationship type, data access flexibility, promotes data integrity. Disadvantages - system complexity, lack of structural dependencies.

> A database schema is the skeleton structure that represents the logical view of the entire database. It defines how the data is organized and how the relations among them are associated.

> Database users - Naive user, application programmer, sophisticated users, specialised users

> Database administrator - schema defination, access control, routine maintainance, storage structure and access method defination

## Keys

* **Superkey:** A combination of one or more attributes that uniquely defines a tuple in a relation are called superkey.
* **Key:** Any combination of one or more attributes that uniquely defines a tuple in a relation and there is no proper subset of that combination of attributes that will uniquely define a tuple in a relation. Then this combination of attribute is called a key.

![](../.gitbook/assets/image%20%28239%29.png)

* **Candidate Keys:** For any relational scheme if more then one key exist then each key is called a candidate key.
* **Primary Key:** It is a candidate key that is chosen during the time of designing schema of the table.
* **Secondary Key:** All candidate keys which are not primary key are secondary key.
* **Foreign Key:** Foreign key is an attribute or a combination of attributes that refrences the primary key of some other relation.
* **Composite Key:** Any key which is a combination of 2 or more attributes.

{% embed url="https://youtu.be/x\_inLVXPlSU?list=PLmXKhU9FNesR1rSES7oLdJaNFgmuj0SYV" %}

## Database Languages

1. DDL \(Data Defination Language\)
   * Define schema
   * Define integrity constraints \(PRIMARY KEY, FOREIGN KEY, UNIQUE, NOT NULL, CHECK\)
2. DML \(Data Manipulation Language\)
   * Retrive data
   * Insert data
   * Update data
   * Delete data
3. Query language \(Data retrieval\)
   * Relational Algebra \(Procedural\)
   * SQL \(Declarative or Non Procedural\)

## SQL \(Structured Query Language\)

It offers - DDL, DML, Integrity, View defination, Transaction control, Embedded SQL & Dynamic SQL, Authorization.  
Basic data type - char\(n\), varchar\(n\), int, small int, numeric\(p, d\), float\(n\), date, time, timestamp

Key functions of DBA:

* Schema defination
* Storage structure and access method defination
* Schema and physical organization modification
* Granting of authority for data access
* Routine maintainance \(periodically backups, ensuring enough free space, monitoring jobs running and ensuring performance\)

```sql
CREATE TABLE STUDENT
(
    Roll_no varchar(12),
    Name char(50),
    Contact_no int,
    email_id varchar(12),
    PRIMARY KEY (Roll_no),
    FOREIGN KEY (Name) refrences (REC.Name)
);

INSERT INTO STUDENT values(...);
DELETE table_name;
DROP table_name;
ALTER TABLE table_name add (last_name VARCHAR(50), ...);
ALTER TABLE table_name DROP COLUMN last_name;
```

> Constraints - Keys \(primary, foreign\), Unique, Not Null

## Relational Algebra

* Selection \(σ\) σaddress = "Rohini" \(table\_name\)
* Projection \(π\) πname, address \(student\)
* Rename \(ρ\) ρs\(student\)
* Cartesian Product \(x\) student x student\_course\_grade
* Joins \(student ⋈ student\_course\_grade\)
  * Theta join\(⋈θ\): Say two tables mobile & laptop wit prices. purchase both but laptop price should be more than mobile then Mobile ⋈mobile.price &lt; laptop.price Laptop
  * Equi join\(⋈e\): Equi join is a theta join with equal condition
  * Natural join\(⋈\): It's equi join without having equal column twice. Optimized way.
* Outer Joins - Left Outer Join, Right Outer Join, Full Outer Join \(Simply append other part with matching values rest set NULL, Full outer join includes elements from both table\).

![](../.gitbook/assets/image%20%28256%29.png)

* Aggregate functions - SUM\(\), COUNT\(\), AVG\(\), MIN\(\), MAX\(\) Gsum \(Marks\)\(student\_course\_grade\)
* Generalisation - Only aggregate \(above\), Only grouping cidG \(table\), both cidGsum \(Marks\) \(table\) This last one will show a table with grouped values for distinct cid with corresponding summed up marks in each groups
* Division: Result contains all tuples appear in R1\(z\) in combination with every tuple from R2\(y\) where Z = \(x U y\). Result will contain R1-R2 attributes. Best suited to queries that include 'for all'

![](../.gitbook/assets/image%20%28216%29.png)

* Set Union \(Applies on relation\) R1 U R2
* Set Intersection \(Applies on relation\) R1 ∩ R2
* Set Difference \(Applies on relation\) R1 - R2

![](../.gitbook/assets/image%20%28262%29.png)

![](../.gitbook/assets/image%20%28228%29.png)

![](../.gitbook/assets/image%20%28238%29.png)

![](../.gitbook/assets/image%20%28225%29.png)

> Relational Algebra do not considers duplicacy as it is based on set theory matlab table me same tuple hue toh sql will show it but relational algebra won't

> Natural join, division, intersection these are derived operators and it means that other operators can make these but still these derived ones are there to speed up the task

[https://cs.stackexchange.com/questions/29897/use-count-in-relational-algebra](https://cs.stackexchange.com/questions/29897/use-count-in-relational-algebra)

> Referential integrity refers to the accuracy and consistency of data within a relationship. It requires that, whenever a foreign key value is used it must reference a valid, existing primary key in the parent table. For example, if we delete record number 15 in a primary table, we need to be sure that there’s no foreign key in any related table with the value of 15. We should only be able to delete a primary key if there are no associated records. So it will prevent - Adding records to a related table if there is no associated record in the primary table, Changing values in a primary table that result in orphaned records in a related table, Deleting records from a primary table if there are matching related records.

## Entity Relation Model

* A non-technical design method works on conceptual level based on the perception of real world.
* Consists of collection of basic objects called entities and of relationship among there objects and attributes which define their properties.
* Free from ambigutes and provides a standard and logical way of visualizing data.
* Basically it is a diagramatic representation easy to understand even by non-technical user.
* Conversion of ER model to Relation model is simple.

ER model describes data as entity, relationship and attributes. An entity is a real world thing that has an independent existence. Entity may have physical existence or conceptual existence. eg - a particular person, car, book, house or a course, a department, a university, a job, etc. 3 markers of same company same appearance cannot be called entity \(Independent existence\) they are still objects. With each entity, a set of attributes are associated that describe the properties of that entity.

Entity type is defined as a collection of entities that have the same attributes.  
Entity set is the collection of all the entities of a particular entity type in a database at any point of time. Entity set is represented by rectangle.

Table of Relational Model, each values of student table is an entity while the student table is entity set.

![](../.gitbook/assets/image%20%28226%29.png)

Attributes are the units that describe the characterstics of entities such that each entity can be differentiated.

* For each attribute there is a set of permitted values called domains
* In ER diagram represented by ellipse or oval while in relational model by a seperate column.

Types:

1. Simple-Composite : Simple cannot be divided any further represented by simple oval. Composite can be divided further in simple attributes, oval connected to oval.
2. Single-Multivalued : Single can have only one value at an instance of time. Multivalued can have more then one value at an instance of time. Multivalued is double oval represented.
3. Stored-Derrived : Stored is how value is stored in the database. Derrived is how value can be computed in runtime using stored attributes. Derrived is represented by dotted. \(Age is derrived by DOB or distance in google maps derrived by destination and source\)

Relationship:  
Relationship is an assosiation between two or more entities of same or different entity set

* No representation in ER diagram as it is an instance or data
* In relational model represented either using a row in table

Relationship Type: a set of simmilar type of relationship

* In ER diagram represented using diamond
* In relational model either by a seperate table or by seperate column \(foreign key\)
* Every relationship type has 3 components
  * Name
  * Degree
  * Cardinality Ratio/Participation Constraint

![](../.gitbook/assets/image%20%28255%29.png)

![](../.gitbook/assets/image%20%28247%29.png)

Degree of a relationship set: Means number of entity set associated \(participated\) in a relationship set. Most of the relationship set in ER diagram are binary. Unary relationship is also possible like in above image student-student student can be a monitor

Mapping Cardinalities Ratio: Express the number of entities to which other entities can be related via a relationship. Can be used in describing a relationship set of any degree but is most useful in Binary relationship

* 1 : 1 \(One to One\) Entity of a set can relate atmost one entity of other set and vice versa. Atmost means there can be an entity not in relationship. Indian Citizen & Aadhar number
* 1 : N \(One to Many\) Indian Citizen & Mobile Number
* N : 1 \(Many to One\) Mobile Number & Indian Citizen
* M : N \(Many to Many\) Teacher & Students

In relationship representation 1 n will be written. Arrow will denote a one one relationship simple line means many

> Every one to one relationship is one to many or many to one or many to many relationship as well but not vice versa

Participation Constraint: Specifies weather the existence of an entity depends on its being related to another entity via a relationship type. These constraint specify the minimum and maximum number of relationship instances that each entity can/must participate in.  
Maximum Cardinality, It defines the maximum number of times an entity occurence participating in a relationship while Minimum Cardinality is the minimum number of times an entity occurence participating in a relationship.

* Partial Participation \(Minimum cardinality = 0\) Not bounded to be in a relationship
* Total Participation \(Minimum cardinality &gt; 0\) Example: A project employee relationship. Each project must have min 5 employee and max 12 employee. Each employee must be in atmost 2 projects. Here project min cardinality is 5 and max 12 while for employee its 0 and 2. Project to employee is total participation while employee to project it's partial participation. \(Double line in total participation\)

> The Strong Entity is the one whose existence does not depend on the existence of any other entity in a schema while a weak entity is the one that depends on its owner entity i.e. a strong entity for its existence. A weak entity is denoted by the double rectangle.

![](../.gitbook/assets/image%20%28242%29.png)

Conversion to Relational Model:

* Converting Entity Sets to table: Simply put entity set as table and all entities as values
* Composite and multi valued attributes: Forget about main attribute \(name forget take fname lname in table\)
* Weak Entity set:
  * 1:1 - Include primary key attribute of strong entity set and all other attribute of weak entity set
  * M:N - take primary key of both
  * 1:N/N:1 - take all attributes of entity set it total participation and for other take primary key

![](../.gitbook/assets/image%20%28233%29.png)

## Functional Dependency

The functional dependency is a relationship that exists between two attributes. It typically exists between the primary key and non-key attribute within a table.

A functional dependency between two sets of attributes X and Y that are subsets if R specifies a constraint on the possible tuples that can form a relation state 'r' of R. The constraint is that, for any 2 tuples t1 and t2 in r that have t1\[x\] = t2\[x\] they must also have t1\[y\] = t2\[y\]

X → Y  
The left side of FD is known as a determinant, the right side of the production is known as a dependent.  
For example: Assume we have an employee table with attributes: Emp\_Id, Emp\_Name, Emp\_Address. Here Emp\_Id attribute can uniquely identify the Emp\_Name attribute of employee table because if we know the Emp\_Id, we can tell that employee name associated with it. Functional dependency can be written as: Emp\_Id → Emp\_Name

Types of functional dependencies - Trivial A B → A \(returns what we already know\), Non trivial A → B

Attribute Closure/Closure on attribute set: Attribute closure of an attribute set A can be defined as a set of attributes which can be functionally determined from it. denoted by f+

```text
uss attribute ki pahuch kaha tak he
A → B, B → C
then A+ = A B C
A → B, C → D E, A C → F, D → A F, E → C F
then D+ = D A F B
```

Full functional dependency: Functional Dependency denoted by X → Y where X & Y are sets of attributes of R X → Y is said to be full FD iff there exists no functional dependency, denoted by A → X such that A is a proper subset of X A ⊂ X

Partial FD: .... if there exists some functional ....

Armstrong's Axioms:

* Axiom is a statement that is taken true and serve as a permise or starting point for further argument
* Armstrong axioms hold on every relational database and can be used to generate closure set.

Primary rules:

* Reflexivity - if Y ⊆ X then X → Y
* Augmentation - if X → Y then XZ → YZ
* Transivity - if X → Y & Y → Z then X → Z

We can prove secondary rules from primary rules

Secondary Rules:

* Union - X → Y & X → Z then X → Y Z
* Decomposition - if X → Y Z then X → Y & X → Z

> Union & Decomposition need to be done on right side, union we can do on left side but usse nuksaan hi hoga [https://youtu.be/vs65S6Nku5g?list=PLmXKhU9FNesR1rSES7oLdJaNFgmuj0SYV&t=959](https://youtu.be/vs65S6Nku5g?list=PLmXKhU9FNesR1rSES7oLdJaNFgmuj0SYV&t=959)

* Psuedo Transitive - if X → Y & W Y → Z then W X → Z
* Composite - if X → Y & Z → W then X Y → Y W

[https://youtu.be/NeITRksKLzs?list=PLmXKhU9FNesR1rSES7oLdJaNFgmuj0SYV](https://youtu.be/NeITRksKLzs?list=PLmXKhU9FNesR1rSES7oLdJaNFgmuj0SYV)  
[https://youtu.be/0XmHRycmrp0?list=PLmXKhU9FNesR1rSES7oLdJaNFgmuj0SYV](https://youtu.be/0XmHRycmrp0?list=PLmXKhU9FNesR1rSES7oLdJaNFgmuj0SYV)  
[https://youtu.be/o0GQQFu-5C0?list=PLmXKhU9FNesR1rSES7oLdJaNFgmuj0SYV](https://youtu.be/o0GQQFu-5C0?list=PLmXKhU9FNesR1rSES7oLdJaNFgmuj0SYV)  
[https://youtu.be/\_-F6QfdheEk?list=PLmXKhU9FNesR1rSES7oLdJaNFgmuj0SYV](https://youtu.be/_-F6QfdheEk?list=PLmXKhU9FNesR1rSES7oLdJaNFgmuj0SYV)

During finding candidate keys we first find which attribute doesn't have any incoming relation then we take it first verifying through closure for finding candidate keys. Attributes inside the final candidate key are prime attributes and left over are non prime attributes.

![](../.gitbook/assets/image%20%28222%29.png)

## Normalization

![](../.gitbook/assets/image%20%28240%29.png)

Idea: In the table studentInfo we have tried to store entire data about student  
Result: Entire branch data of a branch must be repeated for every student of the branch.  
Redundancy: When some data is stored multiple time unnessarily in a database.  
Disadvantages:

* Insertion, deletion and modification anomalies
* Inconsistency \(data\)
* Increase in data size and increase in time Insertion Anomalie: like say civil dept me koi banda nahi toh info table me dept ki bhi nahi hogi. Deletion Anomalie: agar akela banda he dept ka usse delete nahi karsakte warna branch khatam Updatation Anomalie: update karna ho branch related kaa usse multiple jagah karna padega

Solution in Normalization. It is a logic of decomposing a table until the most optimal result is obtained. It is done on the basis of functional dependencies. Functional dependency can decompose till BCNF only for later on scenerios we need to consider lossy decomposition.

![](../.gitbook/assets/image%20%28221%29.png)

1NF implications:

* A table is said to be in 1NF if every cell contains atomic values. Value can be null.
* Every column should also contain values from the same domain.
* Order of rows or column is irrelevant
* Every column should have unique name

2NF implications:

* Check for 1NF then the table must not have partial dependency

![](../.gitbook/assets/image%20%28252%29.png)

* In the above example AB is the key and D is dependent on AB which is complete key so total complete dependency but for C it depends only on B not AB so it is in partial dependency so not in 2NF to make it in 2NF we will decompose it into two tables such that this property still holds. [https://youtu.be/yIN6k57OB3U?list=PLmXKhU9FNesR1rSES7oLdJaNFgmuj0SYV](https://youtu.be/yIN6k57OB3U?list=PLmXKhU9FNesR1rSES7oLdJaNFgmuj0SYV)

3NF implications:

* If it's in 2NF & has no transitive dependancy
* Transitive dependance is a functional dependency from A → B is called transitive if A, B are non-prime. [https://youtu.be/9H4aJqYyd9s?list=PLmXKhU9FNesR1rSES7oLdJaNFgmuj0SYV](https://youtu.be/9H4aJqYyd9s?list=PLmXKhU9FNesR1rSES7oLdJaNFgmuj0SYV)

BCNF \(Boyce Codd Normal Form\) implications:

![](../.gitbook/assets/image%20%28253%29.png)

Here in this example below, there's no partial dependancy and transitive dependancy so last is checking if right one is prime and left one can be prime as well as non prime. Here it is both prime in C → B so it's not in BCNF. If C would have been anything else which is a super key then we can ignore it

![](../.gitbook/assets/image%20%28227%29.png)

[https://youtu.be/mzxnbsmIRNw?list=PLmXKhU9FNesR1rSES7oLdJaNFgmuj0SYV](https://youtu.be/mzxnbsmIRNw?list=PLmXKhU9FNesR1rSES7oLdJaNFgmuj0SYV

)

## File Structures

SRS -&gt; ER Diagram -&gt; Relation Model -&gt; Normalization -&gt; File Structure\(Indexing & Physical Strucutre such that accessing data gets faster\)

Files can be managed in secondary memory in three ways - contiguous allocation, linked allocation & indexed allocation.

The order of rows & columns are significant however their might be some benefits of having them in sorted. Their might be a case when whole record cannot fit inside the memory block then we use spanned or unspanned memory allocation.

In sorted, searching fast \(binary search\) but insertion and deletion is difficult opposite in unsorted.

If we have say a block of size 1024B and our record \(tuple/row of table\) has size 100B there are n tuples. After filling 10 records 24B space is left now we can either utilize that 24B by splliting record 24-76 putting 76 in next block. This is spanned mapping if we don't then unspanned. Spanned mapping make sure there's no internal fragmentation.

![](../.gitbook/assets/image%20%28258%29.png)

Spanned increases search time but reduces space used.

Indexing on the otherhand is done by keeping index file say with key \(like roll number\) and values \(Block pointer that matches it\). After finding block we simply go to it in O\(1\) then in block it will contain like 10 records searching it linearly is also no deal. If Index file is small then then actual memory blocks it is beneficial.

* Now index file just have two columns key-value unlike actual with all information. \(Reduces width\)
* Index file maps to block pointer each block holds like 10 records so index file will be /10 smaller. \(Reduces height\)

> Our Index file will have roll numbers 1, 11, 21, 31 if we are looking for 9th rollnumber applying binary search we know it's 1 since it's immediate lesser than 11. Keeping just few pointers is sparse indexing but let's say in our blocks records are not sorted then we need to map all pointers then it is dense indexing.

> Sparse ka matlab har record ko entry naa milna par dense ka matlab har value ko entry milna. So example ki values repeat ho rahi he 1, 1, 1, 2, 3 and 1 2 3 are mapped in index other two 1s are left so this is sparse kyuki kuch record rehgaye par also dense kyuki har value hogaya. Toh ye dono he.

* Index file only contains frequently accessed elements. \(Different case, Reduces height\)

> Indexing is a secondary mean for accessing tuples, it optimizes search we could simply search over blocks. Even indexing over dense is still beneficial in time. In space it definitely takes new additional space.

Types Of Indexing:

* Single-level Indexing
  * Primary Indexing: Tab use karte he jab main file sorted ho usme primary key uthaake index kardete he \(search attribute\). It is a example of sparse indexing. No. of enteries in index file = No. of blocks acquired by the main file. No. of access recquired = logN + 1. [https://www.youtube.com/watch?v=mbjE4WsWYCA&list=PLmXKhU9FNesR1rSES7oLdJaNFgmuj0SYV&index=47](https://www.youtube.com/watch?v=mbjE4WsWYCA&list=PLmXKhU9FNesR1rSES7oLdJaNFgmuj0SYV&index=47)
  * Clustered Indexing: Main file is sorted on some non-key attribute. There will be one entry for each unique value of the non-key attribute. If number of block acquired by index file is n, then block access required will be logN + 1. It is both sparse and dense

![](../.gitbook/assets/image%20%28223%29.png)

* * Secondary Indexing: Non sorted he primary key ho naa ho koi naa kyuki this is dense indexing saare index me hote he. This is used only non sorted jab ho. Why do we need ki non sorted? Like sometimes we want to find based on non sorted multiple index file ho sakte he for a single main file it won't change main file toh ek ko karsakte he sort dusre ko ek saath nahi. Since it's dense indexing index file ka height main file jitna hoga still index file kaa faayda he because width kam he toh space toh kam lega comparitively main file ke. And we can now apply binary search aswell. [https://www.youtube.com/watch?v=Tmbv15xiIPo&list=PLmXKhU9FNesR1rSES7oLdJaNFgmuj0SYV&index=50](https://www.youtube.com/watch?v=Tmbv15xiIPo&list=PLmXKhU9FNesR1rSES7oLdJaNFgmuj0SYV&index=50)
* Multi-level indexing: Index eetna bada jo search ke liye dusri indexing. We use B & B+ tree typically kyuki uska structure resemble karta he.

## Transactions

Transaction is maintainance of database.

![](../.gitbook/assets/image%20%28231%29.png)

A transaction is set of instructions. Instructions are atomic in nature they get either performed or not performed. Whole transaction is not atomic but it should also be atomic. In this case we have to roll back. Pese kat jaayenge A se par nahi jaayenge B me toh inconsistency hogaya.

ACID Properties, a transaction should follow these:

* Atomicity \(saare instruction execute ho yaa to koi bhi nahi\)
* Consistency \(Transaction ke baad database ka state change ho jata he as in saare data change so pehle database consistent thaa abb bhi rahe we want this then we can say consistency maintained he. In above bank case consistency is checked with A+B constant rahe throughout\)
* Isolation \(Kisi transaction par kisi dusre transaction ka farak nahi padna chahiye. It's logical isolation ofcourse physically aesa kare ki series me transaction run hoye then system will be slow so we just want it logically, parallaly hoye par user ko lage isolation me horaha he\)
* Durability \(Transaction completion ke baad durable rahe uske changes no software hardware failure commit hogaya\)

> Transaction Management Component is a part of DBMS which ensures Atomicity

> No DBMS component ensures consistency kyuki Atomicity, Isolation, Durability ensured he tab ye bhi automatically he

> Concurrency Control Component is a part of DBMS which ensures Isolation

> Recovery Management Component is a part of DBMS which ensures Durability

![](../.gitbook/assets/image%20%28241%29.png)

All the instructions within the transaction is performed on a local buffer of main database after failure the buffer is discarded and if no failure that partially commited local buffer is copied to main database.

Advantages Of Concurrency \(Ek baar me ek se zyada transactions\):

* Waiting Time reduces
* Response Time reduces \(response is pehla interaction like restaurant me waiter aaya sabse pehle is response in the end food aane me kitna time is total waiting time. We may sometimes even want ki response time less ho althought waiting time zyada rah jaaye\)
* Resource utilization is more
* Efficiency increases

Kabhi kabhi inconsistency aasakti he due to concurrency, manage karna mushkil hoga.

> Transactions are run together as schedule

**Dirty Read Problem:** Koi transaction database ki value read naa karke koi aesi uncommited local buffer ki value read kare then it's dirty read.

![](../.gitbook/assets/image%20%28237%29.png)

In this example T1 & T2 run concruently. T1 does some changes on local buffer say changes 10 to 11 and at the same time T2 also reads, reading first takes from local buffer if there's no existence in primary memory then it will move to secondary memory. T2 then commits. Now later on if T1 has some failure then it will rollback this will create inconsistency. This can be avoided if T2 commited later on i.e. after T1 commits.

**Unrepeatable Read Problem:** Jab ek value ko repeatedly read kara and values alag aayi then ye problem he.

![](../.gitbook/assets/image%20%28251%29.png)

Now eesme read jab dusri baar hua value change kardi kisi dusre transaction ne so inconsistency hogayi.

**Phantom Read Problem:**

![](../.gitbook/assets/image%20%28229%29.png)

Now abb humne dusre read se pehle delete hi kardia toh again inconsistency aajayegi.

**Lost Update Problem \(Write-Write Conflict\):**

![](../.gitbook/assets/image%20%28214%29.png)

T1 ne value 10 se 11 kardi but then T2 ne firse change kardi \(yaha T2 ne read karke change nahi kara to a constant i.e. 50 this is blind update\) now T2 ne commit kardia jab T1 commit kar raha he it is commiting it's changes par T2 ne uske changes ko change kardia toh isolation nahi raha T1 kaa T1 kaa change nahi raha.

**Serial Schedule:** Serial running of transactions. Slow. No problems as mentioned above. Let's say there are n transactions in the schedule T1, T2, T3, ... Tn then we have n \* n-1 \* n-2 ... 1 = n! ways of designing a serial schedule.

**Non Serial Schedule:** Concurrent, Fast, May have above mentioned problems. Say we have n transactions in schedule T1, T2, T3, ... Tn with n1, n2, n3, ... nn instructios in each transaction then we have \(\(n1+n2+n3+n4+...+nn\)! / \(n1! n2! + n3! + ... nn!\)\) - n!

## Conflict Serializability in DBMS

![](../.gitbook/assets/image%20%28217%29.png)

Basically we want to check humara non-serial schedule consistent he ya nahi \(problems naa hona is consistent\). If we can swap instructions within transactions \(transaction ke andar ki relative ordering nahi that we should never change\) and swaps ke baad if it becomes serial kyuki serial is always consistent then we can say S1 is consistent as well. If it fails then we are not sure it may or may not be consistent. Like in the image we can do it. If instructions happen on different variable then it won't matter order. Or if it's read-read then no matter konsa transaction pehle read  
karta he swap can be done, but if its read-write, write-read or write-write then we cannot swap.  
[https://youtu.be/QkROSmKbVFQ?list=PLmXKhU9FNesR1rSES7oLdJaNFgmuj0SYV](https://youtu.be/QkROSmKbVFQ?list=PLmXKhU9FNesR1rSES7oLdJaNFgmuj0SYV)  
[https://youtu.be/6feqtT3e-vA?list=PLmXKhU9FNesR1rSES7oLdJaNFgmuj0SYV](https://youtu.be/6feqtT3e-vA?list=PLmXKhU9FNesR1rSES7oLdJaNFgmuj0SYV)

## View Serializability

It is weaker than conflict serializability that means if a schedule is VS then it may or may not CS but if it's CS then it's definitely VS. VS is an NP complete problem so that's why we tend to go with CS first. Agar CS nahi he but usme blind write nahi he then VS nahi hoga but agar he blind write then wo ho sakta he.

![](../.gitbook/assets/image%20%28243%29.png)

[https://youtu.be/FJteasXARxg?list=PLmXKhU9FNesR1rSES7oLdJaNFgmuj0SYV](https://youtu.be/FJteasXARxg?list=PLmXKhU9FNesR1rSES7oLdJaNFgmuj0SYV)

To check this we basically make all possible arrangements, then compare it with non-concurrent schedule like before. This time we check - initial read same, final write same, intermediate read same then we say it's view serializable.

## Recoverable Schedule

![](../.gitbook/assets/image%20%28215%29.png)

Although ye schedule consistent he par still failure kahi bhi ho sakti he say failure hui \(line\) then T1 roll back hua par T2 ne commit kardia toh inconssistency reh jaayegi it should be 5 \(initially A is 10\) but it will be 15. This is irrecoverable schedule and we don't want it. Here T2 ka R\(A\) is dirty read agar transaction me dirty read nahi he then transaction humesha recoverable hoga kyuki uss case me ek ke rollback se dusre ko farak nahi padta. Dirty read hone ke baad bhi recoverable ho sakta he agar jis order me dirty read he ussi order me commit he like

![](../.gitbook/assets/image%20%28254%29.png)

> Cascadeless Schedule [https://youtu.be/qH2iYtuJEwQ?list=PLmXKhU9FNesR1rSES7oLdJaNFgmuj0SYV](https://youtu.be/qH2iYtuJEwQ?list=PLmXKhU9FNesR1rSES7oLdJaNFgmuj0SYV) \[we don't want dirty read\]

## Strict Schedule

Dirty read kehta tha ki write ho raha he kisi pe toh bina commit ke read nahi, strict kehta he read write dono nahi. It doesn't mean serial ho pura see S3. S1 is not strict schedule. S2-S3 are

![](../.gitbook/assets/image%20%28209%29.png)

## B and B+ Trees

[https://www.youtube.com/watch?v=aZjYr87r1b8](https://www.youtube.com/watch?v=aZjYr87r1b8)

![](../.gitbook/assets/image%20%28232%29.png)

M way search tree are simmilar to BST we can say that a 2 degree M way ST is BST.

![](../.gitbook/assets/image%20%28248%29.png)

We need to modify the m way ST to use as Index. It stores now child pointer, key, and record pointer.

Now B Trees are the above tree but with rules since in above tree we can put it like this then it will have more time complexity

![](../.gitbook/assets/image%20%28260%29.png)

so we have rules while insertion in B trees

* Next node can be created if current node is atleast ceil\(m/2\) filled except root and leafs
* All leafs must be at same level
* Creation process is bottom-up

> In B+ Trees we will not have record pointer at every node, we'll just have record pointers at leaf nodes

* A B/B+ tree with order p has maximum p pointers and hence maximum p children.
* A B/B+ tree with order p has minimum ceil\(p/2\) pointers and hence minimum ceil\(p/2\) children.
* A B/B+ tree with order p has maximum \(p – 1\) and minimum ceil\(p/2\) – 1 keys.

[https://www.youtube.com/watch?v=k3ODdIez0-8](https://www.youtube.com/watch?v=k3ODdIez0-8)  
[https://www.youtube.com/watch?v=TXIqXYUT2NE](https://www.youtube.com/watch?v=TXIqXYUT2NE)  
[https://www.youtube.com/watch?v=YZECPU-3iHs](https://www.youtube.com/watch?v=YZECPU-3iHs)

## Lock based protocols

A transaction begin by acquiring a lock on the entire database, other transaction cannot acquire the lock and only after the end of first transaction the lock gets unlocked this way isolation is achieved. It becomes trivially serial. poor performance. cascedless schedule.

Modes in which data item may be locked:

* Shared: Transacation can read but cannot write ---- lock-S\(Q\)
* Exclusive: Both Read Write ----- lock-X\(Q\)
* unlock\(Q\)

Transaction proceed by making request to concruenct-control manager which grants the lock to the transaction. Agar turant grant hojaye toh it's compatible function

```text
lock-X(B)
read(B)
B = B-50
write(B)
unlock(B)
lock-X(A)
read(A)
A = A+50
write(A)
unlock(A4+-)
```

