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


## DML queries for Customers Table

### Inserting data into customers table
```sql
INSERT INTO Customers (first_name, last_name, customer_email, total_amount_spent, date_joined)
VALUES
    ('Vineeth', 'Kanoor', 'vineethkanoor@gmail.com', 500.25, '2015-01-01'),
    ('Reshma', 'Reddy', 'reshureddy@gmail.com', 1200, '2022-12-12'),
    ('Ashlin', 'Michael', 'ashlinmichael@gmail.com', 800, '2022-11-30');
```

### Reading the data from customers table

```sql
SELECT * FROM customers;
```

### Updating a particular field in the table

```sql
UPDATE customers SET total_amount_spent = 5000.00 WHERE customer_id = 3;
```

### Deleting a row from the table

```sql
DELETE FROM customers WHERE customer_id = 3;
```

## SQL Queries for Requirements Listed in the Assignment


### Power writers (authors) with more than X books in the same genre published within the last X years

```sql
SELECT author_id, author_name FROM Authors
WHERE author_id IN (
    SELECT book_author_id FROM Books
    WHERE book_genre = 'Fiction' AND book_publication_date >= CURRENT_DATE - INTERVAL '5 YEAR'
    GROUP BY book_author_id HAVING COUNT(*) > 1
);

```

### Loyal Customers who has spent more than X dollars in the last year

```sql
SELECT customer_id, first_name, last_name FROM customers
WHERE total_amount_spent > 500 AND DATE_PART('year', CURRENT_DATE) - DATE_PART('year', date_joined) = 1;
```


### Well Reviewed books that has a better user rating than average
```sql
SELECT book_id, book_name FROM books
WHERE book_average_rating > ( SELECT AVG(rating) FROM reviews);
```

### The most popular genre by sales 
Here we are using the order quantity of the books to determine which generates high sales
```sql
SELECT book_genre FROM Books
JOIN Orders ON Books.book_id = Orders.book_id GROUP BY book_genre
ORDER BY SUM(Orders.order_quantity) DESC;
```

### The 10 most recent posted reviews by Customers
```sql
SELECT * FROM Reviews ORDER BY review_date DESC LIMIT 10;
```

## TypeScript Interface with all the CRUD operations


```typescript
// customers.ts

import { Pool } from 'pg';

const pool = new Pool({
    user: 'postgres',
    host: 'db',
    database: 'Online_Book_Store',
    password: 'password',
    port: 5432,
});

interface Customer {
    customer_id: number;
    first_name: string;
    last_name: string;
    customer_email: string;
    total_amount_spent: number;
    date_joined: Date;
}

async function createCustomer(customer: Customer): Promise<void> {
    const { first_name, last_name, customer_email, total_amount_spent, date_joined } = customer;
    try {
        await pool.query(
            `INSERT INTO Customers (first_name, last_name, customer_email, total_amount_spent, date_joined)
             VALUES ($1, $2, $3, $4, $5)`,
            [first_name, last_name, customer_email, total_amount_spent, date_joined]
        );
        console.log(`Customer created successfully.`);
    } catch (error) {
        console.error('Unable to create the customer record ');
    }
}

async function readCustomer(customer_id: number): Promise<void> {
    try {
        const result = await pool.query(`SELECT * FROM Customers WHERE customer_id = $1`, [customer_id]);
        if (result.rows.length > 0) {
            console.log('Customer found:', result.rows[0]);
        } else {
            console.log('Customer not found.');
        }
    } catch (error) {
        console.error('Unable to read the table');
    }
}

async function updateCustomer(customer_id: number, customer: Partial<Customer>): Promise<void> {
    const { first_name, last_name, customer_email, total_amount_spent, date_joined } = customer;
    try {
        await pool.query(
            `UPDATE Customers SET
                first_name = COALESCE($1, first_name),
                last_name = COALESCE($2, last_name),
                customer_email = COALESCE($3, customer_email),
                total_amount_spent = COALESCE($4, total_amount_spent),
                date_joined = COALESCE($5, date_joined)
             WHERE customer_id = $6`,
            [first_name, last_name, customer_email, total_amount_spent, date_joined, customer_id]
        );
        console.log(`Customer updated successfully.`);
    } catch (error) {
        console.error('Unable to update the record');
    }
}

async function deleteCustomer(customer_id: number): Promise<void> {
    try {
        await pool.query(`DELETE FROM Customers WHERE customer_id = $1`, [customer_id]);
        console.log(`Customer deleted successfully.`);
    } catch (error) {
        console.error('Unable to delete the record');
    }
}

(async () => {
    const newCustomer: Customer = {
        first_name: 'Narendra',
        last_name: 'Modi',
        customer_email: 'narendramodi@gmail.com',
        total_amount_spent: 100.50,
        date_joined: new Date()
    };

    await createCustomer(newCustomer);

    await readCustomer(1);

    await updateCustomer(1, { total_amount_spent: 150.75 });

    await deleteCustomer(1);
})();
```
