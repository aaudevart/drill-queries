# Apache Drill
Apache Drill : Schema-free SQL Query Engine for Hadoop, NoSQL and Cloud Storage

This github project contains the code source of my [Apache Drill article](https://www.ekito.fr/people/drill/)

![alt text][logo]

[logo]: https://mapr.com/en/blog/importance-apache-drill-big-data-ecosystem/assets/drill-logo-400_5.png "Apache Drill"

## Instructions for use

For installation instructions, go to the [Official documentation](https://drill.apache.org/docs/installing-drill-in-embedded-mode)

## Pre-requisite
Please, refer to my [blog article](https://www.ekito.fr/people/drill/)
- Configure a workspace named ekito : directory where you download this project
- Update default CSV definition in order to add the option extractHeader

## Data
- Download the [flights data](http://stat-computing.org/dataexpo/2009/) into the csv directory
- This project already contains JSON airports data.

## Let's go to request :-)

### Query 1: Display 10 airports :
```
SELECT *
FROM dfs.`ekito`.`/json/`
LIMIT 10;
```

### Query 2: Display the count of airports:
```
SELECT count(*) as nb_airports
FROM dfs.`ekito`.`/json/`;
```

### Query 3: Display the  airports count by state:
```
SELECT state, count(*) as nb_airports
FROM dfs.`ekito`.`/json/`
WHERE state in ('CA', 'FL')
GROUP BY state;
```


### Query 4: Display 10 flights:
```
SELECT `Year`, `Month`, `DayofMonth`, `DepTime`, `ArrTime`, `ArrDelay`, `DepDelay`, `Origin`
FROM dfs.`ekito`.`/csv/`
LIMIT 10;
```


### Query 5: When is the best time to minimize delay?
```
SELECT `DayOfWeek`, ROUND(AVG(CAST(`ArrDelay` as INTEGER)) ,2) as average_delay
FROM dfs.`ekito`.`/csv`
WHERE `ArrDelay` not in ('NA')
GROUP BY `DayOfWeek`
ORDER BY `DayOfWeek`;
```


### Query 6: Display average delay per destination airport
```
SELECT airports.airport, ROUND(AVG(CAST(flights.`ArrDelay` as INTEGER)) ,2) as average_delay
FROM dfs.`ekito`.`/csv/` as flights
JOIN dfs.`ekito`.`/json/` as airports
ON airports.iata = flights.Dest
WHERE flights.`ArrDelay` not in ('NA')
GROUP BY airports.airport
ORDER BY average_delay
LIMIT 10;
```

## That's all for today ! Enjoy ;-)
