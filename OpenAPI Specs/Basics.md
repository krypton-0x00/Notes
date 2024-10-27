# Parts of the spec file

For a simple server

server.js

```javascript
import express from 'express';

const app = express();
const port = 3000;

app.use(express.json());

let users = [
    { id: 1, name: 'John Doe' },
    { id: 2, name: 'Jane Doe' }
];

app.get('/users', (req, res) => {
    const { name } = req.query;

    if (name) {
        const filteredUsers = users.filter(user => user.name.toLowerCase().includes(name.toLowerCase()));
        res.json(filteredUsers);
    } else {
        res.json(users);
    }
});

app.listen(port, () => {
    console.log(`Server running on http://localhost:${port}`);
});
```

OpenAPI Spec

```javascript
openapi: 3.0.0
info:
  title: User API
  description: API to manage users
  version: "1.0.0"
servers:
  - url: http://localhost:3000
paths:
  /users:
    get:
      summary: Get a list of users
      description: Retrieves a list of users, optionally filtered by name.
      parameters:
        - in: query
          name: name
          schema:
            type: string
          required: false
          description: Name filter for user lookup.
      responses:
        '200':
          description: A list of users
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/User'
components:
  schemas:
    User:
      type: object
      properties:
        id:
          type: integer
          format: int64
          description: The unique identifier of the user.
        name:
          type: string
          description: The name of the user.
      required:
        - id
        - name
```
# How to create a spec
1. Write it by hand (bad, but still happens)

2. Auto generate it from your code

1. Easy in languages that have deep types like Rust
2. Slightly harder in languages like Go/Rust
3. Node.js has some libraries/codebases that let you do it

1. With express - [https://www.npmjs.com/package/express-openapi](https://www.npmjs.com/package/express-openapi) (highly verbose)
2. Without express - [https://github.com/lukeautry/tsoa](https://github.com/lukeautry/tsoa) 

5. Hono has a native implementation with zod - [https://hono.dev/snippets/zod-openapi](https://hono.dev/snippets/zod-openapi)
