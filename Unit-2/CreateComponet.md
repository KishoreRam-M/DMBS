# 📘 ER Diagram — Components Made Simple

An **ER Diagram** is a visual representation used to design databases. It shows:

* 🧍‍♂️ **Entities** (Things)
* 🧾 **Attributes** (Details about those things)
* 🔗 **Relationships** (Connections between entities)

---

### 🔹 1. **Entity (Thing)**

* **What**: Represents something we want to store data about
* **Examples**: Student, Teacher, Book
* **Shape**: Rectangle

#### Example:

```plaintext
+-------------+
|  Student   |
+-------------+
```

---

### 🔸 2. **Attribute (Details of the Thing)**

* **What**: Properties or details of an entity
* **Examples**: Name, Age, ID Number
* **Shape**: Oval

#### Example:

```plaintext
         +---------+  
         |  Name   |  
         +---------+  
             |  
+-------------+ |
|  Student    |--
+-------------+ |
|
+---------+
| Roll No |
+---------+
```

---

#### ✅ Types of Attributes:

* **Simple**: Single, indivisible value

  * *Example*: Name, Age, Roll No
* **Composite**: Can be split into smaller, meaningful parts

  * *Example*:

    * Full Name = First Name + Last Name
    * Address = Street + City + Zip Code
* **Derived**: Calculated from other attributes

  * *Example*:

    * Age (calculated from Date of Birth)
    * Total Salary (calculated from basic salary and bonus)
* **Multivalued**: Can have more than one value for an entity

  * *Example*:

    * Phone Numbers (An employee can have multiple phone numbers)
    * Skills (A person can have multiple skills)

---

### 🔹 3. **Relationship (How Entities are Connected)**

* **What**: Shows the connection between two entities
* **Example**: A Student **ENROLLS** in a Course
* **Shape**: Diamond

#### Example:

```plaintext
+-------------+ +------------+
|  Student   |---->|  ENROLLS  |<-----+------------+
+-------------+     +------------+     |  Course    |
+------------+                         +------------+
```

---

### 🔸 4. **Primary Key**

* **What**: An attribute that uniquely identifies each record
* **Example**: Roll No (unique for each student)
* **Marked by**: Underlined

#### Example:

```plaintext
Student (Roll_No underlined)
```

---

### 🔹 5. **Weak Entity**

* **What**: An entity that depends on another entity (no unique ID on its own)
* **Example**: A **Dependent** (child) depends on **Employee**
* **Shape**: Double Rectangle

#### Example:

```plaintext
+-------------------+ +------------+
|   Dependent       |<------->| Employee |
+-------------------+ +------------+
```

---

### 🔸 6. **Cardinality (How Many Are Related?)**

* **What**: Describes how many entities are related to each other
* **Types**:

  * **1:1** → One student has one ID card
  * **1\:N** → One teacher teaches many students
  * **M\:N** → Many students enroll in many courses

---

### 📝 **Summary Table**

| **Component**    | **Symbol/Shape** | **Example**               |
| ---------------- | ---------------- | ------------------------- |
| **Entity**       | Rectangle        | Student, Teacher          |
| **Attribute**    | Oval             | Name, Age, Roll No        |
| **Relationship** | Diamond          | ENROLLS, TEACHES          |
| **Primary Key**  | Underlined       | Roll No                   |
| **Weak Entity**  | Double Rectangle | Dependent (child)         |
| **Cardinality**  | 1:1, 1\:N, M\:N  | Student-Course connection |
