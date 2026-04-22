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
