# 🧠 MicroServices QuizApp

A Spring Boot microservices application for managing and attempting quizzes. Built with Eureka Service Discovery, API Gateway, and PostgreSQL databases — fully containerized with Docker.

---

## 🏗️ Architecture

```
Client
  └──▶ API Gateway (8765)
          ├──▶ Question Service (8080)  ──▶ PostgreSQL (5433)
          └──▶ Quiz Service (8081)      ──▶ PostgreSQL (5434)

All services register with Eureka Service Registry (8761)
```

---

## 🚀 Services

| Service                  | Port |
|--------------------------|------|
| Service Registry (Eureka)| 8761 |
| API Gateway              | 8765 |
| Question Service         | 8080 |
| Quiz Service             | 8081 |
| Postgres (Question DB)   | 5433 |
| Postgres (Quiz DB)       | 5434 |

---

## ⚙️ Prerequisites

- [Docker Desktop](https://www.docker.com/products/docker-desktop/) installed and running

---

## 🛠️ Setup & Run

### 1. Clone the repository
```bash
git clone https://github.com/Chirag626/MicroServices-QuizApp.git
cd MicroServices-QuizApp
```

### 2. Create `.env` file
```bash
cp .env.example .env
```

Open `.env` and set your passwords:
```env
QUESTION_DB=your_DBname
QUESTION_DB_USER=your_username
QUESTION_DB_PASSWORD=your_password

QUIZ_DB=your_Dbname
QUIZ_DB_USER=your_username
QUIZ_DB_PASSWORD=your_password
```

### 3. Run the project
```bash
# First time (builds images)
docker-compose up --build

# After first time
docker-compose up -d
```

### 4. Verify all services are registered
Open Eureka Dashboard: http://localhost:8761

You should see **QUESTION-SERVICE** and **QUIZ-SERVICE** registered.

---

## 📡 API Reference

> All requests go through API Gateway: `http://localhost:8765`

### 📘 Question Service

| Method   | Endpoint                                           | Description                  |
|----------|----------------------------------------------------|------------------------------|
| `GET`    | `/question-service/question/allQuestions`          | Get all questions             |
| `GET`    | `/question-service/question/id/{id}`               | Get question by ID            |
| `GET`    | `/question-service/question/category/{category}`   | Get questions by category     |
| `POST`   | `/question-service/question/add`                   | Add single question           |
| `POST`   | `/question-service/question/addBatch`              | Add multiple questions        |
| `PATCH`  | `/question-service/question/update/{id}`           | Update only the answer        |
| `PATCH`  | `/question-service/question/updateAnyExisting/{id}`| Update any field(s)           |
| `PUT`    | `/question-service/question/updateall/{id}`        | Replace entire question       |
| `DELETE` | `/question-service/question/delete/{id}`           | Delete a question             |

#### Add Single Question — Request Body
```json
{
  "category": "Java",
  "difficulty": "Easy",
  "question": "What is JVM?",
  "option1": "Java Virtual Machine",
  "option2": "Java Variable Method",
  "option3": "Just Virtual Memory",
  "option4": "Java Verified Module",
  "answer": "Java Virtual Machine"
}
```

---

### 📗 Quiz Service

| Method  | Endpoint                          | Description                        |
|---------|-----------------------------------|------------------------------------|
| `POST`  | `/quiz-service/quiz/create`       | Create a new quiz                  |
| `GET`   | `/quiz-service/quiz/get/{id}`     | Get quiz questions (without answer)|
| `POST`  | `/quiz-service/quiz/submit/{id}`  | Submit answers and get score       |

#### Create Quiz — Request Body
```json
{
  "category": "Java",
  "numOfQuestions": 5,
  "title": "Java Basics Quiz"
}
```

#### Submit Quiz — Request Body
```json
[
  { "id": 1, "response": "Java Virtual Machine" },
  { "id": 2, "response": "extends" },
  { "id": 3, "response": "String" }
]
```

---

## 🗂️ Available Question Categories

| Category | Difficulty Levels     |
|----------|-----------------------|
| Java     | Easy, Medium, Hard    |
| Python   | Easy, Medium, Hard    |
| English  | Easy, Medium, Hard    |
| Cloud    | Easy, Medium, Hard    |
| Math     | Easy                  |

---

## 🧪 Recommended Testing Order

```
1. POST /question-service/question/addBatch  → Add questions
2. GET  /question-service/question/allQuestions → Verify
3. POST /quiz-service/quiz/create            → Create a quiz
4. GET  /quiz-service/quiz/get/{id}          → Get quiz questions
5. POST /quiz-service/quiz/submit/{id}       → Submit & get score
```

---

## 🛑 Stop the project

```bash
docker-compose down
```

---

## 📁 Project Structure

```
MicroServices-QuizApp/
├── docker-compose.yml
├── pom.xml
├── .env.example
├── .gitignore
├── service-registry/
├── api-gateway/
├── question-service/
└── quiz-service/
```

### 🔹 Eureka Dashboard

![Eureka Dashboard](https://github.com/user-attachments/assets/1b2240a5-3dd4-4a14-8867-226ac3fb28c5)

### 🔹 Running Servers/Containers

![Running Dashboard](https://github.com/user-attachments/assets/adb8769f-8ee8-4214-9e4b-a7e8feaeabfc)
![Running Dashboard](https://github.com/user-attachments/assets/0d76df3c-a09f-410c-a6e3-2cfd82213425)

