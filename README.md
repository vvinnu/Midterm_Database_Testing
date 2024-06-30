# Midterm Assignment - SENG8071 - Database Testing

## Team Members and their Duties

|        Member Name           |                                Duties Assigned                                                                         |
| ---------------------------  | ---------------------------------------------------------------------------------------------------------------------- |
| Reshma_Reddy_Nalamalapu      | Design the schema for the tables required, write code for creating the tables and also DDL queries for customers table |
| Ashlin_Peediyakkal_Michael   | Write SQL queries for DML and the project requirements mentioned in the assignment for customers table                 |
| Vineeth_Kanoor               | Create TypeScript interface for customers table with the CRUD operations                                               |

## Database Design

#### Tables for the Online Book Store Database

Authors, Publishers, Customers, Books, Reviews and Orders

### Authors Table
| Attribute       | Data Type    | Description        |
| --------------- | ------------ | ------------------ |
| author_id       | SERIAL       | Primary Key        |
| author_name     | VARCHAR      | Name of the author |

### Publishers Table
| Attribute         | Data Type    | Description              |
| ----------------- | ------------ | ------------------------ |
| publisher_id      | SERIAL       | Primary Key              |
| publisher_name    | VARCHAR      | Name of the publisher    |
| publisher_email   | VARCHAR      | Email of the publisher   |
| publisher_address | VARCHAR      | Address of the publisher |

### Customers Table
| Attribute            | Data Type     | Description                                  |
| ---------------------| ------------- | -------------------------------------------- |
| customer_id          | SERIAL        | Primary key                                  |
| first_name           | VARCHAR       | First name of the customer                   |
| last_name            | VARCHAR       | Last name of the customer                    |
| customer_email       | VARCHAR       | Email of the customer                        |
| total_amount_spent   | DECIMAL       | Total amount spent by the customer on buying |
| date_joined          | DATE          | Customer joining date in our platform        |

### Books Table
| Attribute            | Data Type     | Description                                              |
| ---------------------| ------------- | -------------------------------------------------------- |
| book_id              | SERIAL        | Primary Key                                              |
| book_name            | VARCHAR       | Name of the book                                         |
| book_genre           | VARCHAR       | Genre of the book such as Adventure,Scifi,Romantic       |
| book_type            | VARCHAR       | Type of the book - Audiobook, ebook and Physical         |
| book_publication_date| DATE          | Date at the which book is published                      |
| book_publisher_id    | INT           | Foreign key to publishers table                          |
| book_author_id       | INT           | Foreign Key to authors table                             |
| book_language        | VARCHAR       | Language of the book                                     |
| book_average_rating  | FLOAT         | Average rating of the book from data using reviews table |
| book_price           | DECIMAL       | price of the book                                        |


### Reviews Table
| Attribute       | Data Type    | Description                     |
| --------------- | ------------ | --------------------------------|
| review_id       | SERIAL       | Primary Key                     |
| customer_id     | INT          | Foreign key to customers table  |
| book_id         | INT          | Foreign key to books table      |
| rating          | INT          | Ratings given by the customer   |
| review_text     | TEXT         | Review text written by customer |
| review_date     | DATE         | Date of the review written      |

### Orders Table
| Attribute       | Data Type     | Description                           |
| --------------- | ------------- | ------------------------------------- |
| order_id        | SERIAL        | Primary key                           |
| order_date      | DATE          | Date of the book purchased            |
| customer_id     | INT           | Foreign key to customers table        |
| book_id         | INT           | Foreign key to books table            |
| order_amount    | DECIMAL       | Total amount of the customer purchase |
| order_quantity  | INT           | No of books ordered                   |


## SQL code for creating the tables we defined above

```sql
CREATE TABLE Authors (
    author_id SERIAL PRIMARY KEY,
    author_name VARCHAR(100) NOT NULL
);

CREATE TABLE Publishers (
    publisher_id SERIAL PRIMARY KEY,
    publisher_name VARCHAR(100) NOT NULL,
    publisher_email VARCHAR(255),
    publisher_address VARCHAR(255)
);

CREATE TABLE Customers (
    customer_id SERIAL PRIMARY KEY,
    first_name VARCHAR(100) NOT NULL,
    last_name VARCHAR(100) NOT NULL,
    customer_email VARCHAR(255) UNIQUE,
    total_amount_spent DECIMAL(10, 2),
    date_joined DATE

);

CREATE TABLE Books (
    book_id SERIAL PRIMARY KEY,
    book_name VARCHAR(255) NOT NULL,
    book_genre VARCHAR(100),
    book_type VARCHAR(50),
    book_publication_date DATE,
    book_publisher_id INT,
    book_author_id INT,
    book_language VARCHAR(50),
    book_average_rating FLOAT,
    book_price DECIMAL(10, 2),
    FOREIGN KEY (book_publisher_id) REFERENCES Publishers(publisher_id),
    FOREIGN KEY (book_author_id) REFERENCES Authors(author_id)
);

CREATE TABLE Reviews (
    review_id SERIAL PRIMARY KEY,
    customer_id INT,
    book_id INT,
    rating INT,
    review_text TEXT,
    review_date DATE,
    FOREIGN KEY (customer_id) REFERENCES Customers(customer_id),
    FOREIGN KEY (book_id) REFERENCES Books(book_id)
);

CREATE TABLE Orders (
    order_id SERIAL PRIMARY KEY,
    order_date DATE,
    customer_id INT,
    book_id INT,
    order_amount DECIMAL(10, 2),
    order_quantity INT,
    FOREIGN KEY (customer_id) REFERENCES Customers(customer_id),
    FOREIGN KEY (book_id) REFERENCES Books(book_id)
);
```

## DDL statements for Customers Table

### To create table

```sql
CREATE TABLE Customers (
    customer_id SERIAL PRIMARY KEY,
    first_name VARCHAR(100) NOT NULL,
    last_name VARCHAR(100) NOT NULL,
    customer_email VARCHAR(255) UNIQUE,
    total_amount_spent DECIMAL(10, 2),
    date_joined DATE

);
```

### To Modify an existing column

```sql
ALTER TABLE Customers MODIFY COLUMN first_name VARCHAR(500);
```

### To Add a new column

```sql
ALTER TABLE Customers ADD alternate_email VARCHAR(255);
```

### Deleting a column

```sql
ALTER TABLE Customers DROP COLUMN alternate_email;
```

### Deleting a table

```sql
DROP TABLE Customers;
```

### Deleting all the data(rows) in a table

```sql
TRUNCATE TABLE Customers;
```

### Renaming a table name

```sql
RENAME TABLE Customers TO Customer_Details ;
```

