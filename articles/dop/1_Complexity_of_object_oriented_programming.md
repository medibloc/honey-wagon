# 1 복잡한 OOP
이번 단원에서는 이런걸 알아본다.
- 시스템을 복잡하게 만드는 OOP
- OOP 시스템은 왜 이해하기 어려운지
- 오브젝트에 코드와 데이터를 섞는 비용

이번 장에서는 OOP가 복잡해지는 이유를 살펴봅니다.
OOP의 구문이나 의미가 프로그램을 복잡하게 만드는게 아닙니다. State와 state를 수정/접근하는 method로 이뤄진 오브젝트들로 프로그램을 짜나가는 근본 방식에서 비롯합니다.

수년간 OOP생태계는 새로운 기능(e.g., anonymous classes, anonymous funcion)을 추가하고 간단한 인터페이스를 제공하는 프레임워크를 개발해 복잡성을 완화시켰습니다. (e.g., Spring ans Jackson in Java)
프레임워크는 내부적으로 reflection, custom annotation과 같은 고급기능에 의존합니다.

이 장에서 OOP 심층 분석을 하자는건 아니다. 다만 OOP 가 프로그램을 복잡하게 만드는 패러다임이라는 걸 알리고자한다. 이로 인해 복잡도를 줄이는 다른 패러다임에 관심이 간다면 다행이겠다. 이 패러다임을 우리는 데이터지향프로그래밍(DOP)라 한다.

## 1.1 OOP 디자인: Classic or classical?
- NOTE: 테오, 낸시, 걔네들이 하는 신규 프로젝트를 part 1 도입부에서 소개했다. 안 읽고 왔으면 보고 오시길

테오는 빡빡한 데드라인을 가진 걸 알았습니다. 불안한 마음을 가진채 낸시의 냅킨을 주머니에 넣고 사무실로 돌아옵니다.
지난 주 그의 상사 모니카는 무슨 일이 있어도 낸시와의 거래를 성사시켜야 한다고 분명히 말했습니다.

Theo가 근무하는 Albatross는 전 세계에 고객을 보유한 소프트웨어 컨설팅 회사입니다.
원래는 스타트업 사이에 고객이 많았습니다. 그러나 지난 1년 동안 많은 프로젝트가 부실하게 관리되었고 스타트업 부서는 고객의 신뢰를 잃었습니다.
그래서 경영진은 테오를 엔터프라이즈 부서에서 스타트업 부서 테크 리더로 이동했습니다.
그의 업무는 거래를 성사시키고 제시간에 납품하는 것입니다.

### 1.1.1 디자인 단계

테오는 코드짜러 달려들기 전에 종이 몇장에 클라핌 프로토타입의 UML 클래스 다이어그램을 그리기 시작했다. 테오는 OOP 프로그래머이기에 모든 비지니스 엔티티는 오브젝트여야 하고 클래스로 이들을 생성해야 한다는데 의심의 여지가 없었다.

클라핌 프로로타입의 요구사항이다.
- 유저는 도서관 멤버과 사서 두 부류다.
- 유저는 이메일과 패스워드로 로그인한다.
- 멤버는 책을 빌릴 수 있다.
- 멤버와 사서는 책제목, 저자로 책을 검색할 수 있다.
- 사서는 멤버가 책을 반납하지 않을 때 차단하거나 해제할 수 있다. 
- 사서는 멤버가 빌려간 책을 나열할 수 있다.
- 단일 도서에 여러 권 있을 수 있다.
- 한 책은 한 개의 실물 도서관에 속한다.

테오는 시스템 구조를 생각하는데 시간을 들여서 전 지구적 클라핌 도서 관리 시스템의 아래의 메인 클래스들을 도출해냈다.

- Library: 시스템 디자인의 핵심 
- Book: 도서
- BookItem: 한 도서가 여러 카피가 있을 수 있고, 한 카피가 한 아이템에 대응 
- BookLending: 대여해가면 lent, 도서 오브젝트가 만들어지면 lending
- Member: 멤버
- Librarian: 사서
- User: 사서와 멤버의 base class
- Catalog: 책 리스트를 들고 있음
- Author: 저자

여기까진 여러울게 없었고 이제 어려운 부분이다. 클래스간 관계를 알아내는 일이다. 두 시간이 지나고 아래같은 범지구적 도서 관리 시스템에 대한 디자인 초안을 들고 나타났다.

![Figure 1.1 A class diagram for Klafim’s Global Library Management System](resources/Figure_01-01.png)

- NOTE: 제시된 디자인은 가장 스마트한 OOP 디자인을 나타내지 않습니다. 숙련된 OOP 개발자는 아마도 몇가지 디자인 패턴을 사용해 더 나은 디자인을 제안할 수 있을 것입니다.
	이 디자인은 나이브하고 시스템의 모든 기능을 커버하지 않습니다. 이것은 두가지 의도를 가지고 있습니다.
	- For Theo: 개발자인 Theo에겐 코딩을 시작할 수 있을만큼 충분합니다.
	- For Me: 이 책의 저자인 저에게는 전형적인 OOP시스템의 복잡성을 나타내기에 충분합니다.

Theo는 자신이 디자인한 다이어그램에 대한 자부심을 가지고 있습니다. 그는 커피한잔을 받을 자격이 충분합니다!

커피 머신 근처에서 Theo는 몇 주 전에 Albatross에 합류한 주니어 소프트웨어 개발자 Dave를 만납니다.
Dave의 호기심은 Theo에게 도전적인 질문을 하도록 이끌어주기 때문에 서로 감사하고 있습니다.
커피 머신 근처의 회의는 종종 프로그래밍에 대한 흥미로운 토론으로 바뀝니다.

> 테오: 와썹
> 
> 데이브: 버그눈물줄줄! 오브젝트 스테이트가 계속 바뀌는 이유를 모르겠네. 알아내겠지뭐. 너는?
> 
> 테오: 새고객 시스템 디자인 마침
> 
> 데이브: 쩔 함 봐도됨? 디자인 노하우좀 알게
> 
> 테오: 자리로가서 다이어그램리뷰 고고

### 1.1.2 UML 101

... 잡소리...

> 데이브: 자세히도 그렸네
> 
> 테오: 가슴웅장
> 
> 데이브: 화살표 뭔소리더라
> 
> 테오: Composition, association, inheritance, usage 4개 화살표 씀
> 
> 데이브: composition, association 차이가 뭐임
> 
> 테오: 다른 녀석이랑 같이 살아야만 하냐 마냐지. composition은 운명공동체임. association은 각자도생.


다이아몬드 화살표 / 다이아몬드 화살표 반대편 스타
- Library가 Catalog를 소유 - 1-1composition, library 죽으면 catalog도 죽는다
- Library가 Member들을 소유 - 1-many composition, library 죽으면 관련 member들 죽는다

![Figure 1.2 The two kinds of composition: one-to-one and one-to-many. In both cases, when an object dies, the composed object dies with it.](resources/Figure_01-02.png)

> 데이브: association 관계도 있음?
> 
> 테오: 책이랑 저자. 빈다이아몬드 양쪽으로

![Figure 1.3 Many-to-many association relation: each object lives independently.](resources/Figure_01-03.png)

> 데이브: 점선은 뭐임
> 
> 테오: Method caller/callee 관계

![Figure 1.4 Usage relation: a class uses a method of another class.](resources/Figure_01-04.png)

> 데이브: 실선에 빈 삼각형은 상속인가
> 
> 테오: ㅇㅇ

![Figure 1.5 Inheritance relation: a class derives from another class.](resources/Figure_01-05.png)

### 1.1.3 Explaining each piece of the class diagram

> 데이브: UML 알려주셔서 감사합니다. 화살표가 무슨 의미인지 이해했어요.
> 
> 테오: 별말씀을ㅎ 전체 구조 보고싶어?
>
> 데이브: 어떤 클래스 먼저 봐야할까요?
>
> 테오:라이브러리부터 시작해야할 것 같아

#### 라이브러리 클래스

라이브러리 클래스는 도서 시스템의 최상위 클래스다.

![Figure 1.6 The Library class.](resources/Figure_01-06.png) 

코드(동작) 측면에서 Library는 스스로 어떠한 일을 하지 않습니다. 모든 동작을 자신이 소유한 객체에 위임합니다.
데이터 측면에서 Library는 다음을 소유합니다.
- Multiple Member objects
- Multiple Librarian objects
- A single Catalog object

- 노트: 본서에서 코드와 동작은 유사하게 번갈아 사용한다

#### 사서, 멤버, 사용자 클래스
사서와 멤버는 사용자에서 비롯한다.
![Figure 1.7 Librarian and Member derive from User.](resources/Figure_01-07.png)
사용자 클래스는 도서관 사용자를 의미
- 데이터 멤버는 가능한 적게 들고 있도록 한다: id, email, password
- 코드는 로그인하기위한 login있다.
멤버 클래스는 도서관 멤버를 의미
- 사용자 클래스 상속
- 데이터 멤버는 사용자 클래스와 동일
- 코드는
  - checkout으로 책대여
  - returnBook으로 반납
  - block으로 스스로를 차단
  - unblock으로 스스로를 차단해제
  - isBlocked로 차단유무를 대답
- 여러 BookLending 오브젝트를 소유
- checkout 구현에 BookItem 사용
사서 클래스는 사서를 의미
- 사용자 클래스 상속
- 데이터 멤버는 사용자 클래스와 동일
- 코드는
  - 멤버를 block, unblock 할 수 있고
  - getBookLendings로 멤버의 대여 내역 조회
  - addBookItem으로 도서관에 북아이템 추가
- blockMember, unblockMember, getBookLending 구현에 Member 씀
- checkout 구현에 BookItem 씀
- getBookLendings에 BookLending 씀

![Figure 1.8 The Catalog class](resources/Figure_01-08.png)
![Figure 1.9 The Book class](resources/Figure_01-09.png)

### 1.1.4 The implementation phase

다이어그램 리뷰 뒤에 Dave는 커피를 한 모금 마십니다. 그는 Theo에게 감동받았습니다.
> 데이브: 가슴이 웅장해진다...!
> 
> 테오: 감사합니다ㅎ
>
> 테오: 저는 사람들이 코딩하기 전에 이렇게 디자인에 시간을 할애하고 있는지 깨닫지 못했었습니다.
>
> 데이브: 저는 항상 디자인을 먼저 해요. 그러면 코딩할 때 시간을 많이 절약할 수 있더라구요.
> 
> 테오: 그래서 코딩 언제할거에요?ㅎ
>
> 데이브: 라떼 먹고 하겠습니다

테오는 따뜻한 라떼가 아이스 라떼가 된 사실을 알았습니다. 다이어그램을 데이브에게 설명하는게 너무 신나서 커피마시는 걸 잊었습니다.

### 1.2.1 Many relations between classes

![Figure 1.10 A class diagram overview for Klafim’s Library Management System](resources/Figure_01-10.png)
![Figure 1.11 The class Member is involved in five relations.](resources/Figure_01-11.png)
![Figure 1.12 A class diagram where Member is split into code and data entities](resources/Figure_01-12.png)
![Figure 1.13 A class diagram where every class is split into code and data entities](resources/Figure_01-13.png)

### 1.2.2 Unpredictable code behavior


### 1.2.3 Not trivial data serialization

![Figure 1.14 The class diagram for SearchController](resources/Figure_01-14.png)

### 1.2.4 복잡한 클래스 계위

![Figure 1.15 The part of the class diagram that deals with members and librarians](resources/Figure_01-15.png)
![Figure 1.16 A class diagram for a system with VIP members](resources/Figure_01-16.png)
![Figure 1.17 A class diagram for a system with Super and VIP members](resources/Figure_01-17.png)


