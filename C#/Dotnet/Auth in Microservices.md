In a microservices architecture, authentication is often managed centrally by a dedicated **auth service**, which handles user login, token generation, and validation. Here's how you can design your authentication flow when your auth service uses **JWTs** and **cookies**:

---

### **1. Flow Overview**

1. **User Login**:
    
    - User sends credentials to the **Auth Service**.
    - The Auth Service validates credentials, generates a JWT, and sends it back in the response (often in a secure HTTP-only cookie).
2. **JWT in Cookie**:
    
    - The JWT is stored in a **secure, HTTP-only cookie** for the browser to include in subsequent requests automatically.
    - Alternatively, for mobile or non-browser clients, the JWT can be stored in local storage or directly passed in headers.
3. **Token Verification**:
    
    - Each microservice validates the JWT included in requests to ensure the user is authenticated.
4. **User Context**:
    
    - Microservices extract user data (claims) from the validated JWT to authorize actions.

---

### **2. Implementation Steps**

#### **Auth Service**

1. **Login Endpoint**:
    
    - Validate credentials.
    - Generate a JWT containing user claims (e.g., `id`, `role`, `permissions`).
    - Send the JWT in an **HTTP-only, Secure** cookie.
    
    Example (TypeScript with Express and `jsonwebtoken`):
    
    ```typescript
    import jwt from 'jsonwebtoken';
    import { Request, Response } from 'express';
    
    const SECRET = process.env.JWT_SECRET || 'your_secret';
    
    export const login = async (req: Request, res: Response) => {
        const { username, password } = req.body;
    
        // Validate credentials (e.g., check DB)
        const user = await validateUser(username, password);
        if (!user) {
            return res.status(401).json({ message: 'Invalid credentials' });
        }
    
        // Generate JWT
        const token = jwt.sign({ id: user.id, role: user.role }, SECRET, { expiresIn: '1h' });
    
        // Send JWT as a cookie
        res.cookie('auth_token', token, {
            httpOnly: true,
            secure: process.env.NODE_ENV === 'production',
            sameSite: 'strict',
        });
    
        return res.json({ message: 'Logged in successfully' });
    };
    ```
    
2. **Verify Endpoint (Optional)**:
    
    - Validate the JWT in incoming requests (useful for non-browser clients).

---

#### **Middleware for Microservices**

Each microservice needs middleware to validate the JWT.

1. **JWT Middleware**:
    
    - Extract the JWT from the cookie or `Authorization` header.
    - Verify it using the same secret (`JWT_SECRET`) as the Auth Service.
    
    Example (C#):
    
    ```csharp
    using Microsoft.AspNetCore.Http;
    using System.IdentityModel.Tokens.Jwt;
    using System.Linq;
    using System.Threading.Tasks;
    
    public class JwtMiddleware
    {
        private readonly RequestDelegate _next;
        private readonly string _jwtSecret;
    
        public JwtMiddleware(RequestDelegate next, IConfiguration config)
        {
            _next = next;
            _jwtSecret = config["JwtSecret"];
        }
    
        public async Task Invoke(HttpContext context)
        {
            var token = context.Request.Cookies["auth_token"] 
                ?? context.Request.Headers["Authorization"].FirstOrDefault()?.Split(" ").Last();
    
            if (token != null)
            {
                try
                {
                    var handler = new JwtSecurityTokenHandler();
                    var tokenValidationParameters = new TokenValidationParameters
                    {
                        ValidateIssuerSigningKey = true,
                        IssuerSigningKey = new SymmetricSecurityKey(Encoding.UTF8.GetBytes(_jwtSecret)),
                        ValidateIssuer = false,
                        ValidateAudience = false
                    };
    
                    var principal = handler.ValidateToken(token, tokenValidationParameters, out _);
                    context.User = principal;
                }
                catch
                {
                    // Invalid token, user is not authenticated
                }
            }
    
            await _next(context);
        }
    }
    ```
    
2. **Register Middleware**: Add the middleware in your microservice’s startup configuration.
    

---

#### **Token Sharing**

To ensure all microservices can verify JWTs:

- Use a **shared secret** (`JWT_SECRET`) for signing tokens.
- Alternatively, switch to **asymmetric encryption** (e.g., RSA) where the Auth Service signs tokens with a private key, and microservices validate them with a public key.

---

### **3. Authorization in Microservices**

Each microservice uses the claims in the JWT to enforce authorization.

1. **Extract User Info**:
    
    - Use middleware to extract claims like `id`, `role`, and `permissions` from the JWT.
2. **Role/Permission Checks**:
    
    - Implement role-based or permission-based authorization at the controller or service level.

---

### **4. Handling Expired Tokens**

1. **Short Expiry Tokens**:
    
    - Use a short-lived JWT (e.g., 15 minutes) for security.
    - Implement a **refresh token mechanism** to issue new tokens without requiring login.
2. **Refresh Token Flow**:
    
    - The Auth Service issues a refresh token alongside the JWT.
    - The refresh token is stored in an **HTTP-only cookie** or database.
    - When the JWT expires, the client sends the refresh token to get a new JWT.

---

### **5. Example Workflow**

#### **Login Flow**:

1. User logs in to the Auth Service and gets a JWT in a cookie.
2. Browser automatically includes the cookie in subsequent requests.

#### **Authenticated Request**:

1. User makes a request to a microservice.
2. The microservice extracts and validates the JWT.
3. If valid, the request proceeds with the user's identity and claims.

#### **Token Refresh**:

1. If the JWT expires, the client sends the refresh token to the Auth Service.
2. Auth Service validates the refresh token and issues a new JWT.

---

This architecture allows **secure, centralized authentication** while enabling microservices to remain stateless and independently scalable.