# Student Assignment App

## Overview
Welcome to the  Student Assignment App! This application serves as an Assignment submission, where teacher are allowed to login and create assignments and then the students submit these assignments and are graded by the teacher who created that partical assignment.

### Assumptions while Solving the Problem Statement

1. **Assignment submission**
   - As soon as a student submits the assignment, the student is graded on submitting.

2. **Login**
  - Multiple teachers can login and create as many assignments as they wish.
  - Students don not require to login to view the assignments.

### Essential Features

1. **Teacher Login and Sign Up:**
   - Teacher can login and sign up.
   - As Teacher Logs in in they are provided with the JWT Authentication token.
  
2. **Assignment Submission**
   - Students can submit any assignment they want
   - Students are graded by the respective teachers.

## Approach

### Backend:

#### Requirements Analysis:

The first step involved a comprehensive analysis of the requirements:

1. **Unique Identifier (Email):** As enrollment is a key aspect, a unique identifier (email) was deemed essential.
   
2. **Authentication and Login Functionality:** As soon as a teacher is logged in they are provided with a JWT Token for authentication.

3. **Due Date for Assignment:** to sort the assignment based in due date, a Due Date was stored for each assignment created.

4. **Email Services:** For telling student about assignment creation, emails were sent to all the students so that they know that an assignment was created.

## Technologies Used

- Backend: ![Node](https://hub.docker.com/api/media/repos_logo/v1/library%2Fnode)
- Containerization: ![Docker](https://img.shields.io/badge/docker-%230db7ed.svg?style=for-the-badge&logo=docker&logoColor=white)
- Database: ![MySQL](https://hub.docker.com/api/media/repos_logo/v1/library%2Fmysql)

## Project Structure

The project is structured backend components, with each having its own directory.The backend is implemented in Node with a MySQL database. Both components are containerized using Docker, and Docker Compose.

## Backend

### Overview

The backend serves as the API server for the Student Assignment App. It handles API calls initiated from the Postman, managing the application's database. Built using Node, the backend provides a RESTful API, and the database is powered by MySQL. Associations are employed with help of Sequelize to interact seamlessly with the MySQL database, ensuring efficient data management and retrieval.

### Database Design

#### Tables

##### Teacher Table
- **_id**:primary key, integer
- **name**: string
- **email**: string
- **password**: text

##### Student Table
- **_id**:primary key, integer
- **name**: string
- **email**: string
- **password**: text

##### Assignment Table
- **_id**:primary key, integer
- **title**: string
- **description**: string
- **dueDate**: text
- **teacherId**: foreign key, (Teacher),integer

##### Grade Table
- **_id**:primary key, integer
- **score**: integer
- **studentId**: foreign key(Student),integer
- **assignmentId**: foreign key(assignment),integer

  #### Associations

- **Teacher -> Assignment**: One-to-Many relationship teacherId
- **Student -> Asssignment**: Many-to-Many relationship with a junction table Grade

  ### API Endpoints

#### 1. All Assignment

- **Endpoint**: `/allAssignment`
- **Method**: `GET`
- **Description**:  Shows all the assignment created.

#### 2. Create Teacher

- **Endpoint**: `/teacherSignup`
- **Method**: `POST`
- **Description**: Creates a new teacher for the App.
- **Request Body**:
  ```json
  {
    "name": "example_name",
    "email": "example_email",
    "password": "example_password",
  }
  ```
- **Response**:
  - **Success**: Status Code 200
  ```json
  {
    "message": "Teacher created successfully"
  }
  ```
  - **Error**: Status Code 400 or 500
  ```json
  {
    "error": "Error message"
  }
  ```

#### 3. Teacher Login

- **Endpoint**: `/teacherSignin`
- **Method**: `POST`
- **Description**: Validates teacher credentials for login.
- **Request Body**:
  ```json
  {
    "email": "example_email",
    "password": "example_password"
  }
  ```
- **Response**:
  - **Success**: Status Code 200
  ```json
  {
    "message#### 3. Teacher Login

- **Endpoint**: `/teacherSignin`
- **Method**: `POST`
- **Description**: Validates teacher credentials for login.
- **Request Body**:
  ```json
  {
    "email": "example_email",
    "password": "example_password"
  }
  ```
- **Response**:
  - **Success**: Status Code 200
  ```json
  {
    "message": "Login successful"
  }
  ```
  - **Error**: Status Code 401
  ```json
  {
    "error": "Invalid Email or password"
  }
  ```: "Login successful"
  }
  ```
  - **Error**: Status Code 401
  ```json
  {
    "error": "Invalid Email or password"
  }
  ```

  

  

   
