---
title: "Building a Seamless JWT Onboarding Flow with React Router v7 and Django"
datePublished: 2026-03-17T10:47:22.210Z
cuid: cmmuhn6u7001f2bjb0h7s0w0z
slug: building-a-seamless-jwt-onboarding-flow-with-react-router-v7-and-django
cover: https://cdn.hashnode.com/uploads/covers/5f95116240346172a86c20c6/dde205bd-3e48-4afb-9d41-533c71517282.png
ogImage: https://cdn.hashnode.com/uploads/og-images/5f95116240346172a86c20c6/32e1f385-ef22-4daf-aff7-d5cb96632b77.png
tags: javascript, python, django, reactjs

---

Authentication and onboarding are often the highest-friction points in a new user's journey. If the process is clunky or requires too many redirects, users drop off. Recently, I set out to build a streamlined **JWT authentication and onboarding flow** using a modern stack: R**eact Router v7 (with Vite), TypeScript, and Django REST Framework (DRF)**.

In this article, I'll walk through the architecture, the backend implementation, and how to handle protected routes securely on the client side.

## The Architecture Stack

Here is what we are working with:

*   **Backend**: Django, Django REST Framework (DRF), and djangorestframework-simplejwt.
    
*   **Frontend**: React (via Vite) and the newly released React Router v7.
    
*   **Styling**: Tailwind CSS.
    
*   **State Management**: React Context API for lightweight auth state handling.
    

### **The Goal**

1.  A user registers.
    
2.  The user logs in. They receive a short-lived access token and a refresh token.
    
3.  The backend knows if the user has completed their profile setup (`is_onboarded`).
    
4.  If they haven't onboarded, the backend middleware blocks API requests, and the frontend automatically reroutes them to the `/onboarding` page.
    
5.  Once onboarded, new tokens are issued with the updated status, and the user gains full access to the application `/`.
    

## **1\. Setting up the Django Backend**

### **The Custom User Model**

First, we need to track whether a user has finished setting up their profile. We extend Django's `AbstractUser`:

```python
# users/models.py
from django.contrib.auth.models import AbstractUser
from django.db import models

class User(AbstractUser):
    is_onboarded = models.BooleanField(default=False)
```

### **Customizing the JWT Payload**

By default, SimpleJWT only includes the `user_id` inside the token. We want the frontend to immediately know the user's name and onboarding status without making an extra API call.

```python
# users/serializers.py
from rest_framework_simplejwt.serializers import TokenObtainPairSerializer

class MyTokenObtainPairSerializer(TokenObtainPairSerializer): 
    @classmethod 
    def get_token(cls, user): 
        token = super().get_token(user) 
        # Inject custom claims into the JWT payload 
        token['is_onboarded'] = user.is_onboarded 
        token['username'] = user.username 
        return token
```

### **Enforcing Onboarding with Middleware**

We want to strictly prevent un-onboarded users from accessing core application data. We do this via a custom Django middleware that intercepts requests.

```python
# users/middleware.py
from django.http import JsonResponse
from django.urls import reverse

class OnboardingMiddleware: 
    def init(self, get_response): 
        self.get_response = get_response
    def call(self, request): 
        # Exempt authentication and onboarding endpoints 
        exempt_urls = [reverse('token_obtain_pair'), '/api/onboard/submit/', '/api/register/']
        if request.path in exempt_urls or request.path.startswith('/admin/'):
            return self.get_response(request)

        user = request.user
        # Block access if authenticated but not onboarded
        if user.is_authenticated and not user.is_onboarded:
            return JsonResponse({
                "error": "ONBOARDING_REQUIRED",
                "message": "Please complete onboarding before accessing this resource."
            }, status=403)
        return self.get_response(request)
```

Now, even if a user tries to bypass the frontend routing, the API will refuse to serve them data!

### **Refreshing Tokens upon Onboarding**

When the user submits the onboarding form, their status changes in the database. However, their physical JWT won't update automatically. We handle this by issuing completely new tokens in the onboarding response:

```python
# users/views.py
from rest_framework.views import APIView
from rest_framework.response import Response
from rest_framework import status, permissions

class OnboardSubmitView(APIView):
    permission_classes = (permissions.IsAuthenticated,)

    def post(self, request):
        user = request.user
        if user.is_onboarded:
            return Response({"detail": "Already onboarded."}, status=400)
        
        user.is_onboarded = True
        user.save()

        # Generate fresh tokens reflecting the new state
        refresh = MyTokenObtainPairSerializer.get_token(user)
        
        return Response({
            "detail": "Onboarding complete.", 
            "is_onboarded": True,
            "access": str(refresh.access_token),
            "refresh": str(refresh)
        }, status=status.HTTP_200_OK)
```

## **2\. Setting up the React Frontend**

We spun up the frontend using React Router v7's built-in Vite template.

### **Bypassing CORS locally**

To avoid CORS issues during development, we configured Vite to proxy `/api` requests to our Django dev server:

```javascript
// vite.config.ts
import { defineConfig } from "vite";

export default defineConfig({
  // plugins...
  server: {
    proxy: {
      '/api': {
        target: 'http://localhost:8000',
        changeOrigin: true,
      }
    }
  }
});
```

### **Global Authentication Context**

We use React Context to provide user data (`token`, `user` payload, `login`, `logout`) globally.

A critical detail: **Server-Side Rendering (SSR)**. React Router v7 utilizes SSR by default when hydrating the app. If you try to read `localStorage.getItem('access_token')` immediately on the server, Node.js will crash with a `ReferenceError: localStorage is not defined`.

We fix this by waiting for hydration on the client using a simple `useEffect`:

```javascript
// app/contexts/AuthContext.tsx
import React, { createContext, useContext, useEffect, useState } from 'react';

// ... interfaces and parseJwt helper ...

export function AuthProvider({ children }: { children: React.ReactNode }) {
  const [token, setToken] = useState<string | null>(null);
  const [isClient, setIsClient] = useState(false);

  // Safe SSR hydration
  useEffect(() => {
    setIsClient(true);
    setToken(typeof window !== "undefined" ? localStorage.getItem('access_token') : null);
  }, []);

  const user = token ? parseJwt(token) as UserPayload : null;

  useEffect(() => {
    if (token) localStorage.setItem('access_token', token);
    else localStorage.removeItem('access_token');
  }, [token]);

  const login = (access: string, refresh: string) => {
    localStorage.setItem('refresh_token', refresh);
    setToken(access);
  };

  if (!isClient) return null; // Prevent hydration mismatch

  return (
    <AuthContext.Provider value={{ token, user, login, logout, refreshAccessToken }}>
      {children}
    </AuthContext.Provider>
  );
}
```

### **Route Protection and Redirection**

With our context in place, protecting routes and enforcing onboarding becomes incredibly simple. We intercept users in a `useEffect` on our page components.

Here is what our protected **Home Dashboard** looks like:

```javascript
// app/routes/home.tsx
import { useEffect } from "react";
import { useNavigate } from "react-router";
import { useAuth } from "../contexts/AuthContext";

export default function Home() {
  const { token, user, logout } = useAuth();
  const navigate = useNavigate();

  useEffect(() => {
    if (!token) {
      navigate("/login");               // Kick out unauthenticated users
    } else if (user && !user.is_onboarded) {
      navigate("/onboarding");          // Kick out un-onboarded users
    }
  }, [token, user, navigate]);

  // Don't render until verified to prevent brief flashes of content
  if (!user || !user.is_onboarded) return null;

  return (
    <div>
      <h1>Welcome to your portal, {user.username}!</h1>
    </div>
  );
}
```

When the user is pushed to `/onboarding`, they hit our submit button. The API returns the *new* tokens, we update the context, and React Router instantly pushes them back to `/` seamlessly!

```javascript
// Inside Onboarding component submit handler
const handleComplete = async () => {
  const res = await fetch("/api/onboard/submit/", {
    method: "POST",
    headers: { "Authorization": `Bearer ${token}` }
  });
  if (res.ok) {
    const data = await res.json();
    login(data.access, data.refresh); // Instantly updates global context
    navigate("/");                    // Redirect to dashboard
  }
};
```

## **Conclusion**

By embedding the `is_onboarded` claim directly inside the JWT, we eliminate the need for the frontend to constantly ping the database to check a user's status. Coupling this tightly with Django Middleware ensures backend security, while utilizing React Context provides snappy, seamless redirects on the frontend.

Building auth doesn't have to be painful—with the right architecture, it can be safe, scalable, and a great experience for the end user.