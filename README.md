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
![Screenshot 2025-02-25 145342](https://github.com/user-attachments/assets/d8ce2e5f-b676-480b-a274-201e3f7375c4)


![authortable](https://github.com/user-attachments/assets/bcfb2d1c-9eda-4880-a9f4-72f1205396e7)

![bookstable](https://github.com/user-attachments/assets/3bdb4175-b542-4978-a971-3e690f5825ed)

![borrowedbookstable](https://github.com/user-attachments/assets/63ad6661-b7b3-4b37-b56a-b48d253b1d20)

![borrowedbooksupdate](https://github.com/user-attachments/assets/1470b176-d2f8-495f-8981-24f408646065)


<img width="637" alt="rollbackandcommitCommand" src="https://github.com/user-attachments/assets/23c38fd5-5687-4ef6-9ae0-8237bc29514a" />

![aurthorupdate](https://github.com/user-attachments/assets/f0fb467c-10b0-4f57-ab46-3fd09f6eef0d)

![boorrowedbooksjoin](https://github.com/user-attachments/assets/43d1a3af-3f23-48bd-9cd5-98708a85115e)

![retrieve](https://github.com/user-attachments/assets/e76aac5b-c02d-4871-bbf4-b9820fa78525)

<img width="534" alt="Past7days" src="https://github.com/user-attachments/assets/d0ab628c-43a3-4a36-96f5-3157bfcc0983" />

---

![borrowedbooksupdate](https://github.com/user-attachments/assets/ef422607-4e0f-49da-b09b-91d9dd76fb29)

![borrowersupdate](https://github.com/user-attachments/assets/88d97c6d-e255-46ad-9f6a-10abe5ef4c50)

![borrowedbooksdelete](https://github.com/user-attachments/assets/b9629c87-cfd4-4a5c-b08b-34d77e31145e)

![join](https://github.com/user-attachments/assets/5f68768d-163c-4441-9c4e-c5cf1d965390)



<img width="616" alt="ConceptualDiagram" src="https://github.com/user-attachments/assets/0f5233bc-e8ea-4a86-999d-cf5c9fd4b2aa" />

## **📌 9. Conclusion**  
The **Library Management System** database efficiently organizes **book inventory, author records, borrower information, and book borrowing details**. It ensures **data integrity, easy retrieval, and proper tracking of borrowed books**.

### **Future Enhancements**  
- **Automated fine calculation for overdue books.**  
- **A book reservation system.**  
- **Email notifications for due dates.**  

**Repository**: [https://github.com/PLSQL-Champions-2024-25/Tue_26334_Naume_murungi_Ass1]


