-- Create the Books table
CREATE TABLE Books (
    BookID INT PRIMARY KEY AUTO_INCREMENT,
    Title VARCHAR(255) NOT NULL,
    Author VARCHAR(255) NOT NULL,
    Genre VARCHAR(100),
    Price DECIMAL(10,2),
    TotalCopies INT,
    AvailableCopies INT,
    PurchaseCount INT DEFAULT 0
);

-- Insert popular books from the past 10 years
INSERT INTO Books (Title, Author, Genre, Price, TotalCopies, AvailableCopies, PurchaseCount) VALUES
('The Midnight Library', 'Matt Haig', 'Fiction', 15.99, 50, 45, 100),
('Where the Crawdads Sing', 'Delia Owens', 'Mystery', 18.50, 60, 50, 120),
('Project Hail Mary', 'Andy Weir', 'Science Fiction', 22.99, 40, 35, 90),
('The Silent Patient', 'Alex Michaelides', 'Thriller', 14.99, 55, 50, 110),
('Atomic Habits', 'James Clear', 'Self-help', 20.00, 70, 65, 130);

-- Create the Users table
CREATE TABLE Users (
    UserID INT PRIMARY KEY AUTO_INCREMENT,
    Name VARCHAR(255) NOT NULL,
    Email VARCHAR(255) UNIQUE NOT NULL,
    City VARCHAR(100)
);

-- Create the Transactions table
CREATE TABLE Transactions (
    TransactionID INT PRIMARY KEY AUTO_INCREMENT,
    UserID INT,
    BookID INT,
    BorrowDate DATETIME DEFAULT CURRENT_TIMESTAMP,
    ReturnDate DATETIME NULL,
    FOREIGN KEY (UserID) REFERENCES Users(UserID),
    FOREIGN KEY (BookID) REFERENCES Books(BookID)
);

-- Query to find the most expensive book in the library
SELECT * FROM Books ORDER BY Price DESC LIMIT 1;

-- Query to find the most popular books in each city
SELECT b.Title, u.City, COUNT(t.TransactionID) AS BorrowCount
FROM Transactions t
JOIN Books b ON t.BookID = b.BookID
JOIN Users u ON t.UserID = u.UserID
GROUP BY b.Title, u.City
ORDER BY u.City, BorrowCount DESC;

-- Query to find the most purchased book in the library
SELECT * FROM Books ORDER BY PurchaseCount DESC LIMIT 1;

-- Query to find the least chosen book in the library
SELECT * FROM Books ORDER BY PurchaseCount ASC LIMIT 1;
