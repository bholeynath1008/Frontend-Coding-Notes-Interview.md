# API Integration & Data Fetching in React

## Table of Contents

1. [Interceptors](#1-interceptors)
   - Request Interceptors
   - Response Interceptors
2. [CORS: Cross Origin Resource Sharing](#2-cors-cross-origin-resource-sharing)
   - How CORS Works
   - Benefits
   - XSS (Cross Site Scripting)
   - CSRF (Cross-Site Request Forgery)
3. [REST vs GraphQL](#3-rest-vs-graphql)
   - When to choose each
   - REST Overview
   - GraphQL Overview
   - Comparison Table

---

## 1. Interceptors

Interceptors are middleware functions that allow you to modify HTTP requests or responses globally before they are handled by your application code.

### Request Interceptors

Intercept and modify outgoing requests before they are sent to the server.

**Common Use Cases:**
- Adding authentication tokens
- Setting default headers
- Logging requests
- Request transformation
- Adding timestamps or correlation IDs

**Axios Example:**
```javascript
import axios from 'axios';

// Add request interceptor
axios.interceptors.request.use(
  (config) => {
    // Modify request before sending
    const token = localStorage.getItem('access_token');
    if (token) {
      config.headers.Authorization = `Bearer ${token}`;
    }
    
    // Add timestamp
    config.headers['X-Request-Time'] = new Date().toISOString();
    
    // Log request
    console.log('Request:', config.method.toUpperCase(), config.url);
    
    return config;
  },
  (error) => {
    // Handle request error
    console.error('Request Error:', error);
    return Promise.reject(error);
  }
);
```

**Fetch API with Custom Wrapper:**
```javascript
const originalFetch = window.fetch;

window.fetch = function(...args) {
  let [url, config = {}] = args;
  
  // Add default headers
  config.headers = {
    ...config.headers,
    'Authorization': `Bearer ${localStorage.getItem('access_token')}`,
    'Content-Type': 'application/json',
  };
  
  console.log('Fetching:', url);
  
  return originalFetch(url, config);
};
```

**React Query with Custom Fetch:**
```javascript
const customFetch = async (url, options = {}) => {
  const token = localStorage.getItem('access_token');
  
  const config = {
    ...options,
    headers: {
      'Content-Type': 'application/json',
      'Authorization': token ? `Bearer ${token}` : '',
      ...options.headers,
    },
  };
  
  const response = await fetch(url, config);
  
  if (!response.ok) {
    throw new Error(`HTTP error! status: ${response.status}`);
  }
  
  return response.json();
};

// Usage with React Query
const { data } = useQuery({
  queryKey: ['users'],
  queryFn: () => customFetch('/api/users'),
});
```

### Response Interceptors

Intercept and modify incoming responses before they reach your application code.

**Common Use Cases:**
- Handling errors globally
- Token refresh logic
- Response transformation
- Logging responses
- Retry logic

**Axios Example:**
```javascript
axios.interceptors.response.use(
  (response) => {
    // Transform response data
    console.log('Response received:', response.config.url);
    
    // You can modify response data here
    if (response.data && response.data.data) {
      return response.data.data; // Unwrap nested data
    }
    
    return response;
  },
  async (error) => {
    const originalRequest = error.config;
    
    // Handle 401 Unauthorized - Token Refresh
    if (error.response?.status === 401 && !originalRequest._retry) {
      originalRequest._retry = true;
      
      try {
        const refreshToken = localStorage.getItem('refresh_token');
        const response = await axios.post('/api/auth/refresh', {
          refresh_token: refreshToken,
        });
        
        const { access_token } = response.data;
        localStorage.setItem('access_token', access_token);
        
        // Retry original request with new token
        originalRequest.headers.Authorization = `Bearer ${access_token}`;
        return axios(originalRequest);
      } catch (refreshError) {
        // Refresh failed - redirect to login
        localStorage.clear();
        window.location.href = '/login';
        return Promise.reject(refreshError);
      }
    }
    
    // Handle other errors
    if (error.response?.status === 403) {
      console.error('Access denied');
      // Redirect to unauthorized page
    }
    
    if (error.response?.status === 500) {
      console.error('Server error');
      // Show error notification
    }
    
    return Promise.reject(error);
  }
);
```

**Advanced Interceptor with Retry Logic:**
```javascript
const MAX_RETRIES = 3;
const RETRY_DELAY = 1000; // 1 second

axios.interceptors.response.use(
  response => response,
  async (error) => {
    const { config } = error;
    
    // Initialize retry count
    config.retryCount = config.retryCount || 0;
    
    // Check if we should retry
    const shouldRetry = 
      config.retryCount < MAX_RETRIES &&
      error.response?.status >= 500 &&
      error.response?.status < 600;
    
    if (shouldRetry) {
      config.retryCount += 1;
      
      // Exponential backoff
      const delay = RETRY_DELAY * Math.pow(2, config.retryCount - 1);
      
      console.log(`Retry attempt ${config.retryCount} after ${delay}ms`);
      
      await new Promise(resolve => setTimeout(resolve, delay));
      
      return axios(config);
    }
    
    return Promise.reject(error);
  }
);
```

**Creating Axios Instance with Interceptors:**
```javascript
// api.js
import axios from 'axios';

const api = axios.create({
  baseURL: process.env.REACT_APP_API_URL,
  timeout: 10000,
  headers: {
    'Content-Type': 'application/json',
  },
});

// Request interceptor
api.interceptors.request.use(
  (config) => {
    const token = localStorage.getItem('access_token');
    if (token) {
      config.headers.Authorization = `Bearer ${token}`;
    }
    return config;
  },
  (error) => Promise.reject(error)
);

// Response interceptor
api.interceptors.response.use(
  (response) => response.data,
  (error) => {
    if (error.response?.status === 401) {
      // Handle unauthorized
      window.location.href = '/login';
    }
    return Promise.reject(error);
  }
);

export default api;
```

## 2. CORS: Cross Origin Resource Sharing

CORS is a security mechanism implemented by browsers that controls how web pages from one origin can request resources from a different origin.

### How CORS Works

**Same-Origin Policy:**
- Default browser security measure
- Prevents scripts from one origin accessing resources from another origin
- Origin = Protocol + Domain + Port
  - `https://example.com:443` vs `https://api.example.com:443` → Different origins

**CORS Headers:**

**1. Simple Request:**
```
Request Headers:
  Origin: https://myapp.com

Response Headers:
  Access-Control-Allow-Origin: https://myapp.com
  Access-Control-Allow-Credentials: true
```

**2. Preflight Request (for complex requests):**
```
OPTIONS /api/users HTTP/1.1
Origin: https://myapp.com
Access-Control-Request-Method: POST
Access-Control-Request-Headers: Content-Type, Authorization

Response:
Access-Control-Allow-Origin: https://myapp.com
Access-Control-Allow-Methods: GET, POST, PUT, DELETE
Access-Control-Allow-Headers: Content-Type, Authorization
Access-Control-Max-Age: 86400
```

**Backend Configuration Examples:**

**Node.js/Express:**
```javascript
const cors = require('cors');

app.use(cors({
  origin: 'https://myapp.com',
  credentials: true,
  methods: ['GET', 'POST', 'PUT', 'DELETE'],
  allowedHeaders: ['Content-Type', 'Authorization'],
}));
```

**Spring Boot:**
```java
@Configuration
public class CorsConfig {
    @Bean
    public WebMvcConfigurer corsConfigurer() {
        return new WebMvcConfigurer() {
            @Override
            public void addCorsMappings(CorsRegistry registry) {
                registry.addMapping("/api/**")
                    .allowedOrigins("https://myapp.com")
                    .allowedMethods("GET", "POST", "PUT", "DELETE")
                    .allowedHeaders("*")
                    .allowCredentials(true);
            }
        };
    }
}
```

### Benefits

1. **Security:** Prevents unauthorized cross-origin requests
2. **Controlled Access:** Server explicitly allows specific origins
3. **Credential Protection:** Controls when cookies/auth headers are sent
4. **API Protection:** Prevents malicious sites from accessing your API

### XSS (Cross-Site Scripting)

**What is XSS?**
- Attacker injects malicious scripts into web pages viewed by other users
- Scripts execute in victim's browser with victim's privileges

**Types:**
1. **Stored XSS:** Malicious script stored in database
2. **Reflected XSS:** Script in URL parameters, reflected back
3. **DOM-based XSS:** Script manipulates DOM directly

**Prevention in React:**

**1. React automatically escapes values:**
```javascript
const userInput = "<script>alert('XSS')</script>";
return <div>{userInput}</div>; // Safe - rendered as text
```

**2. Dangerous scenarios:**
```javascript
// ❌ DANGEROUS - never do this
<div dangerouslySetInnerHTML={{ __html: userInput }} />

// ✅ Safe alternative - sanitize first
import DOMPurify from 'dompurify';

const sanitized = DOMPurify.sanitize(userInput);
<div dangerouslySetInnerHTML={{ __html: sanitized }} />
```

**3. URL handling:**
```javascript
// ❌ DANGEROUS
<a href={userInput}>Click</a>

// ✅ Safe - validate protocol
const isSafeUrl = (url) => {
  return url.startsWith('http://') || url.startsWith('https://');
};

{isSafeUrl(userInput) && <a href={userInput}>Click</a>}
```

**4. Content Security Policy (CSP):**
```html
<meta 
  http-equiv="Content-Security-Policy" 
  content="default-src 'self'; script-src 'self' https://trusted-cdn.com"
/>
```

### CSRF (Cross-Site Request Forgery)

**What is CSRF?**
- Attacker tricks authenticated user into executing unwanted actions
- Exploits user's authenticated session with a website

**Attack Example:**
```html
<!-- Malicious site -->
<img src="https://bank.com/transfer?to=attacker&amount=1000" />
```
If user is logged into bank.com, this request executes with their credentials.

**Prevention Strategies:**

**1. CSRF Tokens:**
```javascript
// Backend sends token
app.get('/api/csrf-token', (req, res) => {
  const token = generateCSRFToken();
  req.session.csrfToken = token;
  res.json({ csrfToken: token });
});

// Frontend includes token
const csrfToken = await fetchCSRFToken();

axios.post('/api/transfer', data, {
  headers: {
    'X-CSRF-Token': csrfToken,
  },
});
```

**2. SameSite Cookies:**
```javascript
// Backend sets cookie
res.cookie('session', sessionId, {
  httpOnly: true,
  secure: true,
  sameSite: 'strict', // or 'lax'
});
```

**3. Custom Headers:**
```javascript
// Browsers don't allow custom headers in simple CORS requests
axios.defaults.headers.common['X-Requested-With'] = 'XMLHttpRequest';
```

**4. Double Submit Cookie:**
```javascript
// Set CSRF token in cookie and require it in header
res.cookie('XSRF-TOKEN', token);

// Frontend reads cookie and sends in header
const token = getCookie('XSRF-TOKEN');
axios.defaults.headers.common['X-XSRF-TOKEN'] = token;
```

## 3. REST vs GraphQL

### When to Choose Each

**Choose REST when:**
- Simple, resource-based APIs
- Caching is important (HTTP caching works well)
- Multiple clients with different needs
- Team familiar with REST
- Need standard HTTP methods and status codes

**Choose GraphQL when:**
- Complex, nested data requirements
- Multiple clients need different data shapes
- Want to minimize over-fetching/under-fetching
- Real-time updates (subscriptions)
- Strong typing and schema validation needed

### REST Overview

**Characteristics:**
- Resource-based architecture
- Multiple endpoints for different resources
- Uses HTTP methods (GET, POST, PUT, DELETE)
- Server determines response structure
- Versioning often needed (v1, v2)

**Example:**
```javascript
// Fetch user
GET /api/users/123

// Fetch user's posts
GET /api/users/123/posts

// Fetch user's friends
GET /api/users/123/friends

// Create post
POST /api/posts
{
  "title": "New Post",
  "content": "Content here"
}
```

**REST API Implementation:**
```javascript
// React component using REST
function UserProfile({ userId }) {
  const [user, setUser] = useState(null);
  const [posts, setPosts] = useState([]);
  const [loading, setLoading] = useState(true);
  
  useEffect(() => {
    Promise.all([
      fetch(`/api/users/${userId}`).then(r => r.json()),
      fetch(`/api/users/${userId}/posts`).then(r => r.json()),
    ])
    .then(([userData, postsData]) => {
      setUser(userData);
      setPosts(postsData);
      setLoading(false);
    });
  }, [userId]);
  
  if (loading) return <div>Loading...</div>;
  
  return (
    <div>
      <h1>{user.name}</h1>
      <div>
        {posts.map(post => (
          <Post key={post.id} {...post} />
        ))}
      </div>
    </div>
  );
}
```

### GraphQL Overview

**Characteristics:**
- Single endpoint
- Client specifies exact data needed
- Strongly typed schema
- No over-fetching or under-fetching
- Built-in introspection
- Real-time with subscriptions

**Example:**
```javascript
// Single query for all data
query {
  user(id: "123") {
    name
    email
    posts {
      title
      content
      comments {
        text
        author {
          name
        }
      }
    }
    friends {
      name
      avatar
    }
  }
}
```

**GraphQL Implementation:**
```javascript
import { useQuery, gql } from '@apollo/client';

const GET_USER = gql`
  query GetUser($userId: ID!) {
    user(id: $userId) {
      name
      email
      posts {
        id
        title
        content
      }
    }
  }
`;

function UserProfile({ userId }) {
  const { loading, error, data } = useQuery(GET_USER, {
    variables: { userId },
  });
  
  if (loading) return <div>Loading...</div>;
  if (error) return <div>Error: {error.message}</div>;
  
  return (
    <div>
      <h1>{data.user.name}</h1>
      <div>
        {data.user.posts.map(post => (
          <Post key={post.id} {...post} />
        ))}
      </div>
    </div>
  );
}
```

**GraphQL Mutations:**
```javascript
const CREATE_POST = gql`
  mutation CreatePost($title: String!, $content: String!) {
    createPost(title: $title, content: $content) {
      id
      title
      content
      createdAt
    }
  }
`;

function CreatePostForm() {
  const [createPost, { loading, error }] = useMutation(CREATE_POST);
  
  const handleSubmit = async (e) => {
    e.preventDefault();
    await createPost({
      variables: {
        title: e.target.title.value,
        content: e.target.content.value,
      },
    });
  };
  
  return <form onSubmit={handleSubmit}>...</form>;
}
```

### Comparison Table

| Aspect | REST | GraphQL |
|--------|------|---------|
| **Endpoints** | Multiple (one per resource) | Single endpoint |
| **Data Fetching** | Fixed structure per endpoint | Client specifies exact needs |
| **Over-fetching** | Common (get all fields) | Eliminated |
| **Under-fetching** | Requires multiple requests | Single request for nested data |
| **Versioning** | URL versioning (v1, v2) | Schema evolution (no versions) |
| **Caching** | Built-in HTTP caching | Requires custom solution |
| **Learning Curve** | Lower | Steeper |
| **Tooling** | Standard HTTP tools | Specialized tools (GraphiQL, Apollo) |
| **Real-time** | WebSockets/SSE separate | Built-in subscriptions |
| **Type Safety** | Optional (with TypeScript) | Built-in schema types |
| **Network Calls** | Multiple for related data | Single call |
| **Response Size** | Often larger | Optimized |
| **Documentation** | Manual (Swagger/OpenAPI) | Auto-generated (schema) |
| **Error Handling** | HTTP status codes | 200 + errors in response |
| **File Upload** | Standard multipart | Requires special handling |
| **Monitoring** | Easy (HTTP logs) | Requires query analysis |
| **Performance** | Depends on optimization | Can be slower (complex queries) |

**Hybrid Approach:**
Some applications use both:
- REST for simple, cacheable operations
- GraphQL for complex, nested data requirements

**Example:**
```javascript
// Simple resource - REST
const products = await fetch('/api/products').then(r => r.json());

// Complex nested data - GraphQL
const userDashboard = await graphqlClient.query({
  query: GET_DASHBOARD_DATA,
  variables: { userId },
});
```
