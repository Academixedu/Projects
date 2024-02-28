# Small eCommerce Application Guide
![image](https://github.com/Academixedu/Projects/assets/43459668/71569e86-36e7-4fb8-ad6b-3c14424ea9bf)


This document outlines the steps to create a small eCommerce application using Java (Spring Boot) for the backend and React for the frontend. The application features product listings, a simple checkout process, and basic user authentication.

## Backend (Spring Boot)

### Project Setup
- Use [Spring Initializr](https://start.spring.io/) to generate a Spring Boot project.
- Dependencies: Spring Web, Spring Data JPA, Spring Security, and H2 Database.

### Database Configuration
- Configure the H2 database in `src/main/resources/application.properties` for development purposes.
spring.datasource.url=jdbc:h2:mem:testdb
spring.datasource.driverClassName=org.h2.Driver
spring.datasource.username=sa
spring.datasource.password=password
spring.jpa.database-platform=org.hibernate.dialect.H2Dialect




### Model Creation
- Define entity models for `Product` and `User`.
```java
@Entity
public class Product {
    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    private Long id;
    private String name;
    private BigDecimal price;
    private String description;
    // Getters and Setters
}
Repository Interfaces
Create repository interfaces for Product and User.


public interface ProductRepository extends JpaRepository<Product, Long> {
}
REST Controllers
Implement controllers to manage products and users.

@RestController
@RequestMapping("/api/products")
public class ProductController {
    @Autowired
    private ProductRepository productRepository;
    
    @GetMapping
    public List<Product> listProducts() {
        return productRepository.findAll();
    }
}
Security Configuration
Configure Spring Security for basic authentication.
Frontend (React)
Project Setup
Create a new React app using Create React App.

npx create-react-app ecommerce-frontend
Fetching Products
Implement a component to display products.

import React, { useEffect, useState } from 'react';
import axios from 'axios';

function Products() {
    const [products, setProducts] = useState([]);
    
    useEffect(() => {
        axios.get('/api/products')
            .then(res => setProducts(res.data))
            .catch(err => console.log(err));
    }, []);
    
    return (
        <div>
            {products.map(product => (
                <div key={product.id}>
                    <h2>{product.name}</h2>
                    <p>{product.description}</p>
                    <p>{product.price}</p>
                </div>
            ))}
        </div>
    );
}
Routing and Navigation
Use react-router-dom to manage navigation.

import { BrowserRouter as Router, Route, Switch } from 'react-router-dom';

function App() {
    return (
        <Router>
            <Switch>
                <Route path="/products" component={Products} />
                // Define other routes
            </Switch>
        </Router>
    );
}
Running the Application
Backend: Run the Spring Boot application.
Frontend: Start the React app.

npm start
