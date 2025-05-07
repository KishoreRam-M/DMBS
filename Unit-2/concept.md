### 1. **Differentiate Primary Key and Foreign Key**

**What:**

* **Primary Key**: Uniquely identifies each record in a table.
* **Foreign Key**: Refers to the primary key in another table.

**Why:**

* To maintain **uniqueness** (Primary) and **relationships** (Foreign) between tables.

**When:**

* Use Primary Key when defining a unique identifier.
* Use Foreign Key when connecting two related tables.

**Where:**

* Primary Key: In the main table.
* Foreign Key: In a related/child table.

**How:**

* Primary Key: `PRIMARY KEY (id)`
* Foreign Key: `FOREIGN KEY (dept_id) REFERENCES Department(id)`

**Example:**

```sql
CREATE TABLE Department (
  dept_id INT PRIMARY KEY,
  dept_name VARCHAR(50)
);

CREATE TABLE Employee (
  emp_id INT PRIMARY KEY,
  emp_name VARCHAR(50),
  dept_id INT,
  FOREIGN KEY (dept_id) REFERENCES Department(dept_id)
);
```

---

### 2. **Compare Strong Entity Set and Weak Entity Set**

| Feature          | Strong Entity Set         | Weak Entity Set                            |
| ---------------- | ------------------------- | ------------------------------------------ |
| **Definition**   | Exists independently      | Depends on another entity                  |
| **Key**          | Has a primary key         | No primary key (uses partial key)          |
| **Example**      | `Student(student_id)`     | `FeePayment(receipt_no)` linked to Student |
| **Relationship** | Not necessarily dependent | Always uses identifying relationship       |

**Diagram:**

```
Strong Entity (Rectangle) —————— (Double Diamond) —————— Weak Entity (Double Rectangle)
```

---

### 3. **ER Diagram Notations Summary**

**What:**
ER (Entity-Relationship) diagram visually represents entities and relationships.

**Notations:**

| Symbol           | Meaning               |
| ---------------- | --------------------- |
| Rectangle        | Entity                |
| Ellipse          | Attribute             |
| Diamond          | Relationship          |
| Double Ellipse   | Multivalued attribute |
| Dashed Ellipse   | Derived attribute     |
| Double Rectangle | Weak Entity           |
| Underlined Attr. | Primary Key           |

**Diagram:**

```
[Student] —(Enrolled)— [Course]
 |                         |
 name                  course_name
```

---

### 4. **Constraints on Ternary (or Higher-Degree) Relationships**

**What:**
A **ternary relationship** involves 3 entities (or more).

**Why:**
To model real-world scenarios where more than 2 entities participate in a relationship.

**Constraints:**

* **Participation Constraint**: Whether an entity’s participation is total or partial.
* **Cardinality Constraint**: How many instances of one entity relate to others.

**Example:**
Project - Employee - Role

```
(Employee) — uses — (Tool) — for — (Project)
```

---

### 5. **ER Diagram for College, Organization, and Student**

**Diagram:**

```
[College]----offers----[Organization]
     |
  enrolls
     |
[Student]
```

**Attributes:**

* Student: name, reg\_no
* College: name, address
* Organization: org\_name, type

---

### 6. **Types of Constraints in DBMS**

1. **Domain Constraint** – Valid set of values for an attribute.
2. **Key Constraint** – Uniqueness of a record.
3. **Entity Integrity Constraint** – Primary key cannot be NULL.
4. **Referential Integrity Constraint** – Foreign key must refer to an existing primary key.
5. **Check Constraint** – Condition must be true for all rows.

---

### 7. **SQL Queries for Given Schema**

**Schema:**
Customer (Cust\_No, Sales\_Person\_No, City)
Sales\_Person (Sales\_Person\_No, Sales\_Person\_Name, Common\_Prec, Year\_of\_Hire)

(i) `SELECT * FROM Customer;`
(ii) `SELECT Cust_No, City FROM Customer;`
(iii)

```sql
SELECT Sales_Person_Name 
FROM Sales_Person SP 
JOIN Customer C ON SP.Sales_Person_No = C.Sales_Person_No 
WHERE City = 'Delhi';
```

(iv) *(Same as above)*

---

### 8. **TCL for Student Database**

**TCL** – Transaction Control Language

```sql
BEGIN;

CREATE TABLE Student (
  Stud_Name VARCHAR(50),
  Dept VARCHAR(50),
  RegNo INT PRIMARY KEY
);

COMMIT;
```

---

### 9. **DDL Commands for Student & Course Table**

```sql
CREATE TABLE Student (
  name VARCHAR(50),
  id INT PRIMARY KEY,
  DOB DATE,
  branch VARCHAR(50),
  DOJ DATE
);

CREATE TABLE Course (
  course_name VARCHAR(50),
  courseid INT PRIMARY KEY,
  id INT,
  faculty_name VARCHAR(50)
);
```

---

### 10. **Create, Insert, Display Table - Student**

```sql
CREATE TABLE Student (
  name VARCHAR(50),
  id INT,
  age INT
);

INSERT INTO Student VALUES ('Kishore', 1, 20);
INSERT INTO Student VALUES ('Ram', 2, 21);

SELECT * FROM Student;
```

---

### 11. **SQL Aggregate Queries**

```sql
SELECT MIN(salary) FROM Employee;
SELECT MAX(salary) FROM Employee;
SELECT SUM(salary) FROM Employee;
SELECT COUNT(*) FROM Employee;
```

---

### 12. **Rules for First Normal Form (1NF)**

1. Only **atomic (indivisible)** values in each cell.
2. No **repeating groups or arrays**.
3. Each column must have **unique name**.
4. The order of rows/columns doesn't matter.

**Example (Not 1NF):**

| Name | Subjects      |
| ---- | ------------- |
| Raj  | Math, Science |

**In 1NF:**

| Name | Subject |
| ---- | ------- |
| Raj  | Math    |
| Raj  | Science |

---

### 13. **3NF Conversion of R(ABCDE), FD={AB→C, B→D, D→E}**

**Step 1: Identify keys**

* AB → C
* B → D
* D → E
* Candidate Key: AB

**Decompose:**

1. R1(B → D) → R1(B, D)
2. R2(D → E) → R2(D, E)
3. R3(AB → C) → R3(A, B, C)

All FDs now have candidate keys. So, it's in **3NF**.

---

### 14. **Convert to PJNF (Project-Join Normal Form)**

**Given Table:**

| Agent | Company | Product |
| ----- | ------- | ------- |
| Smith | Ford    | Car     |
| Smith | Ford    | Truck   |
| Smith | GM      | Car     |
| Smith | GM      | Truck   |
| Jones | Ford    | Car     |

**Decomposition:**

* Agent\_Company(Agent, Company)
* Agent\_Product(Agent, Product)
* Company\_Product(Company, Product)

Rejoin gives the original table → now in **PJNF**.

---

### 15. **Three Anomalies in Non-Normalized Relations**

1. **Insertion Anomaly** – Can’t insert data without other data.
   *E.g., Can’t add a course without a student.*

2. **Update Anomaly** – Update in one place, but not others → inconsistency.
   *E.g., Changing department name in one row, not others.*

3. **Deletion Anomaly** – Deleting a record loses important info.
   *E.g., Deleting a student removes course data.*
