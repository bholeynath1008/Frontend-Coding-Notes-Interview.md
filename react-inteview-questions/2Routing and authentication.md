# React Routing & Authentication Guide

## Table of Contents

1. [React Router](#1-react-router)
2. [How React Router Works](#2-how-react-router-works)
3. [Protected Routes in React Router v6+](#3-protected-routes-in-react-router-v6)
   - Method 1: Protected Route Component
   - Method 2: Higher-Order Component
   - Method 3: Loader-based (React Router v6.4+)
4. [Role-Based Authorization in Routing](#4-role-based-authorization-in-routing)
   - Permission-based approach
   - Granular component-level authorization
5. [SSO Login (OAuth2/OIDC) in React](#5-sso-login-oauth2oidc-in-react)
   - 5.1. SSO Overview
   - Authentication Flow
   - Token Generation
   - Token Sharing
   - Action Granted
6. [JWT (JSON Web Tokens)](#6-jwt-json-web-tokens)
   - How JWT Works
   - Key Aspects of JWT Signatures
7. [JWT + Refresh Token Flow](#7-jwt--refresh-token-flow)
   - Token Storage
   - Security considerations
   - Complete flow implementation
8. [User Logged Out Mid-Session - Graceful Redirect](#8-user-logged-out-mid-session---graceful-redirect)
9. [Route-Based Code Splitting](#9-route-based-code-splitting)

---

## 1. React Router

React Router is a library for routing in React applications. It enables navigation between different components in React and provides a way to handle client-side routing in single-page applications (SPAs).

**Key Features:**
- Declarative routing
- Dynamic route matching
- Nested routes
- Navigation guards
- URL parameters and query strings
- Programmatic navigation

## 2. How React Router Works

### 1. Browser Router
`<BrowserRouter>` is the main wrapper component that enables routing.
- Provides the context for routing
- Keeps track of the history (URL) of the application
- Uses HTML5 History API for clean URLs

### 2. Routes
`<Routes>` component is the container for defining all the routes in your application.
- Replaces `<Switch>` from React Router v5
- Ensures only the first matching `<Route>` is rendered
- Supports relative routing and nested routes

### 3. Route
`<Route>` defines a path and the component to render when the path matches the current URL.

**Basic Example:**
```javascript
import { BrowserRouter, Routes, Route } from 'react-router-dom';

function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
        <Route path="/user/:id" element={<UserProfile />} />
        <Route path="*" element={<NotFound />} />
      </Routes>
    </BrowserRouter>
  );
}
```

## 3. Protected Routes in React Router v6+

Protected routes restrict access to certain pages based on authentication status or user permissions.

### Method 1: Protected Route Component

Create a reusable component that checks authentication before rendering:

```javascript
import { Navigate } from 'react-router-dom';

function ProtectedRoute({ children, isAuthenticated }) {
  if (!isAuthenticated) {
    return <Navigate to="/login" replace />;
  }
  return children;
}

// Usage
<Route 
  path="/dashboard" 
  element={
    <ProtectedRoute isAuthenticated={user !== null}>
      <Dashboard />
    </ProtectedRoute>
  } 
/>
```

### Method 2: Higher-Order Component

Wrap components with authentication logic:

```javascript
function withAuth(Component) {
  return function AuthenticatedComponent(props) {
    const { user } = useAuth();
    
    if (!user) {
      return <Navigate to="/login" replace />;
    }
    
    return <Component {...props} />;
  };
}

// Usage
const ProtectedDashboard = withAuth(Dashboard);

<Route path="/dashboard" element={<ProtectedDashboard />} />
```

### Method 3: Loader-based (React Router v6.4+)

Use loaders for authentication checks before rendering:

```javascript
import { redirect } from 'react-router-dom';

async function protectedLoader() {
  const user = await getUser();
  if (!user) {
    return redirect('/login');
  }
  return null;
}

const router = createBrowserRouter([
  {
    path: '/dashboard',
    element: <Dashboard />,
    loader: protectedLoader,
  },
]);
```

## 4. Role-Based Authorization in Routing

### Permission-based Approach

Check user roles/permissions before rendering routes:

```javascript
function ProtectedRoute({ children, requiredRole }) {
  const { user } = useAuth();
  
  if (!user) {
    return <Navigate to="/login" replace />;
  }
  
  if (requiredRole && !user.roles.includes(requiredRole)) {
    return <Navigate to="/unauthorized" replace />;
  }
  
  return children;
}

// Usage
<Route 
  path="/admin" 
  element={
    <ProtectedRoute requiredRole="admin">
      <AdminPanel />
    </ProtectedRoute>
  } 
/>
```

### Granular Component-level Authorization

```javascript
function RequirePermission({ permission, children, fallback = null }) {
  const { user } = useAuth();
  
  const hasPermission = user?.permissions?.includes(permission);
  
  return hasPermission ? children : fallback;
}

// Usage inside components
<RequirePermission permission="edit:posts">
  <EditButton />
</RequirePermission>
```

**Advanced Example with Multiple Roles:**

```javascript
const ROLE_PERMISSIONS = {
  admin: ['read', 'write', 'delete', 'manage_users'],
  editor: ['read', 'write'],
  viewer: ['read'],
};

function ProtectedRoute({ children, requiredPermissions = [] }) {
  const { user } = useAuth();
  
  if (!user) {
    return <Navigate to="/login" replace />;
  }
  
  const userPermissions = ROLE_PERMISSIONS[user.role] || [];
  const hasPermission = requiredPermissions.every(
    perm => userPermissions.includes(perm)
  );
  
  if (!hasPermission) {
    return <Navigate to="/unauthorized" replace />;
  }
  
  return children;
}
```

## 5. SSO Login (OAuth2/OIDC) in React

### 5.1. SSO Overview

Single Sign-On (SSO) allows users to authenticate once and gain access to multiple applications without re-entering credentials.

**Common Protocols:**
- OAuth 2.0: Authorization framework
- OIDC (OpenID Connect): Authentication layer on top of OAuth 2.0
- SAML: XML-based authentication

### Authentication Flow

**1. User Initiates Login**
```javascript
function LoginButton() {
  const handleLogin = () => {
    const authUrl = `https://auth-provider.com/oauth/authorize?` +
      `client_id=${CLIENT_ID}&` +
      `redirect_uri=${REDIRECT_URI}&` +
      `response_type=code&` +
      `scope=openid profile email`;
    
    window.location.href = authUrl;
  };
  
  return <button onClick={handleLogin}>Login with SSO</button>;
}
```

**2. Redirect to Identity Provider (IdP)**
- User is redirected to the SSO provider (Google, Okta, Azure AD, etc.)
- User enters credentials on the provider's secure login page

**3. Authorization Code Returned**
- After successful authentication, IdP redirects back to your app
- URL contains authorization code: `https://yourapp.com/callback?code=AUTH_CODE`

### Token Generation

**4. Exchange Code for Tokens**
```javascript
async function handleCallback() {
  const urlParams = new URLSearchParams(window.location.search);
  const code = urlParams.get('code');
  
  // Exchange code for tokens (done on backend for security)
  const response = await fetch('/api/auth/token', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({ code }),
  });
  
  const { access_token, id_token, refresh_token } = await response.json();
  
  // Store tokens securely
  sessionStorage.setItem('access_token', access_token);
  sessionStorage.setItem('refresh_token', refresh_token);
}
```

### Token Sharing

**5. Tokens Received**
- **Access Token:** Short-lived token for API requests (15-60 minutes)
- **ID Token:** Contains user information (JWT format)
- **Refresh Token:** Long-lived token to get new access tokens

### Action Granted

**6. Use Tokens for Authentication**
```javascript
// API interceptor to add token to requests
axios.interceptors.request.use((config) => {
  const token = sessionStorage.getItem('access_token');
  if (token) {
    config.headers.Authorization = `Bearer ${token}`;
  }
  return config;
});
```

**Complete SSO Implementation Example:**

```javascript
// AuthProvider.jsx
import { createContext, useContext, useState, useEffect } from 'react';

const AuthContext = createContext(null);

export function AuthProvider({ children }) {
  const [user, setUser] = useState(null);
  const [loading, setLoading] = useState(true);
  
  useEffect(() => {
    // Check for existing session
    const token = sessionStorage.getItem('access_token');
    if (token) {
      fetchUserProfile(token);
    } else {
      setLoading(false);
    }
  }, []);
  
  const login = () => {
    const authUrl = `${process.env.REACT_APP_AUTH_URL}/authorize?` +
      `client_id=${process.env.REACT_APP_CLIENT_ID}&` +
      `redirect_uri=${window.location.origin}/callback&` +
      `response_type=code&` +
      `scope=openid profile email`;
    window.location.href = authUrl;
  };
  
  const logout = () => {
    sessionStorage.clear();
    setUser(null);
    window.location.href = '/';
  };
  
  return (
    <AuthContext.Provider value={{ user, login, logout, loading }}>
      {children}
    </AuthContext.Provider>
  );
}

export const useAuth = () => useContext(AuthContext);
```

## 6. JWT (JSON Web Tokens)

JWT is a compact, URL-safe token format used for securely transmitting information between parties.

### How JWT Works

**JWT Structure:** `header.payload.signature`

**1. Header**
```json
{
  "alg": "HS256",
  "typ": "JWT"
}
```

**2. Payload (Claims)**
```json
{
  "sub": "user123",
  "name": "John Doe",
  "role": "admin",
  "iat": 1516239022,
  "exp": 1516242622
}
```

**3. Signature**
```
HMACSHA256(
  base64UrlEncode(header) + "." + base64UrlEncode(payload),
  secret
)
```

### Key Aspects of JWT Signatures

**Purpose:**
- Verify token hasn't been tampered with
- Confirm token was issued by trusted source

**Verification Process:**
```javascript
// Backend verification (Node.js example)
const jwt = require('jsonwebtoken');

function verifyToken(token) {
  try {
    const decoded = jwt.verify(token, process.env.JWT_SECRET);
    return { valid: true, data: decoded };
  } catch (error) {
    return { valid: false, error: error.message };
  }
}
```

**Security Best Practices:**
- Never store JWT secret in client-side code
- Use HTTPS to prevent token interception
- Set appropriate expiration times
- Validate tokens on every protected request

## 7. JWT + Refresh Token Flow

### Token Storage

**Options for storing tokens:**

1. **sessionStorage** (Recommended for access tokens)
   - Cleared when tab closes
   - Not shared across tabs
   ```javascript
   sessionStorage.setItem('access_token', token);
   ```

2. **localStorage** (For refresh tokens - with caution)
   - Persists across sessions
   - Vulnerable to XSS attacks
   ```javascript
   localStorage.setItem('refresh_token', token);
   ```

3. **httpOnly Cookies** (Most secure)
   - Not accessible via JavaScript
   - Protected from XSS
   - Backend sets: `Set-Cookie: refresh_token=...; HttpOnly; Secure; SameSite=Strict`

### Security Considerations

**XSS Protection:**
- Sanitize all user inputs
- Use Content Security Policy (CSP)
- Don't store sensitive tokens in localStorage if possible

**CSRF Protection:**
- Use SameSite cookie attribute
- Implement CSRF tokens for state-changing requests

### Complete Flow Implementation

```javascript
// Token management service
class TokenService {
  static getAccessToken() {
    return sessionStorage.getItem('access_token');
  }
  
  static setAccessToken(token) {
    sessionStorage.setItem('access_token', token);
  }
  
  static getRefreshToken() {
    return localStorage.getItem('refresh_token');
  }
  
  static setRefreshToken(token) {
    localStorage.setItem('refresh_token', token);
  }
  
  static clearTokens() {
    sessionStorage.removeItem('access_token');
    localStorage.removeItem('refresh_token');
  }
}

// API interceptor with token refresh
axios.interceptors.response.use(
  (response) => response,
  async (error) => {
    const originalRequest = error.config;
    
    // If 401 and haven't retried yet
    if (error.response?.status === 401 && !originalRequest._retry) {
      originalRequest._retry = true;
      
      try {
        const refreshToken = TokenService.getRefreshToken();
        const response = await axios.post('/api/auth/refresh', {
          refresh_token: refreshToken,
        });
        
        const { access_token } = response.data;
        TokenService.setAccessToken(access_token);
        
        // Retry original request with new token
        originalRequest.headers.Authorization = `Bearer ${access_token}`;
        return axios(originalRequest);
      } catch (refreshError) {
        // Refresh failed - redirect to login
        TokenService.clearTokens();
        window.location.href = '/login';
        return Promise.reject(refreshError);
      }
    }
    
    return Promise.reject(error);
  }
);
```

## 8. User Logged Out Mid-Session - Graceful Redirect

Handle scenarios where user session expires or is terminated:

```javascript
// Global error boundary for auth
function AuthErrorBoundary({ children }) {
  const navigate = useNavigate();
  
  useEffect(() => {
    const handleUnauthorized = (event) => {
      if (event.detail.status === 401) {
        TokenService.clearTokens();
        navigate('/login', { 
          state: { 
            message: 'Your session has expired. Please log in again.',
            returnUrl: window.location.pathname 
          } 
        });
      }
    };
    
    window.addEventListener('unauthorized', handleUnauthorized);
    return () => window.removeEventListener('unauthorized', handleUnauthorized);
  }, [navigate]);
  
  return children;
}

// Axios interceptor to dispatch event
axios.interceptors.response.use(
  response => response,
  error => {
    if (error.response?.status === 401) {
      window.dispatchEvent(new CustomEvent('unauthorized', { 
        detail: { status: 401 } 
      }));
    }
    return Promise.reject(error);
  }
);
```

**Activity monitoring:**
```javascript
function useSessionTimeout(timeoutMinutes = 30) {
  const navigate = useNavigate();
  
  useEffect(() => {
    let timeout;
    
    const resetTimeout = () => {
      clearTimeout(timeout);
      timeout = setTimeout(() => {
        TokenService.clearTokens();
        navigate('/login', { 
          state: { message: 'Session expired due to inactivity' } 
        });
      }, timeoutMinutes * 60 * 1000);
    };
    
    // Reset on user activity
    const events = ['mousedown', 'keydown', 'scroll', 'touchstart'];
    events.forEach(event => {
      document.addEventListener(event, resetTimeout);
    });
    
    resetTimeout(); // Initialize
    
    return () => {
      clearTimeout(timeout);
      events.forEach(event => {
        document.removeEventListener(event, resetTimeout);
      });
    };
  }, [navigate, timeoutMinutes]);
}
```

## 9. Route-Based Code Splitting

Split your application code by routes for faster initial load times:

```javascript
import { lazy, Suspense } from 'react';
import { BrowserRouter, Routes, Route } from 'react-router-dom';

// Lazy load route components
const Home = lazy(() => import('./pages/Home'));
const Dashboard = lazy(() => import('./pages/Dashboard'));
const Profile = lazy(() => import('./pages/Profile'));
const Admin = lazy(() => import('./pages/Admin'));

function App() {
  return (
    <BrowserRouter>
      <Suspense fallback={<LoadingSpinner />}>
        <Routes>
          <Route path="/" element={<Home />} />
          <Route path="/dashboard" element={<Dashboard />} />
          <Route path="/profile" element={<Profile />} />
          <Route path="/admin" element={<Admin />} />
        </Routes>
      </Suspense>
    </BrowserRouter>
  );
}
```

**With React Router v6.4+ and data routers:**

```javascript
import { createBrowserRouter, RouterProvider } from 'react-router-dom';

const router = createBrowserRouter([
  {
    path: '/',
    lazy: () => import('./pages/Home'),
  },
  {
    path: '/dashboard',
    lazy: () => import('./pages/Dashboard'),
    loader: async () => {
      // Prefetch data while loading component
      const data = await fetch('/api/dashboard-data');
      return data.json();
    },
  },
]);

function App() {
  return <RouterProvider router={router} />;
}
```

**Benefits:**
- Smaller initial bundle size
- Faster time to interactive
- Improved performance on slower networks
- Better user experience with loading states
