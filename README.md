# Datajourney-
I'm Jose, a reformed philologist who has ventured into the realms of accounting and data, driven by an insatiable quest for new answers and insights.


 ## SQL Project Apartments Prices in Poland

 The dataset contains apartment sales and rent offers from the 15 largest cities in Poland (Warsaw, Lodz, Krakow, Wroclaw, Poznan, Gdansk, Szczecin, Bydgoszcz, Lublin, Katowice, Bialystok, Czestochowa). The data comes from local websites with apartments for sale. To fully capture the neighborhood of each apartment better, each offer was extended by data from the Open Street Map with distances to points of interest (POI). The data is collected monthly and covers a timespan between August 2023 and June 2024. 

## Goal

The main purpose is to explore and analyze different trends in the aparments in Poland by performing a different set of SQL Queries that can provide insights about housing market between 2023 and 2024. 

## Explore and Analyze

-- 1 What is the average rental and sale price of flats in each city from August 2023 to March 2024?

SELECT City, 
type, 
ROUND(AVG(price), 2) AS Average_Price
FROM (
SELECT City, type, price FROM apartments_pl_2023_08
UNION ALL
SELECT City, type, price FROM apartments_pl_2023_09
UNION ALL
SELECT City, type, price FROM apartments_pl_2023_10
UNION ALL
SELECT City, type, price FROM apartments_pl_2023_11
UNION ALL
SELECT City, type, price FROM apartments_pl_2023_12
UNION ALL
SELECT City, type, price FROM apartments_pl_2024_01
UNION ALL 
SELECT City, type, price FROM apartments_pl_2024_02
UNION ALL
SELECT City, type, price FROM apartments_pl_2024_03) AS All_apartments
WHERE Type = 'apartmentbuilding'
GROUP BY City, type
ORDER BY Average_Price DESC;

![image](https://github.com/user-attachments/assets/04219d57-0a50-4628-b77a-8844916ac3ea)


-- 2 How many flats in Warsaw are larger than 100 metres?

SELECT City, type, squaremeters
FROM apartments_pl_2023_08
WHERE Type = 'apartmentbuilding' AND squaremeters > 100 AND City = "Warszawa"
ORDER BY squareMeters DESC;

-- 3 What type of housing is most common in Poland from August 2023 to March 2024?

SELECT type, COUNT(*) AS count_of_type
FROM (
    SELECT type FROM Apartments_pl_2023_08
    UNION ALL
    SELECT type FROM Apartments_pl_2023_09
    UNION ALL
    SELECT type FROM Apartments_pl_2023_10
    UNION ALL
    SELECT type FROM Apartments_pl_2023_11
    UNION ALL
    SELECT type FROM Apartments_pl_2023_12
    UNION ALL
    SELECT type FROM Apartments_pl_2024_01
    UNION ALL
    SELECT type FROM Apartments_pl_2024_02
    UNION ALL
    SELECT type FROM Apartments_pl_2024_03
) AS all_types
GROUP BY type
ORDER BY count_of_type DESC
LIMIT 1;

![image](https://github.com/user-attachments/assets/09427e80-fe39-439d-b65e-5d654ed425b7)


-- 4 What are the five most expensive flats in Bydgoszcz in December 2023?

SELECT City, type, price, squareMeters
FROM apartments_pl_2023_12
WHERE type =  'apartmentbuilding' AND city = 'Bydgoszcz'
ORDER BY Price DESC
LIMIT 5

![image](https://github.com/user-attachments/assets/7e115ed2-0464-4102-924b-dbae3bf1e370)


-- 5 Merge all month tables

CREATE VIEW Apartments_Poland AS
SELECT * FROM Apartments_pl_2023_08
UNION ALL
SELECT * FROM Apartments_pl_2023_09
UNION ALL
SELECT * FROM Apartments_pl_2023_10
UNION ALL
SELECT * FROM Apartments_pl_2023_11
UNION ALL
SELECT * FROM Apartments_pl_2023_12
UNION ALL
SELECT * FROM Apartments_pl_2024_01
UNION ALL
SELECT * FROM Apartments_pl_2024_02
UNION ALL
SELECT * FROM Apartments_pl_2024_03;

![image](https://github.com/user-attachments/assets/d52f4db6-7b91-431d-b610-3214d2ab3eaa)


-- 6  How many flats in Wroclaw have the word ‘balcony’ in their characteristics?

SELECT City, COUNT(*) AS total_apartments_with_balcony
FROM Apartments_Poland
WHERE City = 'wroclaw' AND hasBalcony = 'yes';

![image](https://github.com/user-attachments/assets/99b2629f-23a0-4271-ba3e-493d4c27ac7e)


-- 7 What type of housing was the most built in Poland during the 1960s and 1990s?

SELECT COUNT(*) AS Total_houses, Type
FROM Apartments_Poland 
WHERE Type IS NOT NULL AND type <> '' AND BuildYear  BETWEEN 1960 AND 1990 
GROUP BY Type;

![image](https://github.com/user-attachments/assets/d3ad2c14-8959-41e4-aa90-a0a757ea12a0)
