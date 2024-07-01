#  SENG8071- DatabaseTesting- Midterm Assignment
### GitHub URL- https://github.com/Sudha-Redrouthu/DatabaseTesting_MidTerm/ 
# Team Responsibilities

## Member 1- karthik: SQL Implementation

*Tasks:*
- SQL Implementation
- Data Modeling: Creating tables, relationships, and constraints
- Data Insertion: Adding initial data for testing
- Implement CRUD operations for one table
- Test SQL queries to confirm they meet project requirements

## Member 2- Sudha: Database Design, Code and Interface Development

*Tasks:*
- Define tables and attributes for the database
- Develop a TypeScript interface for modifying tables
- Create SQL queries for:
  - Identifying Power Writers
  - Identifying Loyal Customers
  - Finding Well Reviewed books
  - Determining the most popular genre by sales
  - Retrieving the 10 most recent customer reviews
  
## Member 3- Mohan: Developing SQL Code for Database Creation (DDL), Writing DML Statements and Documentation

*Tasks:*
- Develop SQL code for DDL
- Write DML statements
- Write the markdown document
- Organize the document with clear headers
- Create tables in markdown format to display
- Ensure all content adheres to formatting guidelines.

## Online Bookstore Database Schema

### Books Table

| Attribute           | Type                         | Description                                      |
|---------------------|------------------------------|--------------------------------------------------|
| BookID              | INT                          | Primary key for identifying books.               |
| BookName            | VARCHAR(50)                  | The name of the book.                            |
| BookGenre           | VARCHAR(50)                  | Genre of the book                                |
| BookPrice           | DECIMAL(10, 2)               | Price of the book                                |
| BookQuantity        | INT                          | Number of copies available in inventory.         |
| BookNoOfReviews     | INT                          | Number of reviews received for the book          |
| BookPublishedDate   | DATE                         | Date when the book was published.                |
| BookNoOfSales       | INT                          | Number of sales for the book                     |
| BookType            | VARCHAR(10)                  | Type of book(Pysical, Ebook..etc)                |
| AuthorID            | INT                          | Foreign key referencing the Authors table.       |
| PublisherID         | INT                          | Foreign key referencing the Publishers table.    |

### Customers Table

| Attribute        | Type            | Description                                      |
|------------------|-----------------|--------------------------------------------------|
| CustomerID       | INT             | Primary key for identifying customers.           |
| CustomerName     | VARCHAR(50)     | Name of the customer.                            |
| PurchaseHistory  | TEXT            | Details of the customer's purchase history.      |
| AmountSpent      | DECIMAL(10, 2)  | Total amount spent by the customer               |
| CustomerPhNo     | VARCHAR(15)     | Phone number of the customer.                    |
| CustomerEmail    | VARCHAR(50)     | Unique email address of the customer.            |

### Authors Table

| Attribute       | Type            | Description                                      |
|-----------------|-----------------|--------------------------------------------------|
| AuthorID        | INT             | Primary key for identifying authors.             |
| AuthorName      | VARCHAR(50)     | Name of the author.                              |
| PrimaryGenre    | VARCHAR(50)     | Primary genre associated with the author         |
| NoOfBooks       | INT             | Number of books authored by the author           |

### Publishers Table

| Attribute            | Type            | Description                                      |
|----------------------|-----------------|--------------------------------------------------|
| PublisherID          | INT             | Primary key for identifying publishers.          |
| PublisherName        | VARCHAR(50)     | Name of the publisher.                           |
| PublisherContactInfo | VARCHAR(100)    | Contact information for the publisher.           |

### Sales Table

| Attribute       | Type            | Description                                      |
|-----------------|-----------------|--------------------------------------------------|
| SaleID          | INT             | Primary key for identifying sales transactions.  |
| BookID          | INT             | Foreign key referencing the Books table.         |
| BookGenre       | VARCHAR(50)     | Genre of the book sold.                          |
| PublisherID     | INT             | Foreign key referencing the Publishers table.    |
| AuthorID        | INT             | Foreign key referencing the Authors table.       |
| QuantitySold    | INT             | Number of copies sold.                           |
| TotalAmount     | DECIMAL(10, 2)  | Total amount generated from the sale.            |
| CustomerID      | INT             | Foreign key referencing the Customers table.     |
| SaleDate        | DATE            | Date of the sale.                                |

### Reviews Table

| Attribute       | Type            | Description                                      |
|-----------------|-----------------|--------------------------------------------------|
| ReviewID        | INT             | Primary key for identifying reviews.             |
| BookID          | INT             | Foreign key referencing the Books table.         |
| AuthorID        | INT             | Foreign key referencing the Authors table.       |
| CustomerID      | INT             | Foreign key referencing the Customers table.     |
| PublisherID     | INT             | Foreign key referencing the Publishers table.    |
| ReviewComment   | TEXT            | Comments provided by the reviewer.               |
| Ratings         | INT             | Ratings given by the customers                   |

### Inventory Table

| Attribute       | Type            | Description                                      |
|-----------------|-----------------|--------------------------------------------------|
| OrderID         | INT             | Primary key for identifying orders.              |
| OrderDate       | DATE            | Date when the order was placed.                  |
| OrderCost       | DECIMAL(10, 2)  | Cost of the order.                               |
| OrderDetails    | TEXT            | Details of the order.                            |

### Feedback Table
| Column         | Data Type      | Description                                        |
|----------------|----------------|------------------------------------------------    |
| FeedbackID     | INT PRIMARY KEY| Unique ID for each feedback                        |
| CustomerID     | INT            | ID of the customer (FK) referencing customer Table |
| FeedbackDetails| TEXT           | Details of the feedback                            |
| FeedbackType   | VARCHAR(255)   | Type of feedback (Complaint, Suggestion, Request)  |
| FeedbackDate   | DATE           | Date of the feedback                               |

## DB Creation Scripts

```sql
CREATE TABLE Books (
    BookID INT PRIMARY KEY,
    BookName VARCHAR(50) NOT NULL,
    BookGenre VARCHAR(50),
    BookPrice DECIMAL(10, 2) NOT NULL,
    BookQuantity INT NOT NULL,
    BookNoOfReviews INT DEFAULT 0,
    BookPublishedDate DATE,
    BookNoOfSales INT DEFAULT 0,
    BookType VARCHAR(10) CHECK (BookType IN ('Physical', 'E-Book', 'Audiobook')),
    AuthorID INT,
    PublisherID INT,
    FOREIGN KEY (AuthorID) REFERENCES Authors(AuthorID),
    FOREIGN KEY (PublisherID) REFERENCES Publishers(PublisherID)
);

CREATE TABLE Customers (
    CustomerID INT PRIMARY KEY,
    CustomerName VARCHAR(50) NOT NULL,
    PurchaseHistory TEXT,
    AmountSpent DECIMAL(10, 2) DEFAULT 0.00,
    CustomerPhNo VARCHAR(15),
    CustomerEmail VARCHAR(50) UNIQUE NOT NULL
);

CREATE TABLE Publishers (
    PublisherID INT PRIMARY KEY,
    PublisherName VARCHAR(50) NOT NULL,
    PublisherContactInfo VARCHAR(100)
);

CREATE TABLE Authors (
    AuthorID INT PRIMARY KEY,
    AuthorName VARCHAR(50) NOT NULL,
    PrimaryGenre VARCHAR(50),
    NoOfBooks INT DEFAULT 0
);

CREATE TABLE Sales (
    SaleID INT PRIMARY KEY,
    BookID INT,
    BookGenre VARCHAR(50),
    PublisherID INT,
    AuthorID INT,
    QuantitySold INT NOT NULL,
    TotalAmount DECIMAL(10, 2) NOT NULL,
    CustomerID INT,
    SaleDate DATE,
    FOREIGN KEY (BookID) REFERENCES Books(BookID),
    FOREIGN KEY (PublisherID) REFERENCES Publishers(PublisherID),
    FOREIGN KEY (AuthorID) REFERENCES Authors(AuthorID),
    FOREIGN KEY (CustomerID) REFERENCES Customers(CustomerID)
);

CREATE TABLE Reviews (
    ReviewID INT PRIMARY KEY,
    BookID INT,
    AuthorID INT,
    CustomerID INT,
    PublisherID INT,
    ReviewComment TEXT,
    Ratings INT CHECK (Ratings BETWEEN 1 AND 5),
    FOREIGN KEY (BookID) REFERENCES Books(BookID),
    FOREIGN KEY (AuthorID) REFERENCES Authors(AuthorID),
    FOREIGN KEY (CustomerID) REFERENCES Customers(CustomerID),
    FOREIGN KEY (PublisherID) REFERENCES Publishers(PublisherID)
);

CREATE TABLE Inventory (
    OrderID INT PRIMARY KEY,
    OrderDate DATE NOT NULL,
    OrderCost DECIMAL(10, 2) NOT NULL,
    OrderDetails TEXT,
    ReceivedDate DATE
);

CREATE TABLE Feedback (
    FeedbackID INT PRIMARY KEY,
    CustomerID INT,
    FeedbackDetails TEXT,
    FeedbackType VARCHAR(255) CHECK (FeedbackType IN ('Complaint', 'Suggestion', 'Request')) NOT NULL,
    FeedbackDate DATE NOT NULL,
    FOREIGN KEY (CustomerID) REFERENCES Customers(CustomerID)
);
```
## DDL for Reviews Table

### Create

```sql
CREATE TABLE Reviews (
    ReviewID INT PRIMARY KEY,
    BookID INT,
    AuthorID INT,
    CustomerID INT,
    PublisherID INT,
    ReviewComment TEXT,
    Ratings INT CHECK (Ratings BETWEEN 1 AND 5),
    FOREIGN KEY (BookID) REFERENCES Books(BookID),
    FOREIGN KEY (AuthorID) REFERENCES Authors(AuthorID),
    FOREIGN KEY (CustomerID) REFERENCES Customers(CustomerID),
    FOREIGN KEY (PublisherID) REFERENCES Publishers(PublisherID)
);
```
### Alter
```sql
ALTER TABLE Reviews ADD COLUMN ReviewTitle VARCHAR(100);
```
### Drop
```sql
DROP TABLE Reviews;
```
### Truncate
```sql
TRUNCATE TABLE Reviews;
```
## DML for Reviews Table
### Insert
```sql
INSERT INTO Reviews (ReviewID, BookID, AuthorID, CustomerID, PublisherID, ReviewComment, Ratings) VALUES (1, 2, 3, 4, 5, 'Good', 5);
```
### Select
```sql
SELECT * FROM Reviews WHERE ReviewID = 1;
```
### Update
```sql
UPDATE Reviews SET ReviewComment = 'Excellent', Ratings = 4 WHERE ReviewID = 1;
```
### Delete
```sql
DELETE FROM Reviews WHERE ReviewID = 1;
```
## Sql queries for the listed requirements. 
### 1. Power writers with more than X books in the same genre published within the last X years 
```sql
SELECT Authors.AuthorID, Authors.AuthorName, Books.BookGenre, COUNT(Books.BookID) AS BooksPublished
FROM Authors JOIN Books ON Authors.AuthorID = Books.AuthorID WHERE Books.BookPublishedDate >= CURRENT_DATE - INTERVAL '5 year'
GROUP BY Authors.AuthorID, Authors.AuthorName, Books.BookGenre HAVING COUNT(Books.BookID) > 1;
```
### 2. Loyal Customers who has spent more than X dollars in the last year 
```sql
SELECT CustomerName, AmountSpent FROM Customers WHERE AmountSpent > 100 AND CustomerID IN (SELECT CustomerID 
FROM Sales WHERE SaleDate > CURRENT_DATE - INTERVAL '2 year');
```
### 3. Well Reviewed books that has a better user rating than average
```sql
SELECT BookName, ROUND(AVG(Ratings), 2) AS AverageRating FROM Books JOIN Reviews ON Books.BookID = Reviews.BookID
GROUP BY BookName HAVING AVG(Ratings) > (SELECT AVG(Ratings) FROM Reviews);
```
### 4. The most popular genre by sales
```sql
SELECT BookGenre, SUM(QuantitySold) AS TotalSales FROM Sales GROUP BY BookGenre ORDER BY TotalSales DESC
LIMIT 1;
```
### 5. The 10 most recent posted reviews by Customers
```sql
SELECT Customers.CustomerName, Books.BookName, Reviews.ReviewComment, Reviews.Ratings, Reviews.ReviewDate FROM Reviews
JOIN Customers ON Reviews.CustomerID = Customers.CustomerID JOIN Books ON Reviews.BookID = Books.BookID ORDER BY Reviews.ReviewDate DESC LIMIT 10;
```
## Typescript interface that will allow modification to a table- Choosen Reviews Table
```sql
import { Pool } from 'pg';

const pool = new Pool({
  user: 'postgres',
  host: 'localhost',
  database: 'dbtesting',
  password: '****',
  port: 5432,
});

interface Review {
  ReviewID: number;
  BookID: number;
  AuthorID: number;
  CustomerID: number;
  PublisherID: number;
  ReviewComment: string;
  Ratings: number;
}

async function createReview(review: Review) {
  const query = `
    INSERT INTO Reviews (ReviewID, BookID, AuthorID, CustomerID, PublisherID, ReviewComment, Ratings)
    VALUES ($1, $2, $3, $4, $5, $6, $7)
    RETURNING *;
  `;
  const values = [
    review.ReviewID, review.BookID, review.AuthorID, review.CustomerID,
    review.PublisherID, review.ReviewComment, review.Ratings,
  ];
  const result = await pool.query(query, values);
  return result.rows[0];
}

async function readReview(reviewID: number) {
  const query = 'SELECT * FROM Reviews WHERE ReviewID = $1;';
  const result = await pool.query(query, [reviewID]);
  return result.rows[0];
}

async function updateReview(reviewID: number, updates: Partial<Review>) {
  const fields = [];
  const values = [reviewID];
  let idx = 2;
  for (const key in updates) {
    fields.push(`${key} = $${idx}`);
    values.push((updates as any)[key]);
    idx++;
  }

  const query = `
    UPDATE Reviews
    SET ${fields.join(', ')}
    WHERE ReviewID = $1
    RETURNING *;
  `;
  const result = await pool.query(query, values);
  return result.rows[0];
}

async function deleteReview(reviewID: number) {
  const query = 'DELETE FROM Reviews WHERE ReviewID = $1 RETURNING *;';
  const result = await pool.query(query, [reviewID]);
  return result.rows[0];
}

async function main() {
  try {
    // Create a new review
    const newReview: Review = {
      ReviewID: 1,
      BookID: 1,
      AuthorID: 1,
      CustomerID: 1,
      PublisherID: 1,
      ReviewComment: 'Great book!',
      Ratings: 5,
    };

    const createdReview = await createReview(newReview);
    console.log('Created Review:', createdReview);

    // Read a review
    const review = await readReview(1);
    console.log('Read Review:', review);

    // Update a review
    const updatedReview = await updateReview(1, { ReviewComment: 'Excellent book!', Ratings: 4 });
    console.log('Updated Review:', updatedReview);

    // Delete a review
    const deletedReview = await deleteReview(1);
    console.log('Deleted Review:', deletedReview);
  } catch (err) {
    console.error('An error occurred:', err);
  }
}

main().catch(err => console.error('Unexpected error occurred:', err));

```
