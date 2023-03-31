    MIST 4610 Final Project

Erica Degue, Ashley Parker, Iti Jain, Isabella Escorcia, Cole Wright

Database Information

Database Name: ns_21479_7

    Github Links:
https://github.com/ericadegue

https://github.com/isa-escorcia

https://github.com/ashleyparker801 

https://github.com/iti-jain/ 

https://github.com/cole16wright 




    Problem Description
The problem we are trying to solve is related to improving the organization and efficiency of operations within a coffee shop franchise. The franchise has multiple locations and each location has a team of employees with different positions such as manager, chef, barista, etc. The main issue is that with the way that the coffee shop is currently structured, it is not optimized for maximum productivity, which results in longer wait times for customers and the store itself not being able to make as much profit. With our improvements, we are hoping to improve the coffee shop’s operations, reduce costs, and improve the customer experience overall.

    Data Model
![Alt Text](https://github.com/cole16wright/COLE-WRIGHT-SQL_PROJECT1/blob/main/original%20dm.png)

    Explanation of Data Model

For our data model, we decided to base ours on a coffee shop franchise. First, we created the Franchisee table to represent the company and the Stores entity to represent each location the company owns. Each store needs employees so we added the Employees entity and the Position entity. A store can have many employees but employees can only work at one store so it is a one to many relationship. A Position can have multiple employees but an employee can only work one position so this is also a one A coffee shop ideally has customers who make purchases which is why we added the Purchases and Customer entity. When there is a purchase, a product is being bought. Many products can be purchased and you can have many purchases of the same product so this would be a many-to-many relationship. An associative entity (ProductPurchase) is created. There are different types of products so we chose to represent this with three entities: food, drink, and merchandise. These would have a one to one relationship (can’t have overlap in products, each are separate products). For our augmentation, we decided to create a series of classes educating coffee fans about different aspects of coffee, the information regarding the class is stored in the Class entity. Since classes can have many customers and a customer can go to many classes, they have a many-to-many relationship. The associative entity tracking information about each customer’s attendance at a specific class is called Visit.

    Data Dictionary

   ![Alt Text](https://github.com/cole16wright/COLE-WRIGHT-SQL_PROJECT1/blob/main/dictionary1.png)

![Alt Text](https://github.com/cole16wright/COLE-WRIGHT-SQL_PROJECT1/blob/main/d2.png)

![Alt Text](https://github.com/cole16wright/COLE-WRIGHT-SQL_PROJECT1/blob/main/d3.png)

![Alt Text](https://github.com/cole16wright/COLE-WRIGHT-SQL_PROJECT1/blob/main/d4.png)

![Alt Text](https://github.com/cole16wright/COLE-WRIGHT-SQL_PROJECT1/blob/main/d5.png)

![Alt Text](https://github.com/cole16wright/COLE-WRIGHT-SQL_PROJECT1/blob/main/d6.png)

![Alt Text](https://github.com/cole16wright/COLE-WRIGHT-SQL_PROJECT1/blob/main/d7.png)

![Alt Text](https://github.com/cole16wright/COLE-WRIGHT-SQL_PROJECT1/blob/main/d8.png)

    Ten Queries

1. #How many baristas are at the Johns Creek store?

SELECT COUNT(employeeID)
FROM Employee
JOIN Store ON Store.storeID = Employee.storeID
JOIN Position ON Position.positionID = Employee.positionID
WHERE location IN ("Johns Creek") AND positionName IN ("Barista");

Explanation: This query lists the number of baristas at the Johns Creek location of the business. This information is helpful for keeping employment records and creating shift schedules for the store. 

2. #List the customers that spent more than the average purchase and their purchase amount, ordered by descending price
SELECT Customer.fName, Customer.lName, totalAmount
FROM Customer
JOIN Purchase ON Customer.customerID = Purchase.customerID
WHERE totalAmount> (SELECT AVG(totalAmount) FROM Purchase)
ORDER BY totalAmount DESC;

Explanation: This query lists the customers that spend a higher amount than average at the coffee shop. This information is useful because these are the business’s “super spenders”, they may be eligible for special discounts or deserve special treatment from employees for their loyalty.

3. #List all the customers in the coffee shop's database
SELECT *
FROM Customer
LEFT JOIN Purchase ON Purchase.customerID = Customer.customerID;

Explanation: This query lists all the employees in the coffee shop’s database, regardless of if they’ve made a purchase or not. This query is useful as it contains every customer who has registered with the store and can be used as a mailing list.

4. #List the drink products that have not been purchased and cost less than the average price of a drink ordered by price
SELECT *
FROM Product
JOIN Drink ON Product.productID = Drink.productID
WHERE NOT EXISTS (SELECT * FROM ProductPurchase WHERE Product.productID = ProductPurchase.productID)
AND Drink.price < (SELECT AVG(Drink.price) FROM Drink)
ORDER BY Drink.price;

Explanation: This lists all the drinks below the average drink price that have not been purchased. This information is useful because it contains the drinks that should potentially be taken off the menu as they are unpopular. 

5. #Count the number of customers that have visited each store and the number of purchases those customers have made
SELECT location, COUNT(DISTINCT Customer.customerID) AS "Customers", COUNT(Purchase.purchaseID) AS "Purchases"
FROM Store
JOIN Purchase ON Store.storeID = Purchase.storeID
JOIN Customer ON Customer.customerID = Purchase.customerID
GROUP BY location;

Explanation: This query records how many customers have visited each store and how many orders those customers have made. This information is important as it can demonstrate which shops are more successful than others as well as the volume of orders from each customer.

6. #What percent of total customers visit each location of the coffeeshop?
SELECT location, CONCAT('%', ROUND(100*COUNT(Customer.customerID)/(SELECT COUNT(*) FROM Customer),2)) AS "total percent"
FROM Customer
JOIN Purchase ON Customer.customerID = Purchase.customerID
JOIN Store ON Store.storeID = Purchase.storeID
GROUP BY location;

Explanation: This query records the percentage of total customers each location gets. This information is important as it displays easy to read information about the proportion of sales from each location.

7. #Write a query to list the customer name and total values of payments made by the customer.
SELECT fName, lName, SUM(totalAmount)
FROM Customer
JOIN Purchase ON Customer.customerID = Purchase.customerID
GROUP BY fName, lName;

Explanation: This query has relevant data that is essential for top management to see about how much each customer is spending and the range of that value. They can use this to see if they should raise or lower prices, have a discount/coupon, and estimate the amount of revenue that the shop will bring in per day/week/month. 

8. #List the first and last name of the customers whose order total was more than $15 and the product ID(s) of what they purchased

SELECT fName, lName, totalAmount, productID
FROM Customer
JOIN Purchase ON Customer.customerID = Purchase.customerID
JOIN ProductPurchase ON Purchase.purchaseID = ProductPurchase.purchaseID
WHERE totalAmount > 15;

Explanation: This query explains information regarding which customers spent more than $15 and which items they were purchasing. The higher each customer’s bill, the more profit for the business as a whole. It’s key to the coffee shop to see what customers seem to be purchasing more so that they can make sure they have enough inventory of that and promote those products more. Additionally, they can see what products are not selling and are not worth keeping on the menu and potentially replace them with other items that can generate more money.


9. #List the employee names of those employees whose position is higher than chef, list their wage, and list where they work

select Employee.fName, Employee.lName, location, wage
from Employee
join Position on Position.positionID = Employee.positionID
join Store on Store.storeID = Employee.storeID
where Employee.positionID <=100102;

Explanation: This query lists out the managers and assistant managers in the database, as well as their wages and location. These employees are typically who will be contacted if there is a concern at a specific branch, or important news to be shared at a location. If there is a problem at a branch, it will be easy to see who is in charge there. With the wages also listed, it will be easy to decide if the employee is doing enough to earn that wage, or if they are due for a raise. It is always important to have quick access to high ranking employees as they hold the most responsibility and highest wages within a branch.

10. #List out the names of food items that are greater that the average price of all the food items and order by descending price order

SELECT name 
FROM Food 
WHERE price > (SELECT AVG(price) FROM Food)
ORDER BY price DESC;


Explanation: This query lists out the name of the food items that are greater than the average price of the rest of the food items on the menu. This query is useful to determine what their most expensive food items are. They can use the results to make sure that the prices that they are charging are fair across the board. We went ahead and ordered the result in descending order.

![Alt Text](https://github.com/cole16wright/COLE-WRIGHT-SQL_PROJECT1/blob/main/checklist.png)

    Augmentation

New Model:

![Alt Text](https://github.com/cole16wright/COLE-WRIGHT-SQL_PROJECT1/blob/main/Augmented%20Data%20Model.png)

    Augmentation Data Dictionary

![Alt Text](https://github.com/cole16wright/COLE-WRIGHT-SQL_PROJECT1/blob/main/d9.png) 

    Explanation

To further improve the business, our team decided to add a coffee-making class where customers can learn how to make different types of drinks offered by the shop and cultivate their appreciation for coffee. With this implementation, more traffic will be driven to the business, increasing profit. We decided to make a new entity called Coffee Class and connect it to our Customer Table using a many-to-many relationship. We decided to call the junction table ‘Attendance’ and use this to help track who comes to the shop, the start time, and end time. 
