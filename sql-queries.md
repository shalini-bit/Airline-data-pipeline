List of SQL queries Performed on the Processed Airline Data : 

## Cheapest Airline (Overall)
**Insight:** Identifies the most budget-friendly airline.
```sql
SELECT TOP 1 airline,
       ROUND(AVG(CAST(price AS FLOAT)), 2) AS avg_price
FROM Airlines
GROUP BY airline
ORDER BY avg_price ASC;
```


---

##  Most Expensive Route
**Insight:** Finds the costliest route in the dataset.
```sql
SELECT TOP 1 source_city, destination_city,
       MAX(CAST(price AS FLOAT)) AS max_price
FROM Airlines
GROUP BY source_city, destination_city
ORDER BY max_price DESC;
```



---

## Average Price by Class (Business vs Economy)
**Insight:** Compares pricing between travel classes.
```sql
SELECT class,
       ROUND(AVG(CAST(price AS FLOAT)), 2) AS avg_price
FROM Airlines
GROUP BY class;
```



---

## Direct vs Connecting Flights Count
**Insight:** Shows distribution of direct and connecting flights.

```sql
SELECT stops,
       COUNT(*) AS total_flights
FROM Airlines
GROUP BY stops;
```



---

## Top 5 Busiest Routes
**Insight:** Identifies most frequently traveled routes.

```sql
SELECT TOP 5 source_city, destination_city,
       COUNT(*) AS total_flights
FROM Airlines
GROUP BY source_city, destination_city
ORDER BY total_flights DESC;
```

---

## Price Trend Based on Days Left
**Insight:** Shows how ticket prices change as departure date approaches.

```sql
SELECT days_left,
       ROUND(AVG(CAST(price AS FLOAT)), 2) AS avg_price
FROM Airlines
GROUP BY days_left
ORDER BY days_left;
```



---

##  Longest Duration Flights
**Insight:** Highlights longest flights.

```sql
SELECT TOP 5 airline, flight, source_city, destination_city,
       CAST(duration AS FLOAT) AS duration_hours
FROM Airlines
ORDER BY duration_hours DESC;
```


---

## Cheapest Flights for Each Route
**Insight:** Finds cheapest travel options per route.

```sql
SELECT source_city, destination_city,
       MIN(CAST(price AS FLOAT)) AS cheapest_price
FROM Airlines
GROUP BY source_city, destination_city
ORDER BY cheapest_price;
```



---

##  Airline with Most Flights
**Insight:** Identifies the most active airline.

```sql
SELECT TOP 1 airline,
       COUNT(*) AS total_flights
FROM Airlines
GROUP BY airline
ORDER BY total_flights DESC;
```

---

## Ranking Flights by Price 
**Insight:** Ranks flights within each route from cheapest to most expensive.

```sql
SELECT source_city, destination_city, airline, flight,
       CAST(price AS FLOAT) AS price,
       RANK() OVER (PARTITION BY source_city, destination_city ORDER BY CAST(price AS FLOAT)) AS price_rank
FROM Airlines;
```
