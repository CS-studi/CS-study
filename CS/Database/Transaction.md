# ๐งพ Transaction

## ๐ Table of Content

> Transaction ์ด๋?

> > Transaction์ ์ฌ์ฉํ๋ ์ด์ 

> ACID

> Transaction ๊ฒฉ๋ฆฌ ์์ค & ๊ฒฉ๋ฆฌ ์์ค ์ค์  ์ ๋ฌธ์ ์ 

> Transaction ๊ด๋ฆฌ๋ฅผ ์ํ DBMS์ ์ ๋ต

<br>

## ๐งพ Transaction ์ด๋?
> ์ชผ๊ฐ์ง ์ ์๋ ์๋ฌด์ฒ๋ฆฌ์ ๋จ์

- ํธ๋์ญ์์ด๋ ๋ฐ์ดํฐ๋ฒ ์ด์ค์ ์ํ๋ฅผ ๋ณํ์ํค๋ ํ๋์ `๋ผ๋ฆฌ์ ์ธ ์์์ ๋จ์`๋ผ๊ณ  ํ  ์ ์๊ณ , ํธ๋์ญ์์๋ ์ฌ๋ฌ๊ฐ์ ์ฐ์ฐ์ด ์ํ๋  ์ ์๋ค.
- ํ๋์ ํธ๋์ญ์์ `Commit`, `Rollback`๋๋ค.

![roll](img/Transaction/roll.png)

- Commit
    - ๋ชจ๋  ๋ถ๋ถ์์์ด ์ ์์ ์ผ๋ก ์๋ฃ๋๋ฉด ์ด ๋ณ๊ฒฝ์ฌํญ์ ํ๊บผ๋ฒ์ DB์ ๋ฐ์
- Rollback
    - ๋ถ๋ถ ์์์ด ์คํจํ๋ฉด ํธ๋์ญ์ ์คํ ์ ์ผ๋ก ๋๋๋ฆฐ๋ค.
        - ํ๋์ ํธ๋์ญ์ ์ฒ๋ฆฌ๊ฐ ๋น์ ์์ ์ผ๋ก ์ข๋ฃ๋์ด ๋ฐ์ดํฐ๋ฒ ์ด์ค ์ผ๊ด์ฑ์ ๊นจ๋จ๋ ธ์ ๋ ํธ๋์ญ์์ ์ผ๋ถ๊ฐ ์ ์์ ์ผ๋ก ์ฒ๋ฆฌ๋์๋๋ผ๋ ํธ๋์ญ์์ ์์์ฑ์ ๊ตฌํํ๊ธฐ ์ํด ์ด ํธ๋์ญ์์ด ํํ ๋ชจ๋  ์ฐ์ฐ์ ์ทจ์ํ๋ค.
    - SAVEPOINT
        - ์ผ๋ฐ์ ์ผ๋ก rollback์ ๋ช์ํ๋ฉด insert, delete, update ๋ฑ์ ์์ ์ ์ฒด๊ฐ ์ทจ์๋์ง๋ง saveporint๋ฅผ ์ฌ์ฉํ๋ฉด ์ ์ฒด๊ฐ ์๋ ํน์  ๋ถ๋ถ์์ ํธ๋์ญ์์ ์ทจ์์ํฌ ์ ์๋ค.
        - ์ทจ์ํ๋ ค๋ ์ง์ ์ savepoint๋ก ๋ช์ํ ๋ค `rollback to savepoint ์ธ์ด๋ธํฌ์ธํธ๋ช;`์ ํ๋ฉด savepoint ์ง์ ๊น์ง ์ฒ๋ฆฌํ ์์์ผ๋ก rollback๋๋ค.

### Transaction์ ์ฌ์ฉํ๋ ์ด์ 

> ํธ๋์ญ์์ ํ๋์ ๋ผ๋ฆฌ์ ์ธ ์์ ๋จ์๋ก ์ฌ๋ฌ๊ฐ์ ์์์ ํ๋์ ๋ผ๋ฆฌ์ ์ธ ๋จ์๋ก ๋ฌถ์ด์ ๋ฐ์๊ณผ ๋ณต๊ตฌ๋ฅผ ์กฐ์ ํ  ์ ์๊ธฐ ์ํด ์ฌ์ฉํ๋ค. ๋ฐ๋ผ์ ๋ฐ์ดํฐ์ ๋ถ์ ํฉ์ด ์ผ์ด๋ฌ์ ๊ฒฝ์ฐ ๋กค๋ฐฑ์ ํ์ฌ ๋ฐ์ดํฐ์ ๋ถ์ ํฉ์ ๋ฐฉ์งํ  ์ ์๋ค.

ATM์ผ๋ก ๊ณ์ข ์ด์ฒด๋ฅผ ํ๋ ์ํ์ผ๋ก ์์๋ฅผ ๋ค์ด๋ณด๋ฉด
```
1. A์ํ์์ ์ถ๊ธํ์ฌ B์ํ์ผ๋ก ์ก๊ธ
2-1/case1 ) ์ก๊ธ ๋์ค์ ์์คํ ๋ฐ ์๋ฒ ์ค๋ฅ๊ฐ ๋ฐ์ํ์ฌ A์ํ ๊ณ์ข์์ 
๋์ ๋น ์ ธ ๋๊ฐ์ง๋ง B์ํ์ ๊ณ์ข์ ์๊ธ๋์ง ์์์
2-2/case2 ) ์ค๋ฅ๊ฐ ๋ฐ์ํ์ง ์๊ณ  A, B ์ํ ๋ชจ๋ ์ฑ๊ณต์ ์ผ๋ก ๋์ํจ.(Commit)
3-1/case1 ) ์ด์ ๊ฐ์ ์ํฉ์ ๋ง๊ธฐ์ํด ๊ฑฐ๋๊ฐ ์ฑ๊ณต์ ์ผ๋ก ๋ชจ๋ ๋๋์ผ ์ด๋ฅผ ์์ ํ 
๊ฑฐ๋๋ก ์น์ธํ๊ณ , ๊ฑฐ๋ ๋์ค์ ์ค๋ฅ๊ฐ ๋ฐ์ํ์ ๋์๋ ์ด ๊ฑฐ๋๋ฅผ ๋ฌดํจํ(Rollback)(๊ฑฐ๋ ์ด์  ์ํฉ์ผ๋ก)์ํด
```

๊ฑฐ๋์ ___์์ ์ฑ์ ํ๋ณดํ๋ ๋ฐฉ๋ฒ___ => ํธ๋์ญ์

๋ฐ์ดํฐ๋ฒ ์ด์ค์์๋ ํ์ด๋ธ์์ ๋ฐ์ดํฐ๋ฅผ ์ฝ์ด ์จ ํ ๋ค๋ฅธ ํ์ด๋ธ์ ๋ฐ์ดํฐ๋ฅผ ์๋ ฅํ๊ฑฐ๋ ๊ฐฑ์ , ์ญ์ ํ๋๋ฐ ์ฒ๋ฆฌ ๋์ค ์ค๋ฅ๊ฐ ๋ฐ์ํ๋ฉด ___์์ํ___ ๋ก ๋ณต๊ตฌํ๋ค.

<br>

## ๐งพ ACID
> ํธ๋์ญ์์ ํน์ง

1. Atomicity-์์์ฑ 
    - ํธ๋์ญ์์ด ๋ฐ์ดํฐ๋ฒ ์ด์ค์ ๋ชจ๋ ๋ฐ์๋๊ฑฐ๋ ์ ํ ๋ฐ์๋์ง ์์์ผ ํ๋ค.
2. Consistency-์ผ๊ด์ฑ
    - ํธ๋์ญ์์ ์์ ์ฒ๋ฆฌ ๊ฒฐ๊ณผ๋ ํญ์ ์ผ๊ด์ฑ์ด ์์ด์ผ ํ๋ค.
3. Isolation-๋๋ฆฝ์ฑ
    - ๋ ์ด์์ ํธ๋์ญ์์ด ๋์์ ๋ณํ ์คํ๋๊ณ  ์์ ๋, ์ด๋ค ํธ๋์ญ์๋ ๋ค๋ฅธ ํธ๋์ญ์ ์ฐ์ฐ์ ๋ผ์ด๋ค ์ ์๋ค.
4. Durability-์์์ฑ
    - ํธ๋์ญ์์ด ์ฑ๊ณต์ ์ผ๋ก ์๋ฃ ๋์๋ค๋ฉด ๊ฒฐ๊ณผ๋ ์๊ตฌ์ ์ผ๋ก ๋ฐ์๋์ด์ผํ๋ค.

<br>

## ๐งพ Trancsaction ๊ฒฉ๋ฆฌ ์์ค(Isolation level) & ๊ฒฉ๋ฆฌ ์์ค ์ค์  ์ ๋ฌธ์ ์ 

### Transaction ๊ฒฉ๋ฆฌ ์์ค
> ํธ๋์ญ์ ๊ฒฉ๋ฆฌ ์์ค์ ๋ฐ๋ผ ๋ฐ์ดํฐ ์กฐํ ๊ฒฐ๊ณผ๊ฐ ๋ฌ๋ผ์ง ์ ์๋ค. ๊ฒฉ๋ฆฌ ๋ ๋ฒจ์ ๋ฐ๋ผ์ ์ฝ๊ธฐ ์ผ๊ด์ฑ์ด ๋ฌ๋ผ์ง๋ค. ํธ๋์ญ์ ๊ฒฉ๋ฆฌ ์์ค์ ๋ฐ๋ผ ๋ฐ์ดํฐ ์กฐํ ๊ฒฐ๊ณผ๊ฐ ๋ฌ๋ผ์ง๊ฒ ํ๋ ๊ธฐ์ ์ MVCC(Multi Version Concurrency Consistency) ๋ผ๊ณ  ํจ.

### DB lock 

1. Shared Lock(Read Lock)

๊ณต์  ๋ฝ์ ๋ฐ์ดํฐ๋ฅผ ์ฝ์ ๋ ์ฌ์ฉํ๋ lock์ด๋ค. read lock ๋ผ๋ฆฌ๋ ๋ฐ์ดํฐ์ ์ผ๊ด์ฑ๊ณผ ๋ฌด๊ฒฐ์ฑ์ ํด์น์ง ์๊ธฐ ๋๋ฌธ์ ๋์์ ์ ๊ทผ์ด ๊ฐ๋ฅํ๋ค. ์ฆ ๋ฆฌ์์ค๋ฅผ ๋ค๋ฅธ ์ฌ์ฉ์๊ฐ ๋์์ ์ฝ์ ์ ์๊ฒ ํ์ง๋ง ๋ณ๊ฒฝ์ ๋ถ๊ฐ๋ฅํ๋ค. ๋ง์ฝ ํน์  ๋ฐ์ดํฐ์ exclusive lock์ด ๊ฑธ๋ฆฌ๋ฉด shared lock์ ๊ฑธ ์ ์์ง๋ง ์ฌ๋ฌ shared lock์ ๋์์ ์ ์ฉ๋  ์ ์๋ค.

1. Exclusive lock(write lock)

๋ฒ ํ๋ฝ์ ๋ฐ์ดํฐ๋ฅผ ๋ณ๊ฒฝํ  ๋ ์ฌ์ฉํ๋ lock์ด๋ค. ํ๋์ ํธ๋์ญ์์ด ์๋ฃ๋  ๋ ๊น์ง ์ ํจํ๋ฉฐ, ๋ฒ ํ๋ฝ์ด ๋๋  ๋ ๊น์ง ์ด๋ ํ ์ ๊ทผ๋ ํ์ฉ๋์ง ์๋๋ค. Exclusive lock์ด ๊ฑธ๋ฆฌ๋ฉด shared lock์ ๊ฑธ ์ ์๋ค. exclusive ์ํ์ ๋ฐ์ดํฐ์ ๋ํด ๋ค๋ฅธ ํธ๋์ญ์์ด exclusive lock์ ๊ฑธ ์ ์๋ค.


๊ฒฉ๋ฆฌ ์์ค ๋ ๋ฒจ
- READ UNCOMMITTED(lv.0) (์ปค๋ฐ๋์ง ์์ ์ฝ๊ธฐ)
- READ COMMITTED(lv.1) (์ปค๋ฐ๋ ์ฝ๊ธฐ)
- REAPEATABLE READ(lv.2) (๋ฐ๋ณต ๊ฐ๋ฅํ ์ฝ๊ธฐ)
- SERIALIZABLE(lv.3) (์ง๋ ฌํ ๊ฐ๋ฅ)

### Read Uncommitted
- select ๋ฌธ์ฅ์ด ์ํ๋๋ ๋์ __ํด๋น ๋ฐ์ดํฐ์ shared lock__ ์ด ๊ฑธ๋ฆฌ์ง ์๋ ๊ณ์ธต
- ํธ๋์ญ์์ ์ฒ๋ฆฌ์ค์ด๊ฑฐ๋, ์์ง commit๋์ง ์์ ๋ฐ์ดํฐ๋ฅผ ๋ค๋ฅธ ํธ๋์ญ์์ด ์ฝ๋ ๊ฒ์ ํ์ฉ
- ๋ฐ์ดํฐ๋ฒ ์ด์ค์ __์ผ๊ด์ฑ์ ์ ์งํ๋ ๊ฒ์ด ๋ถ๊ฐ๋ฅํจ__
- __dirty read ๋ฐ์__

![ru](img/Transaction/ru.png)

```
dirty read ๋ฐ์
1. ํธ๋์ญ์ 1์ ์์ํ๋ค. 
2. ํธ๋์ญ์ 2๋ฅผ ์์ํ๋ค. 
3. ํธ๋์ญ์ 1์ด ID = 1, VAL = 'MIN'์ธ ๋ฐ์ดํฐ์ VAL์ KIM์ผ๋ก ๋ณ๊ฒฝํ๋ค. 
4. ํธ๋์ญ์ 2๊ฐ ID = 1์ ์กฐํํ๋ค. VAL = 'KIM'์ด ์กฐํ๋์๋ค. 
5. ํธ๋์ญ์ 1, 2๊ฐ ์ข๋ฃ๋๋ค.
```

### Read Committed
- select ๋ฌธ์ฅ์ด ์ํ๋๋ ๋์ ํด๋น ๋ฐ์ดํฐ์ shared lock์ด ๊ฑธ๋ฆฌ๋ ๊ณ์ธต
- __ํธ๋์ญ์์ด ์ํ๋๋ ๋์ ๋ค๋ฅธ ํธ๋์ญ์์ด ์ ๊ทผํ  ์ ์์ด ๋๊ธฐํ๋ค.__
- dirty read ๋ฐฉ์ง
    - ํธ๋์ญ์์ด ์ปค๋ฐ๋์ด ํ์ ๋ ๋ฐ์ดํฐ๋ฅผ ์ฝ๋ ๊ฒ์ ํ์ฉํ๋ค.
- ๋๋ถ๋ถ์ DBMS๊ฐ ๊ธฐ๋ณธ์ผ๋ก ์ฑํํ๊ณ  ์๋ ๊ฒฉ๋ฆฌ์์ค์ด๋ค.
- __Commit๋ ์ ๋ณด๋ง ์ฝ๋๋ค.__
- __Non-Repeatable Read ๋ฐ์__

![rc](img/Transaction/rc.png)

```
Non-repeatable read ex / ๋์ด๊ฐ ๋ฐ๋์ด์ ์ถ๋ ฅ์ด๋๋ฉด ์๋๋ ์ํฉ์.
1. ํธ๋์ญ์ 1 ์์
2. ํธ๋์ญ์ 1์ด id=1์ธ ๋ฐ์ดํฐ์ value๋ฅผ kim์ผ๋ก ๋ณ๊ฒฝ
3. ํธ๋์ญ์ 2๊ฐ ์์
4. ํธ๋์ญ์ 2๊ฐ id=1์ธ ๋ฐ์ดํฐ๋ฅผ ์กฐํ. min์ด ๊ฒ์๋๋ค.
5. ํธ๋์ญ์ 1์ด ์ปค๋ฐํ๊ณ  ์ข๋ฃํ๋ค.
6. ํธ๋์ญ์ 2๊ฐ id=1์ธ ๋ฐ์ดํฐ๋ฅผ ์กฐํํ๋ค. kim์ด ๊ฒ์๋๋ค.
7. ํธ๋์ญ์ 2๊ฐ ์ปค๋ฐ์ ํ๊ณ  ์ข๋ฃํ๋ค.
```

### Repeatable Read
- ํธ๋์ญ์์ด ์๋ฃ๋  ๋๊น์ง __select ๋ฌธ์ฅ์ด ์ฌ์ฉ๋๋ ๋ชจ๋  ๋ฐ์ดํฐ์ shared lock์ด ๊ฑธ๋ฆฌ๋ ๊ณ์ธต__ 
- ํธ๋์ญ์์ด ๋ฒ์ ๋ด์์ __์กฐํํ ๋ฐ์ดํฐ ๋ด์ฉ์ด ํญ์ ๋์ผํจ์ ๋ณด์ฅ__
- `๋ค๋ฅธ ์ฌ์ฉ์๋ ํธ๋์ญ์ ์์ญ์ ํด๋น๋๋ ๋ฐ์ดํฐ์ ๋ํ ์์  ๋ถ๊ฐ๋ฅ` -> ์๋ ฅ์ ๊ฐ๋ฅ
- MySQL DBMS์์ ๊ธฐ๋ณธ์ผ๋ก ์ฌ์ฉํ๋ค.
- __Non-Repeatable read ๋ถ์ ํฉ ๋ฐ์ํ์ง ์๋๋ค.__

![rr](img/Transaction/rr.png)

```
Non-Repeatable Read 
1. ํธ๋์ญ์ 1์ ์์
2. ํธ๋์ญ์ 1์ด id=1์ธ ๋ฐ์ดํฐ๋ฅผ ์กฐํํ๋ค.
3. ํธ๋์ญ์ 2๊ฐ ์์๋์๋ค.
4. ํธ๋์ญ์ 2๊ฐ id=1์ธ ๋ฐ์ดํฐ๋ฅผ kim์ผ๋ก ๋ณ๊ฒฝํ๋ค.
5. ํธ๋์ญ์ 1์ด id=1์ธ ๋ฐ์ดํฐ๋ฅผ ์กฐํํ๋ค. ํธ๋์ญ์ 2์ ๋ณ๊ฒฝ ๋ด์ญ์ด ๋ณด์ด์ง ์๋๋ค.
6. ํธ๋์ญ์ 2๊ฐ id=2์ธ ๋ฐ์ดํฐ๋ฅผ ์ฝ์ ํ commitํ์ฌ ํธ๋์ญ์์ ์ข๋ฃํ๋ค.
7. ํธ๋์ญ์ 1์ด id=2์ธ ๋ฐ์ดํฐ๋ฅผ ์กฐํํ๋ค. ๋ฐ์ดํฐ๊ฐ ์ ์์ ์ผ๋ก ํ์ธ๋๋ค.
```

### Serializable 
- __ํธ๋์ญ์์ด ์๋ฃ๋  ๋๊น์ง select ๋ฌธ์ฅ์ด ์ฌ์ฉ๋๋ ๋ชจ๋  ๋ฐ์ดํฐ์ shared lock์ด ๊ฑธ๋ฆฌ๋ ๊ณ์ธต__
- ๊ฐ์ฅ ์๊ฒฉํ ๊ฒฉ๋ฆฌ ์์ค์ผ๋ก __์๋ฒฝํ ์ฝ๊ธฐ ์ผ๊ด์ฑ ๋ชจ๋๋ฅผ ์ ๊ณตํจ__
- `๋ค๋ฅธ ์ฌ์ฉ์๋ ํธ๋์ญ์ ์์ญ์ ํด๋น๋๋ ๋ฐ์ดํฐ์ ๋ํ ์์  ๋ฐ ์๋ ฅ ๋ถ๊ฐ๋ฅ`

MySQL : Repeatable Read
Oracle : Read Committed

๊ฒฐ๋ก ์ ๋ ๋ฒจ์ด ๋์์ง์๋ก ํธ๋์ญ์๊ฐ ๊ณ ๋ฆฝ์ ๋๊ฐ ๋์์ง๋ฉฐ, ์ฑ๋ฅ์ด ๋จ์ด์ง๋ ๊ฒ์ด ์ผ๋ฐ์ ์ด๋ฉฐ, ์ผ๋ฐ์ ์ธ ์จ๋ผ์ธ ์๋น์ค์์๋ READ COMIITTED๋ REPEATABLE READ ์ค์ ํ๋๋ฅผ ์ฌ์ฉํ๋ค๊ณ  ํ๋ค. 


### Transaction ๊ฒฉ๋ฆฌ ์์ค ์ค์  ์ ๋ฌธ์ ์ 
> Isolation level(๊ฒฉ๋ฆฌ ์์ค ๋ ๋ฒจ)์ ๋ํ ์กฐ์ ์ ๋์์ฑ๊ณผ ๋ฐ์ดํฐ ๋ฌด๊ฒฐ์ฑ๊ณผ ์ฐ๊ด๋์ด์๋ค.

___๋์์ฑ์ ์ฆ๊ฐ์ํค๋ฉด ๋ฐ์ดํฐ์ ๋ฌด๊ฒฐ์ฑ์ ๋ฌธ์ ๊ฐ ๋ฐ์ํ๊ณ , ๋ฐ์ดํฐ์ ๋ฌด๊ฒฐ์ฑ์ ์ ์งํ๋ฉด ๋์์ฑ์ด ๋จ์ด์ง๊ฒ ๋๋ค.___

![iso](img/Transaction/isolationlevel.png)

>๋ ๋ฒจ์ด ๋ฎ์์ง์๋ก ๋์์ฑ์ ๋์์ง๊ณ  ๋ฐ์ดํฐ ์ผ๊ด์ฑ์ ๋ฎ์์ง๋ค.
>๋ ๋ฒจ์ด ๋์์ง์๋ก ๋์์ฑ์ ๋ฎ์์ง๊ณ  ๋ฐ์ดํฐ ์ผ๊ด์ฑ์ ๋์์ง๋ค.


#### ๋ฎ์ ๋จ๊ณ Isolation Level์ ํ์ฉํ  ๋ ๋ฐ์ํ๋ ํ์๋ค

- Dirty Read
    - ์ด๋ค ํธ๋์ญ์์์ ์์ง ์คํ์ด ๋๋์ง ์์ ๋ค๋ฅธ ํธ๋์ญ์์ ์ํ ๋ณ๊ฒฝ์ฌํญ์ ๋ณด๊ฒ๋๋ ๊ฒฝ์ฐ
    - ์ปค๋ฐ๋์ง ์์ ์์ ์ค์ธ ๋ฐ์ดํฐ๋ฅผ ๋ค๋ฅธ ํธ๋์ญ์์์ ์ฝ์ ์ ์๋๋ก ํ์ฉํ  ๋ ๋ฐ์ํ๋ ํ์
- Non_repeatable Read
    - ํ ํธ๋์ญ์์์ ๊ฐ์ ์ฟผ๋ฆฌ๋ฅผ ๋ ๋ฒ ์ํํ  ๋ ๊ทธ ์ฌ์ด์ ๋ค๋ฅธ ํธ๋์ญ์ ๊ฐ์ ์์  ๋๋ ์ญ์ ํจ๋์ ๋ ์ฟผ๋ฆฌ์ ๊ฒฐ๊ณผ๊ฐ ์์ดํ๊ฒ ๋ํ๋๋ ์ผ๊ด์ฑ์ด ๊นจ์ง ํ์
    - ํ ํธ๋์ญ์์์ ๋๊ฐ์ select๋ฅผ ์ํํ์ ๋ ํญ์ ๊ฐ์ ๊ฒฐ๊ณผ๋ฅผ ๋ฐํํด์ผ ํ๋ค๋ repeatable read ์ ํฉ์ฑ์ ์ด๊ธ๋จ
- Phantom Read
    - ํ ํธ๋์ญ์ ์์์ ์ผ์  ๋ฒ์์ ๋ ์ฝ๋๋ฅผ ๋ ๋ฒ ์ด์ ์ฝ์์ ๋, ์ฒซ ๋ฒ์งธ ์ฟผ๋ฆฌ์์ ์๋ ๋ ์ฝ๋๊ฐ ๋ ๋ฒ์งธ ์ฟผ๋ฆฌ์์ ๋ํ๋๋ ํ์
    - ํธ๋์ญ์ ๋์ค ์๋ก์ด ๋ ์ฝ๋๋ฅผ ์ฝ์์ ํ์ฉํ๊ธฐ ๋๋ฌธ์ ๋ํ๋จ

![chrt](img/Transaction/chart.png)

<br>

## Transaction ๊ด๋ฆฌ๋ฅผ ์ํ DBMS์ ์ ๋ต

์ดํด๊ฐ ํ์ํ ๊ฐ๋ 

> DBMS์ ๊ตฌ์กฐ

> Buffer ๊ด๋ฆฌ ์ ์ฑ


### DBMS์ ๊ตฌ์กฐ

DBMS ๊ตฌ์ฑ์์ :Query Processor (์ง์ ์ฒ๋ฆฌ๊ธฐ), Storage System(์ ์ฅ ์์คํ)

์์ถ๋ ฅ ๋จ์ :๊ณ ์ ๊ธธ์ด์ page ๋จ์๋ก disk์ ์ฝ๊ฑฐ๋ ์

์ ์ฅ ๊ณต๊ฐ : ๋นํ๋ฐ์ฑ ์ ์ฅ ์ฅ์น์ธ disk์ ์ ์ฅ, ์ผ๋ถ๋ถ์ main memory์ ์ ์ฅ

![dbms](img/Transaction/buffer.png)

### Page Buffer Manager or Buffer manager

> DBMS์ storage system์ ์ํ๋ ๋ชจ๋ ์ค ํ๋๋ก, main memory์ ์ ์งํ๋ ํ์ด์ง๋ฅผ ๊ด๋ฆฌํ๋ ๋ชจ๋
> 

Buffer ๊ด๋ฆฌ ์ ์ฑ์ ๋ฐ๋ผ undo ๋ณต๊ตฌ์ redo ๋ณต๊ตฌ๊ฐ ์๊ตฌ๋๊ฑฐ๋ ๊ทธ๋ ์ง ์๊ฒ ๋๋ฏ๋ก transaction ๊ด๋ฆฌ์ ๋งค์ฐ ์ค์ํ ๊ฒฐ์ ์ ๊ฐ์ ธ์จ๋ค.

#### Undo

ํ์ํ ์ด์  : ์์ ๋ page๋ค์ด **buffer ๊ต์ฒด ์๊ณ ๋ฆฌ์ฆ์ ๋ฐ๋ผ์ ๋์คํฌ์ ์ถ๋ ฅ**๋  ์ ์์. Buffer ๊ต์ฒด๋ transaction๊ณผ๋ ๋ฌด๊ดํ๊ฒ buffer์ ์ํ์ ๋ฐ๋ผ์ ๊ฒฐ์ ๋๋ค. ์ด๋ก ์ธํด์ ์ ์์ ์ผ๋ก ์ข๋ฃ๋์ง ์์ transaction์ด ๋ณ๊ฒฝํ page๋ค์ ์์ ๋ณต๊ตฌ ๋์ด์ผ ํ๋๋ฐ, ์ด๋ฅผ undo๋ผ๊ณ  ํ๋ค.

- steal : ์์ ๋ ํ์ด์ง๋ฅผ ์ธ์ ๋ ์ง ๋์คํฌ์ ์ธ ์ ์๋ ์ ์ฑ
    - ๋๋ถ๋ถ์ dbms๊ฐ ์ฑํํ๋ buffer ๊ด๋ฆฌ ์ ์ฑ
    - undo logging๊ณผ ๋ณต๊ตฌ๋ฅผ ํ์๋ก ํจ.
- not steal : ์์ ๋ ํ์ด์ง๋ค์ EOT (End Of Transaction)๊น์ง๋ ๋ฒํผ์ ์ ์งํ๋ ์ ์ฑ
    - undo ์์์ด ํ์ํ์ง ์์ง๋ง, ๋งค์ฐ ํฐ ๋ฉ๋ชจ๋ฆฌ ๋ฒํผ๊ฐ ํ์ํจ

#### Redo

์ด๋ฏธ commitํ transaction์ ์์ ์ ์ฌ๋ฐ์ํ๋ ๋ณต๊ตฌ ์์

buffer๊ด๋ฆฌ ์ ์ฑ์ ์ํฅ์ ๋ฐ๋๋ค. โ transaction์ด ์ข๋ฃ๋๋ ์์ ์ ํด๋น transaction์ด ์์ ํ page๋ฅผ ๋์คํฌ์ ์ธ ๊ฒ์ธ๊ฐ ์๋๊ฐ๋ก ๊ธฐ์ค

- FORCE : ์์ ํ๋ ๋ชจ๋  ํ์ด์ง๋ฅผ Transaction commit ์์ ์ disk์ ๋ฐ์
    - transaction์ด commit ๋์์ ๋ ์์ ๋ ํ์ด์ง๋ค์ด disk ์์ ๋ฐ์๋๋ฏ๋ก redo ํ์ ์์
- not FORCE :commit ์์ ์ ๋ฐ์ํ์ง ์๋ ์ ์ฑ
    - transaction์ด disk ์์ db์ ๋ฐ์๋์ง ์์ ์ ์๊ธฐ์ redo ๋ณต๊ตฌ๊ฐ ํ์(๋๋ถ๋ถ์ dbms์ ์ฑ)

<br><br>

### ๐ ์ฐธ๊ณ 

[ํธ๋์ญ์1](https://devuna.tistory.com/30)

[ํธ๋์ญ์2](https://github.com/NKLCWDT/cs/blob/main/Database/Transaction.md)

[ํธ๋์ญ์ ๊ฒฉ๋ฆฌ์์ค1](https://dar0m.tistory.com/225)

[ํธ๋์ญ์ ๊ฒฉ๋ฆฌ์์ค2](https://zzang9ha.tistory.com/381)

<br><br>

### โ๏ธ ๋ฉด์  ์์ ์ง๋ฌธ

> ํธ๋์ญ์ ๊ฒฉ๋ฆฌ ์์ค์ ๋ฌด์์ธ๊ฐ์? ๊ฒฉ๋ฆฌ ์์ค์ ์ข๋ฅ๋ฅผ ์ค๋ชํด์ฃผ์ธ์

>> ๊ฒฉ๋ฆฌ ์์ค์ ์ข๋ฅ์ ๋ฐ๋ฅธ ์ค๋ฅ๋ฅผ ๋งํ๊ณ  ์ค๋ฅ๋ฅผ ์ค๋ชํ์์ค

> Transaction์ ๋ฌด์์ธ๊ฐ์?

> Transaction์ ํน์ฑ์ด ์๋๋ฐ ๊ฐ๊ฐ์ ์ค๋ชํด์ฃผ์ธ์.
