# URL Shortening App Guide
![image](https://github.com/Academixedu/Projects/assets/43459668/91644638-4ec1-4b5d-893e-a6fd77fbe708)


This document outlines the steps to create a URL Shortening Application. The app generates shorter aliases for long URLs, making them easier to share and manage. It utilizes Java (Spring Boot) for the backend, React for the frontend, and MongoDB for data storage.

## Backend (Spring Boot)

### Project Setup
- Generate a new Spring Boot project using [Spring Initializr](https://start.spring.io/). Include dependencies for Spring Web, Spring Data MongoDB.

### MongoDB Configuration
Configure MongoDB in `src/main/resources/application.properties`:
```properties
spring.data.mongodb.uri=mongodb://localhost:27017/urlShortener
Model Creation
Define a model for ShortenedUrl:


@Document(collection = "shortenedUrls")
public class ShortenedUrl {
    @Id
    private String id;
    private String originalUrl;
    private String shortenedUrl;
    private LocalDateTime createdDate = LocalDateTime.now();
    // Constructors, Getters, and Setters
}
Repository Interface
Create a repository interface for ShortenedUrl:


public interface ShortenedUrlRepository extends MongoRepository<ShortenedUrl, String> {
    Optional<ShortenedUrl> findByShortenedUrl(String shortenedUrl);
}
URL Shortening Logic
Implement logic to generate a shortened URL. Example using a simple hash function or a library like Apache Commons:


public class UrlShortenerService {
    // Example method to generate a short URL
    public String generateShortUrl(String originalUrl) {
        // Implement logic to generate a short URL
    }
}
REST Controllers
Implement a controller to handle URL shortening requests:


@RestController
@RequestMapping("/api")
public class UrlShortenerController {
    @Autowired
    private ShortenedUrlRepository repository;

    @PostMapping("/shorten")
    public ShortenedUrl shortenUrl(@RequestBody String originalUrl) {
        String shortenedUrl = urlShortenerService.generateShortUrl(originalUrl);
        ShortenedUrl url = new ShortenedUrl(originalUrl, shortenedUrl);
        return repository.save(url);
    }

    @GetMapping("/{shortenedUrl}")
    public ResponseEntity<Void> redirectToOriginalUrl(@PathVariable String shortenedUrl) {
        ShortenedUrl url = repository.findByShortenedUrl(shortenedUrl)
                                      .orElseThrow(() -> new ResponseStatusException(HttpStatus.NOT_FOUND));
        return ResponseEntity.status(HttpStatus.FOUND)
                             .location(URI.create(url.getOriginalUrl()))
                             .build();
    }
}
Frontend (React)
Project Setup
Create a new React app:


npx create-react-app url-shortener-frontend
URL Shortening Form
Develop a component for users to input URLs to be shortened:


import React, { useState } from 'react';
import axios from 'axios';

function UrlShortener() {
    const [originalUrl, setOriginalUrl] = useState('');
    const [shortenedUrl, setShortenedUrl] = useState('');

    const handleSubmit = async (event) => {
        event.preventDefault();
        try {
            const response = await axios.post('/api/shorten', { originalUrl });
            setShortenedUrl(response.data.shortenedUrl);
        } catch (error) {
            console.error('Error shortening URL:', error);
        }
    };

    return (
        <form onSubmit={handleSubmit}>
            <input type="text" value={originalUrl} onChange={e => setOriginalUrl(e.target.value)} />
            <button type="submit">Shorten</button>
            {shortenedUrl && <div>Shortened URL: {shortenedUrl}</div>}
        </form>
    );
}
Running the Application
Backend: Run the Spring Boot application.
Frontend: Start the React app:

npm start
This guide provides a basic framework for a URL Shortening Application, including URL shortening logic and redirection. Enhancements can include adding authentication, tracking URL usage metrics, and custom URL aliases.
