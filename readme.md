# Database Model Design for ZenClass

This database model is designed for managing the ZenClass platform, facilitating interaction between students, mentors, and classes.

## Database Tables

1. **Students**
   - `student_id`: int, primary key, auto_increment
   - `email`: varchar(255), unique, not null
   - `password`: varchar(255), not null
   - `first_name`: varchar(255), not null
   - `last_name`: varchar(255), not null
   - `date_of_birth`: date, not null
   - `created_at`: datetime, not null
   - `updated_at`: datetime, not null

2. **Courses**
   - `course_id`: int, primary key, auto_increment
   - `course_name`: varchar(255), not null
   - `course_description`: text
   - `start_date`: date, not null
   - `end_date`: date, not null
   - `created_at`: datetime, not null
   - `updated_at`: datetime, not null

3. **Classes_Details**
   - `class_id`: int, primary key, auto_increment
   - `course_id`: int, foreign key to courses, not null
   - `class_name`: varchar(255), not null
   - `class_description`: text
   - `class_start_date`: date, not null
   - `class_end_date`: date, not null
   - `created_at`: datetime, not null
   - `updated_at`: datetime, not null

4. **Students_Classes**
   - `student_id`: int, foreign key references students(student_id), not null
   - `class_id`: int, foreign key to classes_details, not null
   - `created_at`: datetime, not null

5. **Mentors**
   - `mentor_id`: int, primary key, auto_increment
   - `mentor_name`: varchar(255), not null
   - `mentor_email`: varchar(255), not null
   - `created_at`: datetime, not null
   - `updated_at`: datetime, not null

6. **Modules**
   - `module_id`: int, primary key, auto_increment
   - `class_id`: int, foreign key to classes_details, not null
   - `module_name`: varchar(255), not null
   - `module_description`: text
   - `module_start_date`: date, not null
   - `module_end_date`: date, not null
   - `created_at`: datetime, not null
   - `updated_at`: datetime, not null

7. **Tasks**
   - `task_id`: int, primary key, auto_increment
   - `module_id`: int, foreign key to modules, not null
   - `class_id`: int, foreign key to classes_details, not null
   - `task_name`: varchar(255), not null
   - `task_description`: text
   - `task_deadline`: date
   - `created_at`: datetime, not null
   - `updated_at`: datetime, not null

8. **Attendances**
   - `attendance_id`: int, primary key, auto_increment
   - `class_id`: int, foreign key to classes_details, not null
   - `student_id`: int, foreign key references students(student_id), not null
   - `attendance_date`: date, not null
   - `status`: varchar(255), not null
   - `created_at`: datetime, not null
   - `updated_at`: datetime, not null

9. **Student_Report**
   - `report_id`: int, primary key, auto_increment
   - `student_id`: int, foreign key references students(student_id), not null
   - `class_id`: int, foreign key to classes_details, not null
   - `grade`: varchar(255)
   - `comments`: text
   - `created_at`: datetime, not null
   - `updated_at`: datetime, not null

10. **Mock_Interview**
    - `interview_id`: int, primary key, auto_increment
    - `student_id`: int, foreign key references students(student_id), not null
    - `interview_date`: date, not null
    - `interviewer_id`: int, foreign key to mentors, not null
    - `interview_notes`: text
    - `created_at`: datetime, not null
    - `updated_at`: datetime, not null

11. **Code_Kata**
    - `Codekata_id`: int, primary key, auto_increment
    - `student_id`: int, foreign key references students(student_id), not null
    - `Codekata_date`: date, not null
    - `Codekata_score`: int
    - `created_at`: datetime, not null
    - `updated_at`: datetime, not null

12. **Web_Kata**
    - `Webkata_id`: int, primary key, auto_increment
    - `student_id`: int, foreign key references students(student_id), not null
    - `Webkata_date`: date, not null
    - `Webkata_score`: int
    - `created_at`: datetime, not null
    - `updated_at`: datetime, not null

13. **Certificates**
    - `certificate_id`: int, primary key, auto_increment
    - `student_id`: int, foreign key references students(student_id), not null
    - `course_id`: int, foreign key to courses, not null
    - `created_at`: datetime, not null
    - `updated_at`: datetime, not null

## SQL Queries

```sql
-- SQL queries to create the tables

CREATE TABLE Students (
   student_id INT PRIMARY KEY AUTO_INCREMENT,
   email VARCHAR(255) UNIQUE NOT NULL,
   password VARCHAR(255) NOT NULL,
   first_name VARCHAR(255) NOT NULL,
   last_name VARCHAR(255) NOT NULL,
   date_of_birth DATE NOT NULL,
   created_at DATETIME NOT NULL,
   updated_at DATETIME NOT NULL
);

CREATE TABLE Courses (
   course_id INT PRIMARY KEY AUTO_INCREMENT,
   course_name VARCHAR(255) NOT NULL,
   course_description TEXT,
   start_date DATE NOT NULL,
   end_date DATE NOT NULL,
   created_at DATETIME NOT NULL,
   updated_at DATETIME NOT NULL
);

CREATE TABLE Classes_Details (
   class_id INT PRIMARY KEY AUTO_INCREMENT,
   course_id INT NOT NULL,
   class_name VARCHAR(255) NOT NULL,
   class_description TEXT,
   class_start_date DATE NOT NULL,
   class_end_date DATE NOT NULL,
   created_at DATETIME NOT NULL,
   updated_at DATETIME NOT NULL,
   FOREIGN KEY (course_id) REFERENCES Courses(course_id)
);

CREATE TABLE Students_Classes (
   student_id INT NOT NULL,
   class_id INT NOT NULL,
   created_at DATETIME NOT NULL,
   FOREIGN KEY (student_id) REFERENCES Students(student_id),
   FOREIGN KEY (class_id) REFERENCES Classes_Details(class_id)
);

CREATE TABLE Mentors (
   mentor_id INT PRIMARY KEY AUTO_INCREMENT,
   mentor_name VARCHAR(255) NOT NULL,
   mentor_email VARCHAR(255) NOT NULL,
   created_at DATETIME NOT NULL,
   updated_at DATETIME NOT NULL
);

CREATE TABLE Modules (
   module_id INT PRIMARY KEY AUTO_INCREMENT,
   class_id INT NOT NULL,
   module_name VARCHAR(255) NOT NULL,
   module_description TEXT,
   module_start_date DATE NOT NULL,
   module_end_date DATE NOT NULL,
   created_at DATETIME NOT NULL,
   updated_at DATETIME NOT NULL,
   FOREIGN KEY (class_id) REFERENCES Classes_Details(class_id)
);

CREATE TABLE Tasks (
   task_id INT PRIMARY KEY AUTO_INCREMENT,
   module_id INT NOT NULL,
   class_id INT NOT NULL,
   task_name VARCHAR(255) NOT NULL,
   task_description TEXT,
   task_deadline DATE,
   created_at DATETIME NOT NULL,
   updated_at DATETIME NOT NULL,
   FOREIGN KEY (module_id) REFERENCES Modules(module_id),
   FOREIGN KEY (class_id) REFERENCES Classes_Details(class_id)
);

CREATE TABLE Attendances (
   attendance_id INT PRIMARY KEY AUTO_INCREMENT,
   class_id INT NOT NULL,
   student_id INT NOT NULL,
   attendance_date DATE NOT NULL,
   status VARCHAR(255) NOT NULL,
   created_at DATETIME NOT NULL,
   updated_at DATETIME NOT NULL,
   FOREIGN KEY (class_id) REFERENCES Classes_Details(class_id),
   FOREIGN KEY (student_id) REFERENCES Students(student_id)
);

CREATE TABLE Student_Report (
   report_id INT PRIMARY KEY AUTO_INCREMENT,
   student_id INT NOT NULL,
   class_id INT NOT NULL,
   grade VARCHAR(255),
   comments TEXT,
   created_at DATETIME NOT NULL,
   updated_at DATETIME NOT NULL,
   FOREIGN KEY (student_id) REFERENCES Students(student_id),
   FOREIGN KEY (class_id) REFERENCES Classes_Details(class_id)
);

CREATE TABLE Mock_Interview (
   interview_id INT PRIMARY KEY AUTO_INCREMENT,
   student_id INT NOT NULL,
   interview_date DATE NOT NULL,
   interviewer_id INT NOT NULL,
   interview_notes TEXT,
   created_at DATETIME NOT NULL,
   updated_at DATETIME NOT NULL,
   FOREIGN KEY (student_id) REFERENCES Students(student_id),
   FOREIGN KEY (interviewer_id) REFERENCES Mentors(mentor_id)
);

CREATE TABLE Code_Kata (
   Codekata_id INT PRIMARY KEY AUTO_INCREMENT,
   student_id INT NOT NULL,
   Codekata_date DATE NOT NULL,
   Codekata_score INT,
   created_at DATETIME NOT NULL,
   updated_at DATETIME NOT NULL,
   FOREIGN KEY (student_id) REFERENCES Students(student_id)
);

CREATE TABLE Web_Kata (
   Webkata_id INT PRIMARY KEY AUTO_INCREMENT,
   student_id INT NOT NULL,
   Webkata_date DATE NOT NULL,
   Webkata_score INT,
   created_at DATETIME NOT NULL,
   updated_at DATETIME NOT NULL,
   FOREIGN KEY (student_id) REFERENCES Students(student_id)
);

CREATE TABLE Certificates (
   certificate_id INT PRIMARY KEY AUTO_INCREMENT,
   student_id INT NOT NULL,
   course_id INT NOT NULL,
   created_at DATETIME NOT NULL,
   updated_at DATETIME NOT NULL,
   FOREIGN KEY (student_id) REFERENCES Students(student_id),
   FOREIGN KEY (course_id) REFERENCES Courses(course_id)
);

CREATE TABLE Class_Feedback (
   feedback_id INT PRIMARY KEY AUTO_INCREMENT,
   class_id INT NOT NULL,
   student_id INT NOT NULL,
   mentor_id INT NOT NULL,
   feedback_text TEXT,
   feedback_date DATETIME NOT NULL,
   created_at DATETIME NOT NULL,
   updated_at DATETIME NOT NULL,
   FOREIGN KEY (class_id) REFERENCES Classes_Details(class_id),
   FOREIGN KEY (student_id) REFERENCES Students(student_id),
   FOREIGN KEY (mentor_id) REFERENCES Mentors(mentor_id)
);

