# 상품 관리 프로그램 동작 원리

수업 시간에 만든 MySQL과 파이썬을 연동한 간단한 상품관리 프로그램의 동작 방법 분석이다. 모든 것을 응용하여 혼자 만들어볼 때는 무엇부터 해야 할 지 알기 힘들었으나, 강사님과 함께 배우면서 만든 예제를 통해 관리 프로그램을 혼자 설계하는 것은 꽤 재미있었다. 지금부터 하나씩 원리를 파헤쳐보려 한다.

```python
from PrdController import *

class Serve:
    def __init__(self):
        self.pc = PrdController()

    def start_menu(self):
        
        while True:
            ctg = input("\n1. 상품 정보 조회 2. 상품 등록 3. 상품 정보 수정 4. 상품 삭제 5. 검색 6. 종료 : ")
        
            if ctg == '1':
                self.pc.prd_select()
            elif ctg == '2':
                self.pc.prd_insert()
            elif ctg == '3':
                self.pc.prd_update()
            elif ctg == '4':
                self.pc.prd_delete()
            elif ctg == '5':
                self.pc.prd_search()
            elif ctg == '6':
                print("프로그램 종료합니다")
                break
            else:
                print("입력값을 확인해주십시오")

if __name__ == "__main__":
    print("메인 메뉴입니다.")
    main = Serve()
    main.start_menu()
```

사용자가 가장 먼저 마주할 공간이다. 사용자가 각 메뉴 항목으로 가는 번호를 입력하면 그곳으로 보내주는 역할을 한다. while문으로 반복되는 메뉴는 입력값에 따라 PrdController의 함수들로 보내준다.

```if __name__=="__main__"```은 스크립트가 터미널 환경에서 실행될 때 어떤 것부터 수행할 지 지정해준다. GPT에게 역할을 물어보니 파이썬 프로그램 실행 관례라고 한다. Serve() 클래스를 main 변수에 넣어 Serve().start_menu() 함수를 실행해 메뉴를 불러오게 만드는 것이다.

```def__init__(self)```에서 PrdController를 사용할 수 있도록 self로 넣어준다. 메뉴가 호출되면 사용자의 input값을 ctg 변수에 넣고, ctg를 검사하여 PrdController의 각 항목으로 이동시킨다.

```python
from ProductDAO import *
from ProductVO import Product

class PrdController:
    def __init__(self):
        self.dao = ProductDAO()

    def prd_select(self):
        print("전체 상품 조회")
        self.dao.select()

    def prd_insert(self):
        print("신규 상품 등록")
        prdNo = input("상품 번호 : ")
        prdName = input("상품명 : ")
        prdPrice = input("상품가격 : ")
        prdMaker = input("제조사 : ")
        prdColor = input("색상 : ")
        ctgNo = input("카테고리 지정 : ")

        product = Product(prdNo, prdName, prdPrice, prdMaker, prdColor, ctgNo)
        self.dao.insert(product)

    def prd_update(self):
        print("기존 상품 수정")
        prdNo = input("상품 번호 : ")
        prdName = input("상품명 : ")
        prdPrice = input("상품가격 : ")
        prdMaker = input("제조사 : ")
        prdColor = input("색상 : ")
        ctgNo = input("카테고리 지정 : ")
        
        product = Product(prdNo, prdName, prdPrice, prdMaker, prdColor, ctgNo)
        self.dao.update(product)

    def prd_delete(self):
        print("상품 삭제")
        self.dao.delete()

    def prd_search(self):
        while True:
            num = input("\n1. 상품명 검색 2. 제조사 검색 3. 메뉴로 돌아가기 : ")

            if num == '1':
                pName = input("\n검색할 상품명 : ")
                self.dao.prd_search(pName)
            elif num == '2':
                mName = input("\n검색할 제조사 : ")
                self.dao.maker_search(mName)
            elif num == '3':
                break
            else:
                print("\n입력값을 확인해주세요")
```

Controller는 사용자가 선택한 항목별로 어떤 명령을 수행할 지 조정하는 곳이다. Controller에서도 명령을 보내고 수행할 각 class들을 import 시켜 불러오도록 만든다. 그렇기에 생성자에서 명령을 수행할 핵심 class인 ProductDAO()를 변수에 넣어둔다.

prd_insert와 prd_update는 모두 입력을 받아 매개변수로 보내주는 역할을 하고 prd_select와 prd_delete는 ProductDAO()의 각 항목으로 보내주기만 한다.

prd_search의 경우 상품명, 제조사별 검색 기능이 따로 붙어있어 하위 메뉴가 나뉘게 된다. 나뉜 하위 메뉴값으로 각기 다른 기능을 수행하도록 설계되었다.

처음에는 그냥 바로 명령을 수행하면 안될까 생각했지만, 명령을 입력받는 곳과 수행하는 곳이 함께 붙어있다면 유지보수나 기능 추가 면에서 매우 복잡해질 수 있으므로 프로그램의 수행 주체나 기능에 따라 나눠놓는 것이 더욱 편리할 수 있을 것이다.

이것은 프로그램 설계를 많이 해보면서 알아가는 수 밖에 없다.

```python
import pymysql

class ProductDAO:
    def __init__(self):
        self.conn = None
        self.cursor = None

    def connect(self):
        self.conn = pymysql.connect(host='localhost',
                               port=3306,
                               db='사용할 DB',
                               user='유저명',
                               passwd='비밀번호',
                               charset='utf8')
        self.cursor = self.conn.cursor()

    def disconnect(self):
        self.cursor.close()
        self.conn.close()

    def select(self):

        self.connect()

        sql = "SELECT * FROM product"
        self.cursor.execute(sql)
        result = self.cursor.fetchall()

        for re in result:
            for r in re:
                print(r, end=" ")
            print()

        self.disconnect()
        print("모든 상품정보 조회 완료")

    def insert(self, product):
        
        try:
            self.connect()
            sql = "INSERT INTO product VALUES(%s, %s, %s, %s, %s, %s)"
            prdName = product.get_prdName()
            values = (product.get_prdNo(), prdName, product.get_prPrice(), product.get_prdMaker(), product.get_prdColor(), product.get_ctgNo())
            self.cursor.execute(sql, values)
            self.conn.commit()

            self.disconnect()
            print(f"신상품 '{prdName}' 등록 완료")
        except:
            print("신상 등록 오류")

    def update(self, product):

        try:
            self.connect()
            sql = "UPDATE product SET prdName=%s, prdPrice=%s, prdMaker=%s, prdColor=%s, ctgNo=%s WHERE prdNo=%s"
            
            prdName = product.get_prdName()
            values = (prdName, product.get_prPrice(), product.get_prdMaker(), product.get_prdColor(), product.get_ctgNo(), product.get_prdNo())
            self.cursor.execute(sql, values)
            self.conn.commit()

            self.disconnect()
            print(f"상품명 '{prdName}' 수정 완료")
        except:
            print("상품 수정 오류")

    def delete(self):
        try:
            self.connect()
            pNo = input("삭제할 상품 번호 입력 : ")
            sql = 'DELETE FROM product WHERE prdNo=' + pNo
            self.cursor.execute(sql)
            self.conn.commit()

            self.disconnect()
            print(f'{pNo}번 상품 삭제 완료')

        except:
            print("삭제 오류")

    def prd_search(self, pName):

        try:
            self.connect()

            sql = "SELECT * FROM product WHERE prdName LIKE '%" + pName + "%'"

            self.cursor.execute(sql)
            result = self.cursor.fetchall()

            for re in result:
                for r in re:
                    print(r, end=" ")
                print()

            self.disconnect()
            print(f"상품명 '{pName}' 검색 완료")
        except:
            print("검색 오류")

    def maker_search(self, mName):

        try:
            self.connect()

            sql = "SELECT * FROM product WHERE prdMaker LIKE '%" + mName + "%'"
            self.cursor.execute(sql)
            result = self.cursor.fetchall()

            for re in result:
                for r in re:
                    print(r, end=" ")
                print()

            self.disconnect()
            print(f"제조사 '{mName}' 검색 완료")
        except:
            print("검색 오류")
```
productDAO()에서는 본격적으로 복잡한 명령을 실행한다. 각 함수별로 수행 기능을 알아보기 전에 pymysql 패키지를 알아보자.

**pymysql**이란 파이썬 환경에서 sql을 연결하여 데이터를 주고받을 수 있도록 해주는 패키지이다. 설치가 안 되어있다면 !pip install pymysql로 설치해야 한다.

class 전역에서 사용할 수 있도록 self.conn과 self.cursor 값을 None으로 지정하고 sql 서버와 연결을 수행하는 함수 connect를 만들어준다. 여기에는 sql에 저장된 DB에 접근토록 하는 기능이 수행된다. cursor에 sql 문법을 적어 보내면 입력받은 명령을 실행하여 반환해줄 것이다.

결과값을 갖고 오려면 fetchall, fetchone, fetchmany같은 내장함수를 사용하면 된다.

```def disconnect()```를 이용해 사용이 끝나면 오류가 나지 않도록 닫아주도록 한다. 모든 함수 끝부분에 삽입하여 명령을 실행하고 나면 닫아줘야 한다.

여기서 주의할 점은, cursor를 먼저 닫고 connection을 끊어야 한다는 점이다. cursor를 닫기 전에 connection을 끊어버리면 내부 프로그램이 꼬일 수도 있다.

```python
def select(self):

    self.connect()

    sql = "SELECT * FROM product"
    self.cursor.execute(sql)
    result = self.cursor.fetchall()

    for re in result:
        for r in re:
            print(r, end=" ")
        print()

    self.disconnect()
    print("모든 상품정보 조회 완료")
```
상품 전체 정보 조회 함수이다.

