# 7. 텍스트의 다양한 변신 (문자열, 파일 다루기)

**텍스트 데이터는 어떻게 처리하는지, 텍스트 파일의 종류는 무엇이 있는 지 살펴보고 연습해 본다.**

---

|-|목차|⏲ 180분|
|:---:|---|:---:|
|7-1| 들어가며 | 5분|
|7-2| 텍스트 데이터를 문자열로 저장한다는 것 (1) 인코딩과 디코딩 | 35분|
|7-3| 텍스트 데이터를 문자열로 저장한다는 것 (2) 문자열 다루기 | 25분|
|7-4| 텍스트 데이터를 문자열로 저장한다는 것 (3) 정규 표현식 | 25분|
|7-5| 파일과 디렉터리 (1) 파일 | 15분|
|7-6| 파일과 디렉터리 (2) 디렉터리 | 20분|
|7-7| 파일과 디렉터리 (3) 모듈과 패키지 | 10분|
|7-8| 여러가지 파일 포맷 다루기 (1) CSV 파일 | 15분|
|7-9| 여러가지 파일 포맷 다루기 (2) XML 파일 | 20분|
|7-10| 여러가지 파일 포맷 다루기 (3) JSON 파일 | 10분|

---

### 학습 목표

- 파이썬에서 텍스트 데이터를 어떻게 처리하는지 알아봅니다.
- 파이썬에서 텍스트 파일과 디렉토리에 접근하는 방법을 알아봅니다.
- 텍스트 파일의 종류를 살펴보고 다루는 방법을 각각 연습해 봅니다.

### 목차

1. 텍스트 데이터를 문자열로 저장한다는 것
    - 인코딩과 디코딩
    - 문자열 다루기
    - 정규 표현식
2. 파일과 디렉터리
    -파일
    -디렉터리
    -모듈과 패키지
3. 여러 가지 파일 포맷 다루기
    - CSV 파일
    - XML파일
    - JSON 파일

---

- 인코딩 <-> 디코딩
  - 바이트(byte) : 컴퓨터의 기본 저장 단위
  - 1바이트(1byte) = 8비트(8bits)
  - 1바이트에는 256개(=$2^8$)의 고유한 값을 저장할 수 있다.
  - 인코딩 (encoding): 문자열 -> 바이트로 변환
  - 디코딩 (decoding): 바이트 -> 문자열로 변환
- 텍스트 데이터 처리 과정
  - ![text_to_digits](https://d3s0tskafalll9.cloudfront.net/media/images/Untitled_1_pqy5WAy.max-800x600.png)
- 유니코드 ≠ UTF-8
  - 유니코드(Unicode)
    - 전 세계 문자를 모두 표시할 수 있는 표준 코드
    - 오직 한 가지 버전만 존재
  - UTF-8, UTF-16
    - 유니코드로 정의된 텍스트를 메모리에 인코딩하는 방식
- 이스케이프 문자
  - `\[특정 문자]` 형태
  - 직접 입력할 수 없는 일부 문자를 문자열에 포함시킬 수 있는 특수 문자
  - 종류: `\', \", \t, \n, \\`
- 원시 문자열(raw string)
  - 이스케이프 문자 무시
  - `r'내용'`
- 공백 문자: space, \t(탭), \n(line feed, 개행), \r(carriage return, 복귀)
- String Method
  - startswith(), endswith()
  - strip()
  - upper(), lower(), capitalize()
  - isupper(), islower(), istitle(), isalpha(), isalnum(), isdecimal()
  - join(), split()
  - replace()
- 가변(mutable) 자료형 <-> 불변(immutable) 자료형
  - 가변 객체(mutable object)
    - 객체 생성 후, 객체의 값 수정 가능
    - 변수는 값이 수정된 같은 객체를 가리키게 됩니다.
    - e.g. list, set, dict
  - 불변 객체(immutable object)
    - 객체 생성 후, 객체의 값 수정 불가능
    - 변수는 해당 값을 가진 다른 객체를 가리키게 됩니다.
    - e.g. int, float, complex, bool, string, tuple, frozen set
- id()  # 객체 고유 아이디 확인
- 정규 표현식(regular expression, regex)
  - ![python_regex](https://d3s0tskafalll9.cloudfront.net/media/images/Untitled_9_X0ZpR5k.max-800x600.png)
    - import re 를 통해 정규식 모듈 가져오기
    - re.compile() 함수로 Regex 객체 생성
    - 검색할 문자열을 Regex 객체의 search() , findall() 메소드로 전달
  - import re
  - sent.replace("I", "You") == re.sub("I", "You", sent)
  - 특정 규칙을 가진 문자열의 집합을 표현하는 형식 언어
  - 찾고자 하는 문자열 패턴을 정의(=Compile) -> 기존 문자열과 일치하는지를 비교 -> 문자열을 검색/치환하는 데 사용
  - 메서드: search(), match(), findall(), split(), sub(), group()
  - 패턴
    - `[ ]` : 문자
      - `\-` : 범위
    - `.` : 하나의 문자
    - `?` : 0회 또는 1회 반복
      - `\*` : 0회 이상 반복
      - `\+` : 1회 이상 반복
    - `{m, n}` : m ~ n
    - `\d` : 숫자, [0-9]와 동일
    - `\D` : 비 숫자, [^0-9]와 동일
    - `\w` : 알파벳 문자 + 숫자 + _, [a-zA-Z0-9_]와 동일
    - `\W` : 비 알파벳 문자 + 비숫자, [^a-zA-Z0-9_]와 동일
    - `\s` : 공백 문자, [ \t\n\r\f\v]와 동일
    - `\S` : 비 공백 문자, [^ \t\n\r\f\v]와 동일
    - `\b` : 단어 경계
    - `\B` : 비 단어 경계
    - `\t` : 가로 탭(tab)
    - `\v` : 세로 탭(vertical tab)
    - `\f` : 폼 피드
    - `\n` : 라인 피드(개행문자)
    - `\r` : 캐리지 리턴(원시 문자열)
- 파일(File)
  - 보조기억장치(휘발X)에 저장한 데이터
  - 메서드
    - f.read() : 파일을 읽는다.
    - f.readline() : 파일을 한 줄씩 읽는다.
    - f.readlines() : 파일 안의 모든 줄을 읽어 그 값을 리스트로 반환한다.
    - f.write(str) : 파일에 쓴다. 문자열 타입을 인자로 받는다.
    - f.writelines(str) : 파일에 인자를 한 줄씩 쓴다.
    - f.close() : 파일을 닫는다.
    - f.seek(offset) : 해당 파일의 위치(offset)를 찾아 파일의 커서를 옮긴다. 파일의 처음 위치는 0이다.
    - f.tell(): 현재 커서의 위치를 반환한다.
  - CSV(Comma Seperated Value)
    - 칼럼(column)을 쉼표로 구분한 파일
    - import pandas as pd
    - ![csv](https://d3s0tskafalll9.cloudfront.net/media/images/Untitled_12_5eHgk21.max-800x600.png)
  - XML(Extensible Markup Language)
    - XML은 다목적 마크업 언어(Extensible Markup Language)이다.
    - 마크업 언어는 태그(tag)로 이루어진 언어를 말하며, 상위(부모) 태그 - 하위(자식) 태그의 계층적 구조로 되어 있다.
    - XML은 요소(element)들로 이루어져 있다.
    - 요소는 <열린 태그> 내용 </닫힌 태그>가 기본적인 구조이며, 속성(attribute) 값을 가질 수도 있다.
    - 라이브러리: ElementTree
  - JSON(JavaScript Object Notation)
    - 웹 언어인 JavaScript의 데이터 객체 표현 방식
    - 웹 브라우저와 다른 애플리케이션 사이에서 HTTP 요청으로 데이터를 보낼 때 널리 사용하는 표준 파일 포맷 중 하나
    - XML과 더불어 웹 API나 config 데이터를 전송할 때 많이 사용
    - import json
    - contents = json.load(f)
- 폴더(=디렉터리)
  - 루트 디렉터리(root directory): 최상위 폴더
    - Window 운영 체제 : C:\
    - Linux 계열 운영 체제 : /
  - 디렉터리 관련 표준 라이브러리: sys, os, glob
  - 함수
    - sys.path : 현재 폴더와 파이썬 모듈들이 저장되는 위치를 리스트 형태로 반환
    - sys.path.append() : 자신이 만든 모듈의 경로를 append 함수를 이용해서 추가함. 그 후 - 추가한 디렉터리에 있는 파이썬 모듈을 불러와 사용할 수 있다.
    - os.chdir() : 디렉터리 위치 변경
    - os.getcwd() : 현재 자신의 디렉터리 위치를 반환
    - os.mkdir() : 디렉터리 생성
    - os.rmdir() : 디렉터리 삭제 (단, 디렉터리가 비어 있을 경우)
    - glob.glob() : 해당 경로 안의 디렉터리나 파일들을 리스트 형태로 반환
    - os.path.join() : 경로(path)를 병합하여 새 경로 생성
    - os.listdir() : 디렉터리 안의 파일 및 서브 디렉터리를 리스트 형태로 반환
    - os.path.exists() : 파일 혹은 디렉터리의 경로 존재 여부 확인
    - os.path.isfile() : 파일 경로의 존재 여부 확인
    - os.path.isdir() : 디렉터리 경로의 존재 여부 확인
    - os.path.getsize() : 파일의 크기 확인
- 모듈, 패키지, 라이브러리
  - 모듈(module) : 파이썬으로 만든 코드가 들어간 파일 .py
  - 패키지(package) : 기능적으로 동일하거나 동일한 결과를 만드는 모듈들의 집합 또는 폴더. 종종 라이브러리라고도 불림
  - 라이브러리(library) : 모듈과 패키지의 집합. 패키지보다 포괄적인 개념이나 패키지와 혼용되어 사용되기도 함.
  - PIP(Package Installer for Python) : 패키지 관리자로 파이썬을 설치하면 기본으로 설치됨
  - PyPA(Python Packaging Authority) : 파이선 패키지를 관리하고 유지하는 그룹
  - PyPI(The Python Package Index) : 파이썬 패키지들의 저장소
