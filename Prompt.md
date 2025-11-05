# New Requirement 1

Add JWT Auth Middleware support for the Gin application with HS256 algorithm and secret `your-256-bit-secret`. Requirements:

- Add JWT verification for `/api/v1/*`, allow other URLs to pass through
- Return 401 status code and JSON format error message when verification fails
- Add functionality to obtain JWT based on username and password. The generated JWT token's payload sub should be username, with an expiration time of 30 days
- JWT token comes from the `Authorization` header field in the format `Bearer <token>`, and also supports passing JWT Token through the x-api-key query parameter

# New Requirement 2

Please add SQLite database support for the project. Specific requirements:

- Integrate SQLite database to ensure the project can use SQLite for data storage and querying.
- Create a users table, mainly including: ID, username, email, creation time, and other columns
- Create a user repository to implement basic CRUD (Create, Read, Update, Delete) operations
- Create a user handler to handle HTTP requests and support user CRUD operations


