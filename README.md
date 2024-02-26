# Student Assignment App

- Postman Collection Link: https://api.postman.com/collections/25423999-1ba01caf-f0d7-4515-9cd0-e68197910cc1?access_key=PMAT-01HQGSP9W57CMD36C9W1H7JDNA

## Overview
Welcome to the  Student Assignment App! This application serves as an Assignment submission, where teacher are allowed to login and create assignments and then the students submit these assignments and are graded by the teacher who created that partical assignment.

### Assumptions while Solving the Problem Statement

1. **Assignment submission**
   - As soon as a student submits the assignment, the student is graded on submitting.
   - Student is not allowed to submit the same assignment twice.

2. **Login**
  - Multiple teachers can login and create as many assignments as they wish.
  - Students don not require to login to view the assignments.

### Essential Features

1. **Teacher Login and Sign Up:**
   - Teacher can login and sign up.
   - As Teacher Logs in in they are provided with the JWT Authentication token.
  
2. **Assignment Creation**
   - When an assignment is created all the students recieve emails os the assignment.
   - Used Sendgrid API for emailing the students.
  
3. **Assignment Submission**
   - Students can submit any assignment they want.
   - Students are graded by the respective teachers on suibmitting the assignment.



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
- Email Service: ![Senggrid Api](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTa1gIr8JEsK497znTLE4Gj8O7-zjGzjJ8hx-Vmyl7_JAsMIdjddcFKjR16lI0FJegXiUI&usqp=CAU)
- Running Queries- ![Sequelize](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSteA6g8hsh_hjZN28tUcRJRsxY-Xi5_QwS9g&usqp=CAU)

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
- **score**:integer
- **teacherId**: foreign key (Teacher),integer

##### Grade Table
- **_id**:primary key, integer
- **score**: integer
- **studentId**: foreign key(Student),integer
- **assignmentId**: foreign key(Assignment),integer

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
    "token": "example_token",
     "teacher": { "example_id", "example_name", "exmaple_email" }
  }
  ```
  - **Error**: Status Code 402
  ```json
  {
    "error": "Invalid Email or password"
  }
  ```

#### 4.   Create Assignment

- **Endpoint**: `/createAssign`
- **Method**: `POST`
- **Description**: Validates teacher credentials for creating the assignment and sending email to each and every student.
- **Request Body**:
  ```json
  {
    "title": "example_title",
    "description": "example_description",
     "dueDate":"example_duedate",
     "score":"exmaple_score",
     "teacherId":"exapmle_teacherId"
  }
  ```
- **Response**:
  - **Success**: Status Code 200
  ```json
  {
    "message": "Assignment Created Successfully"
  }
  ```
  - **Error**: Status Code 402
  ```json
  {
    "error": "error message"
  }
  ```

#### 5.   Update Assignment

- **Endpoint**: `/updateAssign/:assignid`
- **Method**: `POST`
- **Description**: Validates teacher credentials for updateing the assignment.
- **Request Body**:
  ```json
  {
    "title": "example_title",
    "description": "example_description",
     "dueDate":"example_duedate"
     "score":"example_score"
  }
  ```
- **Response**:
  - **Success**: Status Code 200
  ```json
  {
    "message": "updated Successfully"
  }
  ```
  - **Error**: Status Code 402
  ```json
  {
    "error": "error meesage"
  }
  ```

#### 6.   Delete Assignment

- **Endpoint**: `/deleteAssign/:assignid`
- **Method**: `Delete`
- **Description**: Validates teacher credentials for deleteing the assignment.
- **Response**:
  - **Success**: Status Code 200
  ```json
  {
    "message": "Deleted Successfully"
  }
  ```
  - **Error**: Status Code 402
  ```json
  {
    "error": "This assignment cannot be deleted as it is created by some other Teacher"
  }
  ```

### 7. Submit Assignment

- **Endpoint**: `/allAssignment/submitAssign/:assignid`
- **Method**: `POST`
- **Description**: Helps Student to submit the assignment.
- **Request Body**:
  ```json
  {
    "name": "example_name",
    "email": "example_email",
  }
  ```
- **Response**:
  - **Success**: Status Code 200
  ```json
  {
    "message": "Assignment submitted and marks were assigned"
  }
  ```
  - **Error**: Status Code 402
  ```json
  {
    "error": "error meesage"
  }
  ```

#### 6.   view Student Report

- **Endpoint**: `/studentReport/:studentid`
- **Method**: `GET`
- **Description**: All reports of a particular student are given.
- **Response**:
  - **Success**: Status Code 200
  ```json
  {
    "student_reports": {
        "result": [
              {
                 "_id": "example_id",
                 "score": "exmpale_score",
                 "studentId": "example_studentId",
                 "assignmentId": "example_assignmentId"
              }
        ]
     }
  }
  ```
  - **Error**: Status Code 402
  ```json
  {
    "error": "Error: Duplicate entry 'x-y' for key 'grades.grades_studentId_assignmentId_unique'"
  }
  ```

## Containerization

### Docker Compose

Docker Compose is used to orchestrate the deployment of backend components. Ensure you have Docker and Docker Compose installed on your system.

#### Configuration

Create an `.env` file in the root directory and include the following environment variables:
- DB_HOST=localhost
- DB_USER=root
- DB_PASSWORD=rootpassword
- DB_NAME=node-complete1
- DB_PORT=3306
- SERVER_PORT=8000

#### Building and Running

Use the following commands to build and run the Docker containers:


```bash
# Build and run containers
docker-compose up --build

# To run in detached mode
docker-compose up --build -d
```

- Pull the images from docker hub https://hub.docker.com/repositories/rishijain27
- repositories are test and mysql






  

  

   
