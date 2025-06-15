# mongodb-eduhub-project
The EduHub MongoDB Project is a hands-on data management and analysis solution for an educational platform using MongoDB and PyMongo. It handles various collections such as user registration, course creation, lesson tracking, student enrollments, assignment submissions, analytics, and schema validations.
This project includes well-structured queries, optimization strategies, performance benchmarks, and data integrity validations to provide a scalable and efficient backend for an e-learning platform..

---
##  Project Setup Instructions
### Clone the repository  
- git clone https://github.com/skapichy/eduhub_mongodb_project.git

### Install Dependencies 
- Ensure Python is installed. Then install required packages:
- pip install pymongo notebook

### Import libraries.
- from pymongo import MongoClient
- from datetime import datetime, timedelta
- import matplotlib.pyplot as plt
- import seaborn as sns
- from faker import Faker
- import pandas as pd
- import random

### Tech Stack
- MongoDB
- Python (with pymongo)
- Faker – for generating synthetic user and course data
- Pandas, Matplotlib, Seaborn – for data analysis and visualization
- Jupyter Notebook – for interactive documentation and code execution

### Start MongoDB Server
- Ensure MongoDB is running locally or update your connection string in the .py and .ipynb files: 
- client = MongoClient("mongodb://localhost:27017/")

### Run the Interactive Notebook
- jupyter notebook eduhub_mongodb_project.ipynb

### Run Python File (for backup queries)
- python eduhub_queries.py

---
## Connection and Database Setup
### Connection to MongoClient
- client = MongoClient("mongodb://localhost:27017/")
- db = client["eduhub_dbs"]

### Creation of collections to store Data
- users_collection = db["users"]
- courses_collection = db["courses"]
- enrollments_collection = db["enrollments"]
- lessons_collection = db["lessons"]
- assignments_collection = db["assignment"]
- submissions_collection = db["submissions"]

### Faker Initialization to generate sample Data
fake = Faker()

---
## Database Schema Documentation
```python
**Users Collection**
user_schema = { 
    "user_id": 1,
    "email": "john@mich.com",
    "firstname": "micheal",
    "lastname": "mathew",
    "role": "student",
    "date_joined": datetime(2024, 8, 1),
    "profile": {
        "bio": "Data Engineer",
        "avatar": "https://example.com/avatar.jpg",
        "skills": "Python",
    },
    "is_active": True}

**Courses Collection**
course_schema = {
    "course_id": "MongoDB-101",
    "title": "Introduction to MongoDB",
    "description": "learn MongoDB",
    "instructorId": user_schema["user_id"],
    "category": "Database",
    "level": "advanced",
    "duration": 4.0,
    "price": 105.00,
    "tags": "MongoDB",
    "createdAt": datetime(2024, 8, 1),
    "updatedAt": datetime.now(),
    "isPublished": True}

**Enrollments Collection**
enrollment = {
     "course_id": random.choice(course_ids),
    "enrolled_at": enrollment_date,
    "completed": random.choice([True, False])}

**Lessons Collection**
lesson = {
    "course_id": random.choice(course_ids),  # valid course ID
    "title": random.choice(lesson_titles),
    "content": random.choice(lesson_content),
    "order": i + 1,
    "duration": round(random.uniform(4, 20), 1),  # in hours
    "rating": round(random.uniform(3.0, 5.0), 1)  # lesson rating between 3.0 and 5.0}

**Assignments Collection**
assignment = {
    "course_id": random.choice(course_ids),
    "title": random.choice(["Intro to MongoDB", "MongoDB intermediate"]),
    "description": random.choice(["Learn MongoDB", "Learn Python", "Be an Expert in MongoDB"]),
    "due_date": datetime(2025, 4, 30)}

**Submissions Collection**
submission = {
    "user_id": random.choice(student_ids),
    "submitted_at": datetime(2025, 5, 31),
    "grade": random.randint(50, 100)}
```
    
---
### Key Tasks Covered & Query Explanations
- Database Setup & Data Modeling
- Create collections: users, courses, enrollments, lessons, assignments, submissions
- Generate and insert sample data with Faker
- Student Enrollment
- Checks if user exists and is a student
- Confirms course exists
- Prevents duplicate enrollment

### Data Analysis, Aggregation, Schema Validation, Performance Analysis Results
- Count users by roles (e.g., student, instructor)
- Average course enrollments
- Submission rates and completion metrics
- Date-based queries (e.g., active users in the last 30 days)
- Joins enrollments with course info
- Groups by course and category
- Computes total enrollments and average rating
- Lesson Removal
- Assignment Queries
- Retrieves assignments due within the next 7 days using $gte and $lte operators
- Define and apply JSON schema validations (e.g., enum types, email format)
- Before Optimization (Some queries took 200–500ms due to missing indexes)
- Optimizations Applied Created indexes on: email in users,title, category in courses, due_date in assignments
- Rewrote aggregation pipelines to minimize $lookup depth
- Used explain() to identify and eliminate collection scans
- After Optimization (Query latency reduced to ~20–80ms range)

### Challenges Faced and Solutions

#### Duplicate Enrollment Prevention
- Challenge: Preventing students from enrolling multiple times
- Solution: Added a find_one check before insertion into enrollments

#### Schema Enforcement
- Challenge: MongoDB is schema-less by default
- Solution: Used $jsonSchema validation with collMod to enforce structure

#### Slow Aggregations
- Challenge: Aggregation pipelines with $lookup slowed down
- Solution: Indexed course_id fields and used $project to limit data transferred

#### Handling Invalid Inserts
- Challenge: Users inserting documents with wrong data types
- Solution: Added robust error handling and type validations

#### Email Format Validation
- Challenge: Inconsistent or malformed emails
- Solution: Used regex pattern in schema validator

