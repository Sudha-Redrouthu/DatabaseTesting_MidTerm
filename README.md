# DatabaseTesting_MidTerm
## Database Schema

### Author Table

| Column       | Data Type      | Description                |
|--------------|----------------|----------------------------|
| author_id    | INT PRIMARY KEY| Unique ID for each author  |
| author_name  | VARCHAR(100)   | Name of the author         |
| author_description  | TEXT           | Author's description       |
| total_books  | INT            | Total number of books      |

### Genre Table

| Column    | Data Type      | Description                |
|-----------|----------------|----------------------------|
| genre_id  | INT PRIMARY KEY| Unique ID for each genre   |
| genre_name      | VARCHAR(50)   | Name of the genre          |
| genre_description | TEXT         | Genre description          |

### Publisher Table

| Column       | Data Type      | Description                |
|--------------|----------------|----------------------------|
| publisher_id | INT PRIMARY KEY| Unique ID for each publisher|
| publisher_name         | VARCHAR(50)   | Name of the publisher      |
| publisher_address      | VARCHAR(100)   | Publisher's address        |

### Book Table

| Column       | Data Type      | Description                |
|--------------|----------------|----------------------------|
| book_id      | INT PRIMARY KEY| Unique ID for each book    |
| book_name      | VARCHAR(100)   | Title of the book          |
| genre_id     | INT            | Genre ID (FK)              |
| publish_date | DATE           | Publication date           |
| author_id    | INT            | Author ID (FK)             |
| publisher_id | INT            | Publisher ID (FK)          |
| book_format       | VARCHAR(20)    | Format (Physical, eBook, Audio) |
| book_price        | DECIMAL(10,2)  | Price of the book          |
| ISBN         | VARCHAR(20)    | ISBN number                |

### Customer Table

| Column          | Data Type      | Description                |
|------------------|----------------|----------------------------|
| customer_id      | INT PRIMARY KEY| Unique ID for each customer|
| customer_name             | VARCHAR(100)   | Name of the customer       |
| customer_email            | VARCHAR(100)   | Email of the customer      |
| loyalty_points   | INT            | Loyalty points earned      |
| membership_date  | DATE           | Date of membership         |
| amount_spent     | DECIMAL(10,2)  | Total amount spent         |

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
