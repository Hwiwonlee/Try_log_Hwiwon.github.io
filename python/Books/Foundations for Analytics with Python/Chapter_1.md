# 1. 파이썬 기초
## 1.1 파이썬 스크립트를 생성하는 방법
## 1.2 파이썬 스크립트 실행 방법
## 1.3 명렬 줄에서 유용한 팁 몇가지
```python
x = 4
y = 5
z = x + y
print("output #1 : {0:d}".format(z))

# {0:d}를 위한 예제
for x in range(1, 11):
    print('{0:2d} {1:3d} {2:4d}'.format(x, x*x, x*x*x)) ## x를 두자리, x^2를 세자리. x^3를 네자리로 print

## Q. 2d로 통일하면 두자리로만 나올까?
for x in range(1, 11):
    print('{0:2d} {1:2d} {2:2d}'.format(x, x*x, x*x*x))
## A. 돌려보면 알겠지만 nd에서 말하는 n자리는 해당 값이 할당될 공간이다. 쉽게 말해서 자릿 수에 따른 '줄맞춤'이라고 이해하면 편하다. 
        
a = [1,2,3,4]
b = ["first","second","third","fourth"]
c = a+b
print("output #2 : {0}, {1}, {2}".format(a,b,c))
print("output #3 : ",a,",",b,",",c) ## format없이 똑같은 결과를 내기 위한 code
```
https://docs.python.org/ko/3.8/tutorial/inputoutput.html
Print()를 이용할 때 str.format() method를 이용하는 것이 눈에 띈다. Data camp나 Jump to Python의 방법과는 또 다르다. 위의 링크와 구글링을 통해 확인한 바, f-string이 나옴으로 인해 str.format()이 구버전이 되었지만 복잡한 문자열을 처리할 때 오타를 방지한다는 측면은 꽤 괜찮은 장점으로 보인다. 그러나 여전히 문자열의 길이가 늘어나고 문자열 안에 포함될 변수들이 많아지는 경우 이용이 복잡하다는 단점은 존재하는 것 같다. 

## 1.4 파이썬 기본 구성 요소
### 1.4.1 숫자
```python
a = 38819/float(3)
print("output #4 : {0:2f}".format(a))
print("output #5 : {0:4f}".format(a))
## 책 내용대로라면 각각 소수점 아래 두자리, 네자리까지 print되어야 하는데 아니네?
```
### 1.4.2 문자열
```python
print("output #6 : {0:s}".format('Python is an interpreted, high-level, general-purpose programming language.'))
sentence = "Python is"
print(f'output #7 : {sentence} an interpreted, high-level, general-purpose programming language.') ## f-string을 이용한 print
print("output #8 : {0:s}".format("""Python is an interpreted, high-level, general-purpose programming language.
Created by Guido van Rossum and first released in 1991,
Python's design philosophy emphasizes code readability with its notable use of significant whitespace.""")) 
## 책에는 '\'로 줄 띄어쓰기를 했는데 에러 나서 """로 그냥 해봄.
print("output #9 : {0:s}".format('''Python is an interpreted, high-level, general-purpose programming language.
Created by Guido van Rossum and first released in 1991,
Python's design philosophy emphasizes code readability with its notable use of significant whitespace.''')) 
## output #9와 같은 결과 

# split(기준, n번 분할) : 선언된 argument에 따라 string을 분할, List로 return시키는 method 
sentence = "Python is an interpreted, high-level, general-purpose programming language"
sentence_1 = sentence.split() ## default setting : '공백'을 기준으로 최대한 분할 
sentence_2 = sentence.split(" ", 2) ## 공백을 기준으로 2번 분할 
sentence_3 = sentence.split("i") ## String i를 기준으로 최대한 분할
print("output #10 : {0}".format(sentence_1))
print("output #11 : First Part:{0}, Second Part:{1}, Third Part:{2}".format(sentence_2[0], sentence_2[1], sentence_2[2]))
print("output #12 : First Part:{0}, Second Part:{1}, Third Part:{2}, Fourth Part:{3}, Fifth Part:{4}"
.format(sentence_3[0], sentence_3[1], sentence_3[2], sentence_3[3], sentence_3[4]))

# join() : str.join(object)로 사용, 선언된 str를 기준으로 list의 element를 join시켜서 return시키는 method
print("output # 13 : {0}".format(" ".join(sentence_1)))

# object.strip(str) : 선언된 str를 object에서 삭제한 후 return
sen = "++Python is an interpreted, high-level, general-purpose programming language--"
print("output #14 : {0}".format(sen.strip("+-")))

# object.replace(str1, str2) : 선언된 str1를 str2로 교체시킨 후 return
print("output #15 : {0}".format(sen.replace("+", "-")))

# object.lower, upper, capitalize() : object의 str를 각각 소문자, 대문자, 첫 글자 대문자로 변환시키는 method
print("output #16 : {0}".format(sentence.lower()))
print("output #17 : {0}".format(sentence.upper()))
print("output #18 : {0}".format(sentence.capitalize()))
```
### 1.4.3 정규표현식(Regular expression)
보통 '정규표현식'은 후반에 나오는데 이 책은 놀랍게도 초반에 나와서 초보자들의 멘탈을 공격하고 있다. 데이터 분석에서 정규표현식은 자료의 패턴을 찾는데 사용되곤 하는데, 정규표현식 자체가 그렇게 사용자 친화적이지 않기 때문에 진입장벽이 있다. 그렇지만 나온 김에 짚고 넘어가자. 
```python
python_wiki = """Python is an interpreted, high-level, general-purpose programming language. Created by Guido van Rossum and first released in 1991, Python's design philosophy emphasizes code readability with its notable use of significant whitespace. Its language constructs and object-oriented approach aim to help programmers write clear, logical code for small and large-scale projects.[27]Python is dynamically typed and garbage-collected. It supports multiple programming paradigms, including procedural, object-oriented, and functional programming. Python is often described as a "batteries included" language due to its comprehensive standard library.[28] Python was conceived in the late 1980s as a successor to the ABC language. Python 2.0, released in 2000, introduced features like list comprehensions and a garbage collection system capable of collecting reference cycles. Python 3.0, released in 2008, was a major revision of the language that is not completely backward-compatible, and much Python 2 code does not run unmodified on Python 3. Due to concern about the amount of code written for Python 2, support for Python 2.7 (the last release in the 2.x series) was extended to 2020. Language developer Guido van Rossum shouldered sole responsibility for the project until July 2018 but now shares his leadership as a member of a five-person steering council.[29][30][31]The Python 2 language, i.e. Python 2.7.x, is "sunsetting" on January 1, 2020, and the Python team of volunteers will not fix security issues, or improve it in other ways after that date.[32][33] With the end-of-life, only Python 3.5.x and later will be supported.[citation needed]Python interpreters are available for many operating systems. A global community of programmers develops and maintains CPython, an open source[34] reference implementation. A non-profit organization, the Python Software Foundation, manages and directs resources for Python and CPython development."""

# python_wiki에서 일정한 패턴 찾기 
import re ## re module을 import
python_wiki_list = python_wiki.split() ## python_wiki를 공백 기준으로 split
pattern = re.compile(r"Python", re.I) 
## compile() function : 텍스트 기반의 패턴을 정규표현식으로 컴파일
### r"Python" : 'raw string'임을 의미하는 'r'을 사용
### re.I function : 컴파일에서 선언된 텍스트 기반의 패턴에서 대소문자를 무시하도록 함. 
### Q. raw string은 뭐지?
count = 0
for x in python_wiki_list:
    if pattern.search(x): ## object.search(var) : re module에 속한 object.search method() 해당 object가 var가 포함되어 있는지 check 
        count += 1 ## if의 조건을 만족한다면 count에 1을 더한다. 
print("Output #19 : {0:d}".format(count))  

# 문자열 내에서 발견된 패턴 출력하기
pattern = re.compile(r"(?P<match_word>Python)", re.I)
## 일치하는 문자열을 출력하기 위한 (?P<그룹 이름>찾을 패턴) : 메타 문자
print("Output #20:")
for x in python_wiki_list:
    if pattern.search(x):
        print("{:s}".format(pattern.search(x).group('match_word')))
        ## 결과가 True라면 pattern.search(x)의 자료구조에서.group('match_word')로 match_word 그룹의 값을 가져온 후 그 값 = 찾을 패턴 = Python을 출력한다
        
# 문자열 내 "Python"을 "파이썬"으로 대체하기
string_check = r"Python"
pattern = re.compile(string_check, re.I)
print("Output #21: {:s}".format(pattern.sub("파이썬", python_wiki)))
## pattern.sub(str. object) method : object를 대상으로 pattern에 일치하는 부분을 찾아 str으로 대체함. 
```

### 1.4.4 날짜