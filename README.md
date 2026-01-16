# Student Management System (SMS) Database  
### SQL Database Project

## Project Overview
This project focuses on the design and implementation of a relational database for a Student Management System (SMS) using SQL. The database efficiently stores and manages information related to students, courses, instructors, and enrollments.

It supports core administrative operations such as student registration, course assignment, and the generation of dynamic reports through SQL queries.

The project demonstrates the practical use of SQL Data Definition Language (DDL) and Data Manipulation Language (DML), including the creation of tables with appropriate constraints, as well as the use of complex queries and JOIN operations to retrieve meaningful insights from the database.

## Database Features
- Student and course data management
- Enrollment tracking
- Data integrity enforcement
- Report generation using SQL queries

## Entity Relationship Diagram (ERD)

![Student Management System (SMS) Database](/documentation/erd.png)

The Entity Relationship Diagram (ERD) illustrates the logical structure of the
Student Management System database, showing entities, attributes, and the
relationships between students, courses, departments, instructors and enrollments.

## Tables and Relationships
### Students
- **Columns**: studentID(PK), Name, Gender, DOB, DepartmentID
- **Description**: Stores information about each student.
- **Relationships**:
  - One-to-Many with enrollments (one student can have multiple enrollments).
  
### Courses
- **Columns**: CourseID(PK), CourseName, DepartmentID(FK)
- **Description**: contains all available course
- **Relationships:
  -  One-to-Many with enrollments (one course can have multiple enrollments)
  -  Many-to-One with departments (multiple courses can be from the same department)

### Departments
- **Columns**: DepartmentID(PK), DepartmentName
- **Description:** Stores all departments in the institution.
- **Relationships:**
  - One-to-Many with Courses (one department can have many courses)
  - One-to-Many with Instructors (one department can have many instructors)

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

## Sample SQL Queries
### How many students are currently enrolled in each course?
```sql
SELECT c.CourseName,c.CourseID, COUNT(e.StudentID) AS EnrolledStudents
FROM Courses c
LEFT JOIN Enrollments e ON c.CourseID = e.CourseID
GROUP BY c.CourseName, c.CourseID
ORDER BY EnrolledStudents DESC;
```

### Which courses have the highest number of enrollments?
```sql
SELECT TOP 5 WITH TIES c.CourseName,c.CourseID, COUNT(e.StudentID) AS EnrollmentCount
FROM Courses c
JOIN Enrollments e ON c.CourseID = e.CourseID
GROUP BY c.CourseName,c.CourseID
ORDER BY EnrollmentCount DESC;
```

### Which students are enrolled in multiple courses, and which courses are they taking?
```sql
SELECT s.Name,s.StudentID, COUNT(e.CourseID) AS CourseCount, GROUP_CONCAT(c.CourseName) AS Courses
FROM Students s
JOIN Enrollments e ON s.StudentID = e.StudentID
JOIN courses c on e.CourseID = c.CourseID
GROUP BY s.Name, s.StudentID
HAVING COUNT(e.CourseID) > 1;
```
### What is the total number of students per department across all courses?
```sql
SELECT d.DepartmentName,
COUNT(DISTINCT e.StudentId) AS No_Of_Students
FROM departments d
LEFT JOIN courses c ON d.DepartmentID = c.DepartmentId
LEFT JOIN enrollments e ON c.CourseId = e.CourseId
GROUP BY d.DepartmentName
ORDER BY No_Of_Students DESC;
```
