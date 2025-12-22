>데이터를 조작하는 명령어
---
## DML 유형
| 구분       | DML 명령어 | 내용           |
| -------- | ------- | ------------ |
| 데이터 `조회` | SELECT  | 테이블 내용을 `조회` |
| 데이터 `삭제` | DELETE  | 테이블 내용을 `삭제` |
| 데이터 `변경` | UPDATE  | 테이블 내용을 `변경` |
| 데이터 `추가` | INSERT  | 테이블 내용을 `추가` |

---
## DML 활용
### 데이터 조회
> 기본 형식:
```sql
SELECT [OPTION] columns FROM table [WHERE 절];
```

**SELECT** 문에서 사용되는 요소

| 요소      | 요소값             | 내용                                              |
| ------- | --------------- | ----------------------------------------------- |
| OPTION  | ALL<br>DISTINCT | 중복을 `포함한` 조회 결과 출력<br>중복을 `제외한` 조회 결과 출력        |
| columns | 컬럼명 목록<br>와일드카드 | SELECT를 통해 조회할 `컬럼명` 지정<br>모두 또는 전체를 의미하는 `'*'` |

### 데이터 삭제
> 레코드를 삭제할 때 쓰는 명령어. 조건절 없이 사용하는 경우, 테이블 `전체`가 삭제됨.
```sql
	DELETE FROM table [WHERE 절];
```

### 데이터 변경
> 데이터를 변경할 때 쓰는 명령어. 보통 WHERE 절을 통해 어떤 조건이 만족하는 경우에만 특정 컬럼의 값을 변경하는 용도로 많이 사용된다.
```sql
UPDATE table SET columns1 = value1, column2 = value2, ... [WHERE 절];
```

### 데이터 추가
> 데이터를 추가할 때 쓰는 명령어. 삽입에 사용되는 정보는 하나의 레코드를 충분히 묘사해야 한다.

특정 컬럼만 값을 추가하는 경우:
```sql
INSERT INTO table_name (columns1, columns2, ...) VALUES (value1, value2, ...);
```

모든 컬럼에 대해 값을 추가하는 경우:
```sql
INSERT INTO table_name VALUES (value1, value2, ...);
```