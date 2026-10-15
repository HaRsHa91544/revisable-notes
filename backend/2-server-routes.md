# Why do we need a Server?
- When applications wants to store the data in database for persistant storage.
- They need to access the database with credentials and without a server the client can access the credentials if **client is allowed to directly access the database** and misuse the credentials.
- To **validate and control the access to data and operations** of an user we need a system to manage them.
- Therefore to solve the above problems we need a computer (server) that can store the credentials, verify the user's identity and allow them to perform operations.