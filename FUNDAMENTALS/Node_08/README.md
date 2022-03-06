# 8. 파이썬 잘하는 척 해보자

**파이썬을 파이썬답게 활용하기 위해 알아야 할 핵심개념과 문법을 이해하고 실습해 본다.**

---

|-|목차|⏲ 150분|
|:---:|---|:---:|
|8-1| 들어가며 | 5분|
|8-2| 파이썬 어디까지 써 봤니?! | 10분|
|8-3| 파이썬을 더 잘 사용해보자! (1) for문 잘 사용하기 | 20분|
|8-4| 파이썬을 더 잘 사용해보자! (2) Try - Except 예외 처리하기 | 15분|
|8-5| 파이썬을 더 잘 사용해보자! (3) Multiprocessing | 15분|
|8-6| 같은 코드 두 번 짜지 말자! (1) 함수 사용하기 | 10분|
|8-7| 같은 코드 두 번 짜지 말자! (2) 함수 사용 팁 | 20분|
|8-8| 같은 코드 두 번 짜지 말자! (3) 람다 표현식 | 15분|
|8-9| 같은 코드 두 번 짜지 말자! (4) 클래스, 모듈, 패키지 | 10분|
|8-10| 프로그래밍 패러다임과 함수형 프로그래밍 | 15분|
|8-11| 파이써닉하게 코드를 짜보자 | 15분|

---

### 학습 목표

- 파이썬에 대해서 알아보자.
- 코드 재사용에 대해서 알아보자.
- 프로그래밍 패러다임에 대해서 알아보자.
- 파이썬 코드를 이쁘게 해보자.

### 목차

1. 파이썬 어디까지 써 봤니?!
2. 파이썬을 더 잘 사용해 보자!
    - 2.1 For문 잘 써보기
    - 2.2 Try - Except
    - 2.3 Multiprocessing
3. 같은 코드 두 번 짜지 말자!
    - 3.1 함수(Function)
    - 3.2 람다 표현식
    - 3.3 클래스(Class), 모듈(Module), 패키지(Package)
4. 프로그래밍 패러다임과 함수형 프로그래밍
5. 파이써닉하게 코드를 짜보자!

---

- 프로그래밍 언어
  - 퍼포먼스 vs 생산성 => 생산성이 올라가면 퍼포먼스가 떨어지고, 퍼포먼스가 올라가면 생산성이 떨어짐
    - 퍼포먼스(성능):코드를 짜서 실행을 시켰을 때 얼마나 빨리 처리되는가
      - ![programming_languge_performance](https://d3s0tskafalll9.cloudfront.net/media/original_images/F-9-1.png)
    - 생산성: 똑같은 기능을 하는 프로그램을 얼마나 빨리 작성할 수 있는가
  - 어떤 것을 우선적으로 선택해야 하는가?
    - 목적에 맞게, 상황에 맞게 언어를 선택 => 기존에 사용하고 있는 언어, 개발하고자 하는 프로젝트의 성능과 개발 기간 고려해서 선택
- 파이썬(Python)
  - 높은 생산성 => 대부분의 기능은 이미 어떤 패키지로 만들어져 있을 가능성이 높음
  - 코드의 간결함
  - 빠른 개발 속도
    - ![coding_time](https://d3s0tskafalll9.cloudfront.net/media/original_images/F-9-3.png)
  - 스크립트 언어(=인터프리터 언어)
- 컴파일 언어 vs 인터프리터 언어
  - 컴파일 언어
    - 실행 전 소스 코드를 컴파일하여 기계어로 변환 후 해당 파일을 실행
    - 이미 기계어로 변환된 것을 실행하므로 비교적 빠름
    - 컴파일 시점에 소스 코드의 오류를 잡기 쉬움
    - 같은 소스 코드도 다른 환경(PC, mobile 등)에서 실행하려면 다시 컴파일(기계어로 변환) 해야함
  - 스크립트 언어(인터프리터 언어)
    - 코드를 작성함과 동시에 인터프리터가 기계어로 번역하고 실행함
    - 코드 번역 과정이 있어 비교적 느림
    - 주 사용 목적이 뚜렷하게 개발되어 사용하기 쉬운 편
    - 명령줄로 코드를 즉시 실행할 수 있음
- for 문
  - enumerate(): 순서와 리스트의 값을 함께 반환
  - 이중 for 문
  - List Comprehension: 리스트 등 순회형 컨테이너 객체로부터 이를 가공한 새로운 리스트를 생성(Set, Dict 적용 가능)
  - Generator: 데이터셋을 하나씩 가져와서 공급
    - 이중 for 문
      - 이중 for 문이 다 돌아가는 걸 기다린 후, 반환된 result_list 값에 대해 또 for 문을 돌려야 함
      - e.g. 1억 개의 데이터를 전부 메모리에 올려놔야 함
    - 제너레이터
      - yield: 코드 실행의 순서를 밖으로 "양보"
      - e.g. 1억 개의 데이터를 전부 메모리에 올려놓을 필요가 없이 현재 처리해야 할 데이터를 1개씩 로드해서 사용

```python
my_list = ['a','b','c','d']

# 인자로 받은 리스트를 가공해서 만든 데이터셋 리스트를 리턴하는 함수
def get_dataset_list(my_list):
    result_list = []
    for i in range(2):
        for j in my_list:
            result_list.append((i, j))
    print('>>  {} data loaded..'.format(len(result_list)))
    return result_list

# 이중 for 문
for X, y in get_dataset_list(my_list):
    print(X, y)

# 인자로 받은 리스트로부터 데이터를 하나씩 가져오는 제너레이터를 리턴하는 함수
def get_dataset_generator(my_list):
    result_list = []
    for i in range(2):
        for j in my_list:
            yield (i, j)   # 이 줄이 이전의 append 코드를 대체했습니다
            print('>>  1 data loaded..')

# 제너레이터
for X, y in get_dataset_generator(my_list):
    print(X, y)
```

- 예외 처리(Try-Except)
  - 예외(Except): 코드 실행 중 발생한 에러
  - 예외 처리:  예외(에러)가 발생했을 때 그 예외(에러)를 무시 or 예외(에러) 대신 적절한 처리를 해주게 하는 등의 작업
  - ![try-except](https://d3s0tskafalll9.cloudfront.net/media/original_images/F-9-4.png)

```python
try:
    # 실행 코드
    print(10/0)
except:
    # 에러가 발생했을 때 처리하는 코드
    print('에러가 발생했습니다.')
    print("값 수정 : ", 10/2)
```

- 멀티 프로세싱(Multiprocessing)
  - cf. 내가 짠 코드 실행 소요 시간 구하기
    - import time
    - start = time.time()  # 시작 시간 저장
    - ...
    - end = time.time()  # 종료 시간 저장
    - run_time = end - start  # 소요 시간(결과 '초' 단위)
  - 컴퓨터가 작업을 처리하는 속도를 높여주는 방법 중 하나
  - ![Multiprocessing](https://d3s0tskafalll9.cloudfront.net/media/images/F-9-6.max-800x600.png)
    - parallel processing: 병렬 처리, serial processing: 순차 처리

```python
import multiprocessing


def count(name):
    for i in range(0, 100000000):
        a = 1 + 2
    print("finish:"+name+"\n")


num_list = ['p1','p2', 'p3', 'p4']

if __name__ == '__main__':
    pool = multiprocessing.Pool(processes = 4)  # 병렬 처리 시, 사용할 CPU 코어 개수
    pool.map(count, num_list)  # 병렬화시키는 함수 # count('p1'), count('p2'), count('p3'), count('p4')
    pool.close()  # 병렬화 종료
    pool.join()  # 병렬처리 작업이 끝날 때까지 기다림
```

- 함수(Function)
  - y = f(x)
    - x: 입력(정의역)
    - f(x): 수식
    - y: 결과(치역)
  - 장점: 코드 효율성, 재사용성, 가독성
  - pass 문
    - 아무 작업도 하지 않음
    - 문법적으로 해당 문장이 필요하지만, 프로그램이 특별히 할 일이 없을 때 사용
  - 여러 변수로 반환하기
    - return x_min, x_max
  - 함수 연달아 사용
    - 함수1(함수2())
  - 함수 안의 함수

```python
def f1():

    def f2():
        return r1
    
    r2 = f2()
    return r2
```

- 람다 표현식(lambda expression)
  - 이름이 없는 함수
  - 사용하는 가장 중요한 이유: 함수의 인수 부분을 간단히 하기 위함
  - 람다 표현식과 자주 쓰이는 함수: map(), filter(), reduce() 등
    - map(function, iterable): 입력받은 자료형의 각 요소가 함수에 의해 수행된 결과를 묶어서 map iterator 객체로 출력

```python
result = list(map(lambda i: i * 2 , [1, 2, 3]))
```

- 클래스, 모듈, 패키지
  - 클래스(Class)
    - 비슷한 역할을 하는 함수들의 집합
  - 모듈(Module)
    - 함수, 변수, 클래스를 모아 놓은 파일
    - import 모듈
    - 모듈.함수()
  - 패키지(라이브러리)
    - 여러 모듈을 하나로 모아둔 폴더
  - ![class, module, package](https://d3s0tskafalll9.cloudfront.net/media/images/F-9-9.max-800x600.png)
  - 패키지 설치: pip install 패키지
- 프로그래밍 패러다임
  - 패러다임(Paradigm)
    - 어떤 한 시대의 사람들의 견해나 사고를 근본적으로 규정하고 있는 테두리
    - 인식의 체계 또는 사물에 대한 이론적인 틀이나 체계
    - e.g. 천동설 vs 지동설
  - 프로그래밍 패러다임
    - 프로그래머에게 프로그래밍의 관점을 갖게 해 주고, 결정하는 역할
    - 절차 지향 프로그래밍, 객체 지향 프로그래밍, 함수형 프로그래밍
      - 절차 지향 프로그래밍: 일이 진행되는 순서대로 프로그래밍
        - 장점
          - 순서대로 읽기만 하면 이해 가능
        - 단점
          - 위에서 하나가 잘못되면 아래도 연쇄적으로 문제 발생 => 유지 보수 어려움
          - 일반적으로 코드 길이가 길어서 코드 분석하기 어려움
      - 객체 지향 프로그래밍(OOP, Object Oriented Programming): 개발자가 프로그램을 상호작용하는 객체들의 집합으로 볼 수 있게 함
        - 장점
          - 코드 재사용 쉬움
          - 코드 분석 쉬움
          - 아키텍처 바꾸기 쉬움
        - 단점
          - 객체 간 상호작용 => 설계에서 많은 시간 소요 => 설계가 잘못되면 전체적으로 바꿔야 함
      - 함수형 프로그래밍
        - 데이터 사이언티스트에게 적합
        - 함수로 문제를 분해
        - 장점: 효율성, 버그 없는 코드, 병렬 프로그래밍에서 장점이 있음
        - 특징
          - 순수성
            - 부작용이 전혀 없는 함수 == 순수 함수
            - 프로그램이 실행될 때 해당 프로그램이 수정될 수 있는 상황을 엄격히 제한
            - 모든 함수의 출력은 입력에만 의존
            - e.g. 함수 안에 함수 밖에서 바로 가져오는 함수X, 밖에 있는 변수를 변경X
          - 모듈성
            - 문제를 작은 조각(=한 가지 작업을 수행하는 작은 함수)으로 분해 => 가독성 좋음, 오류 확인 쉬움 => 모듈화
          - 디버깅과 테스트 용이성
            - 각각의 함수가 작고 명확하게 명시 => 디버깅 쉬움
            - 각 함수는 잠재적 단위 테스트의 대상 => 테스트 쉬움
- PEP8: Python Code Style Guide
  - Whitespace
    - 한 줄 코드 길이 79자 이하
    - 함수와 클래스는 다른 코드와 빈 줄 2개로 구분
    - 클래스에서 함수는 빈 줄 1개로 구분
    - 변수 할당 앞뒤에 스페이스 하나만 사용
    - 리스트 인덱스, 함수 호출에는 스페이스 사용X
    - 쉼표(,), 쌍점(:), 쌍반점(;) 앞에서는 스페이스 사용X
  - 주석
    - 코드 내용 일치하지 않는 주석X
    - 불필요한 주석X
  - 이름 규칙
    - 변수명 앞에 밑줄(_)이 붙으면 함수 등의 내부에서만 사용되는 변수를 뜻함
    - 변수명 뒤에 밑줄(_)이 붙으면 라이브러리/키워드와 충돌을 피할 수 있음
    - 소문자 L, 대문자 O, 대문자 I는 가독성이 안 좋으니 사용하지 말 것
    - 모듈(Module)명은 짧은 소문자로 구성, 밑줄로 나눔
    - 클래스명은 파스칼 케이스(PascalCase) 컨벤션으로 작성
    - 함수명은 소문자로 구성, 밑줄로 나눔
    - 상수(Constant)는 모듈 단위에서만 정의, 모든 단어는 대문자, 밑줄로 나눔
  - 네이밍 컨벤션(Naming Convention)
    - snake_case
      - 모든 공백을 "_"로 바꾸고 모든 단어는 소문자입니다.
      - 파이썬에서는 함수, 변수 등을 명명할 때 사용합니다.
      - ex) this_snake_case
    - PascalCase
      - 모든 단어가 대문자로 시작합니다.
      - UpperCamelCase, CapWords라고 불리기도 합니다.
      - 파이썬에서는 클래스를 명명할 때 사용합니다.
      - ex) ThisPascalCase
    - camelCase
      - 처음은 소문자로 시작하고 이후의 모든 단어의 첫 글자는 대문자로 합니다.
      - lowerCamelCase라고 불리기도 합니다.
      - 파이썬에서는 거의 사용하지 않습니다 (Java 등에서 사용)
      - ex) thisCamelCase
