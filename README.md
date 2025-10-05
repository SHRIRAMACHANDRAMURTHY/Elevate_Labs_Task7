# Elevate_Labs_Task7
Views, Data Abstraction

1. What is a view?

Definition:
A view is a virtual table that displays data from one or more tables through a saved SQL query.

Explanation:

A view does not store data physically (it shows data from the underlying tables).

It acts like a table — you can query it using SELECT, WHERE, etc.

It simplifies complex queries by saving them as reusable objects.

Example:

CREATE VIEW EmployeeInfo AS
SELECT e.EmployeeName, d.DepartmentName
FROM Employees e
JOIN Departments d ON e.DepartmentID = d.DepartmentID;


→ You can now use SELECT * FROM EmployeeInfo; to get employee–department data.

In short:
A view is a saved SQL query that acts like a virtual table.

2. Can we update data through a view?

Answer:
Yes, but only if the view is simple and based on a single table without complex operations.

Explanation:
You can update, insert, or delete through a view if:

The view is based on one table.

It does not include aggregations, DISTINCT, GROUP BY, or joins.

All columns being updated exist in the base table.

Example:

UPDATE SimpleView
SET Salary = 50000
WHERE EmployeeID = 101;


In short:
Yes — updates are allowed on simple, single-table views only.

3. What is a materialized view?

Definition:
A materialized view is a physically stored copy of a query result that can be refreshed periodically.

Explanation:

Unlike normal views, it stores data instead of computing it every time.

It improves performance for complex or slow queries.

You can refresh it manually or automatically to sync with base tables.

Example:

CREATE MATERIALIZED VIEW SalesSummary AS
SELECT Region, SUM(SalesAmount) AS TotalSales
FROM Orders
GROUP BY Region;


In short:
A materialized view stores the actual data of a query for faster access.

4. Difference between view and table
Feature	View	Table
Storage	Virtual; does not store data (except materialized view)	Physically stores data
Definition	Based on an SQL query	Database object holding actual data
Performance	Computed every time it’s queried	Direct data access
Modification	Usually read-only (limited updates)	Fully insert/update/delete capable
Purpose	Simplify queries, restrict access	Store persistent data

In short:
A table holds data physically; a view is a virtual layer built on top of tables.

5. How to drop a view?

Command:
You can delete a view using the DROP VIEW statement.

Example:

DROP VIEW EmployeeInfo;


Explanation:

It removes the view definition from the database.

The base tables remain unaffected.

In short:
Use DROP VIEW view_name; to delete a view.

6. Why use views?

Explanation:
Views are used for several practical reasons:

Simplify complex queries – Save joins or aggregations for reuse.

Enhance security – Restrict access to sensitive columns.

Data abstraction – Present data differently without altering tables.

Maintain consistency – Provide a unified way of viewing data.

Example:
A view showing only non-sensitive employee details:

CREATE VIEW PublicEmployeeInfo AS
SELECT EmployeeName, DepartmentName FROM Employees;


In short:
Views make queries simpler, safer, and more consistent.

7. Can we create indexed views?

Answer:
Yes, in some databases (like SQL Server and Oracle), you can create indexed views.

Explanation:

An indexed view physically stores data like a materialized view.

It has a unique clustered index that makes it faster for frequent reads.

There are restrictions — the view must be schema-bound and follow certain rules (no outer joins, no aggregate functions without grouping, etc.).

Example (SQL Server):

CREATE VIEW SalesView WITH SCHEMABINDING AS
SELECT CustomerID, SUM(SalesAmount) AS TotalSales
FROM dbo.Orders
GROUP BY CustomerID;

CREATE UNIQUE CLUSTERED INDEX idx_SalesView ON SalesView(CustomerID);


In short:
Yes — indexed views store data for better performance, mainly in SQL Server.

8. How to secure data using views?

Explanation:
Views help protect sensitive data by limiting what users can see.

Techniques:

Create views that exclude confidential columns (like salary, password).

Grant users access to the view instead of the base table.

Combine views with the WITH CHECK OPTION to restrict unwanted updates.

Example:

CREATE VIEW SafeEmployeeView AS
SELECT EmployeeID, EmployeeName, DepartmentName
FROM Employees;


→ HR staff can see names and departments but not salaries.

In short:
Use views to expose only the necessary data and hide sensitive columns.

9. What are limitations of views?

Key limitations:

Cannot include ORDER BY (unless with TOP or LIMIT).

Some views are read-only (can’t perform DML operations).

Cannot use parameters (unlike stored procedures).

Performance may drop for complex views with multiple joins.

Dependent on base tables — if base schema changes, view may fail.

In short:
Views are flexible but limited in updates, parameters, and performance.

10. How does WITH CHECK OPTION work?

Definition:
WITH CHECK OPTION ensures that any data modification through the view follows the view’s conditions.

Explanation:

When you insert or update through a view, the new data must satisfy the view’s WHERE clause.

Prevents inserting records that would not appear in the view.

Example:

CREATE VIEW ActiveEmployees AS
SELECT * FROM Employees WHERE Status = 'Active'
WITH CHECK OPTION;


→ If you try to update an employee’s status to 'Inactive' through this view, it will fail — because that record would disappear from the view.

In short:
WITH CHECK OPTION enforces the view’s filter rules during updates or inserts.
