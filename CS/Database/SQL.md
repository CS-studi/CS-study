# ๐ฌ SQL
## ๐ Table of contents
> SQL

> JOIN

> SQL injection

## ๐ฌ SQL ์ด๋?

SQL์ Structured Query Language, ๊ตฌ์กฐ์  ์ง์ ์ธ์ด์ ์ค์๋ง๋ก, ๊ด๊ณํ ๋ฐ์ดํฐ๋ฒ ์ด์ค ์์คํ(RDBMS)์์ ์๋ฃ๋ฅผ ๊ด๋ฆฌ ๋ฐ ์ฒ๋ฆฌํ๊ธฐ ์ํด ์ค๊ณ๋ ์ธ์ด์๋๋ค.

### SQL ๋ฌธ๋ฒ์ ์ข๋ฅ
- DDL(Data Definition Language, ๋ฐ์ดํฐ ์ ์ ์ธ์ด)
    - ๊ฐ ๋ฆด๋ ์ด์์ ์ ์ํ๊ธฐ ์ํด ์ฌ์ฉํ๋ ์ธ์ด(CREATE, ALTER, DROP)
- DML(Data Manipulation Language, ๋ฐ์ดํฐ ์กฐ์ ์ธ์ด)
    - ๋ฐ์ดํฐ๋ฅผ ์ถ๊ฐ/์์ /์ญ์ ํ๊ธฐ ์ํ, ์ฆ ๋ฐ์ดํฐ ๊ด๋ฆฌ๋ฅผ ์ํ ์ธ์ด(SELECT, INSERT, UPDATE)
- DCL(Data Control Language, ๋ฐ์ดํฐ ์ ์ด ์ธ์ด)
    - ์ฌ์ฉ์ ๊ด๋ฆฌ ๋ฐ ์ฌ์ฉ์๋ณ๋ก ๋ฆฐ๋ ์ด์ ๋๋ ๋ฐ์ดํฐ๋ฅผ ๊ด๋ฆฌํ๊ณ  ์ ๊ทผํ๋ ๊ถํ์ ๋ค๋ฃจ๊ธฐ ์ํ ์ธ์ด(GRANT, REVOKE)

### SQL ์ธ์ด์  ํน์ง

1. SQL์ ๋์๋ฌธ์๋ฅผ ๊ฐ๋ฆฌ์ง ์์
2. SQL ๋ช๋ น์ด ๋์ `;`์ ๋ถ์ฌ์ผ ํ๋ค.
3. ๊ณ ์ ์ ๊ฐ์ `''` ๋ฐ์ดํ๋ก ๊ฐ์ผ๋ค. `ex. SELECT * FRIM LAWYER WHERE NAME = 'steve';`
4. SQL์์ ๊ฐ์ฒด๋ฅผ ๋ํ๋ผ ๋๋ ๋ฐฑํฑ(``)์ผ๋ก ๊ฐ์ผ๋ค. `ex. SELECT `COST`, `TYPE` FROM `INVOICE`;`
5. ์ฃผ์์ ๋ฌธ์ฅ์์ `--`์ ๋ถ์
6. ์ฌ๋ฌ ์ค ์ฃผ์ `/* */`

<br><br>

## ๐ฌ JOIN
> JOIN์ ๋๊ฐ ์ด์์ ํ์ด๋ธ์ด๋ ๋ฐ์ดํฐ๋ฒ ์ด์ค๋ฅผ ์ธ๋ํค๋ฅผ ํตํ์ฌ ์ฐ๊ฒฐํ์ฌ ๋ฐ์ดํฐ๋ฅผ ๊ฒ์ํ๋ ๋ฐฉ๋ฒ(์ ์ด๋ ํ๋์ attribute๋ฅผ ๊ณต์ ํ๊ณ  ์์ด์ผ ํ๋ค.)

### JOIN์ ์ข๋ฅ
- INNET JOIN
- LEFT OURTER JOIN
- RIGHT OUTER JOIN
- FULL OUTER JOIN
- CROSS JOIN
- SELF JOIN

Table ์์

>ANIMAL_INS

|NAME|	TYPE|	NULLABLE|
|----|------|-----------|
|ANIMAL_ID|VARCHAR(N)|FALSE|
|ANIMAL_TYPE|VARCHAR(N)|FALSE|
|DATETIME	|DATETIME	|FALSE|
|INTAKE_CONDITION|	VARCHAR(N)	|FALSE|
|NAME|	VARCHAR(N)|	TRUE|
|SEX_UPON_INTAKE|	VARCHAR(N)|

<br>

> ANIMAL_OUTS table

|NAME|	TYPE|	NULLABLE|
|----|------|-----------|
|ANIMAL_ID	|VARCHAR(N)	|FALSE|
|ANIMAL_TYPE|	VARCHAR(N)|	FALSE|
|DATETIME	|DATETIME	|FALSE|
|NAME	|VARCHAR(N)|	TRUE|
|SEX_UPON_OUTCOME	|VARCHAR(N)|	FALSE|

[์์ ](https://programmers.co.kr/learn/courses/30/lessons/59043)

#### INNER JOIN

>![IJ](img/SQL/ij.png)

=> ๊ต์งํฉ์ผ๋ก ๊ธฐ์ค ํ์ด๋ธ๊ณผ join ํ์ด๋ธ์ ์ค๋ณต๋ ๊ฐ์ ๋ณด์ฌ์ค๋ค.
Inner Join์ ๊ณตํต๋ ์์๋ค์ ํตํด ๊ฒฐํฉํ๋ ์กฐ์ธ ๋ฐฉ์์ผ๋ก sql์์ ์ผ๋ฐ์ ์ผ๋ก ์ฌ์ฉ๋๋ join์๋๋ค.

> query : ๋ณดํธ์์ ๋ค์ด์จ ์ ๋ณด์ ๋๊ฐ ์ ๋ณด๊ฐ ์๋ ๋๋ฌผ์ id๋ฅผ ์ถ๋ ฅํ์์ค.
```SQL
select ins.animal_id
from animal_ins as ins
join animal_outs as outs on ins.animal_id = outs.animal_id
```

#### LEFT OUTER JOIN

>![loj](img/SQL/loj.png)

=> ๊ธฐ์คํ์ด๋ธ๊ฐ๊ณผ ์กฐ์ธ ํ์ด๋ธ๊ณผ ์ค๋ณต๋ ๊ฐ์ ๋ณด์ฌ์ค๋ค. ์ผ์ชฝํ์ด๋ธ ๊ธฐ์ค์ผ๋ก join์ ํ๋ค๊ณ  ์๊ฐํ๋ฉด ํธํ๋ค. ์ผ์ชฝ ํ์ด๋ธ์ ๊ธฐ์ค์ผ๋ก joinํ๊ธฐ ๋๋ฌธ์ ์ผ์ชฝ์๋ id๊ฐ์ด ์์ง๋ง, ์ค๋ฅธ์ชฝ ํ์ด๋ธ์ id๊ฐ ๋๊ฐ๋ ํฌํจํ์ฌ ์ถ๋ ฅํ๋ค.

```SQL
select ins.animal_id
from animal_ins as ins
left outer join animal_outs as outs on ins.animal_id = outs.animal_id
```

[์์ ](https://programmers.co.kr/learn/courses/30/lessons/59043)

> ๋ฌธ์ )๊ด๋ฆฌ์์ ์ค์๋ก ์ผ๋ถ ๋๋ฌผ์ ์์์ผ์ด ์๋ชป ์๋ ฅ๋์์ต๋๋ค. ๋ณดํธ ์์์ผ๋ณด๋ค ์์์ผ์ด ๋ ๋น ๋ฅธ ๋๋ฌผ์ ์์ด๋์ ์ด๋ฆ์ ์กฐํํ๋ SQL๋ฌธ์ ์์ฑํด์ฃผ์ธ์. ์ด๋ ๊ฒฐ๊ณผ๋ ๋ณดํธ ์์์ผ์ด ๋น ๋ฅธ ์์ผ๋ก ์กฐํํด์ผํฉ๋๋ค.

>์ ๋ต
```sql
SELECT ins.animal_id, ins.name
from animal_ins as ins
left outer join animal_outs as outs on ins.animal_id = outs.animal_id
where outs.datetime < ins.datetime
order by ins.datetime
```

#### RIGHT OUTER JOIN

>![roj](img/SQL/roj.png)

=> ๊ธฐ์คํ์ด๋ธ๊ฐ๊ณผ ์กฐ์ธ ํ์ด๋ธ๊ณผ ์ค๋ณต๋ ๊ฐ์ ๋ณด์ฌ์ค๋ค. ์ค๋ฅธ์ชฝํ์ด๋ธ ๊ธฐ์ค์ผ๋ก join์ ํ๋ค๊ณ  ์๊ฐํ๋ฉด ํธํ๋ค. ์ค๋ฅธ์ชฝ ํ์ด๋ธ์ ๊ธฐ์ค์ผ๋ก join์ ํ๊ธฐ ๋๋ฌธ์ ์ผ์ชฝ ํ์ด๋ธ์ id๊ฐ null์ธ ๊ฒ๋ ํฌํจํ์ฌ ์ถ๋ ฅํ๋ค.

```SQL
select ins.animal_id
from animal_ins as ins
right outer join animal_outs as outs on ins.animal_id = outs.animal_id
```

[์์ ](https://programmers.co.kr/learn/courses/30/lessons/59042)

> ๋ฌธ์ ) ์ฒ์ฌ์ง๋ณ์ผ๋ก ์ธํด ์ผ๋ถ ๋ฐ์ดํฐ๊ฐ ์ ์ค๋์์ต๋๋ค. ์์ ๊ฐ ๊ธฐ๋ก์ ์๋๋ฐ, ๋ณดํธ์์ ๋ค์ด์จ ๊ธฐ๋ก์ด ์๋ ๋๋ฌผ์ id์ ์ด๋ฆ์ id ์์ผ๋ก ์กฐํํ๋ sql๋ฌธ์ ์์ฑํด์ฃผ์ธ์.

>์ ๋ต ์ฝ๋
```sql
SELECT a.animal_id, b.animal_id, b.name
from animal_ins as a right outer join animal_outs as b
on a.animal_id = b.animal_id
-- where a.animal_id is null
order by b.animal_id asc
```

#### FULL OUTER JOIN

>![loj](img/SQL/foj.png)

=> Ins์ Outs ํ์ด๋ธ์ ๋ชจ๋  ๋ฐ์ดํฐ๊ฐ ๊ฒ์๋๋ค.

```SQL
select ins.animal_id
from animal_ins as ins
full outer join animal_outs as outs on ins.animal_id = outs.animal_id
```

#### CROSS JOIN

>![cj](img/SQL/cj.png)

=> ๋ชจ๋  ๊ฒฝ์ฐ์ ์๋ฅผ ์ ๋ถ ํํํด์ฃผ๋ ๋ฐฉ์์ด๋ค. A๊ฐ 3๊ฐ, B๊ฐ 4๊ฐ๋ผ๋ฉด ์ด 3*4 = 12๊ฐ์ ๋ฐ์ดํฐ๊ฐ ๊ฒ์๋๋ค.

```SQL
select ins.animal_id, outs.animal_id, hour(ins.datetime) as hour
from animal_ins as ins
cross join animal_outs as outs
where hour(ins.datetime) < 10
order by hour
```

#### SELF JOIN

>![sj](img/SQL/sj.png)

=> ์๊ธฐ ์์ ๊ณผ ์๊ธฐ ์์ ์ ์กฐ์ธํ๋ ๊ฒ์ด๋ค. ํ๋์ ํ์ด๋ธ์ ์ฌ๋ฌ๋ฒ ๋ณต์ฌํด์ ์กฐ์ธํ๋ค๊ณ  ์๊ฐํ๋ฉด ํธํ๋ค. ์์ ์ด ๊ฐ์ง๊ณ  ์๋ ์นผ๋ผ์ ๋ค์ํ๊ฒ ๋ณํ์์ผ ํ์ฉํ  ๋ ์์ฃผ ์ฌ์ฉํ๋ค. Self join์ ์ด์ฉํ  ๋๋ ๋ณ์นญ์ ํ์๋ก ์๋ ฅํด์ฃผ์ด์ผํ๋ค. ๊ฐ์ ํ์ด๋ธ 2๊ฐ ๋๋ ๊ทธ ์ด์ ์ฌ์ฉํ๋๋ฐ ๋ณ์นญ์ ์ ํด์ฃผ์ง ์์ผ๋ฉด ํผ๋๋๊ณ  ์๋ฌ๊ฐ ๋จ๊ธฐ๋ ํ๋ค.

```SQL
select ins1.animal_id, ins2.animal_id
from animal_ins as ins1
join animal_ins as ins2 on ins2.animal_id = ins1.animal_id
```
<br><br>

## ๐ฌ SQL Injection

> ์์์ ์ธ ์ฌ์ฉ์๊ฐ ๋ณด์์์ ์ทจ์ฝ์ ์ ์ด์ฉํ์ฌ ์์์ SQL๋ฌธ์ ์ฃผ์ํ๊ณ  ์คํ๋๊ฒ ํ์ฌ ๋ฐ์ดํฐ๋ฒ ์ด์ค๊ฐ ๋น์ ์์ ์ธ ๋์์ ํ๋๋ก ์กฐ์ํ๋ ํ์์ด๋ค. ์ธ์ ์ ๊ณต๊ฒฉ์ top10 ์ค ์ฒซ ๋ฒ์งธ์ ์ํด ์์ผ๋ฉฐ, ๊ณต๊ฒฉ์ด ๋น๊ต์  ์ฌ์ดํธ์ด๊ณ  ๊ณต๊ฒฉ์ ์ฑ๊ณตํ  ๊ฒฝ์ฐ ํฐ ํผํด๋ฅผ ์ํ ์ ์๋ ๊ณต๊ฒฉ์ด๋ค.

### Injection ๊ณต๊ฒฉ ๋ฐฉ๋ฒ

1. ์ธ์ฆ ์ฐํ

๋ณดํต ๋ก๊ทธ์ธ์ ํ  ๋, ์์ด๋์ ํจ์ค์๋๋ฅผ input ์ฐฝ์ ์๋ ฅํ๋ค. ๊ทธ ๋ ์ ์ก๋๋ ์ฟผ๋ฆฌ์ ๋ชจ์์ ์๋์ ๊ฐ๋ค. id = loouserid, password = 1111์ผ ๋,

```sql
select * from user where id='loouserid' and password = '1111';
```

SQL injection์ผ๋ก ๊ณต๊ฒฉํ  ๋, input ์ฐฝ์ ๋น๋ฐ๋ฒํธ๋ฅผ ์๋ ฅํจ๊ณผ ๋์์ ๋ค๋ฅธ ์ฟผ๋ฆฌ๋ฌธ์ ํจ๊ป ์๋ ฅํ๋ค.

```sql
1111; delete * user from id = '1';
```

๋ณด์์ ์ทจ์ฝํ๋ค๋ฉด ๋น๋ฐ๋ฒํธ์ ์์ด๋๊ฐ ์ผ์นํด์ True๋ก ๋ฆฌํดํ๊ณ  ๋ค์ ์์ฑํ delete๋ฌธ๋ ๋ฐ์ดํฐ๋ฒ ์ด์ค์ ์ํฅ์ ์ค ์ ์๋ ์ํฉ์ด ์ฌ ์ ์๋ค.

> ![sqlinject](img/SQL/sqlinject.png)

์์ ์ฌ์ง ์ฒ๋ผ ๋ค์ where์ ์ or๋ฌธ์ ์ถ๊ฐํ์ฌ true๋ฅผ ๋ฐํํ๋ ์ฟผ๋ฆฌ๋ก ๋ง๋ค์ด ๋ฌด์กฐ๊ฑด ์ ์ฉ๋๋๋กํ์ฌ db๋ฅผ ์กฐ์ํ  ์๋ ์๋ค.

2. ๋ฐ์ดํฐ ๋ธ์ถ

์์คํ์์ ๋ฐ์ํ๋ ์๋ฌ ๋ฉ์์ง๋ฅผ ์ด์ฉํด ๊ณต๊ฒฉํ๋ ๋ฐฉ๋ฒ์ด๋ค. ๋ณดํต ์๋ฌ๋ ๊ฐ๋ฐ์๊ฐ ๋ฒ๊ทธ๋ฅผ ์์ ํ๋ ๋ฉด์์ ๋์์ ๋ฐ์ ์ ์๋ ์กด์ฌ์ธ๋ฐ, ํด์ปค๋ค์ ์ด๋ฅผ ์ญ์ด์ฉํด ์์์ ์ธ ๊ตฌ๋ฌธ์ ์ฝ์ํ์ฌ ์๋ฌ๋ฅผ ์ ๋ฐ์ํจ๋ค.

ex) ํด์ปค๊ฐ GET ๋ฐฉ์์ผ๋ก ๋์ํ๋ URL ์ฟผ๋ฆฌ ์คํธ๋ง์ ์ถ๊ฐํ์ฌ ์๋ฌ๋ฅผ ๋ฐ์๊ธฐํจ๋ค. ์ด์ ํด๋นํ๋ ์ค๋ฅ๊ฐ ๋ฐ์ํ๋ฉด ์ด๋ฅผ ํตํด ํด๋น ์น์ฑ์ ๋ฐ์ดํฐ๋ฒ ์ด์ค ๊ตฌ์กฐ๋ฅผ ์ ์ถํ  ์ ์๊ณ  ํดํน์ ํ์ฉํ๋ค.

### Injection ๋ฐฉ์ด๋ฐฉ๋ฒ

1. input ๊ฐ์ ๋ฐ์ ์ ํน์ ๋ฌธ์ ์ฌ๋ถ ๊ฒ์ฌ
- ๋ก๊ทธ์ธ ์ ์ ๊ฒ์ฆ ๋ก์ง์ ์ถ๊ฐํ์ฌ ํน์ ๋ฌธ์๋ค์ด ํฌํจ๋์ด ์์ ๊ฒฝ์ฐ ์์ฒญ์ ๊ฑฐ๋ถํ๋ค.

2. SQL ์๋ฒ ์ค๋ฅ ๋ฐ์ ์ ํด๋นํ๋ ์๋ฌ ๋ฉ์์ง ๊ฐ์ถค
- view๋ฅผ ํ์ฉํด์ ์๋ณธ ๋ฐ์ดํฐ๋ฒ ์ด์ค ํ์ด๋ธ์ ์ ๊ทผ ๊ถํ์ ๋์ธ๋ค. ์ผ๋ฐ ์ฌ์ฉ์๋ view๋ก๋ง ์ ๊ทผํ์ฌ ์๋ฌ๋ฅผ ํ์ธํ  ์ ์๋๋ก ๋ง๋ ๋ค.

3. Prepare statement ์ฌ์ฉ
- prepare statement๋ฅผ ์ฌ์ฉํ๋ฉด ํน์๋ฌธ์๋ฅผ ์๋์ผ๋ก escaping ํด์ค๋ค. ์ด ๊ธฐ๋ฅ์ ์ด์ฉํ๋ฉด ์๋ฒ ์ธก์์ ํํฐ๋ง ๊ณผ์ ์ ํตํด์ ๊ณต๊ฒฉ์ ๋ฐฉ์ดํ๋ค.




<br><br>

### ๐ ์ฐธ๊ณ 

[SQL์ด๋?](https://edu.goorm.io/learn/lecture/15413/%ED%95%9C-%EB%88%88%EC%97%90-%EB%81%9D%EB%82%B4%EB%8A%94-sql/lesson/767683/sql%EC%9D%B4%EB%9E%80)

[JOIN1](https://github.com/gyoogle/tech-interview-for-developer/blob/master/Computer%20Science/Database/%5BDatabase%20SQL%5D%20JOIN.md)

[JOIN2](https://doorbw.tistory.com/223)

[SQL injection1](https://github.com/gyoogle/tech-interview-for-developer/blob/master/Computer%20Science/Database/SQL%20Injection.md)

[SQL injection2](https://noirstar.tistory.com/264)


### ๋ฉด์  ์ง๋ฌธ

> 1. SQL injection์ ๊ณต๊ฒฉ ๋ฐฉ์์๋ ๋ฌด์์ด ์๋์?

> 2. injection์ ๋ฐฉ์ดํ๊ธฐ ์ํ ๋ฐฉ๋ฒ์ ์ด๋ค๊ฒ ์๋์?

> 3. join์์ left์ right join์ ์ฐจ์ด์ ์ ๋งํด์ฃผ์ธ์.

> 4. SQL ๋ฌธ๋ฒ ์ข๋ฅ์ ์์๋ฅผ ๋ค์ด์ฃผ์ธ์.