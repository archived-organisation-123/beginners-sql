# Employee Absence Crossjoin Example

Run the below SQL Statements in order against MySql. You can either install and configure MySql on your machine, or if you're familiar with Docker you can use:  
`docker run -p 3306:3306 --name mysql -e MYSQL_ROOT_PASSWORD=pAssw0rD -d mysql:8`

Then use a DB / SQL Editor to insert the following statements. I used DBeaver in this case (https://dbeaver.io/)

    CREATE TABLE Dates (
        Date datetime,
        DayOfWeek int,
        IsWeekend boolean
    );

    INSERT INTO Dates (Date, DayOfWeek, IsWeekend) VALUES 
    ('2018-11-19', 1, false),
    ('2018-11-20', 2, false),
    ('2018-11-21', 3, false),
    ('2018-11-22', 4, false),
    ('2018-11-23', 5, false),
    ('2018-11-24', 6, true),
    ('2018-11-25', 7, true);

    CREATE TABLE Employee (
        EmployeeID int,
        LastName varchar(255),
        FirstName varchar(255),
        Address varchar(255),
        City varchar(255) 
    );

    INSERT INTO Employee (EmployeeID, LastName, FirstName, Address, City) VALUES 
    (1, 'Simpson', 'Homer', '742 Evergreen Terrace', 'Springfield');
    
    CREATE TABLE Absence (
        EmployeeID int,
        AbsenceDate datetime,
        AbsenceReason varchar(20)
    );
    
    INSERT INTO Absence (EmployeeID, AbsenceDate, AbsenceReason) VALUES 
    (1, '2018-11-19', 'Duff-Related'),
    (1, '2018-11-21', 'Duff-Related'),
    (1, '2018-11-22', 'Duff-Related');
    
    SELECT d.`Date`, 
    d.IsWeekend, 
    p.*, 
    CASE WHEN a.EmployeeID IS NOT NULL THEN 1 ELSE 0 END AS Absent, 
    a.AbsenceReason
    FROM Dates d
    CROSS JOIN Employee p
    LEFT OUTER JOIN Absence a ON d.Date = a.AbsenceDate
                            AND p.EmployeeID = a.EmployeeID;