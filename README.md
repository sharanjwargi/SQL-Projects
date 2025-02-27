# SQL-Projects
Employee Management System
-- Project: Employee Management System using PostgreSQL

-- Step 1: Create a New Database
CREATE DATABASE employee_management;

-- Step 2: Connect to the Database
\c employee_management;

-- Step 3: Create a Table for Employees
CREATE TABLE employees (
    employee_id SERIAL PRIMARY KEY,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    phone_number VARCHAR(20),
    hire_date DATE NOT NULL,
    job_title VARCHAR(50) NOT NULL,
    salary NUMERIC(10,2) CHECK (salary > 0),
    department_id INT,
    manager_id INT,
    FOREIGN KEY (department_id) REFERENCES departments(department_id),
    FOREIGN KEY (manager_id) REFERENCES employees(employee_id)
);

-- Step 4: Create a Table for Departments
CREATE TABLE departments (
    department_id SERIAL PRIMARY KEY,
    department_name VARCHAR(50) NOT NULL UNIQUE
);

-- Step 5: Create a Table for Projects
CREATE TABLE projects (
    project_id SERIAL PRIMARY KEY,
    project_name VARCHAR(100) NOT NULL UNIQUE,
    start_date DATE NOT NULL,
    end_date DATE,
    budget NUMERIC(12,2) CHECK (budget > 0)
);

-- Step 6: Create a Table for Employee-Project Assignments
CREATE TABLE employee_projects (
    assignment_id SERIAL PRIMARY KEY,
    employee_id INT,
    project_id INT,
    role VARCHAR(50) NOT NULL,
    assigned_date DATE NOT NULL,
    FOREIGN KEY (employee_id) REFERENCES employees(employee_id),
    FOREIGN KEY (project_id) REFERENCES projects(project_id)
);

-- Step 7: Insert Data into Departments
INSERT INTO departments (department_name) VALUES 
('HR'), 
('IT'), 
('Finance'), 
('Marketing');

-- Step 8: Insert Data into Employees
INSERT INTO employees (first_name, last_name, email, phone_number, hire_date, job_title, salary, department_id) VALUES 
('John', 'Doe', 'john.doe@example.com', '123-456-7890', '2023-01-10', 'Software Engineer', 60000, 2),
('Jane', 'Smith', 'jane.smith@example.com', '987-654-3210', '2022-07-15', 'HR Manager', 75000, 1),
('Alice', 'Johnson', 'alice.johnson@example.com', '555-666-7777', '2021-10-20', 'Financial Analyst', 70000, 3);

-- Step 9: Insert Data into Projects
INSERT INTO projects (project_name, start_date, end_date, budget) VALUES 
('AI Development', '2023-05-01', '2024-06-30', 150000),
('Marketing Strategy 2024', '2023-09-01', '2024-12-31', 80000);

-- Step 10: Assign Employees to Projects
INSERT INTO employee_projects (employee_id, project_id, role, assigned_date) VALUES 
(1, 1, 'Lead Developer', '2023-05-10'),
(2, 2, 'Project Coordinator', '2023-09-15');

-- Step 11: Retrieve Data with Joins
SELECT e.employee_id, e.first_name, e.last_name, e.job_title, e.salary, d.department_name, p.project_name, ep.role
FROM employees e
JOIN departments d ON e.department_id = d.department_id
LEFT JOIN employee_projects ep ON e.employee_id = ep.employee_id
LEFT JOIN projects p ON ep.project_id = p.project_id;

-- Step 12: Update an Employee's Salary
UPDATE employees
SET salary = 80000
WHERE first_name = 'John' AND last_name = 'Doe';

-- Step 13: Delete an Employee Record
DELETE FROM employees
WHERE employee_id = 3;

-- Step 14: Drop Tables (if needed)
DROP TABLE employee_projects;
DROP TABLE projects;
DROP TABLE employees;
DROP TABLE departments;

-- Step 15: Drop the Database (if needed)
DROP DATABASE employee_management;


