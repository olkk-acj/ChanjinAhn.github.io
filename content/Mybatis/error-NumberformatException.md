---
emoji: ๐ป
title: '[Mybatis] String NumberformatException Error๊ฐ ๋๋ ๊ฒฝ์ฐ'
date: '2022-01-08 20:42:00'
author: olkk
tags: 
categories: Spring Mybatis
---

Mybatis๋ฅผ ์ฌ์ฉํ์ฌ ๊ตฌํํ๋ค๋ณด๋ฉด ์๋์ ๊ฐ์ ์๋ฌ ๋ฌธ๊ตฌ๊ฐ ๋ฐ์ํ๋ ๊ฒฝ์ฐ๊ฐ ์กด์ฌํฉ๋๋ค.

```console
org.mybatis.spring.MyBatisSystemException: nested exception is org.apache.ibatis.exceptions.PersistenceException: 
### Error querying database.  Cause: java.lang.NumberFormatException: For input string: "?"
### Cause: java.lang.NumberFormatException: For input string: "?"
```

์๋ฌ๋ฅผ ๋ณด๊ณ  ์ ์ถํ๋ฉด ํ ๋ณํ ๋ฌธ์ ๋ก ๋ณด์ด๋๋ฐ, ์๋ฌด๋ฆฌ ์ฐพ์๋ด๋ ๋ฐ์ดํฐ ํ์์๋ ๋ฌธ์ ๊ฐ ์๋ค๋ฉด SQL ๋์  ์ฟผ๋ฆฌ๋ฅผ ์ดํด๋ณผ ํ์๊ฐ ์์ต๋๋ค.

---

## ์์ 

```SQL
...
    <if test="value != null and value != ''">
        ...
    </if>
...
````

๋ค์๊ณผ ๊ฐ์ ๋์  ์ฟผ๋ฆฌ๊ฐ ์กด์ฌํ  ๊ฒฝ์ฐ ์๋์ ๊ฐ์ด ๋ณ๊ฒฝํด์ผ ํฉ๋๋ค.

```SQL
...
    <if test='value != null value != ""'>
        ...
    </if>
...    
```

## ์ค๋ช

ํด๊ฒฐ ๋ฐฉ๋ฒ์ ๋ช ๊ฐ์ง ์กด์ฌํ์ง๋ง ๊ฐ์ฅ ๊น๋ํ ๋ฐฉ๋ฒ์ ์์ ๊ฐ์ด ์์๋ฐ์ดํ('')๋ฅผ ๋จผ์  ์ฌ์ฉํ ๋ค ํฐ๋ฐ์ดํ("")๋ฅผ ์ฌ์ฉํด์ผ ํฉ๋๋ค.

ํด๋น ์ค๋ฅ๊ฐ ๋ฐ์ํ๋ ์ด์ ๋ฅผ ๊ฐ๋ตํ ์ค๋ชํ์๋ฉด OGNL(Object Graph Navigation Language)์ ๋ฌธ์  ๋๋ฌธ์๋๋ค.
OGNL์ธํฐํ๋ฆฌํฐ์์๋ ''๋ '?'๋ฅผ charํ์ผ๋ก ์ธ์ํ๊ณ  '??'๋ "?"๋ String์ผ๋ก ์ธ์ํฉ๋๋ค. ๋๋ฌธ์ ๋์  ์ฟผ๋ฆฌ์ ๋น๊ต๋ฌธ์ ์์ฑํ ๋ NumberFormat์ผ๋ก ๋น๊ต๋ฅผ ์๋ํ๊ณ  ํด๋น ์๋ฌ๊ฐ ๋ฐ์ํ๋ ๊ฒ์๋๋ค.

## ๊ฒฐ๋ก 
""๋ฅผ ์ฐ๋ ์ต๊ด์ ๋ค์ด์.