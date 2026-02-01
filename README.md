The First Months of SQL
This repository contains SQL practice tasks completed during the first month of a 9‑month data analytics training program.
The exercises are based on real assignments and cover essential analytical topics used in junior data roles.
The tasks are grouped into two practical domains:
- Hospital analytics (wards, buildings, examinations)
- Academic analytics (teachers, departments, groups)
Each query is written in clean, readable SQL and demonstrates practical business logic.

📘 Topics Covered
- Aggregation: COUNT, SUM, AVG, MIN, MAX
- Filtering: WHERE, BETWEEN, logical operators
- Grouping: GROUP BY, HAVING
- Subqueries and nested logic
- Date and time functions (DATEDIFF)
- Conditional filtering based on averages
- Real‑world analytical scenarios:
- hospital ward distribution
- department financing
- examination scheduling
- teacher salaries and premiums
- group ratings and course years

📁 Repository Structure
/The-First-Months-of-SQL
│
├── hospital_tasks.sql      # Wards, Buildings, Examinations
├── academic_tasks.sql      # Teachers, Departments, Groups
└── README.md               # Project documentation


Each .sql file contains:
- a task description
- a clean SQL solution
- comments explaining the logic

🏥 Hospital Analytics Tasks
1. Number of wards in each building
SELECT Building, COUNT(*) AS 'Number of wards'
FROM Wards
GROUP BY Building;


2. Total financing of departments per floor
SELECT Floor, SUM(Financing) AS 'Total financing of departments'
FROM Departments
GROUP BY Floor;


3. Wards in buildings 2–5 above the 2nd floor
SELECT Building, COUNT(*) AS 'Number of wards'
FROM Wards
WHERE Floor > 2
  AND Building BETWEEN 2 AND 5
GROUP BY Building;


4. Earliest examination start per weekday
SELECT DayOfWeek, MIN(StartTime) AS 'Earliest examination start'
FROM Examinations
GROUP BY DayOfWeek;


5. Buildings with more than 5 wards
SELECT Building, COUNT(*) AS 'Number of wards'
FROM Wards
GROUP BY Building
HAVING COUNT(*) > 5;


6. Buildings with total financing > 120000
SELECT Building, SUM(Financing) AS 'Total financing'
FROM Departments
GROUP BY Building
HAVING SUM(Financing) > 120000;


7. Examination duration statistics per weekday
SELECT 
    DayOfWeek,
    MIN(DATEDIFF(MINUTE, StartTime, EndTime)) AS 'Minimum duration',
    AVG(DATEDIFF(MINUTE, StartTime, EndTime)) AS 'Average duration',
    COUNT(*) AS 'Number of examinations'
FROM Examinations
GROUP BY DayOfWeek
HAVING MAX(DATEDIFF(MINUTE, StartTime, EndTime)) >
       (SELECT AVG(DATEDIFF(MINUTE, StartTime, EndTime)) FROM Examinations);


8. Buildings with above‑average number of wards
SELECT Building, COUNT(*) AS 'Number of wards'
FROM Wards
GROUP BY Building
HAVING COUNT(*) > (
    SELECT AVG(MidCount)
    FROM (
        SELECT COUNT(*) AS MidCount
        FROM Wards
        GROUP BY Building
    ) AS MidCountTable
);



🎓 Academic Analytics Tasks
1. Number of teachers per position
SELECT Position, COUNT(*) AS 'Number of Teachers'
FROM Teachers
GROUP BY Position
ORDER BY Position;


2. Average salary per position
SELECT Position, ROUND(AVG(Salary), 2) AS 'Average Salary'
FROM Teachers
GROUP BY Position;


3. Min and max premium per position
SELECT Position,
       MIN(Premium) AS 'Minimum Premium',
       MAX(Premium) AS 'Maximum Premium'
FROM Teachers
GROUP BY Position;


4. Number of groups by rating and year
SELECT Year, Rating, COUNT(*) AS 'Number of Groups'
FROM Groups
GROUP BY Year, Rating
ORDER BY Year;


5. Positions with at least 5 teachers
SELECT Position,
       COUNT(*) AS 'Number of Teachers',
       ROUND(AVG(Salary), 2) AS 'Average Salary'
FROM Teachers
GROUP BY Position
HAVING COUNT(*) > 5;


6. Courses with average rating < 4
SELECT Year,
       COUNT(*) AS 'Number of Groups',
       AVG(Rating) AS 'Average Rating'
FROM Groups
GROUP BY Year
HAVING AVG(Rating) < 4;


7. Salary range for positions with >3 teachers
SELECT Position,
       MIN(Salary) AS 'Minimum Salary',
       MAX(Salary) AS 'Maximum Salary',
       COUNT(*) AS 'Total Teachers'
FROM Teachers
GROUP BY Position
HAVING COUNT(*) > 3;


8. Departments with above‑average funding
SELECT Name, Financing
FROM Departments
WHERE Financing > (
    SELECT AVG(Financing)
    FROM Departments
);


9. Teachers in positions with avg salary > 25000
SELECT Surname, Name, Salary
FROM Teachers
WHERE Position IN (
    SELECT Position
    FROM Teachers
    GROUP BY Position
    HAVING AVG(Salary) > 25000
);


10. Position with highest total salary
SELECT Position, SUM(Salary) AS 'Total Salary'
FROM Teachers
GROUP BY Position
HAVING SUM(Salary) = (
    SELECT MAX(MaxSalary)
    FROM (
        SELECT SUM(Salary) AS MaxSalary
        FROM Teachers
        GROUP BY Position
    ) AS x
);


11. Highest avg salary among positions with below‑avg premium
SELECT Position, AVG(Salary) AS 'Average Salary'
FROM Teachers
GROUP BY Position
HAVING AVG(Salary) = (
    SELECT MAX(AVGSal)
    FROM (
        SELECT AVG(Salary) AS AVGSal
        FROM Teachers
        WHERE Position IN (
            SELECT Position
            FROM Teachers
            GROUP BY Position
            HAVING AVG(Premium) < (
                SELECT AVG(AvgPre)
                FROM (
                    SELECT AVG(Premium) AS AvgPre
                    FROM Teachers
                    GROUP BY Position
                ) AS Y
            )
        )
        GROUP BY Position
    ) AS X
);



👤 Author
Vitalii — Junior Data Analyst
Based in Stockholm, Sweden
Learning SQL, Python, Power BI, and full‑cycle analytics
Building a public portfolio on GitHub and LinkedIn
LinkedIn: www.linkedin.com/in/vitalii-nikitin-120a06392


