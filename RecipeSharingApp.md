# Recipe Sharing App Guide

This document provides a detailed guide to building a Recipe Sharing Application. The application allows users to post, browse, and share recipes. It uses Java (Spring Boot) for the backend, React for the frontend, and MongoDB for data storage.

## Backend (Spring Boot)

### Project Setup
- Generate a new Spring Boot project using [Spring Initializr](https://start.spring.io/). Include dependencies for Spring Web, Spring Data MongoDB, and Spring Security for handling user authentication.

### MongoDB Configuration
Configure MongoDB in `src/main/resources/application.properties`:
```properties
spring.data.mongodb.uri=mongodb://localhost:27017/recipeApp
Model Creation
Define models for Recipe and User. Here's an example for the Recipe model:


@Document(collection = "recipes")
public class Recipe {
    @Id
    private String id;
    private String name;
    private String description;
    private List<String> ingredients;
    private String instructions;
    private String createdBy;
    // Constructors, Getters, and Setters
}
Repository Interface
Create repository interfaces for Recipe and User:


public interface RecipeRepository extends MongoRepository<Recipe, String> {
    List<Recipe> findByCreatedBy(String createdBy);
}
REST Controllers
Implement controllers to manage recipes and user operations. Example for recipes:


@RestController
@RequestMapping("/api/recipes")
public class RecipeController {
    @Autowired
    private RecipeRepository recipeRepository;

    @GetMapping
    public List<Recipe> getAllRecipes() {
        return recipeRepository.findAll();
    }

    @PostMapping
    public Recipe addRecipe(@RequestBody Recipe recipe) {
        return recipeRepository.save(recipe);
    }

    // Additional endpoints as required
}
Frontend (React)
Project Setup
Create a new React app:


npx create-react-app recipe-sharing-frontend
Displaying Recipes
Develop components to display recipes. Example for a recipe list component:


import React, { useEffect, useState } from 'react';
import axios from 'axios';

function RecipeList() {
    const [recipes, setRecipes] = useState([]);

    useEffect(() => {
        axios.get('/api/recipes')
            .then(res => setRecipes(res.data))
            .catch(err => console.log(err));
    }, []);

    return (
        <div>
            {recipes.map(recipe => (
                <div key={recipe.id}>
                    <h3>{recipe.name}</h3>
                    <p>{recipe.description}</p>
                    // Display other recipe details
                </div>
            ))}
        </div>
    );
}
Recipe Submission Form
Implement a form for users to submit new recipes:


// Simplified example for a recipe submission form component
Routing and Navigation
Use react-router-dom to handle navigation between different components:


// Example setup for routing in your React app
Running the Application
Backend: Run the Spring Boot application.
Frontend: Start the React app:

npm start
This guide outlines the basic setup for a Recipe Sharing Application, including functionalities for viewing and posting recipes. Future enhancements can include user authentication, recipe ratings, comments, and advanced search features.



This markdown guide is designed for direct copy-paste into a GitHub README or similar documentation. It provides a foundation for building a Recipe Sharing App with Spring Boot, React, and MongoDB, outlining essential steps and code snippets.
