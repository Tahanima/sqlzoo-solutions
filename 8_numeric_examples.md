1. The example shows the number who responded for:

- question 1
- at 'Edinburgh Napier University'
- studying '(8) Computer Science'

**Show the the percentage who STRONGLY AGREE**

```sql
SELECT a_strongly_agree
FROM   nss
WHERE  question = 'Q01'
       AND institution = 'Edinburgh Napier University'
       AND subject = '(8) Computer Science';
```

2. **Show the institution and subject where the score is at least 100 for question 15.**

```sql
SELECT institution,
       subject
FROM   nss
WHERE  question = 'Q15'
       AND score >= 100;
```

3. **Show the institution and score where the score for '(8) Computer Science' is less than 50 for question 'Q15'**

```sql
SELECT institution,
       score
FROM   nss
WHERE  question = 'Q15'
       AND subject = '(8) Computer Science'
       AND score < 50;
```

4. **Show the subject and total number of students who responded to question 22 for each of the subjects '(8) Computer Science' and '(H) Creative Arts and Design'.**

```sql
SELECT subject,
       SUM(response)
FROM   nss
WHERE  question = 'Q22'
       AND subject IN ( '(8) Computer Science', '(H) Creative Arts and Design' )
GROUP  BY subject;
```

5. **Show the subject and total number of students who A_STRONGLY_AGREE to question 22 for each of the subjects '(8) Computer Science' and '(H) Creative Arts and Design'.**

```sql
SELECT subject,
       SUM(a_strongly_agree * response / 100)
FROM   nss
WHERE  question = 'Q22'
       AND subject IN ( '(8) Computer Science', '(H) Creative Arts and Design' )
GROUP  BY subject;
```

6. **Show the percentage of students who A_STRONGLY_AGREE to question 22 for the subject '(8) Computer Science' show the same figure for the subject '(H) Creative Arts and Design'.**

Use the **ROUND** function to show the percentage without decimal places.

```sql
SELECT subject,
       SUM(a_strongly_agree * response / 100) * 100 / SUM(response)
FROM   nss
WHERE  question = 'Q22'
       AND subject IN ( '(8) Computer Science', '(H) Creative Arts and Design' )
GROUP  BY subject;
```

7. **Show the average scores for question 'Q22' for each institution that include 'Manchester' in the name.**

The column **score** is a percentage - you must use the method outlined above to multiply the percentage by the **response** and divide by the total response. Give your answer rounded to the nearest whole number.

```sql
SELECT institution,
       SUM(score * response) / SUM(response)
FROM   nss
WHERE  question = 'Q22'
       AND ( institution LIKE '%Manchester%' )
GROUP  BY institution;
```

8. **Show the institution, the total sample size and the number of computing students for institutions in Manchester for 'Q01'.**

```sql
SELECT institution,
       SUM(sample),
       SUM(CASE
             WHEN subject = '(8) Computer Science' THEN sample
             ELSE 0
           end)
FROM   nss
WHERE  question = 'Q01'
       AND institution LIKE '%Manchester%'
GROUP  BY institution;
```
