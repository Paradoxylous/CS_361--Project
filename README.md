# Motivational Quote Microservice

## Project Overview

This project implements a simple **Flask-based microservice** that generates and returns random motivational quotes. The microservice is designed to be used by other applications via a RESTful API.

The goal of this README is to guide how to **request** and **receive data** from this microservice programmatically.

---

## How to Request Data from the Microservice

To request a random motivational quote from the Flask microservice, your application must send an HTTP **GET** request to the `/api/quote` endpoint.

### Example Request in a Backend (Node.js with Axios)

If your backend is built using **Node.js** with the **Axios** library, here's how to make the request:

1. **Install Axios (if you haven't already):**
   ```bash
   npm install axios
   ```

2. **Send a GET request** to the Flask microservice:
   ```javascript
   const axios = require('axios');

   async function getMotivationalQuote() {
       try {
           // Send GET request to Flask microservice
           const response = await axios.get('http://127.0.0.1:5000/api/quote');
           console.log(response.data.quote); // Log the received quote
       } catch (error) {
           console.error('Error fetching quote:', error);
       }
   }

   getMotivationalQuote(); // Example function call to fetch a quote
   ```

### Request URL:
- **GET** `http://127.0.0.1:5000/api/quote`

This URL assumes the Flask microservice is running locally. Replace `127.0.0.1:5000` with your deployed server’s URL if it’s hosted remotely.

---

## How to Receive Data from the Microservice

The response from the Flask microservice will be a JSON object containing the quote.

### Expected Response Format:
```json
{
  "quote": "Believe you can and you're halfway there."
}
```

### Example Code to Handle the Response (Frontend or Backend)

#### In **Node.js (Express)**:
Once the quote is fetched, you can process and forward it to the frontend (if you're using a backend to fetch quotes for a frontend):

```javascript
const express = require('express');
const axios = require('axios');
const app = express();

// Route to fetch quote from Flask microservice
app.get('/api/quote', async (req, res) => {
    try {
        const response = await axios.get('http://127.0.0.1:5000/api/quote');
        res.json(response.data);  // Send quote back to frontend
    } catch (error) {
        res.status(500).json({ message: 'Error fetching quote', error: error.message });
    }
});

const PORT = 5001;
app.listen(PORT, () => {
    console.log(`Backend is running on port ${PORT}`);
});
```

#### In **React (Frontend)**:
In a React application, you can use **Axios** to fetch the quote and display it:

```javascript
import axios from 'axios';
import React, { useState, useEffect } from 'react';

const MotivationalQuote = () => {
    const [quote, setQuote] = useState('');

    useEffect(() => {
        async function fetchQuote() {
            try {
                const response = await axios.get('/api/quote');
                setQuote(response.data.quote);  // Set the quote to display
            } catch (error) {
                console.error('Error fetching quote:', error);
            }
        }

        fetchQuote();  // Fetch quote on component mount
    }, []);

    return (
        <div>
            <h2>Motivational Quote:</h2>
            <p>{quote}</p>  {/* Display the quote */}
        </div>
    );
};

export default MotivationalQuote;
```

---

## UML Sequence Diagram

Below is a **UML Sequence Diagram** that illustrates the process of requesting and receiving data between the **frontend**, **backend**, and **Flask microservice**.

```plaintext
+----------------+           +-----------------+           +--------------------+
| React Frontend |           | Express Backend |           | Flask Microservice |
+----------------+           +-----------------+           +--------------------+
        |                             |                               |
        |-----(1) Request Quote------>|                               |
        |                             |                               |
        |                             |----(2) GET /api/quote-------->|
        |                             |                               |
        |                             |<---(3) Return Quote-----------|
        |                             |                               |
        |<----(4) Response------------|                               |
        |  Display Quote              |                               |
+---- -----------+           +-----------------+           +--------------------+
```

### **Sequence Steps:**
1. The **React frontend** sends a **GET** request to the **Express backend** at `/api/quote`.
2. The **Express backend** makes a **GET** request to the Flask microservice at `http://127.0.0.1:5000/api/quote` to fetch the motivational quote.
3. The **Flask microservice** responds with a JSON object containing the quote.
4. The **Express backend** forwards the quote back to the **React frontend**.
5. The **React frontend** displays the quote to the user.

---

## Additional Notes

- **Running the Flask Microservice**:
    - Ensure you have Python and Flask installed. If not, install Flask using `pip install Flask`.
    - Run the Flask application with the command:
      ```bash
      python app.py
      ```
    - The Flask microservice will be available at `http://127.0.0.1:5000`.

- **Running the Express Backend**:
    - Ensure you have Node.js and Axios installed. If not, install Axios via:
      ```bash
      npm install axios
      ```
    - Start the backend server with:
      ```bash
      node server.js
      ```
    - The Express server will be running on `http://127.0.0.1:5001`.

---

**End of README**
