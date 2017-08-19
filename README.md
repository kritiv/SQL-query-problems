# SQL-query-problems
Various query problems solved

Short database description "Computer firm":

The database scheme consists of four tables:
Product(maker, model, type)
PC(code, model, speed, ram, hd, cd, price)
Laptop(code, model, speed, ram, hd, screen, price)
Printer(code, model, color, type, price)

#Q-1 : List the models of any type having the highest price of all products present in the database.
Note: Usage of Common Table Expression(CTE)

with Any_Expensive_Model 
As
(Select Model, price from PC
UNION 
Select Model, price from Laptop
UNION 
Select Model, price from Printer 
)
select model from Any_Expensive_Model where price=(Select max(price) from Any_Expensive_Model)

#Q-2
