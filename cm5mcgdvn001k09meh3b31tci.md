---
title: "Building a Contact Form Backend with FastAPI and Discord Integration"
datePublished: Tue Jan 07 2025 10:44:57 GMT+0000 (Coordinated Universal Time)
cuid: cm5mcgdvn001k09meh3b31tci
slug: building-a-contact-form-backend-with-fastapi-and-discord-integration
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1736246678031/bce191aa-19fd-4927-989b-77f6026ed288.webp
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1736246680699/7b84e0fa-d5ea-4877-833b-c703ec17c71b.webp
tags: javascript, python, fastapi

---

In this tutorial, we'll build a secure backend API using FastAPI that handles contact form submissions and forwards them to a Discord channel using webhooks. We'll also cover how to properly set up CORS to allow requests from specific domains.

## Prerequisites

* Python 3.11+
    
* FastAPI
    
* httpx (for making async HTTP requests)
    
* A Discord webhook url
    

## Step 1 : Setting up the project

First, create a new directory for your project and install the required dependencies:\\

```python
python -m pip install fastapi uvicorn httpx python-dotenv
```

## Step 2 : Creating the FastAPI Application

Create a new file called `main.py`

```python
import os
from fastapi import FastAPI, HTTPException
from fastapi.middleware.cors import CORSMiddleware
from pydantic import BaseModel
import httpx

app = FastAPI()

# Configure CORS
app.add_middleware(
    CORSMiddleware,
    allow_origins=[
        "https://vicentereyes.org",
        "https://www.vicentereyes.org"
    ],
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"],
)
```

## Step 3 : Define the Data Model

We’ll use Pydantic to define our form data structure:

```python
class FormData(BaseModel):
    name: str
    email: str
    message: str
    service: str
    companyName: str
    companyUrl: str
```

## Step 4 : Create the submission endpoint

Add the endpoint that handles form submissions:

```python
@app.post("/submit/")
@app.post("/submit")  # Handle both with and without trailing slash
async def submit_form(form_data: FormData):
    try:
        # Format the message for Discord
        message_content = {
            "content": f"New form submission: \n"
                      f"**Name:** {form_data.name}\n"
                      f"**Email:** {form_data.email}\n"
                      f"**Message:** {form_data.message}\n"
                      f"**Service:** {form_data.service}\n"
                      f"**Company Name:** {form_data.companyName}\n"
                      f"**Company URL:** {form_data.companyUrl}"
        }

        # Send to Discord webhook
        async with httpx.AsyncClient() as client:
            response = await client.post(DISCORD_WEBHOOK_URL, json=message_content)

        if response.status_code != 204:
            raise HTTPException(status_code=response.status_code, 
                              detail="Failed to send message to Discord")
        
        return {"message": "Form data sent to Discord successfully"}
    
    except Exception as e:
        raise HTTPException(status_code=500, detail=str(e))
```

## Step 5 : Environment Configuration

```bash
FASTAPI_DISCORD_WEBHOOK_URL=your_discord_webhook_url_here
```

## How it works:

1\. **CORS Configuration**: - The \`CORSMiddleware\` is configured to only accept requests from specified domains - This is crucial for security as it prevents unauthorized domains from accessing your API - The middleware is set up to allow all HTTP methods and headers while maintaining origin restrictions 2. **Data Validation**: - The \`FormData\` Pydantic model ensures that all incoming data matches the expected structure - If any required fields are missing or have incorrect types, FastAPI automatically returns a validation error 3. **Discord Integration**: - When a form is submitted, the data is formatted into a Discord message - The message is sent to a Discord channel using a webhook URL - We use \`httpx\` for making async HTTP requests to Discord's API 4. **Error Handling**: - The endpoint is wrapped in a try-catch block to handle potential errors - If Discord's webhook fails, we return an appropriate HTTP error - Any other exceptions are caught and returned as 500 Internal Server errors

## Running the application

Start the server with:

```bash
uvicorn main:app --reload
```

The API will be available at `http://localhost:8000`

## Security Considerations

1\. **CORS**: Only allow domains that actually need to access your API 2. **Environment Variables**: Keep sensitive data like webhook URLs in environment variables  
3\. **Input Validation**: Use Pydantic models to validate all incoming data  
4\. **Error Handling**: Never expose internal error details to clients

## Frontend Integration

To use this API from your frontend, make sure your requests include the correct headers and match the expected data structure:

```javascript
const response = await fetch('your_api_url/submit', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({
    name: 'John Doe',
    email: 'john@example.com',
    message: 'Hello!',
    service: 'Consulting',
    companyName: 'Example Corp',
    companyUrl: 'https://example.com'
  })
});
```

## Conclusion

This setup provides a secure and efficient way to handle contact form submissions and forward them to Discord. The use of FastAPI makes the code clean and maintainable, while proper CORS configuration ensures security. The async nature of the application means it can handle multiple submissions efficiently without blocking.

Code: [https://github.com/reyesvicente/Portfolio-Contact-Form-Backend](https://github.com/reyesvicente/Portfolio-Contact-Form-Backend)

Live: [https://vicentereyes.org/contact](https://vicentereyes.org/contact)