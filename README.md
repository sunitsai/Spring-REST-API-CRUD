# Spring Boot CRUD Example using MySQL Database

This project demonstrates a simple Spring Boot application that performs CRUD (Create, Read, Update, Delete) operations using a MySQL database.

## Prerequisites

Before you begin, ensure you have the following installed on your machine:

- Java Development Kit (JDK)
- An Integrated Development Environment (IDE) like IntelliJ IDEA or Eclipse
- MySQL database

## Step 1: Setting Up Your Project

1. **Install Java Development Kit (JDK)**
   - Download and install it from the [Oracle website](https://www.oracle.com/java/technologies/javase-downloads.html).

2. **Install an IDE**
   - Download and install IntelliJ IDEA, Eclipse, or another IDE of your choice.

3. **Create a New Spring Boot Project**
   - Go to [Spring Initializr](https://start.spring.io/).
   - Fill in the details:
     - **Project**: Maven
     - **Language**: Java
     - **Spring Boot**: 3.3.8
     - **Project Metadata**: 
       - **Group**: com.example
       - **Artifact**: spring-crud
     - **Packaging**: Jar
     - **Java**: 17 (or the latest version you have installed)
   - Add the following dependencies:
     - Spring Web
     - Spring Data JPA
     - MySQL Driver
   - Click "Generate" to download the project zip file. Unzip it and open it in your IDE.

## Step 2: Configure MySQL Database

1. **Install MySQL**
   - Download and install MySQL from the [MySQL website](https://dev.mysql.com/downloads/installer/).

2. **Create a Database**
   - Open MySQL Workbench or command line.
   - Create a new database with the name `school`.

   ```sql
   CREATE DATABASE school;

## Configure Spring Boot to Connect to MySQL

Open the application.properties file in your Spring Boot project and add the following lines:
properties
# Database connection properties
spring.datasource.url=jdbc:mysql://localhost:3306/school
spring.datasource.username=root
spring.datasource.password=yourpassword  # Replace 'yourpassword' with your MySQL root password
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver

# Hibernate properties
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQLDialect
Step 3: Create the Student Entity
Create a New Java Class

## In your IDE, create a new Java class named Student in the com.example.springcrud package.
Java
package com.example.springcrud;

import jakarta.persistence.Entity;
import jakarta.persistence.GeneratedValue;
import jakarta.persistence.GenerationType;
import jakarta.persistence.Id;

@Entity
public class Student {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String name;
    private int grade;

    // Getters and Setters
    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getGrade() {
        return grade;
    }

    public void setGrade(int grade) {
        this.grade = grade;
    }
}
## Step 4: Create the Repository
Create a New Interface

Create a new interface named StudentRepository in the com.example.springcrud package.
Java
package com.example.springcrud;

import org.springframework.data.jpa.repository.JpaRepository;

public interface StudentRepository extends JpaRepository<Student, Long> {
}
Step 5: Create the Controller
Create a New Java Class

## Create a new Java class named StudentController in the com.example.springcrud package.
Java
package com.example.springcrud;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;

import java.util.List;

@RestController
@RequestMapping("/students")
public class StudentController {
    @Autowired
    private StudentRepository studentRepository;

    @PostMapping
    public Student createStudent(@RequestBody Student student) {
        return studentRepository.save(student);
    }

    @GetMapping
    public List<Student> getAllStudents() {
        return studentRepository.findAll();
    }

    @GetMapping("/{id}")
    public Student getStudentById(@PathVariable Long id) {
        return studentRepository.findById(id).orElse(null);
    }

    @PutMapping("/{id}")
    public Student updateStudent(@PathVariable Long id, @RequestBody Student studentDetails) {
        Student student = studentRepository.findById(id).orElse(null);
        if (student != null) {
            student.setName(studentDetails.getName());
            student.setGrade(studentDetails.getGrade());
            return studentRepository.save(student);
        }
        return null;
    }

    @DeleteMapping("/{id}")
    public void deleteStudent(@PathVariable Long id) {
        studentRepository.deleteById(id);
    }
}
## Public code references from 6 repositories
## Step 6: Run the Application
## Run the Spring Boot Application
In your IDE, find the SpringCrudApplication class (or the class with the @SpringBootApplication annotation) and run it.
Step 7: Test the CRUD Operations
Use Postman or Curl: You can use Postman to test the API endpoints.
Create a Student:
POST request to http://localhost:8080/students
Body: { "name": "John Doe", "grade": 5 }
Get All Students:
GET request to http://localhost:8080/students
Get a Student by ID:
GET request to http://localhost:8080/students/1
Update a Student:
PUT request to http://localhost:8080/students/1
Body: { "name": "Jane Doe", "grade": 6 }
Delete a Student:
DELETE request to http://localhost:8080/students/1
