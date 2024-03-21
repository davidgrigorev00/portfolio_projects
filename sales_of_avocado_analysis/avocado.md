
##### Final Dashboard:
[Link to the dashboard](http://redash.lab.karpov.courses/public/dashboards/WPi05JdtZSaOiKqUsdSCPSlTrcSaINfllY6aZYQ6?org_slug=default)




##### Data description:

| Column name      | Data Type         | Description                                     |
|------------------|-------------------|-------------------------------------------------|
| date             | Date              | Date                                            |
| average_price    | Decimal(12, 2)    | The average price of one avocado                |
| total_volume     | Decimal(12, 2)    | The number of avocados sold                     |
| plu4046          | Decimal(12, 2)    | The number of avocados sold with PLU 4046       |
| plu4225          | Decimal(12, 2)    | The number of avocados sold with PLU 4225       |
| plu4770          | Decimal(12, 2)    | The number of avocados sold with PLU 4770       |
| total_bags       | Decimal(12, 2)    | Total number of bags                            |
| small_bags       | Decimal(12, 2)    | Small bags                                      |
| large_bags       | Decimal(12, 2)    | Large bags                                      |
| xlarge_bags      | Decimal(12, 2)    | Extra large bags                                |
| type             | String            | Conventional or organic                         |
| year             | String            | Year                                            |
| region           | String            | The city or region of the observation           |


##### NY and SF volume proportion

###### My query:
```sql
WITH
    (SELECT SUM(total_volume) FROM default.avocado WHERE region = 'TotalUS') AS country_volume,
    (SELECT SUM(total_volume) FROM default.avocado WHERE region IN ('SanFrancisco', 'NewYork')) AS sfny_volume,
    (SELECT (country_volume - sfny_volume)) AS diff
SELECT
    region,
    CASE
        WHEN region IN ('SanFrancisco', 'NewYork') THEN SUM (total_volume)
        ELSE diff
    END AS volume,
    CASE
        WHEN region IN ('SanFrancisco', 'NewYork') THEN region
        ELSE 'Other US regions'
    END AS regions
        
FROM default.avocado
WHERE region IN ('SanFrancisco', 'NewYork', 'TotalUS')
GROUP BY region
```

###### Outcome:

| region    | volume          | regions         |
|-----------|-----------------|-----------------|
| TotalUS   | 5,488,175,862.49| Other US regions|
| NewYork   | 240,734,127.53  | NewYork         |
| SanFrancisco | 135,830,191.78  | SanFrancisco     |

###### Visualization:



##### Volume dynamics

###### My query:
```sql
SELECT
    month,
    SUM(us - (sf+ny)) AS Other_US,
    SUM(sf) AS SanFrancisco,
    SUM(ny) AS NewYork
FROM
    (
    SELECT
        toStartOfMonth(date) AS month,
        region,
        SUM(total_volume) filter (WHERE region = 'TotalUS') AS us,
        SUM(total_volume) filter (where region = 'SanFrancisco') AS sf,
        SUM(total_volume) filter (where region = 'NewYork') AS ny
    FROM default.avocado
    WHERE region IN ('TotalUS', 'SanFrancisco', 'NewYork')
    GROUP BY month, region
    ORDER BY month ASC
    )
GROUP BY month
```

###### Outcome:

| month    | Other_US       | SanFrancisco   | NewYork       |
|----------|----------------|----------------|---------------|
| 01/01/15 | 112,417,111.84 | 3,412,453.05   | 4,623,953.17  |
| 01/02/15 | 128,334,216.76 | 3,901,284.29   | 5,267,939.13  |
| 01/03/15 | 148,351,405.36 | 4,374,888.14   | 6,031,017.72  |
| 01/04/15 | 122,426,865.06 | 3,087,772.47   | 5,037,854.94  |
| 01/05/15 | 170,223,923.70 | 4,366,759.17   | 8,161,470.84  |
| 01/06/15 | 135,320,965.51 | 2,865,966.41   | 6,117,156.02  |
| 01/07/15 | 126,797,084.64 | 2,776,162.07   | 5,515,607.05  |
| 01/08/15 | 147,622,511.25 | 3,202,480.67   | 7,366,832.01  |
| 01/09/15 | 116,437,192.31 | 2,538,158.76   | 5,282,471.86  |
| 01/10/15 | 107,291,713.50 | 2,536,071.22   | 5,403,265.46  |
| 01/11/15 | 127,914,229.78 | 3,686,864.89   | 7,118,530.48  |
| 01/12/15 | 104,064,564.02 | 2,696,940.64   | 4,681,537.23  |
| 01/01/16 | 167,115,386.87 | 4,170,324.67   | 7,832,537.77  |
| 01/02/16 | 150,761,769.93 | 3,768,721.86   | 7,036,325.82  |
| 01/03/16 | 138,262,007.59 | 3,087,248.29   | 6,337,612.92  |
| 01/04/16 | 143,266,708.63 | 3,523,617.13   | 5,145,734.94  |
| 01/05/16 | 197,575,845.16 | 4,632,948.55   | 9,605,379.54  |
| 01/06/16 | 145,590,136.88 | 3,330,739.98   | 6,064,601.64  |
| 01/07/16 | 166,326,590.08 | 4,300,684.54   | 6,131,290.61  |
| 01/08/16 | 131,096,024.50 | 3,600,684.43   | 5,182,631.87  |
| 01/09/16 | 128,064,515.34 | 3,480,721.68   | 5,199,484.33  |
| 01/10/16 | 128,211,415.02 | 2,965,686.45   | 4,598,419.81  |
| 01/11/16 | 92,042,557.08  | 2,071,454.38   | 3,792,194.71  |
| 01/12/16 | 117,769,994.72 | 2,596,795.88   | 4,620,349.18  |
| 01/01/17 | 189,450,439.34 | 4,941,736.87   | 7,069,711.28  |
   |

###### Visualization:




##### Types of Avocado in NY & SF

###### My query:
```sql
SELECT 
    sum(total_volume),
    region,
    type
FROM default.avocado
WHERE
    region in ('SanFrancisco', 'NewYork')
GROUP BY region, type

```

###### Outcome:

| sum(total_volume) | region      | type         |
|-------------------|-------------|--------------|
| 8,990,571.40      | NewYork     | organic      |
| 3,785,935.69      | SanFrancisco| organic      |
| 231,743,556.13    | NewYork     | conventional |
| 132,044,256.09    | SanFrancisco| conventional |

###### Visualization:



##### plu & bags volume

###### My query:
```sql
SELECT
    SUM(plu4046) AS plu4046,
    SUM(plu4225) AS plu4225,
    SUM(plu4770) AS plu4770,
    SUM(small_bags) AS small_bags,
    SUM(large_bags) AS large_bags,
    SUM(xlarge_bags) AS xlarge_bags,
    region

FROM default.avocado
WHERE region IN ('SanFrancisco', 'NewYork')
GROUP BY region

```

###### Outcome:

| plu4046     | plu4225      | plu4770      | small_bags   | large_bags   | xlarge_bags | region       |
|-------------|--------------|--------------|--------------|--------------|-------------|--------------|
| 7,639,326.78| 163,353,842.88| 1,746,802.11 | 58,401,682.50 | 9,232,997.58 | 359,477.68 | NewYork      |
| 34,136,889.78| 83,153,061.55| 3,649,315.48 | 14,502,351.14 | 176,939.24   | 211,634.59 | SanFrancisco |


###### Visualizations:
