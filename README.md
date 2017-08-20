# SQL-query-problems
Various query problems solved

Short database description "Computer firm":

The database scheme consists of four tables:
Product(maker, model, type)
PC(code, model, speed, ram, hd, cd, price)
Laptop(code, model, speed, ram, hd, screen, price)
Printer(code, model, color, type, price)

#Q-24 : List the models of any type having the highest price of all products present in the database.
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

#Q-25 :Find the printer makers also producing PCs with the lowest RAM capacity and the highest processor speed of all PCs having the lowest RAM capacity. 

select T1.maker from 
(Select maker, min(Ram) As ram from Product p inner join PC on p.model=pc.model group by ram, maker) T1, 
(Select maker from Product p inner join printer pr on p.model=pr.model) T2, 
(Select maker, max(speed) As speed from Product p inner join PC on p.model=pc.model group by speed, maker) T3 
where T1.maker=T2.maker AND T2.maker=T3.maker 
group by T1.maker
-------------
SELECT distinct maker FROM product WHERE type = 'printer'
AND maker IN (SELECT maker
FROM product JOIN ( 
SELECT model, speed, ram FROM pc WHERE speed =
                (SELECT MAX(speed) FROM pc
                WHERE ram = (SELECT MIN(ram) FROM pc)
                )
  AND ram = (SELECT MIN(ram) FROM pc)
            ) b
ON product.model = b.model)
