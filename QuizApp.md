# Quiz App Guide
![image](https://github.com/Academixedu/Projects/assets/43459668/b29fa8b9-53be-4ec0-8ae5-ba30a9d40bd9)


This document outlines the development of a Quiz Application. The app allows users to take quizzes, see questions with multiple-choice answers, and get their scores at the end. It utilizes Java (Spring Boot) for the backend, React for the frontend, and MongoDB for data storage.

## Backend (Spring Boot)

### Project Setup
- Generate a new Spring Boot project using [Spring Initializr](https://start.spring.io/). Include dependencies for Spring Web, Spring Data MongoDB.

### MongoDB Configuration
Configure MongoDB in `src/main/resources/application.properties`:
```properties
spring.data.mongodb.uri=mongodb://localhost:27017/quizApp
Model Creation
Define models for Question and Quiz:


@Document(collection = "questions")
public class Question {
    @Id
    private String id;
    private String text;
    private List<String> choices;
    private String answer;
    // Constructors, Getters, and Setters
}

@Document(collection = "quizzes")
public class Quiz {
    @Id
    private String id;
    private String title;
    private List<Question> questions;
    // Constructors, Getters, and Setters
}
Repository Interface
Create repository interfaces for Question and Quiz:


public interface QuestionRepository extends MongoRepository<Question, String> {
}

public interface QuizRepository extends MongoRepository<Quiz, String> {
}
REST Controllers
Implement controllers to manage quizzes and questions. Example for quizzes:


@RestController
@RequestMapping("/api/quizzes")
public class QuizController {
    @Autowired
    private QuizRepository quizRepository;

    @GetMapping
    public List<Quiz> getAllQuizzes() {
        return quizRepository.findAll();
    }

    @PostMapping
    public Quiz createQuiz(@RequestBody Quiz quiz) {
        return quizRepository.save(quiz);
    }

    // Additional endpoints as required
}
Frontend (React)
Project Setup
Create a new React app:


npx create-react-app quiz-app-frontend
Displaying Quizzes
Develop components to display quizzes and questions. Example for a quiz component:


import React, { useEffect, useState } from 'react';
import axios from 'axios';

function Quiz() {
    const [quizzes, setQuizzes] = useState([]);

    useEffect(() => {
        axios.get('/api/quizzes')
            .then(res => setQuizzes(res.data))
            .catch(err => console.log(err));
    }, []);

    return (
        <div>
            {quizzes.map(quiz => (
                <div key={quiz.id}>
                    <h2>{quiz.title}</h2>
                    // Display questions and choices
                </div>
            ))}
        </div>
    );
}
Taking a Quiz
Implement logic for users to select answers, submit their responses, and view their score.

Running the Application
Backend: Run the Spring Boot application.
Frontend: Start the React app:

npm start
