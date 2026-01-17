# Student Management System (SMS) Database  
### SQL Database Project

## Project Overview
This project focuses on the design and implementation of a relational database for a Student Management System (SMS) using SQL. The database efficiently stores and manages information related to students, courses, departments, instructors and enrollments.

It supports core administrative operations such as student registration, course assignment, and the generation of dynamic reports through SQL queries.

The project demonstrates the practical use of SQL Data Definition Language (DDL) and Data Manipulation Language (DML), including the creation of tables with appropriate constraints, as well as the use of complex queries and JOIN operations to retrieve meaningful insights from the database.

## 📊 Project Highlights

| Metric | Value |
|--------|-------|
| **Students** | 100 |
| **Courses** | 18 across 6 departments |
| **Enrollments** | 309 total registrations |
| **Instructors** | 18 faculty members |
| **Avg Courses/Student** | 3.09 |
| **Database Size** | 5 normalized tables |

## Database Features
- Student and course data management
- **Data Validation** - CHECK constraints for gender values
- **Third Normal Form (3NF)** - Eliminated data redundancy
- Enrollment tracking
- Data integrity enforcement
- Report generation using SQL queries

## Entity Relationship Diagram (ERD)

![Student Management System (SMS) Database](documentation/erd.png)

The Entity Relationship Diagram (ERD) illustrates the logical structure of the
Student Management System database, showing entities, attributes, and the
relationships between students, courses, departments, instructors and enrollments.

## Tables and Relationships
### Students
- **Columns**: StudentID(PK), Name, Gender, DOB, DepartmentID(FK)
- **Description**: Stores information about each student.
- **Relationships**:
  - One-to-Many with Enrollments (one student can have multiple enrollments).
  - Many-to-One with Departments (Many students can belong to one department)
  
### Courses
- **Columns**: CourseID(PK), CourseName, DepartmentID(FK)
- **Description**: contains all available courses
- **Relationships**:
  -  One-to-Many with Enrollments (one course can have multiple enrollments)
  -  Many-to-One with Departments (multiple courses can be from the same department)
  -  One-to-One with Instructors (one instructor is assigned to a course)

### Departments
- **Columns**: DepartmentID(PK), DepartmentName
- **Description:** Stores all departments in the institution.
- **Relationships:**
  - One-to-Many with Courses (one department can have many courses)
  - One-to-Many with Instructors (one department can have many instructors)
  - One-to-Many with Students (one department can have multiple students) 

### Instructors
- **Columns**: InstructorID(PK), InstructorName, Gender, DepartmentID(FK), CourseID, HireDate, Email
- **Description**: Stores all instructors information
- **Relationships**:
  - Many-to-One with Departments (many instructors belong to one department)
  - One-to-One with Courses (each course is assigned to an instructor)
 
### Enrollments
- **Columns**: EnrollmentID(PK), StudentID(FK), CourseID(FK), EnrollmentDate
- **Description:** Tracks which students are enrolled in which courses.
- **Relationships:**
  - Many-to-One with Students
  - Many-to-One with Courses
  - Implements Many-to-Many relationship between Students and Courses

## SQL Techniques Demonstrated
- Complex multi-table JOINs (INNER, LEFT)
- Aggregate functions with GROUP BY/HAVING
- Subqueries for filtering
- CASE WHEN for conditional logic
- Window functions (COUNT DISTINCT)

## Sample SQL Queries
### 1️⃣Enrollment analysis
**Business Question**: Which courses have the highest number of enrollments?
```sql
SELECT c.CourseName,c.CourseID, COUNT(e.StudentID) AS EnrolledStudents
FROM Courses c
LEFT JOIN Enrollments e ON c.CourseID = e.CourseID
GROUP BY c.CourseName, c.CourseID
ORDER BY EnrolledStudents DESC;
```
**📈 Key Finding:** Fluid Mechanics (Course 302) leads with 20 enrollments, indicating high demand for mechanical engineering courses.

### 2️⃣Student Workload Analysis
**Business Question**: Which students are enrolled in multiple courses, and which courses are they taking?
```sql
SELECT s.Name,s.StudentID, COUNT(e.CourseID) AS CourseCount, GROUP_CONCAT(c.CourseName) AS Courses
FROM Students s
JOIN Enrollments e ON s.StudentID = e.StudentID
JOIN courses c on e.CourseID = c.CourseID
GROUP BY s.Name, s.StudentID
HAVING COUNT(e.CourseID) > 1;
```
**📈 Key Finding:** 97 out of 100 students enrolled in multiple courses, with maximum of 4 courses per student, showing healthy academic engagement.

### 3️⃣Department Distribution
**Business Question**: What is the total number of students per department across all courses?
```sql
SELECT d.DepartmentName,
COUNT(DISTINCT e.StudentID) AS No_Of_Students
FROM departments d
LEFT JOIN courses c ON d.DepartmentID = c.DepartmentID
LEFT JOIN enrollments e ON c.CourseID = e.CourseID
GROUP BY d.DepartmentName
ORDER BY No_Of_Students DESC;
```
**📈 Key Finding:** Computer Science and Biological Sciences are the most popular departments, each attracting students from multiple programs.

## Tools Used
- MySQL / SQL Server (for database creation and queries)
- MySQL Workbench (for ERD diagram)
- dbdiagram.io

## Folder Structure
```
student-management-system/
├── README.md                  # Project documentation
├── database/                  # SQL database scripts
│   ├── schema/                # Scripts to create tables
│   │   ├── 1_create_departments.sql
│   │   ├── 2_create_courses.sql
│   │   ├── 3_create_students.sql
│   │   ├── 4_create_instructors.sql
│   │   └── 5_create_enrollments.sql
│   ├── data/                  # Scripts to populate tables with sample data
│   │   ├── 6_fill_departments.sql
│   │   ├── 7_fill_students.sql
│   │   ├── 8_fill_instructors.sql
│   │   ├── 9_fill_courses.sql
│   │   └── 10_fill_enrollments.sql
│   └── queries/               # Pre-written reporting queries
│       └── reporting_queries.sql
├── documentation/             # Project documentation and visuals
│   ├── erd.png
│   └── dataset.xlsx
└── setup.sql                  # Master script to set up the entire database
```

## Author
Wojuade Oluwajuwonlo Enoch.

## 👨‍💻 About This Project

This project was developed as part of **Team WiDa** during my SQL mentorship program, where I focused on:
- Designing the normalized database schema (3NF)
- Writing complex analytical queries with JOINs and aggregations
- Ensuring data integrity through constraints and foreign keys
- Creating comprehensive documentation and ERD diagrams

### Key Challenges Solved
✅ Implemented many-to-many relationships using junction table  
✅ Maintained referential integrity across 309 enrollments  
✅ Optimized queries for performance with proper indexing on foreign keys  
✅ Handled data validation with CHECK constraints  


## 📧 Connect With Me

**Wojuade Oluwajuwonlo Enoch**

[![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/oluwajuwonlo-wojuade-b2188023a/)
[![GitHub](https://img.shields.io/badge/GitHub-100000?style=for-the-badge&logo=github&logoColor=white)](https://github.com/dev-rage)
[![Email](https://img.shields.io/badge/Email-D14836?style=for-the-badge&logo=gmail&logoColor=white)](mailto:wojuadejuwon@gmail.com)

---

## 📄 License

This project is open source and available under the [MIT License](LICENSE).

---

**⭐ If you found this project useful, please give it a star! It helps others discover this work.**

---

### Team WiDa Contributors
- Bashir Aminat
- Lateefah Abdul-Rahman  
- Hawwa Atta
- Jimoh Abdul-Rahman
- Damilare
- Oluwajuwonlo Wojuade
- Toyyib A. Olalekan
