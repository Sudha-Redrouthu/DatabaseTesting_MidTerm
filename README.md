# Team Responsibilities

## Member 1 karthik: SQL Implementation

*Tasks:*
- SQL Implementation
- Data Modeling: Creating tables, relationships, and constraints.
- Data Insertion: Adding initial data for testing or production use.
- Implement CRUD operations for one table.
- Ensure code readability and include code blocks in markdown.
- Test SQL queries to confirm they meet project requirements.

## Member 2- Sudha: Database Design, Code and Interface Development

*Tasks:*
- Define tables and attributes for the database.
- Develop a TypeScript interface for modifying tables.
- Create SQL queries for:
  - Identifying Power Writers.
  - Identifying Loyal Customers.
  - Finding Well Reviewed books.
  - Determining the most popular genre by sales.
  - Retrieving the 10 most recent customer reviews.
  
## Member 3 Mohan: Developing SQL Code for Database Creation (DDL), Writing DML Statements and Documentation

*Tasks:*
- Develop SQL code for database creation (DDL).
- Write DML statements to insert sample data.
- Write the markdown document.
- Organize the document with clear headers.
- Create tables in markdown format to display:
  - Database tables and their attributes.
  - Data types and attribute descriptions.
- Ensure all content adheres to formatting guidelines.# DatabaseTesting_MidTerm

## Database Schema

### Books Table

| Attribute           | Type                         | Description                                      |
|---------------------|------------------------------|--------------------------------------------------|
| BookID              | INT                          | Primary key for identifying books.               |
| BookName            | VARCHAR(50)                  | The name of the book.                            |
| BookGenre           | VARCHAR(50)                  | Genre of the book (e.g.,     Mystery, Romance).  |
| BookPrice           | DECIMAL(10, 2)               | Price of the book.                               |
| BookQuantity        | INT                          | Number of copies available in inventory.         |
| BookNoOfReviews     | INT                          | Number of reviews received for the book          |
| BookPublishedDate   | DATE                         | Date when the book was published.                |
| BookNoOfSales       | INT                          | Number of sales for the book (default is 0).     |
| BookType            | VARCHAR(10)                  | Type of book                                     |
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

### Genre Table

| Column    | Data Type      | Description                |
|-----------|----------------|----------------------------|
| genre_id  | INT PRIMARY KEY| Unique ID for each genre   |
| genre_name      | VARCHAR(50)   | Name of the genre          |
| genre_description | TEXT         | Genre description          |

### Order Table

| Column           | Data Type      | Description                |
|------------------|----------------|----------------------------|
| order_id         | INT PRIMARY KEY| Unique ID for each order   |
| customer_id      | INT            | Customer ID (FK)           |
| order_date       | DATE           | Date of the order          |
| total_amount     | DECIMAL(10,2)  | Total order amount         |
| shipping_address | VARCHAR(255)   | Shipping address           |
| order_status     | VARCHAR(50)    | Status of the order        |

### Items_Purchased Table

| Column           | Data Type      | Description                |
|------------------|----------------|----------------------------|
| purchased_item_id| INT PRIMARY KEY| Unique ID for each purchase|
| order_id         | INT            | Order ID (FK)              |
| book_id          | INT            | Book ID (FK)               |
| book_quantity         | INT            | Quantity purchased         |
| book_price            | NUMERIC(10,2)  | Price of the item          |

### Reviews Table

| Column       | Data Type      | Description                |
|--------------|----------------|----------------------------|
| review_id    | INT PRIMARY KEY| Unique ID for each review  |
| customer_id  | INT            | Customer ID (FK)           |
| book_id      | INT            | Book ID (FK)               |
| rating       | INT | Rating (1 to 5)|
| comment      | TEXT           | Review comment             |
| review_date  | DATE           | Date of the review         |
| votes        | INT            | Number of helpful votes    |


CREATE TABLE Books (
    BookID INT PRIMARY KEY,
    BookName VARCHAR(50) NOT NULL,
    BookGenre VARCHAR(50),
    BookPrice DECIMAL(10, 2) NOT NULL,
    BookQuantity INT NOT NULL,
    BookNoOfReviews INT DEFAULT 0,
    BookPublishedDate DATE,
    BookNoOfSales INT DEFAULT 0,
    BookType ENUM('Physical', 'E-Book', 'Audiobook'),
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
    CustomerEmail VARCHAR(100) UNIQUE NOT NULL
);


CREATE TABLE Publishers (
    PublisherID INT PRIMARY KEY,
    PublisherName VARCHAR(150) NOT NULL,
    PublisherContactInfo VARCHAR(150)
);


CREATE TABLE Authors (
    AuthorID INT PRIMARY KEY,
    AuthorName VARCHAR(150) NOT NULL,
    PrimaryGenre VARCHAR(100),
    NoOfBooks INT DEFAULT 0
);


CREATE TABLE Sales (
    SaleID INT PRIMARY KEY,
    BookID INT,
    BookGenre VARCHAR(100),
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
    FeedbackType ENUM('Complaint', 'Suggestion', 'Praise') NOT NULL,
    FeedbackDate DATE NOT NULL,
    FOREIGN KEY (CustomerID) REFERENCES Customers(CustomerID)
);
