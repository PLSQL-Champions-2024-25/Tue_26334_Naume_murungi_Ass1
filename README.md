# **Library Management System**  

## **1. Introduction**  

**Project Information**  
- **Name**: Naume Murungi  
- **Student ID**: 26334  
- **Specialization**: Programming  
- **Database**: Oracle PDB (tue_26334_PDB_assII)  

## **2. Problem Statement**  

Managing a library manually is inefficient and can lead to issues like **book loss, tracking difficulties, and borrower mismanagement**. This project aims to design a **Library Management System** that efficiently handles:  

- **Authors**: Storing information about book authors.  
- **Books**: Managing book details and their availability.  
- **Borrowers**: Keeping track of people who borrow books.  
- **BorrowedBooks**: Recording book loans, including due dates and return status.  

## **3. Database Schema**  

This database consists of four **well-related** tables:  

1. **Authors** – Stores author details (ID, name, country).  
2. **Books** – Contains book details, including the author (FK).  
3. **Borrowers** – Stores borrower information.  
4. **BorrowedBooks** – Links borrowers to books they borrow (FKs for both).  

---

## **4. SQL Commands Executed**  

### **Table Creation Queries**  

```sql
-- Table: Authors
CREATE TABLE Authors (
    author_id NUMBER PRIMARY KEY,
    author_name VARCHAR2(100) NOT NULL,
    country VARCHAR2(50)
);

-- Table: Books
CREATE TABLE Books (
    book_id NUMBER PRIMARY KEY,
    title VARCHAR2(150) NOT NULL,
    genre VARCHAR2(50),
    author_id NUMBER,
    available_copies NUMBER CHECK (available_copies >= 0),
    FOREIGN KEY (author_id) REFERENCES Authors(author_id)
);

-- Table: Borrowers
CREATE TABLE Borrowers (
    borrower_id NUMBER PRIMARY KEY,
    borrower_name VARCHAR2(100) NOT NULL,
    email VARCHAR2(100) UNIQUE,
    phone VARCHAR2(15)
);

-- Table: BorrowedBooks
CREATE TABLE BorrowedBooks (
    borrow_id NUMBER PRIMARY KEY,
    borrower_id NUMBER,
    book_id NUMBER,
    borrow_date DATE,
    return_date DATE,
    status VARCHAR2(20) CHECK (status IN ('Borrowed', 'Returned')),
    FOREIGN KEY (borrower_id) REFERENCES Borrowers(borrower_id),
    FOREIGN KEY (book_id) REFERENCES Books(book_id)
);
```

---

## **5. Data Insertion Queries**  

```sql
-- Inserting into Authors
INSERT INTO Authors VALUES (1, 'George Orwell', 'United Kingdom');
INSERT INTO Authors VALUES (2, 'Harper Lee', 'United States');
INSERT INTO Authors VALUES (3, 'J.K. Rowling', 'United Kingdom');
INSERT INTO Authors VALUES (4, 'Chinua Achebe', 'Nigeria');

-- Inserting into Books
INSERT INTO Books VALUES (101, '1984', 'Dystopian', 1, 3);
INSERT INTO Books VALUES (102, 'To Kill a Mockingbird', 'Fiction', 2, 2);
INSERT INTO Books VALUES (103, 'Harry Potter and the Philosopher\'s Stone', 'Fantasy', 3, 5);
INSERT INTO Books VALUES (104, 'Things Fall Apart', 'Historical', 4, 4);

-- Inserting into Borrowers
INSERT INTO Borrowers VALUES (201, 'Alice Johnson', 'alice@email.com', '555-111-2222');
INSERT INTO Borrowers VALUES (202, 'Brian Smith', 'brian@email.com', '555-222-3333');
INSERT INTO Borrowers VALUES (203, 'Catherine Lee', 'catherine@email.com', '555-333-4444');
INSERT INTO Borrowers VALUES (204, 'Daniel Kim', 'daniel@email.com', '555-444-5555');

-- Inserting into BorrowedBooks
INSERT INTO BorrowedBooks VALUES (301, 201, 101, TO_DATE('2024-03-10', 'YYYY-MM-DD'), TO_DATE('2024-03-20', 'YYYY-MM-DD'), 'Returned');
INSERT INTO BorrowedBooks VALUES (302, 202, 102, TO_DATE('2024-03-11', 'YYYY-MM-DD'), NULL, 'Borrowed');
INSERT INTO BorrowedBooks VALUES (303, 203, 103, TO_DATE('2024-03-12', 'YYYY-MM-DD'), NULL, 'Borrowed');
INSERT INTO BorrowedBooks VALUES (304, 204, 104, TO_DATE('2024-03-15', 'YYYY-MM-DD'), TO_DATE('2024-03-25', 'YYYY-MM-DD'), 'Returned');
```

---

## **6. Data Retrieval Queries**  

### **Updating a Book's Availability**  
```sql
UPDATE Books SET available_copies = available_copies - 1 WHERE book_id = 102;
```

### **Deleting a Borrowed Record (Returned Book)**  
```sql
DELETE FROM BorrowedBooks WHERE borrow_id = 304;
```

### **Performing a JOIN to Retrieve Borrower Details and Borrowed Books**  
```sql
SELECT br.borrower_name, b.title, bb.borrow_date, bb.status  
FROM BorrowedBooks bb  
JOIN Borrowers br ON bb.borrower_id = br.borrower_id  
JOIN Books b ON bb.book_id = b.book_id;
```

### **LEFT JOIN: List All Books and Their Borrowers (if any)**  
```sql
SELECT b.title, br.borrower_name, bb.borrow_date  
FROM Books b  
LEFT JOIN BorrowedBooks bb ON b.book_id = bb.book_id  
LEFT JOIN Borrowers br ON bb.borrower_id = br.borrower_id;
```

### **Inserting a New Borrower**  
```sql
INSERT INTO Borrowers VALUES (205, 'Emma Watson', 'emma@email.com', '555-555-6666');
```

### **Rollback**  
```sql
ROLLBACK;
```

### **Commit**  
```sql
COMMIT;
```

### **Retrieve Books Borrowed in the Last 7 Days**  
```sql
SELECT * FROM BorrowedBooks  
WHERE borrow_date BETWEEN TRUNC(SYSDATE) - 7 AND TRUNC(SYSDATE) - 1;
```

### **Finding the Top 3 Most Borrowed Books**  
```sql
SELECT b.title, COUNT(bb.book_id) AS borrow_count  
FROM BorrowedBooks bb  
JOIN Books b ON bb.book_id = b.book_id  
GROUP BY b.title  
ORDER BY borrow_count DESC  
FETCH FIRST 3 ROWS ONLY;
```

### **Retrieve Borrowers Who Borrowed More Than 1 Book**  
```sql
SELECT br.borrower_id, br.borrower_name, COUNT(bb.borrow_id) AS total_borrowed  
FROM Borrowers br  
JOIN BorrowedBooks bb ON br.borrower_id = bb.borrower_id  
GROUP BY br.borrower_id, br.borrower_name  
HAVING COUNT(bb.borrow_id) > 1;
```

---

## **7. Results and Analysis**  

This project effectively utilizes **SQL-DDL, DML, DCL, and TCL** operations to manage a **Library Management System**, ensuring proper handling of **authors, books, borrowers, and borrowing transactions**.  

---

## **8. Screenshots of Query Outputs**  

_(Make sure to generate your own screenshots.)_  

---

## **9. Conclusion**  

The **Library Management System** database efficiently organizes **book inventory, author records, borrower information, and book borrowing details**. It ensures **data integrity**, **easy retrieval**, and **proper tracking of borrowed books**.  

### **Future Enhancements**  
- Implement an **automated fine calculation** for overdue books.  
- Add a **reservation system** for books.  
- Introduce **notifications for due dates** to borrowers.  
