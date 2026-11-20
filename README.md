
# Assignment Day 18 - Specialized SQL Techniques

**Nama:** Muhammad Ferran Hilmadiansyah

---

## 1. The Report

**Link soal:** [The Report - HackerRank](https://www.hackerrank.com/challenges/the-report/problem?isFullScreen=true)

### Query

```sql
SELECT
    CASE
        WHEN (
            CASE
                WHEN MARKS BETWEEN 0 AND 9 THEN 1
                WHEN MARKS BETWEEN 10 AND 19 THEN 2
                WHEN MARKS BETWEEN 20 AND 29 THEN 3
                WHEN MARKS BETWEEN 30 AND 39 THEN 4
                WHEN MARKS BETWEEN 40 AND 49 THEN 5
                WHEN MARKS BETWEEN 50 AND 59 THEN 6
                WHEN MARKS BETWEEN 60 AND 69 THEN 7
                WHEN MARKS BETWEEN 70 AND 79 THEN 8
                WHEN MARKS BETWEEN 80 AND 89 THEN 9
                WHEN MARKS BETWEEN 90 AND 100 THEN 10
            END
        ) < 8
        THEN NULL
        ELSE NAME
    END AS NAME,

    CASE
        WHEN MARKS BETWEEN 0 AND 9 THEN 1
        WHEN MARKS BETWEEN 10 AND 19 THEN 2
        WHEN MARKS BETWEEN 20 AND 29 THEN 3
        WHEN MARKS BETWEEN 30 AND 39 THEN 4
        WHEN MARKS BETWEEN 40 AND 49 THEN 5
        WHEN MARKS BETWEEN 50 AND 59 THEN 6
        WHEN MARKS BETWEEN 60 AND 69 THEN 7
        WHEN MARKS BETWEEN 70 AND 79 THEN 8
        WHEN MARKS BETWEEN 80 AND 89 THEN 9
        WHEN MARKS BETWEEN 90 AND 100 THEN 10
    END AS GRADE,

    MARKS
FROM STUDENTS
ORDER BY GRADE DESC, NAME ASC
```

### Hasil

![Hasil Query No 1](Hasil%20Query%20No%201.png)

---

## 2. Weather Observation Station 18

**Link soal:** [Weather Observation Station 18 - HackerRank](https://www.hackerrank.com/challenges/weather-observation-station-18/problem?isFullScreen=true)

### Query

```sql
SELECT
    ROUND(
        (MAX(LAT_N) - MIN(LAT_N))
        +
        (MAX(LONG_W) - MIN(LONG_W)),
        4
    ) AS LOCATION
FROM STATION;
```

### Hasil

![Hasil Query No 2](Hasil%20Query%20No%202.png)

---

## 3. Top Competitors

**Link soal:** [Top Competitors - HackerRank](https://www.hackerrank.com/challenges/full-score/problem?isFullScreen=true)

### Query

```sql
SELECT
    h.hacker_id,
    h.name
FROM Submissions s
JOIN Hackers h ON s.hacker_id = h.hacker_id
JOIN Challenges c ON s.challenge_id = c.challenge_id
JOIN Difficulty d ON c.difficulty_level = d.difficulty_level
WHERE s.score = d.score
GROUP BY h.hacker_id, h.name
HAVING COUNT(DISTINCT s.challenge_id) > 1
ORDER BY COUNT(DISTINCT s.challenge_id) DESC,
         h.hacker_id ASC;
```

### Hasil

![Hasil Query No 3](Hasil%20Query%20No%203.png)

---

## 4. Ollivander's Inventory

**Link soal:** [Ollivander's Inventory - HackerRank](https://www.hackerrank.com/challenges/harry-potter-and-wands/problem?isFullScreen=true)

### Query

```sql
WITH minw AS (
    SELECT
        wp.age,
        w.power,
        MIN(w.coins_needed) AS min_coins
    FROM Wands w
    JOIN Wands_Property wp ON w.code = wp.code
    WHERE wp.is_evil = 0
    GROUP BY wp.age, w.power
)

SELECT
    w.id,
    wp.age,
    w.coins_needed,
    w.power
FROM Wands w
JOIN Wands_Property wp ON w.code = wp.code
JOIN minw mw
    ON wp.age = mw.age
    AND w.power = mw.power
    AND w.coins_needed = mw.min_coins
WHERE wp.is_evil = 0
ORDER BY w.power DESC, wp.age DESC;
```

### Hasil

![Hasil Query No 4](Hasil%20Query%20No%204.png)

---

## 5. Contest Leaderboard

**Link soal:** [Contest Leaderboard - HackerRank](https://www.hackerrank.com/challenges/contest-leaderboard/problem?isFullScreen=true)

### Query

```sql
WITH A AS (
    SELECT
        hacker_id,
        challenge_id,
        MAX(score) AS max_score
    FROM Submissions
    GROUP BY hacker_id, challenge_id
)

SELECT
    h.hacker_id,
    h.name,
    SUM(A.max_score) AS total_score
FROM Hackers h
JOIN A ON h.hacker_id = A.hacker_id
GROUP BY h.hacker_id, h.name
HAVING SUM(A.max_score) > 0
ORDER BY total_score DESC, h.hacker_id ASC;
```

### Hasil

![Hasil Query No 5](Hasil%20Query%20No%205.png)
