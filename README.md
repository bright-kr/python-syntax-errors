# Python 구문 오류를 피하고 수정하는 방법

[![Promo](https://github.com/bright-kr/LinkedIn-Scraper/raw/main/Proxies%20and%20scrapers%20GitHub%20bonus%20banner.png)](https://brightdata.co.kr/) 

이 가이드는 흔한 Python 구문 오류, 이를 예방하기 위해 사용할 수 있는 사전 전략, 그리고 이를 효율적으로 해결하기 위해 사용할 수 있는 사후 방법을 설명합니다:

- [Types of Syntax Errors](#types-of-syntax-errors)
- [Avoiding Syntax Errors](#avoiding-syntax-errors)
  - [Proactive Strategies](#proactive-strategies)
  - [Reactive Strategies](#reactive-strategies)

## Python의 구문 오류

[Python Interpreter](https://docs.python.org/3/tutorial/interpreter.html)는 Python 코드를 실행하면서 이를 기계어로 번역합니다. 구문 규칙을 위반하면 실행이 중지되고, traceback과 함께 오류 메시지가 표시됩니다.

구문 오류는 언어에서의 문법 오류와 유사한 구조적 실수에서 발생합니다. 예를 들어, Python은 `if` 문과 루프 같은 블록에 대해 올바른 들여쓰기를 요구합니다.

프로그램 실행 중에 발생하는 [runtime errors](https://docs.python.org/3/library/exceptions.html#RuntimeError)와 달리, 구문 오류는 실행 자체를 완전히 막습니다.

## Types of Syntax Errors

Python은 많은 [syntax rules](https://peps.python.org/pep-0008/)를 따르므로 구문 오류가 흔합니다. 이 섹션에서는 자주 발생하는 여러 오류와 그 해결 방법을 살펴봅니다.

### 잘못된 위치, 누락 또는 불일치한 구두점

Python은 코드 구조를 잡기 위해 구두점에 의존합니다. 구문 오류를 피하려면 구두점이 올바른 위치에 있고 올바르게 짝지어졌는지 확인하십시오.

예를 들어, 괄호 `()`, 대괄호 `[]`, 중괄호 `{}`는 항상 쌍으로 사용되어야 합니다. 하나를 열면 반드시 닫아야 합니다.

아래 예시에서는 중괄호가 닫히지 않은 채로 남아 있습니다:

```python
# Incorrect
proxies = {
    'http': proxy_url,
    'https': proxy_url

```

이를 실행하려고 하면 인터프리터가 `SyntaxError`를 발생시킵니다:

```python
File "python-syntax-errors.py", line 2
    proxies = {
            ^
SyntaxError: '{' was never closed
```

Python 인터프리터는 파일 이름, 줄 번호, 그리고 오류가 발생한 위치를 가리키는 화살표를 포함하여 자세한 오류 메시지를 제공합니다. 여기서는 `'{' was never closed`라고 지정하고 있습니다.

이 정보를 통해 중괄호를 닫아 문제를 쉽게 식별하고 수정할 수 있습니다.

```python
# Correct
proxies = {
    'http': proxy_url,
    'https': proxy_url
} # Closed a curly bracket
```

따옴표(`'` 또는 `"`)는 Python에서 자주 문제를 일으킵니다. 많은 프로그래밍 언어와 마찬가지로 Python은 따옴표로 문자열을 정의합니다. 문자열을 열고 닫을 때는 항상 같은 유형의 따옴표를 사용해야 합니다.

```python
# Incorrect
host = "brd.superproxy.io'
```

작은따옴표와 큰따옴표를 섞으면 구문 오류가 발생합니다:

```python
File "python-syntax-errors.py", line 2
    host = "brd.superproxy.io'
        ^
SyntaxError: unterminated string literal (detected at line 2)
```

여기서 인터프리터는 2번째 줄의 문자열 리터럴이 종료되지 않았다고 알려줍니다:

```python
# Correct
host = "brd.superproxy.io"
```

문자열에 작은따옴표와 큰따옴표가 모두 포함되어 있다면, 삼중 따옴표(`'''` 또는 `"""`)로 문자열을 감싸십시오. 예시는 다음과 같습니다:

```python
quote = """He said, "It's the best proxy service you can find!", and showed me this provider."""
```

쉼표는 리스트, 튜플, 그리고 함수 인자에서 항목을 구분합니다. 쉼표가 누락되면 예기치 않은 오류가 발생할 수 있습니다.

```python
# Incorrect
proxies= [
    {"http": "http://123.456.789.1:8080", "https": "https://123.456.789.1:8080"}
    {"http": "http://98.765.432.1:3128", "https": "https://98.765.432.1:3128"}
    {"http": "http://192.168.1.1:8080", "https": "https://192.168.1.1:8080"}
]
```

이 코드를 실행하면 다음 오류 메시지가 나타납니다:

```python
File "python-syntax-errors.py", line 3
{"http": "http://123.456.789.1:8080", "https": "https://123.456.789.1:8080"}
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
SyntaxError: invalid syntax. Perhaps you forgot a comma?
```

오류 메시지는 유용하지만 모든 문제를 다 잡아내지는 못할 수 있습니다. 쉼표 누락이 감지되었다면, 올바른 구문을 보장하기 위해 주변 코드에 다른 쉼표 누락이 없는지도 확인하십시오.

```python
# Correct
proxies = [
    {"http": "http://123.456.789.1:8080", "https": "https://123.456.789.1:8080"},
    {"http": "http://98.765.432.1:3128", "https": "https://98.765.432.1:3128"},
    {"http": "http://192.168.1.1:8080", "https": "https://192.168.1.1:8080"}
]
```

쉼표와 달리 콜론은 새로운 코드 블록(예: `if` 문 또는 `for` 루프)을 시작하는 데 사용됩니다:

```python
import requests
from bs4 import BeautifulSoup

# Incorrect
response = requests.get('https://example.com')
if response.status_code == 200
    soup = BeautifulSoup(response.content, 'html.parser')
    title = soup.title.text
    print(title)
)
```

콜론을 잊으면 다음과 같은 구문 오류가 발생합니다:

```python
if response.status_code == 200
    ^
SyntaxError: expected ':'   
```

이 오류 메시지에서 콜론이 누락되었음을 쉽게 알 수 있으며, 제안된 위치에 콜론을 추가하여 문제를 수정할 수 있습니다:

```python
import requests
from bs4 import BeautifulSoup

# Correct
response = requests.get('https://example.com')
if response.status_code == 200:
    soup = BeautifulSoup(response.content, 'html.parser')
    title = soup.title.text
    print(title)

```

### 철자가 틀리거나 잘못된 위치에 있거나 누락된 Python 키워드

[Python keywords](https://docs.python.org/3/reference/lexical_analysis.html#keywords)는 특정 의미를 가진 예약어이며 변수 이름으로 사용할 수 없습니다. 키워드를 잘못 입력하거나, 잘못된 위치에 두거나, 누락하면 오류가 발생합니다.

예를 들어, `requests` 및 `pprint` 모듈을 가져올 때 `import` 키워드를 잘못 입력하면 문제가 발생할 수 있습니다.

```python
# Incorrect
improt requests
import pprint
```

이 오타로 인해 인터프리터가 다음과 같은 `invalid syntax` 오류를 발생시킵니다:

```python
File "python-syntax-errors.py", line 2
    improt requests
        ^^^^^^^^
SyntaxError: invalid syntax
```

일부 오류 메시지는 모호할 수 있어 추가적인 주의가 필요합니다. 이 경우 화살표는 `requests`를 가리키며, 구문 오류가 감지된 위치를 나타냅니다. 모듈 이름의 오타는 구문 오류를 유발하지 않으므로, 문제는 `import` 키워드의 오타일 가능성이 큽니다.

`import`를 올바르게 수정하면 오류가 해결됩니다.

```python
# Correct
import requests
import pprint
```

또한 `from ... import …` 문을 다음과 같이 잘못 쓸 수도 있습니다:

```python
import BeautifulSoup from bs4
```

겉보기에는 괜찮아 보이지만, 앞선 코드를 실행하면 `from` 키워드가 `import` 앞에 와야 하므로 오류가 발생합니다:

```python
File "python-syntax-errors.py", line 2
import BeautifulSoup from bs4
    ^^^^
SyntaxError: invalid syntax
```

`from`과 `import`의 순서를 바꾸면 문제가 해결됩니다:

```python
from bs4 import BeautifulSoup
```

키워드를 잊으면 Python에서 다양한 오류가 발생할 수 있습니다. 이 문제는 미묘하며, 누락된 키워드에 따라 서로 다른 오류가 나타날 수 있습니다.

예를 들어, 값을 반환해야 하는 함수에서 `return` 키워드를 누락하면 예기치 않은 동작이 발생할 수 있습니다.

```python
def fetch_data():
    response = requests.get('https://example.com')
    soup = BeautifulSoup(response.content, 'html.parser')
    data = soup.find_all('div', class_='data')
    # Missing a return statement here
    
data = fetch_data()
```

이는 구문 오류를 발생시키지는 않지만, 함수가 기대한 결과 대신 `None`을 반환합니다. `return` 키워드를 추가하면 앞선 코드가 수정됩니다:

```python
def fetch_data():
    response = requests.get('https://example.com')
    soup = BeautifulSoup(response.content, 'html.parser')
    data = soup.find_all('div', class_='data')
    return data

data = fetch_data()
```

함수를 정의할 때 `def` 키워드를 잊으면 구문 오류가 발생합니다:

```python
# Missing the `def` keyword
fetch_data():
    response = requests.get('https://example.com')
    soup = BeautifulSoup(response.content, 'html.parser')
    data = soup.find_all('div', class_='data')
    return data

data = fetch_data()
```

앞선 코드는 인터프리터가 함수 이름 앞에서 키워드를 기대하기 때문에 구문 오류가 발생합니다:

```python
File "python-syntax-errors.py", line 1
   fetch_data():
               ^
SyntaxError: invalid syntax
```

`def` 키워드를 추가하면 오류가 해결됩니다:

```python
def fetch_data():
    response = requests.get('https://example.com')
    soup = BeautifulSoup(response.content, 'html.parser')
    data = soup.find_all('div', class_='data')
    return data

data = fetch_data()
```

조건문에서 `if` 키워드를 잊으면, 인터프리터가 조건 앞에 키워드를 기대하기 때문에 오류가 발생합니다.

```python
import requests
from bs4 import BeautifulSoup

response = requests.get('https://example.com')
# Missing the if keyword
response.status_code == 200:
    soup = BeautifulSoup(response.content, 'html.parser')
    title = soup.title.text
    print(title)
```

```python
File "python-syntax-errors.py", line 6
   response.status_code == 200:
                              ^
SyntaxError: invalid syntax
```

이 문제를 해결하려면 `if` 키워드를 포함하기만 하면 됩니다:

```python
import requests
from bs4 import BeautifulSoup

response = requests.get('https://example.com')
if response.status_code == 200:
    soup = BeautifulSoup(response.content, 'html.parser')
    title = soup.title.text
    print(title)
```

> **Note**:
> 누락된 키워드는 다른 종류의 오류도 발생시킬 수 있으므로 특히 주의하십시오.

### 할당 연산자의 잘못된 사용

Python에서 `=`는 [assignment](https://docs.python.org/3/reference/expressions.html)에 사용되고, `==`는 [comparison](https://docs.python.org/3/library/stdtypes.html#comparisons)에 사용됩니다. 이를 혼동하면 구문 오류가 발생할 수 있습니다.

```python
import requests
from bs4 import BeautifulSoup

# Incorrect
response = requests.get('https://example.com', proxies=proxies)
if response = requests.get('https://example.com/data', proxies=proxies):
    soup = BeautifulSoup(response.content, 'html.parser')
    data = soup.find_all('div', class_='data')
    for item in data:
    print(item.text)
else:
    print("Failed to retrieve data")
```

이전 코드에서 인터프리터는 무엇이 문제를 일으켰는지 올바르게 감지합니다:

```python
File "python-syntax-errors.py", line 5
if response = requests.get('https://example.com/data', proxies=proxies)
     ^^^^^^
```

여기서는 response가 `request.get()`의 결과와 일치하는지 확인하고 있습니다. 오류를 수정하려면 `if` 문에서 할당 연산자(`=`)를 비교 연산자(`==`)로 바꾸십시오.

```python
import requests
from bs4 import BeautifulSoup

# Correct
response = requests.get('https://example.com', proxies=proxies)
# Change in the following line
if response == requests.get('https://example.com/data', proxies=proxies):
    soup = BeautifulSoup(response.content, 'html.parser')
    data = soup.find_all('div', class_='data')
    for item in data:
    print(item.text)
else:
    print("Failed to retrieve data")
```

### Indentation Errors

Python은 들여쓰기를 사용하여 코드 블록을 정의합니다. 들여쓰기가 잘못되면 인터프리터가 블록 구조를 인식할 수 없어 `IndentationError`가 발생합니다.

```python
# Incorrect
async with async_playwright() as playwright:
await run(playwright)
```

앞선 예시에서는(콜론 뒤의) 블록 정의에 들여쓰기가 없습니다. 코드를 실행하면 오류가 발생합니다.

```python
File "python-syntax-errors.py", line 2
    await run(playwright)
    ^
IndentationError: expected an indented block after the with statement on line 1
```

이 문제를 해결하려면 Python의 구문 규칙을 따르고 코드 블록을 올바르게 들여쓰기 하십시오:

```python
# Correct
async with async_playwright() as playwright:
    await run(playwright)
```

### 변수 선언 관련 문제

변수 이름은 문자 또는 밑줄로 시작해야 하며, 문자, 숫자, 밑줄만 포함할 수 있습니다. Python은 대소문자를 구분하므로 `myvariable`, `myVariable`, `MYVARIABLE`는 서로 다른 변수입니다.

변수 이름은 숫자로 시작할 수 없습니다. 아래 예시는 `1`로 시작하여 이 규칙을 위반합니다.

```python
# Incorrect
1st_port = 22225
```

앞선 코드를 실행하면 인터프리터가 `SyntaxError`를 발생시킵니다:

```python
File "python-syntax-errors.py", line 2
    1st_port = 1
    ^
SyntaxError: invalid decimal literal
```

이를 수정하려면 변수 이름을 문자 또는 밑줄로 시작해야 합니다. 다음 옵션 중 어느 것이든 사용할 수 있습니다:

```python
# Correct
first_port = 22225
port_no_1 = 22225
```

### 함수 정의 및 호출 오류

함수를 정의하려면 `def` 키워드를 사용하고, 그 뒤에 함수 이름, 괄호, 콜론을 붙입니다. 함수를 호출할 때는 함수 이름 뒤에 괄호를 사용합니다. 이 요소 중 하나라도 누락되면 구문 오류가 발생합니다.

```python
import requests
from bs4 import BeautifulSoup

# Incorrect
def fetch_data
response = requests.get('https://example.com')
soup = BeautifulSoup(response.content, 'html.parser')
data = soup.find_all('div', class_='data')
return data

# Incorrect
data = fetch_data
```

이 예시에서는 누락된 요소로 인해 여러 구문 오류가 발생합니다. 이를 수정하려면 함수 정의에서 `fetch_data` 뒤에 괄호와 콜론을 추가하고, 마지막 줄의 함수 호출 뒤에도 괄호를 추가하십시오.

```python
import requests
from bs4 import BeautifulSoup

# Corrected
def fetch_data():
    response = requests.get('https://example.com')
    soup = BeautifulSoup(response.content, 'html.parser')
    data = soup.find_all('div', class_='data')
    return data

# Corrected
data = fetch_data()

```

함수 정의에서 괄호나 콜론이 누락되면 항상 구문 오류가 발생합니다. 그러나 함수를 호출할 때 괄호(`fetch_data()`)를 잊는 것은 예외를 발생시키지 않을 수 있어, 예기치 않은 동작으로 이어질 수 있습니다.

## 구문 오류를 피하는 방법

오류 없는 코드를 작성하는 것은 연습을 통해 얻는 기술입니다. 다음 모범 사례를 이해하고 구현하면 흔한 구문 오류를 피하는 데 도움이 됩니다.

### Proactive Strategies

구문 오류를 처리하는 가장 좋은 방법은 이를 예방하는 것입니다. 프로젝트를 시작하기 전에, 해당 언어의 가장 흔한 구문 규칙을 숙지하십시오.  

#### 구문 강조 표시 및 들여쓰기 검사 기능이 있는 코드 편집기 사용

좋은 코드 편집기는 구문 강조 표시 및 들여쓰기 검사 같은 기능을 통해 구문 오류를 피하는 데 도움이 됩니다. 이러한 도구는 코드를 실행하기 전에 문제를 발견할 수 있습니다.

예를 들어, 편집기에서 붉은 표시가 나타나면 `if response.status_code == 200`에서 콜론 누락을 의미할 수 있습니다.

![A red mark suggesting there is an error ](https://github.com/bright-kr/python-syntax-errors/blob/main/images/A-red-mark-suggesting-there-is-an-error-1024x525.png)

#### 일관된 코딩 스타일 가이드라인 준수

일관성은 깔끔하고 오류가 적은 코드를 작성하는 핵심입니다. 일관된 스타일을 따르면 가독성이 향상되고 오류를 더 쉽게 찾을 수 있습니다.

Python에서는 [PEP 8 Style Guide](https://peps.python.org/pep-0008/)가 표준이며, 변수 명명, 들여쓰기, 공백 사용에 대한 가이드라인을 제공합니다.

#### 작고 명확하게 정의된 함수로 코드 작성

코드를 작고 명확하게 정의된 함수로 나누면 관리와 디버깅이 개선됩니다. 각 함수는 하나의 명확한 목적을 가져야 합니다. 너무 많은 일을 하는 함수는 이해하고 디버깅하기가 더 어려워집니다.

예를 들어 `scrape_and_analyze()` 함수를 보겠습니다:

```python
import requests
from bs4 import BeautifulSoup
from textblob import TextBlob

def scrape_and_analyze():
    url = "https://example.com/articles"
    response = requests.get(url)
    soup = BeautifulSoup(response.content, "html.parser")
    titles = soup.find_all("h2", class_="article-title")
    sentiments = []
    for title in titles:
    title_text = title.get_text()
    blob = TextBlob(title_text)
    sentiment = blob.sentiment.polarity
    sentiments.append(sentiment)
    return sentiments

print(scrape_and_analyze())

```

이 예시에서는 이 함수를 여러 개의 더 작은 함수로 분해하는 편이 가독성이 더 좋으며, 각각이 더 작고 관리하기 쉬운 코드 조각을 실행하도록 할 수 있습니다:

```python
import requests
from bs4 import BeautifulSoup
from textblob import TextBlob

def scrape_titles(url):
    """Scrape article titles from a given URL."""
    response = requests.get(url)
    soup = BeautifulSoup(response.content, "html.parser")
    titles = soup.find_all("h2", class_="article-title")
    return [title.get_text() for title in titles]

def analyze_sentiment(text):
    """Analyze sentiment of a given text."""
    blob = TextBlob(text)
    return blob.sentiment.polarity

def analyze_titles_sentiment(titles):
    """Analyze sentiment of a list of titles."""
    return [analyze_sentiment(title) for title in titles]

def scrape_and_analyze(url):
    """Scrape titles from a website and analyze their sentiment."""
    titles = scrape_titles(url)
    sentiments = analyze_titles_sentiment(titles)
    return sentiments

url = "https://example.com/articles"
print(scrape_and_analyze(url))
```

### Reactive Strategies

최선을 다하더라도 오류는 여전히 발생할 수 있습니다. Python은 문제와 위치를 설명하는 오류 메시지를 제공합니다.

#### 오류 메시지를 주의 깊게 읽기

Python 오류 메시지는 문제와 그 위치에 대한 세부 정보를 제공합니다. 이를 주의 깊게 분석하면 문제를 식별하고 해결책을 찾는 데 도움이 됩니다.

#### Print 문을 전략적으로 사용

`print()` 문을 사용하는 것은 실행 흐름을 추적하고 작은 프로젝트에서 변수 값을 점검하는 빠른 방법입니다. 빠른 디버깅에는 유용하지만, 보안 및 성능 위험 때문에 프로덕션에서는 사용하지 않아야 합니다.

복잡한 문제나 더 큰 코드베이스에서는 디버거가 더 효과적입니다. 디버거를 사용하면 중단점을 설정하고, 코드를 단계별로 실행하며, 여러 함수 호출에 걸쳐 변수를 검사할 수 있어 더 통제된 디버깅 프로세스를 제공할 수 있습니다.

#### 온라인 리소스와 커뮤니티 활용

해결하기 까다로운 오류에서 막혔다면 도움을 요청하는 것을 주저하지 마십시오. [Python Docs](https://docs.python.org/3/) 및 [Real Python](https://realpython.com/) 같은 온라인 리소스는 유용한 가이드를 제공합니다. [r/Python](https://www.reddit.com/r/Python/), [r/LearnPython](https://www.reddit.com/r/learnpython/), [Stack Overflow](https://stackoverflow.com/questions/tagged/python), 그리고 [Python Forum](https://python-forum.io/) 같은 커뮤니티는 답변과 해결책을 찾기 좋은 곳입니다.

## Conclusion

[신뢰할 수 있는 プロキシ 서비스](https://brightdata.co.kr/proxy-types), 자동화된 데이터 수집, [즉시 사용 가능한 データセット](https://brightdata.co.kr/products/datasets), 또는 [Webスクレイピング 작업 자동화](https://brightdata.co.kr/products/web-scraper)가 필요하든, Bright Data는 Webスクレイピング 프로젝트를 더 효율적이고 생산적으로 만들 수 있는 솔루션을 제공합니다. Python으로 Webスクレイピング을 배우고 싶으십니까? 자세한 [단계별 Python 스크레이핑 가이드](https://brightdata.co.kr/blog/how-tos/web-scraping-with-python)를 읽어보십시오.

지금 가입하여 필요에 맞는 제품을 찾고 오늘 무료 체험을 시작하십시오!