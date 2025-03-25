## 📌 **README for Library Management System**  
### **1. Introduction**  
#### **Project Information**  
- **Name**: Naume Murungi  
- **Student ID**: 26334  
- **Specialization**: Programming  
- **Database**: Oracle PDB (tue_26334_PDB_assII)  

### **2. Problem Statement**  
Managing a library manually can lead to issues like book loss, tracking difficulties, and borrower mismanagement. This project aims to design a **Library Management System** that efficiently handles:  

- **Authors** – Stores author details.  
- **Books** – Manages book details and availability.  
- **Borrowers** – Keeps track of registered borrowers.  
- **BorrowedBooks** – Records book borrowing details.  

---

## **📌 3. Database Schema**  
The database consists of four **related tables**:  

- **Authors**: Stores author information (ID, Name, Country).  
- **Books**: Contains book details (ID, Title, Genre, Author ID as FK).  
- **Borrowers**: Stores borrower details (ID, Name, Email).  
- **BorrowedBooks**: Tracks borrowed books (FKs for Book ID and Borrower ID).  

---

## **📌 4. SQL Commands Executed**  

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
    FOREIGN KEY (author_id) REFERENCES Authors(author_id)
);

-- Table: Borrowers
CREATE TABLE Borrowers (
    borrower_id NUMBER PRIMARY KEY,
    borrower_name VARCHAR2(100) NOT NULL,
    email VARCHAR2(100) UNIQUE
);

-- Table: BorrowedBooks
CREATE TABLE BorrowedBooks (
    borrow_id NUMBER PRIMARY KEY,
    borrower_id NUMBER,
    book_id NUMBER,
    borrow_date DATE,
    return_date DATE,
    FOREIGN KEY (borrower_id) REFERENCES Borrowers(borrower_id),
    FOREIGN KEY (book_id) REFERENCES Books(book_id)
);
```

---

## **📌 5. Data Insertion Queries**  
```sql
-- Inserting into Authors
INSERT INTO Authors VALUES (1, 'George Orwell', 'United Kingdom');
INSERT INTO Authors VALUES (2, 'Harper Lee', 'United States');

-- Inserting into Books
INSERT INTO Books VALUES (101, '1984', 'Dystopian', 1);
INSERT INTO Books VALUES (102, 'To Kill a Mockingbird', 'Fiction', 2);

-- Inserting into Borrowers
INSERT INTO Borrowers VALUES (201, 'Alice Johnson', 'alice@email.com');
INSERT INTO Borrowers VALUES (202, 'Brian Smith', 'brian@email.com');

-- Inserting into BorrowedBooks
INSERT INTO BorrowedBooks VALUES (301, 201, 101, TO_DATE('2024-03-10', 'YYYY-MM-DD'), TO_DATE('2024-03-20', 'YYYY-MM-DD'));
INSERT INTO BorrowedBooks VALUES (302, 202, 102, TO_DATE('2024-03-11', 'YYYY-MM-DD'), NULL);
```

---

## **📌 6. Data Retrieval Queries**  

### **Updating a Book Title**  
```sql
UPDATE Books SET title = 'Nineteen Eighty-Four' WHERE book_id = 101;
```

### **Deleting a Borrowed Record (Returned Book)**  
```sql
DELETE FROM BorrowedBooks WHERE borrow_id = 301;
```

### **Retrieving Borrowers and Their Borrowed Books**  
```sql
SELECT br.borrower_name, b.title, bb.borrow_date, bb.return_date
FROM BorrowedBooks bb  
JOIN Borrowers br ON bb.borrower_id = br.borrower_id  
JOIN Books b ON bb.book_id = b.book_id;
```

### **Listing All Books and Their Borrowers (if any)**  
```sql
SELECT b.title, br.borrower_name, bb.borrow_date  
FROM Books b  
LEFT JOIN BorrowedBooks bb ON b.book_id = bb.book_id  
LEFT JOIN Borrowers br ON bb.borrower_id = br.borrower_id;
```

### **Rollback Example**  
```sql
ROLLBACK;
```

### **Commit Example**  
```sql
COMMIT;
```

---

## **📌 7. Results and Analysis**  
This project effectively uses SQL commands to manage a **Library Management System**, ensuring proper handling of authors, books, borrowers, and borrowing transactions.

---

## **📌 8. Screenshots of Query Outputs**  
*(You will add screenshots of query outputs here.)*  

---

## **📌 9. Conclusion**  
The **Library Management System** database efficiently organizes **book inventory, author records, borrower information, and book borrowing details**. It ensures **data integrity, easy retrieval, and proper tracking of borrowed books**.

### **Future Enhancements**  
- **Automated fine calculation for overdue books.**  
- **A book reservation system.**  
- **Email notifications for due dates.**  

---

  


