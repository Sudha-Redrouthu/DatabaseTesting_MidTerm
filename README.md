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
