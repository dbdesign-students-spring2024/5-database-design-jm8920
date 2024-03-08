# Data Normalization and Entity-Relationship Diagramming

An assignment to normalize the structure of data and establish a set of Entity-Relationship Diagrams for the data.

## Original 1NF Data Set
Here is a few sample rows of the original table:

| assignment_id | student_id | due_date | professor | assignment_topic                | classroom | grade | relevant_reading    | professor_email   |
| :------------ | :--------- | :------- | :-------- | :------------------------------ | :-------- | :---- | :------------------ | :---------------- |
| 1             | 1          | 23.02.21 | Melvin    | Data normalization              | WWH 101   | 80    | Deumlich Chapter 3  | l.melvin@foo.edu  |
| 2             | 7          | 18.11.21 | Logston   | Single table queries            | 60FA 314  | 25    | Dümmlers Chapter 11 | e.logston@foo.edu |
| 1             | 4          | 23.02.21 | Melvin    | Data normalization              | WWH 101   | 75    | Deumlich Chapter 3  | l.melvin@foo.edu  |
| 5             | 2          | 05.05.21 | Logston   | Python and pandas               | 60FA 314  | 92    | Dümmlers Chapter 14 | e.logston@foo.edu |
| 4             | 2          | 04.07.21 | Nevarez   | Spreadsheet aggregate functions | WWH 201   | 65    | Zehnder Page 87     | i.nevarez@foo.edu |
| ...           | ...        | ...      | ...       | ...                             | ...       | ...   | ...                 | ...               |

Here is a new table with additional fields: course_name & course_id
| assignment_id | student_id | due_date | professor | assignment_topic                | classroom | grade | relevant_reading    | professor_email   | course_id | course_name                      |
|---------------|------------|----------|-----------|---------------------------------|-----------|-------|---------------------|-------------------|-----------|----------------------------------|
| 1             | 1          | 23.02.21 | Melvin    | Data normalization              | WWH 101   | 80    | Deumlich Chapter 3  | l.melvin@foo.edu  | CS61      | Database Design & Implementation |
| 2             | 7          | 18.11.21 | Logston   | Single table queries            | 60FA 314  | 25    | Dümmlers Chapter 11 | e.logston@foo.edu | CS102     | Data Structure                   |
| 1             | 4          | 23.02.21 | Melvin    | Data normalization              | WWH 101   | 75    | Deumlich Chapter 3  | l.melvin@foo.edu  | CS61      | Database Design & Implementation |
| 5             | 2          | 05.05.21 | Logston   | Python and pandas               | 60FA 314  | 92    | Dümmlers Chapter 14 | e.logston@foo.edu | CS001     | Intro to Web Programming         |
| 4             | 2          | 04.07.21 | Nevarez   | Spreadsheet aggregate functions | WWH 201   | 65    | Zehnder Page 87     | i.nevarez@foo.edu | CS102     | Data Structure                   |
| ...           | ...        | ...      | ...       | ...                             | ...       | ...   | ...                 | ...               |           | ...                              |

## Description
1. The original dataset is not compliant with 4NF because there are multivalued dependencies. For example, a single assignment_id is linked to multiple student_id, due_date, grade, and potentially relevant_reading. 
2. These dependencies suggest that the dataset can contain redundant information, such as the same assignment_topic, classroom, professor, and professor_email are repeated for different students.
3. To solve the problem, we need to split the data up into multiple tables to fulfill the requirments of 4NF.

## Tables after Converting to 4NF
### Assignment Table
The Primary Key is `assignment_id`.
| assignment_id | course_id | due_date | assignment_topic                | relevant_reading    |
|---------------|-----------|----------|---------------------------------|---------------------|
| 1             | CS61      | 23.02.21 | Data normalization              | Deumlich Chapter 3  |
| 2             | CS102     | 18.11.21 | Single table queries            | Dümmlers Chapter 11 |
| 5             | CS001     | 05.05.21 | Python and pandas               | Dümmlers Chapter 14 |
| 4             | CS102     | 04.07.21 | Spreadsheet aggregate functions | Zehnder Page 87     |

### Professor Table
The primary key is `professor_email`, assuming that each professor has one unique university email.
| professor_id | professor_name  | professor_email        |
|--------------|-----------------|------------------------|
| 1            | Melvin          | l.melvin@foo.edu       |
| 2            | Logston         | e.logston@foo.edu      |
| 3            | Nevarez         | i.nevarez@foo.edu      |

### Student Table
The primary key is `student_id`.
| student_id |
| :----------|
| 1          |
| 2          |
| 3          |
| ...        |

### Grade Table
The primary key is `student_id` and `assignment_id`.
| assignment_id | student_id | grade  |
|---------------|------------|--------|
| 1             | 1          | 80     |
| 2             | 7          | 25     |
| 1             | 4          | 75     |
| 5             | 2          | 92     |
| 4             | 2          | 65     |

### Courses Table
The primary key is `course_id`.
| course_id | course_name                      |
|-----------|----------------------------------|
| CS61      | Database Design & Implementation |
| CS102     | Data Structure                   |
| CS001     | Intro to Web Programming         | 
|   ...     |   ...                            |   

### Section Table
The primary key is `section_id`.
| section_id | professor_id | classroom |
|------------|--------------|-----------|
| 1          | 1            | WWH 101   |
| 2          | 2            | 60FA 314  |
| 3          | 3            | WWH 201   |

## Draw an Entity-Relationship Diagram
![ER Diagram](./images/ERD.drawio.svg)

## Description of changes and the diagram
I firstly add additional fields course_name & course_id, which I think make sense to this data set.
In order to eliminate redundancy and ensure each table describes only one thing, I decomposed the data into several related tables: 
+ Assignment Table: The Primary Key is assignment_id. This table stores details about each assignment, including the courses, due date, the topic of the assignment, and any relevant readings.  
+ Professor Table: The Primary Key is professor_email. It contains their email addresses and professor names, and a generated professor_id for easier referencing in other tables.
+ Student Table: The Primary Key is student_id. This table can be extended to include more information, such as name, email, or program of study, if needed.
+ Grade Table: The primary key is student_id and assignment_id. This table links students to their assignments and records the grades they received.
+ Courses Table: The Primary Key is course_id. It holds details about courses, including course_id and the course name. 
+ Section Table: The Primary Key is section_id. This table includes the details of courses, including professor_id and classroom. 

Each table now focuses on a single aspect of the educational process (assignments, professors, students, grades, courses, and sections), and ensure that changes in one area do not require multiple updates across unrelated records. 