# CORS — First-Principles Revision Notes

## 1. What problem does CORS solve?

CORS (Cross-Origin Resource Sharing) allows a server to control which origins can read its responses through a browser.

Imagine a malicious website tries to read your bank data using your browser. The browser's same-origin policy restricts cross-origin reading by default. CORS lets the bank selectively permit trusted origins.

Remember: CORS is enforced by the browser, not by Postman or Node.js.

## 2. What is an origin?

An origin consists of three parts:

Origin = Scheme + Host + Port

| URL A                   | URL B                       | Result       |
| ----------------------- | --------------------------- | ------------ |
| `http://localhost:3000` | `http://localhost:3000/api` | Same origin  |
| `http://localhost:3000` | `http://localhost:5000`     | Cross-origin |
| `https://app.com`       | `https://api.app.com`       | Cross-origin |
| `http://localhost:3000` | `http://127.0.0.1:3000`     | Cross-origin |

Important: The path doesn't determine the origin. The scheme, host, and port do.

## 3. How does CORS work?

Suppose your frontend is `http://localhost:5173` and your backend is `http://localhost:3000`.

1\. Frontend

Sends an HTTP request

2\. Backend

Processes the request and sends a response

3\. Browser

Checks CORS permission before exposing the response to JavaScript

If the required CORS permission is missing or invalid, the browser blocks JavaScript from reading the response.

The server may have sent the response successfully. CORS doesn't necessarily prevent the request from reaching the server.

## 4. The most important CORS header

```
Access-Control-Allow-Origin: http://localhost:5173
```

This tells the browser that JavaScript running at that origin is allowed to read the response, subject to other CORS requirements.

### How many origins can this header contain?

Normally, it contains one origin or `*`.

Incorrect:

```
Access-Control-Allow-Origin: http://localhost:5173,http://127.0.0.1:5500
```

If you want to allow multiple origins, keep them in a server-side list. Check the incoming `Origin` header and return only the matching, permitted origin.

## 5. What is CORS preflight?

Some cross-origin requests require the browser to ask permission before sending the actual request.

For example, a cross-origin `DELETE` request triggers a preflight.

The browser sends:

```
OPTIONS /users
Origin: http://localhost:5173
Access-Control-Request-Method: DELETE
```

The server needs to permit the origin and method in its response.

For example:

```
Access-Control-Allow-Origin: http://localhost:5173
Access-Control-Allow-Methods: GET, POST, DELETE
```

If preflight succeeds, the browser can send the actual request. If it fails, the browser does not send that actual request.

Remember: Not every cross-origin request triggers preflight.

## 6. My Express CORS middleware

This is the pattern you learned for allowing your two development origins:

```
const allowedOrigins = [
    'http://localhost:5173',
    'http://127.0.0.1:5500'
];

const cors = (req, res, next) => {
    const origin = req.headers.origin;

    if (allowedOrigins.includes(origin)) {
        res.setHeader('Access-Control-Allow-Origin', origin);
        res.setHeader('Vary', 'Origin');
    }

    res.setHeader(
        'Access-Control-Allow-Methods',
        'GET, POST, PUT, PATCH, DELETE'
    );

    if (req.method === 'OPTIONS') {
        return res.sendStatus(204);
    }

    next();
};

export default cors;
```

What each part does:

- `allowedOrigins`: stores the origins you trust.
- `req.headers.origin`: reads the request's origin.
- `includes(origin)`: checks whether that origin is permitted.
- `Access-Control-Allow-Origin`: grants browser reading permission to the matching origin.
- `Access-Control-Allow-Methods`: lists permitted methods for CORS requests.
- `OPTIONS`: handles the preflight request.
- `next()`: passes normal requests to the next middleware or route.

`Vary: Origin` helps caches distinguish responses generated for different origins.

This is a learning example. If your frontend sends custom headers, you'll also need to configure `Access-Control-Allow-Headers` appropriately.

## 7. What CORS does NOT do

| CORS does not…                        | Why?                                                               |
| ------------------------------------- | ------------------------------------------------------------------ |
| Authenticate users                    | Use authentication.                                                |
| Stop requests from Postman or Node.js | Those clients don't enforce browser CORS rules.                    |
| Prevent every cross-origin request    | Some requests can be sent even when their responses can't be read. |
| Replace CSRF protection               | Cookie-based cross-site requests can still create risks.           |

## 8. Final revision — remember these five things

1. Purpose: CORS controls which origins can read cross-origin responses through a browser.
2. Origin: Scheme + host + port.
3. Enforcement: The browser enforces CORS.
4. Permission: `Access-Control-Allow-Origin` identifies the allowed origin for that response.
5. Preflight: Some requests require an `OPTIONS` permission check before the actual request.

The core mental model is simple: The server sends a response and declares CORS permissions; the browser decides whether JavaScript can read that response.