# Weather App Project Guide
Creating a basic weather app involves fetching weather data from an external API and displaying it. For this guide, we'll use the OpenWeatherMap API for weather data, Spring Boot for the backend to handle API requests, and React for the frontend to display the weather information. This documentation is structured for simplicity and basic functionality:
![image](https://github.com/Academixedu/Projects/assets/43459668/0c926b54-666f-448c-8a05-343b6d1b7e83)


This document outlines the development of a simple weather application using Java (Spring Boot) for the backend, React for the frontend, and the OpenWeatherMap API for fetching weather data.

## Backend (Spring Boot)

### Project Setup
- Use [Spring Initializr](https://start.spring.io/) to generate a Spring Boot project with dependencies for Spring Web.

### OpenWeatherMap API Integration
- Sign up for a free API key at [OpenWeatherMap](https://openweathermap.org/api).
- Store your API key in `src/main/resources/application.properties`.
openweathermap.apikey=YOUR_API_KEY

kotlin
Copy code

### Controller to Fetch Weather Data
- Create a controller to call the OpenWeatherMap API and return weather data.
```java
@RestController
@RequestMapping("/api/weather")
public class WeatherController {
    
    @Value("${openweathermap.apikey}")
    private String apiKey;

    private final RestTemplate restTemplate = new RestTemplate();

    @GetMapping
    public ResponseEntity<String> getWeather(@RequestParam String city) {
        String url = "http://api.openweathermap.org/data/2.5/weather?q=" + city + "&appid=" + apiKey;
        return restTemplate.getForEntity(url, String.class);
    }
}
Handling API Requests
Use RestTemplate to make HTTP requests to the OpenWeatherMap API.
Frontend (React)
Project Setup
Create a new React app.
lua
Copy code
npx create-react-app weather-frontend
Fetching Weather Data
Implement a component to input a city name and display its weather.
jsx
Copy code
import React, { useState } from 'react';
import axios from 'axios';

function Weather() {
    const [city, setCity] = useState('');
    const [weather, setWeather] = useState(null);

    const fetchWeather = async () => {
        const response = await axios.get(`/api/weather?city=${city}`);
        setWeather(response.data);
    };

    return (
        <div>
            <input type="text" value={city} onChange={e => setCity(e.target.value)} />
            <button onClick={fetchWeather}>Get Weather</button>
            {weather && (
                <div>
                    <div>Temperature: {weather.main.temp}°C</div>
                    <div>Condition: {weather.weather[0].main}</div>
                </div>
            )}
        </div>
    );
}
CORS Configuration
Ensure your Spring Boot application is configured to accept cross-origin requests from your React app by adding CORS configuration in your WeatherController.
Running the Application
Backend: Run the Spring Boot application.
Frontend: Start the React app.
sql
Copy code
npm start
This guide provides a basic setup for a weather application, allowing users to input a city name and view its current weather conditions. Further enhancements can include displaying more detailed weather information, adding authentication, and improving the UI/UX.

csharp
Copy code

This markdown guide serves as a straightforward documentation for setting up a simple weather application using Spring Boot and React, demonstrating basic integration with an external API for fetching weather data.
