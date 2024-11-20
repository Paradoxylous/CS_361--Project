# Motivational Quote Microservice

## Project Overview

This project is a Flask-based API that serves random motivational quotes. It is designed to be integrated with other applications via an HTTP API. The API provides a simple interface for requesting a random quote in JSON format.

### Features
- Random motivational quote generation.
- Simple RESTful API with a single endpoint: `/api/quote`.
- JSON response with the quote text.

### Dependencies
- Python 3.x
- Flask
- `requests` (for testing)

## How to Request Data from the Microservice

To request a motivational quote from the Flask microservice, you will send an HTTP GET request to the `/api/quote` endpoint.

### Steps:

1. **Install Axios**: If you're using Node.js/Express, you can use the `axios` library to send HTTP requests. Install it via npm:
    ```bash
    npm install axios
    ```

2. **Create a Route in Your Backend**: In your Express server, create a route that will handle sending the request to the Flask microservice. Here's an example of how you can do this:

    ```javascript
    const express = require('express');
    const axios = require('axios');
    const app = express();

    // Route to fetch a motivational quote
    app.get('/api/quote', async (req, res) => {
        try {
            // Send GET request to Flask microservice (replace with actual URL if needed)
            const response = await axios.get('http://127.0.0.1:5000/api/quote');
            res.json(response.data);  // Send the quote back to the frontend
        } catch (error) {
            console.error(error);
            res.status(500).json({ message: "Failed to fetch quote." });
        }
    });

    const PORT = 5001;
    app.listen(PORT, () => {
        console.log(`Server is running on port ${PORT}`);
    });
    ```

3. **Make the Request**: The above code sets up an Express route `/api/quote` that sends a GET request to your Flask API (`http://127.0.0.1:5000/api/quote`). Once the Flask API responds with a quote, it will be returned to the frontend.

### Example Request (from a React frontend):

If you want to directly call the backend from your frontend (e.g., in React):

```javascript
import axios from 'axios';

const getMotivationalQuote = async () => {
    try {
        const response = await axios.get('/api/quote');  // Call the backend route
        console.log(response.data.quote);  // Handle the returned quote
    } catch (error) {
        console.error("Error fetching quote:", error);
    }
};

getMotivationalQuote();  // Example function call

+----------------+           +-----------------+           +--------------------+
| React Frontend |           | Express Backend |           | Flask Microservice |
+----------------+           +-----------------+           +--------------------+
        |                            |                              |
        |-----(1) Request Quote----->|                              |
        |                            |                              |
        |                            |----(2) GET /api/quote------->|
        |                            |                              |
        |                            |<---(3) Return Quote----------|
        |                            |                              |
        |<-------(4) Response--------|                              |
        |   Display Quote            |                              |
        |                            |                              |
+----------------+           +-----------------+           +--------------------+

