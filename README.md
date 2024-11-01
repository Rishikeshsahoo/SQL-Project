# SQL-Project

-- Rishikesh Sahoo A766652

use company;

-- creating the manager table
CREATE TABLE Manager (
id INT PRIMARY KEY,
first_name VARCHAR(40),
last_name VARCHAR(40),
project varchar(50)
);

-- creating the employee table
-- with manager_id as the foreign key with manager table
CREATE TABLE Employee (
id INT PRIMARY KEY,
first_name VARCHAR(20),
last_name VARCHAR(20),
manager_id INT,
FOREIGN KEY (manager_id) REFERENCES Manager(id) ON DELETE CASCADE
);

INSERT INTO Manager (id, first_name, last_name, project)
VALUES 
    (1, 'Rakesh', 'Verma', 'E-Commerce Platform'),
    (2, 'Sonal', 'Sharma', 'Inventory Management System'),
    (3, 'Ajay', 'Kapoor', 'Mobile Banking App'),
    (4, 'Priya', 'Mehta', 'Healthcare Portal'),
    (5, 'Yatharth', 'Burari', 'Finance Portal'),
    (6, 'Ankit', 'Mehta', 'Travel Sector');
    

INSERT INTO Employee (id, first_name, last_name, manager_id)
VALUES 
    (101, 'Ankit', 'Singh', 1),
    (102, 'Neha', 'Mishra', 1),
    (103, 'Rohit', 'Kumar', 1),
    (104, 'Pooja', 'Patel', 2),
    (105, 'Vikram', 'Desai', 2),
    (106, 'Meera', 'Nair', 3),
    (107, 'Arjun', 'Reddy', 3),
    (108, 'Divya', 'Gupta', 4),
    (109, 'Kiran', 'Joshi', 4),
    (110, 'Suresh', 'Yadav', 4),
    (111, 'Rishikesh', 'Sahoo',null),
    (112, 'Yatharth', 'Yadav',null),
    (113, 'Rishabh', 'Mehta',null);
    

-- GET ALL EMPLOYEES UNDER EACH MANAGER
SELECT 
    m.first_name AS manager_first_name,
    m.last_name AS manager_last_name,
    m.project AS project_name,
    e.first_name AS employee_first_name,
    e.last_name AS employee_last_name
FROM 
    Employee e
JOIN 
    Manager m ON e.manager_id = m.id
ORDER BY 
    m.id;
    
    
-- ___________________________________________________________________________________
    
    
-- FUNCTION TO GET FULL NAME OF A MANAGER
-- setting the delimiter for the function scope
DELIMITER $$ 
CREATE FUNCTION get_manager_full_name(manager_id int)
returns VARCHAR(400) deterministic 
	BEGIN
	 declare full_name VARCHAR(400); -- declaring a new variable for the result
		 SELECT CONCAT(first_name, ' ', last_name)
		 INTO full_name -- inserting the desired value to the variable
		 FROM Manager
		 WHERE id = manager_id; -- where the manager id is equal the to parameter of the function
	RETURN full_name; -- returning the result
	END $$
    
-- ________________________________________________________________________________________________________


-- Q) HOW MANY EMPLOYEES ARE THERE UNDER A PERTICULAR MANAGER
-- here i am useing the above created function to get the results
SELECT get_manager_full_name(m.id), COUNT(*) FROM Manager m
JOIN 
Employee e on e.manager_id=m.id
GROUP BY get_manager_full_name(m.id);
 
 
-- ________________________________________________________________________________________________________
 
-- Finding the employees who are not assigned with any manager

Select CONCAT(e.first_name ,' ', e.last_name) as "employee name" from
Employee e
LEFT JOIN
Manager m on e.manager_id=m.id
where 
m.id is null;

-- ____________________________________________________________________________

-- get all details of a manager

SELECT 
    get_manager_full_name(m.id) AS "manager name",
    COUNT(e.id) AS "number of employees",
    m.project
FROM 
    Manager m
JOIN 
    Employee e ON e.manager_id = m.id
GROUP BY 
    m.id;


 
