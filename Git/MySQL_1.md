# My SQL

## 1. 데이터베이스 생성
데이터베이스를 My SQL에서는 Schema(스키마)라고 한다. 스키마 생성은 다음과 같이 할 수 있다.

```bash
CREATE SCHEMA 스키마이름;
```

소문자로 적어도 상관없긴 한데, 강사님께서는 대소문자를 구분하셨다. 개개인마다 다르겠지만, 나는 명령을 내리는 명령줄은 대문자로, 그 외 변수들은 소문자로 구분하려 한다.

CREATE SCHEMA만 적어서 데이터베이스를 생성해도 괜찮지만, 나중에 UTF-8 문자열을 사용하는 환경에서 한글이 깨질 수도 있으니 CHARACTER SET을 설정하는 게 좋다.

```bash
CREATE SCHEMA 스키마이름 DEFAULT CHARACTER SET utf8mb4;
```

생성한 스키마를 삭제하려면 DROP 명령어를 쓰면 된다.

```bash
DROP SCHEMA 스키마이름;
```

생성한 스키마서 데이터를 불러오려면 use를 쓴다.

```bash
use 스키마이름;
```
왼쪽 SQL 환경에서 내가 사용할 **스키마이름**이 굵게 변할 것이다.

## 2. 테이블 생성
테이블이란, 데이터베이스에서 사용할 데이터들의 집합이다. HTML에서 표를 만들 때 table 태그를 통해 그 안에 열과 행을 채워넣었던 것과 같이 스키마에서도 데이터들이 들어있는 표를 table이라 부른다.

그럼 table을 생성해보자.

```bash
CREATE TABLE 테이블명 (
    데이터명
);
```
쇼핑몰에 쓰일 데이터 테이블을 만든다고 가정해보자. product 안에 상품이름과 가격만 넣어놓으면 충분할까? 그렇다면 나중에 상품 데이터를 불러오고 싶을 때 어떻게 구별할 것인가?

테이블에서는 각 요소들을 구분할 수 있는 중복되지 않는 값이 필요하다. 그것을 **기본키**라고 한다. 영어로 primary key이다.

그럼 데이터를 한번 입력해보자.

```bash
CREATE TABLE product (
    productNo VARCHAR(10) NOT NULL PRIMARY KEY,
    productName VARCHAR(60) NOT NULL,
    productPrice INT,
    productDate DATE
);
```
각 항목에는 서로 다른 데이터값을 입력할 수 있다.

 1. VARCHAR(숫자) : 문자열을 입력할 수 있게 한다. 숫자는 최대로 입력 가능한 문자열이다.
 2. INT : 정수형 숫자이다. 나중에 계산식을 세울 때 사용할 수 있다.
 3. DATE : 날짜 데이터이다. 2020-01-01과 같은 형식으로 저장한다.

위 표에서는 productNo가 기본키가 되어 각 행을 구분 짓는 기준이 된다. 나중에 데이터를 불러올 때나 외래키를 추가할 때 중복값이 없는 기본키를 기준으로 추가하게 된다.

## 3. 외래키 지정

```bash
CREATE TABLE student(
    stdNo INT NOT NULL PRIMARY KEY,
    stdName VARCHAR(30) NOT NULL,
    stdGrade INT NOT NULL DEFAULT 1 CHECK(stdGrad >= AND stdGrade <= 4),
    depNo INT NOT NULL
);

CREATE TABLE department (
    depNo INT NOT NULL PRIMARY KEY,
    depName VARCHAR(30) NOT NULL
);
```
위와 같이 2개의 테이블을 만들었다고 가정하다. 그런데, 잘 보면 student 테이블과 department 테이블에 depNo라는 동일행이 들어가 있는 것이 보일 것이다. depNo는 department 테이블에서는 기본키이지만 student 테이블에서는 외래키가 되는 것이다.

department 테이블은 학과명과 학과번호만 따로 저장해놓고 학생이나 교수, 기타 직원들의 소속과를 지정할 때 이곳저곳 쓰일 수 있으므로 따로 지정하면 나중에 활용하기 편리하다.

외래키 지정은 FOREIGN KEY로 하고 REFERENCES 테이블명 (외래키)를 통해 할 수 있다.

```bash
CREATE TABLE student(
    stdNo INT NOT NULL PRIMARY KEY,
    stdName VARCHAR(30) NOT NULL,
    stdGrade INT NOT NULL DEFAULT 1 CHECK(stdGrad >= AND stdGrade <= 4),
    depNo INT NOT NULL
    FOREIGN KEY (depNo) REFERENCES department (depNo)
);

CREATE TABLE department (
    depNo INT NOT NULL PRIMARY KEY,
    depName VARCHAR(30) NOT NULL
);
```