---
title: "Creating a URL Shortener with FastAPI, ReactJs and TailwindCSS"
datePublished: Tue Jan 02 2024 12:04:02 GMT+0000 (Coordinated Universal Time)
cuid: clqwax1ym000b08jodauw9e3v
slug: creating-a-url-shortener-with-fastapi-reactjs-and-tailwindcss
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1704196998398/92fbdb4d-692d-47f4-8fba-0f067bae873b.jpeg
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1704196980382/b0b41e6b-d277-4f99-9730-ef4003ad7639.jpeg
tags: python, reactjs, axios, fastapi

---

In this article, we'll create a URL Shortener using FastAPI for the backend and ReactJs and TailwindCSS for the frontend design.

This code is a simple implementation of a URL shortening service using the FastAPI framework in Python. Let's break down the code and understand its functionality:

## Backend with FastAPI

First create a virtual environment and install the dependencies

```bash
python -m venv env
source env/bin/activate
python -m pip install fastapi uvicorn
```

Then, let's import the necessary imports needed

```python
from fastapi import FastAPI, HTTPException
from fastapi.middleware.cors import CORSMiddleware
from fastapi.responses import RedirectResponse
from pydantic import BaseModel
import secrets
```

Next, create a FastAPI instance:

```python
app = FastAPI()
```

Next, define a Pydantic model for the request payload:

```python
class URLItem(BaseModel):
    original_url: str
```

Next, we'll use the in-memory database to store the mapping between the short URL and the original URL

```python
url_database = {}
```

Next, let's configure the CORS middleware

```python
origins = ["*"]
app.add_middleware(
    CORSMiddleware,
    allow_origins=origins,
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"],
)
```

Next, let's create an endpoint for shortening a URL

```python
@app.post("/shorten/")
def shorten_url(url_item: URLItem):
    # Generate a short URL using secrets module
    short_url = secrets.token_urlsafe(6)
    
    # Store the mapping between short URL and original URL in the database
    url_database[short_url] = url_item.original_url
    
    # Return the short URL in the response
    return {"short_url": short_url}
```

Finally, create an endpoint for redirecting to the original URL based on the short URL

```python
@app.get("/{short_url}")
def redirect_to_original(short_url: str):
    # Retrieve the original URL from the database
    original_url = url_database.get(short_url)
    
    # If the original URL exists, redirect to it
    if original_url:
        return RedirectResponse(url=original_url)
    else:
        # If the short URL is not found in the database, return an error response
        return {"error": "URL not found"}
```

Run `uvicorn main:app --reload` to run the backend

## Frontend with ReactJS

Create a Vite project and install dependencies including axios

```bash
npm init vite@latest url-shortener --template react
cd url-shortener
npm install && npm install axios
```

Import the `useState` hook from React to manage the components state and the `axios` library to make HTTP Requests

```javascript
import { useState } from 'react';
import axios from 'axios';
```

The component uses the `useState` hook to manage state variables for the original URL (`originalUrl`), the shortened URL (`shortUrl`), and a loading indicator (`loading`).

```javascript
const ShortenerForm = () => {
  // State variables for the original URL, short URL, and loading state
  const [originalUrl, setOriginalUrl] = useState('');
  const [shortUrl, setShortUrl] = useState('');
  const [loading, setLoading] = useState(false);
```

This function is called when the user clicks the "Shorten URL" button. It first checks if the original URL is provided and shows an alert if not. If the URL is valid, it sets the loading state to `true`, sends a POST request to the specified URL shortening service, and updates the state with the shortened URL. Any errors that occur during the process are logged, and the loading state is reset to `false` regardless of success or failure.

```javascript
const shortenUrl = async () => {
  if (!originalUrl) {
    alert('Please enter a URL.');  
    return;
  }

  setLoading(true);

  try {
    // Making a POST request to a URL shortening service
    const response = await axios.post('http://localhost:8000/shorten/', {
      original_url: originalUrl,
    });
    // Updating state with the shortened URL
    setShortUrl(`http://localhost:8000/${response.data.short_url}`);
  } catch (error) {
    console.error('Error shortening URL:', error);
  } finally {
    setLoading(false);
  }
};
```

The render method returns JSX that defines the component's UI. It includes a form with an input field for the original URL, a button to trigger URL shortening, and a display area for the shortened URL.

```javascript
 return (
    <div className="flex items-center justify-center h-screen">
      <div className="bg-gray-100 p-6 rounded shadow-md w-96">
        <h1 className="text-2xl font-semibold mb-4">URL Shortener</h1>
        <input
          className="w-full border p-2 mb-4"
          type="text"
          value={originalUrl}
          onChange={(e) => setOriginalUrl(e.target.value)}
          placeholder="Enter URL to shorten"
        />
        <button
          className={`bg-blue-500 text-white px-4 py-2 rounded ${loading ? 'opacity-50 cursor-not-allowed' : ''}`}
          onClick={shortenUrl}
          disabled={loading}
        >
          {loading ? 'Loading...' : 'Shorten URL'}
        </button>
        {shortUrl && (
          <div className="mt-4">
            <p className="font-semibold">Shortened URL:</p>
            <a
              href={shortUrl}
              target="_blank"
              rel="noopener noreferrer"
              className="text-blue-500 hover:underline"
            >
              {shortUrl}
            </a>
          </div>
        )}
      </div>
    </div>
  );
};
```

The component is exported as the default export, making it available for use in other parts of the application.

```javascript
export default ShortenerForm;
```

Now run `npm run dev` to see the frontend

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1704196872548/4d38f141-2106-4b77-9e2d-246b955bb954.png align="center")

In summary, this React component provides a simple form for users to enter a URL, click a button to shorten it using a specified service, and then displays the shortened URL. The loading state is used to provide feedback to the user during the URL shortening process.

Full code can be seen at [https://github.com/reyesvicente/urlshortener](https://github.com/reyesvicente/urlshortener) and the site can be seen at [https://urlshrtnr.vercel.app/](https://urlshrtnr.vercel.app/)