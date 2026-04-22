# Library Catalogue REST API Design

## 1. Topic

This project describes a RESTful API for a **Library Catalogue System**.  
The system is designed to manage books, authors, categories, and library copies, and to allow users to search and browse the catalogue.

---

## 2. Functional Requirements

1. The system must allow clients to create, read, update, and delete books.
2. The system must allow clients to create, read, update, and delete authors.
3. The system must allow clients to create, read, update, and delete categories.
4. The system must allow clients to create, read, update, and delete book copies.
5. The system must allow searching books by title.
6. The system must allow filtering books by author, category, language, and publication year.
7. The system must allow sorting book collections.
8. The system must support pagination for all collection endpoints.
9. The system must allow retrieving information about available and unavailable copies.
10. The system must support authentication for write operations.
11. The system must return meaningful errors for invalid requests.
12. The system must provide hypermedia links in responses.

---

## 3. Non-Functional Requirements

1. The API must follow REST principles.
2. The API must use JSON for requests and responses.
3. The API must be stateless.
4. The API must be secure and support token-based authentication.
5. The API must be scalable and support pagination for large collections.
6. The API must use meaningful HTTP status codes.
7. The API must support caching for read-only endpoints where appropriate.
8. The API documentation must be clear and unambiguous.

---

## 4. Model Description

### 4.1 Book
Represents a book in the library catalogue.

Fields:
- `id` : integer
- `title` : string
- `isbn` : string
- `description` : string
- `publicationYear` : integer
- `language` : string
- `authorId` : integer
- `categoryId` : integer

### 4.2 Author
Represents the author of a book.

Fields:
- `id` : integer
- `firstName` : string
- `lastName` : string
- `birthDate` : date
- `country` : string

### 4.3 Category
Represents a book category.

Fields:
- `id` : integer
- `name` : string
- `description` : string

### 4.4 Copy
Represents a physical or digital copy of a book.

Fields:
- `id` : integer
- `bookId` : integer
- `inventoryNumber` : string
- `status` : string (`AVAILABLE`, `BORROWED`, `LOST`, `RESERVED`)
- `location` : string

---

## 5. Operations Description

### Books
- Create a book
- Get all books
- Get book by id
- Update a book
- Delete a book
- Search and filter books

### Authors
- Create an author
- Get all authors
- Get author by id
- Update an author
- Delete an author

### Categories
- Create a category
- Get all categories
- Get category by id
- Update a category
- Delete a category

### Copies
- Create a copy
- Get all copies
- Get copy by id
- Update a copy
- Delete a copy
- Filter copies by status or book

---

## 6. Authentication

The API uses **Bearer JWT authentication** for protected operations.

### Header
```http
Authorization: Bearer <token>
JWT Payload Example
{
  "sub": "user123",
  "role": "LIBRARIAN",
  "exp": 1893456000
}
Access Rules
GET requests are available for authenticated and non-authenticated users.
POST, PUT, PATCH, DELETE require authentication.
Only users with role LIBRARIAN or ADMIN may create, update, or delete resources.
Authentication Errors
401 Unauthorized — token is missing or invalid
403 Forbidden — token is valid, but the user does not have enough permissions
7. Error Format

All errors use a common JSON format.

{
  "timestamp": "2026-04-22T12:00:00Z",
  "status": 404,
  "error": "Not Found",
  "message": "Book with id 15 was not found",
  "path": "/api/books/15"
}
8. REST API Description

Base URL:

/api
8.1 Books Collection
GET /api/books

Returns a paginated list of books.

Query Parameters
page — page number
size — items per page
sort — sorting field, for example title,asc
title — filter by title
authorId — filter by author
categoryId — filter by category
language — filter by language
publicationYear — filter by publication year
Example
GET /api/books?page=0&size=10&sort=title,asc&authorId=2
Response Codes
200 OK
Response Example
{
  "items": [
    {
      "id": 1,
      "title": "Clean Code",
      "isbn": "9780132350884",
      "publicationYear": 2008,
      "language": "English",
      "_links": {
        "self": { "href": "/api/books/1" },
        "author": { "href": "/api/authors/2" },
        "category": { "href": "/api/categories/1" },
        "copies": { "href": "/api/copies?bookId=1" }
      }
    }
  ],
  "page": 0,
  "size": 10,
  "totalItems": 1,
  "totalPages": 1,
  "_links": {
    "self": { "href": "/api/books?page=0&size=10" }
  }
}
POST /api/books

Creates a new book.

Request Body
{
  "title": "Clean Code",
  "isbn": "9780132350884",
  "description": "A handbook of agile software craftsmanship",
  "publicationYear": 2008,
  "language": "English",
  "authorId": 2,
  "categoryId": 1
}
Response Codes
201 Created
400 Bad Request
401 Unauthorized
403 Forbidden
409 Conflict
GET /api/books/{id}

Returns a single book by id.

Response Codes
200 OK
404 Not Found
PUT /api/books/{id}

Fully updates an existing book.

Response Codes
200 OK
400 Bad Request
401 Unauthorized
403 Forbidden
404 Not Found
PATCH /api/books/{id}

Partially updates an existing book.

Response Codes
200 OK
400 Bad Request
401 Unauthorized
403 Forbidden
404 Not Found
DELETE /api/books/{id}

Deletes a book.

Response Codes
204 No Content
401 Unauthorized
403 Forbidden
404 Not Found
8.2 Authors Collection
GET /api/authors

Returns a paginated list of authors.

Query Parameters
page
size
sort
country
lastName
Response Codes
200 OK
POST /api/authors

Creates a new author.

Response Codes
201 Created
400 Bad Request
401 Unauthorized
403 Forbidden
GET /api/authors/{id}
200 OK
404 Not Found
PUT /api/authors/{id}
200 OK
400 Bad Request
401 Unauthorized
403 Forbidden
404 Not Found
DELETE /api/authors/{id}
204 No Content
401 Unauthorized
403 Forbidden
404 Not Found
8.3 Categories Collection
GET /api/categories

Returns a paginated list of categories.

Query Parameters
page
size
sort
name
Response Codes
200 OK
POST /api/categories
201 Created
400 Bad Request
401 Unauthorized
403 Forbidden
GET /api/categories/{id}
200 OK
404 Not Found
PUT /api/categories/{id}
200 OK
400 Bad Request
401 Unauthorized
403 Forbidden
404 Not Found
DELETE /api/categories/{id}
204 No Content
401 Unauthorized
403 Forbidden
404 Not Found
8.4 Copies Collection
GET /api/copies

Returns a paginated list of copies.

Query Parameters
page
size
sort
bookId
status
location
Response Codes
200 OK
POST /api/copies

Creates a new copy.

Response Codes
201 Created
400 Bad Request
401 Unauthorized
403 Forbidden
GET /api/copies/{id}
200 OK
404 Not Found
PUT /api/copies/{id}
200 OK
400 Bad Request
401 Unauthorized
403 Forbidden
404 Not Found
DELETE /api/copies/{id}
204 No Content
401 Unauthorized
403 Forbidden
404 Not Found
9. Meaningful Status Codes
200 OK — successful retrieval or update
201 Created — resource created
204 No Content — resource deleted successfully
400 Bad Request — invalid request body or parameters
401 Unauthorized — no token or invalid token
403 Forbidden — insufficient permissions
404 Not Found — resource not found
409 Conflict — duplicate ISBN or conflicting update
500 Internal Server Error — unexpected server error
10. Pagination

All collection endpoints are paginated:

GET /api/books
GET /api/authors
GET /api/categories
GET /api/copies

Pagination parameters:

page
size
sort

This is required because collection responses may become large.

11. Caching

Caching is used only for safe read-only endpoints.

Cached Endpoints
GET /api/books
GET /api/books/{id}
GET /api/authors
GET /api/authors/{id}
GET /api/categories
GET /api/categories/{id}
Not Cached
POST
PUT
PATCH
DELETE
GET /api/copies can be short-lived cached or not cached depending on frequent availability changes
Example Headers
Cache-Control: public, max-age=300
ETag: "book-15-v3"
Last-Modified: Tue, 22 Apr 2026 10:00:00 GMT

Reason:

catalogue information changes rarely
copies availability may change more often, so copy-related endpoints should have very short cache or no cache
12. Richardson Maturity Model

This design applies the Richardson Maturity Model:

Level 0

Uses HTTP as transport.

Level 1

Uses separate URIs for resources:

/api/books
/api/authors
/api/categories
/api/copies
Level 2

Uses HTTP methods properly:

GET
POST
PUT
PATCH
DELETE

Also uses meaningful HTTP status codes.

Level 3

Uses HATEOAS links in resource responses.

Example:

{
  "id": 1,
  "title": "Clean Code",
  "_links": {
    "self": { "href": "/api/books/1" },
    "author": { "href": "/api/authors/2" },
    "category": { "href": "/api/categories/1" },
    "copies": { "href": "/api/copies?bookId=1" }
  }
}
13. Example Resource Relationships
One author can write many books
One category can contain many books
One book can have many copies
Each copy belongs to exactly one book
14. Self-Evaluation
Functional and non-functional requirements

Both functional and non-functional requirements are provided and clear.

Model description

The model is complete and unambiguous.

Operations description

All operation descriptions are complete.

Meaningful status codes

All status codes are meaningful and mapped to their description.

Richardson model application

HATEOAS is used, and the API matches level 3.

Authentication

Authentication method and authentication errors are described.

Pagination

All collection endpoints are paginated.

Caching

Caching is described correctly, and safe GET endpoints are cached where appropriate.
