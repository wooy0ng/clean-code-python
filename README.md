# clean-code-python

[![Build Status](https://travis-ci.com/zedr/clean-code-python.svg?branch=master)](https://travis-ci.com/zedr/clean-code-python)
[![](https://img.shields.io/badge/python-3.8+-blue.svg)](https://www.python.org/download/releases/3.8.3/)


<br>

## Author

[@zedr](https://github.com/zedr), Thank you for making a great document!!!

<br>

## 목차

1. [소개](#introduction)
2. [변수](#variables)
3. [함수](#functions)
4. [클래스 (객체지향 5원칙)](#classes)
    1. [S: 단일 책임 원칙 (Single Responsibility Principle; SRP)](#single-responsibility-principle-srp)
    2. [O: 개방/폐쇄 원칙 (Open/Closed Principle; OCP)](#openclosed-principle-ocp)
    3. [L: 리스코프 치환 원칙 (Liskov Substitution Principle; LSP)](#liskov-substitution-principle-lsp)
    4. [I: 인터페이스 분리 원칙 (Interface Segregation Principle; ISP)](#interface-segregation-principle-isp)
    5. [D: 의존성 역전 원리 (Dependency Inversion Principle; DIP)](#dependency-inversion-principle-dip)
5. [반복은 지양합시다! (Don't repeat yourself; DRY)](#dont-repeat-yourself-dry)
6. [Translations](#translations)

<br>

## 소개

Robert C. Martin의 책, [*Clean
Code*](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882)을 참고하였습니다.

<br>

이 문서는 Python에 맞게 수정되었으며 style guide가 아닙니다. 

이 문서는 Python으로 읽을 수 있고(readable) 재사용 가능하며(reusable), 리펙토링 가능한(refactorable) 소프트웨어를 만들어 내기 위한 가이드라인을 제시합니다.

<br>

이 문서의 모든 것을 완전히 따를 필요는 없으며, 각 구성원 간 보편적 합의에 따라가면 됩니다.

다시 말하지만 이 문서에서 언급하는 것들은 모두 지침일 뿐입니다.   
다만, *Clean Code*의 저자들에 의해 수년간의 경험에 의해 정립된 것들입니다.

<br>

[clean-code-javascript](https://github.com/ryanmcdermott/clean-code-javascript)의 문서를 `Python 3.7+` 버전에 맞게 수정하였습니다.


<br><br>

## **변수**

### 변수 이름은 의미가 있어야 하며, 발음할 수 있어야 합니다. 
(meaningful, pronounceable)


<br>

**나쁜 예:**

```python
import datetime

ymdstr = datetime.date.today().strftime("%y-%m-%d")
```

<br>

**좋은 예:**

```python
import datetime

current_date: str = datetime.date.today().strftime("%y-%m-%d")
```

**[⬆ 목차로 이동](#목차)**

<br><br>

### 동일 대상(entity)의 변수에 대해서는 동일 어휘를 사용합시다.

<br>

**나쁜 예:**
아래 예제는 동일한 동일한 entity에 대해 3개의 다른 이름을 사용합니다.

```python
def get_user_info(): pass


def get_client_data(): pass


def get_customer_record(): pass
```

<br>

**좋은 예:**  
만약 entity가 동일하다면, 일관성 있게(consistent) 변수나 함수의 이름을 짓는 것이 좋습니다.

```python
def get_user_info(): pass


def get_user_data(): pass


def get_user_record(): pass
```

<br>

**참고하면 더 좋은 예:**  
Python은 객체 지향 프로그래밍 언어입니다. 필요한 경우 인스턴스의 속성(attribute), 프로퍼티 메소드(property method)나 메소드(method)와 함께 코드에서 entity의 구체적인 구현 및 패키지화하는 것이 좋습니다.

```python
from typing import Union, Dict


class Record:
    pass


class User:
    info: str

    @property
    def data(self) -> Dict[str, str]:
        return {}

    def get_record(self) -> Union[Record, None]:
        return Record()
```

**[⬆ 목차로 이동](#목차)**


<br><br>

### 검색에 용이한 이름을 사용합시다.
우리는 코딩을 하며 많은 코드를 읽습니다. 때문에 우리가 작성하는 코드를 읽기 쉽고 검색 가능한 이름으로 선언하는 것은 중요합니다. 

만약 변수를 선언할 때 의미가 없거나 검색에 어려움을 주는 이름으로 선언한다면, 우리의 코드를 읽는 다른 사람들이 힘들어할 것입니다.

검색 가능한(유추 가능한) 이름을 사용합시다.

<br>

**나쁜 예:**

```python
import time

# What is the number 86400 for again?
time.sleep(86400)
```

<br>

**좋은 예:**

```python
import time

# Declare them in the global namespace for the module.
SECONDS_IN_A_DAY = 60 * 60 * 24
time.sleep(SECONDS_IN_A_DAY)
```

**[⬆ 목차로 이동](#목차)**

<br><br>

### 변수는 독립적이어야 합니다. 
(explanatory)

<br>

**나쁜 예:**

```python
import re

address = "One Infinite Loop, Cupertino 95014"
city_zip_code_regex = r"^[^,\\]+[,\\\s]+(.+?)\s*(\d{5})?$"

matches = re.match(city_zip_code_regex, address)
if matches:
    print(f"{matches[1]}: {matches[2]}")
```

<br>

**나쁘지는 않은 예:**

나쁘지는 않지만, 여전히 regex의 결과에 의존하고 있습니다.

```python
import re

address = "One Infinite Loop, Cupertino 95014"
city_zip_code_regex = r"^[^,\\]+[,\\\s]+(.+?)\s*(\d{5})?$"

matches = re.match(city_zip_code_regex, address)
if matches:
    city, zip_code = matches.groups()
    print(f"{city}: {zip_code}")
```

<br>

**좋은 예:**

하위 패턴의 이름을 지정함으로써 regex 결과에 대한 의존성을 줄일 수 있습니다.

```python
import re

address = "One Infinite Loop, Cupertino 95014"
city_zip_code_regex = r"^[^,\\]+[,\\\s]+(?P<city>.+?)\s*(?P<zip_code>\d{5})?$"

matches = re.match(city_zip_code_regex, address)
if matches:
    print(f"{matches['city']}, {matches['zip_code']}")
```

**[⬆ 목차로 이동](#목차)**

<br><br>

### 읽는 사람으로 하여금 기능을 유추하도록 만드는 이름을 짓지 마세요.
변수가 의미하는 바가 무엇인지를 코드를 상세히 보지 않아도 알 수 있도록 하세요.

명시적인 것이 암묵적인 것보다 좋습니다.

<br>

**나쁜 예:**

```python
seq = ("Austin", "New York", "San Francisco")

for item in seq:
    # do_stuff()
    # do_some_other_stuff()

    # Wait, what's `item` again?
    print(item)
```

<br>

**좋은 예:**

```python
locations = ("Austin", "New York", "San Francisco")

for location in locations:
    # do_stuff()
    # do_some_other_stuff()
    # ...
    print(location)
```

**[⬆ 목차로 이동](#목차)**

<br><br>

### 불필요한 context는 추가하지 마세요.

클래스/객체 이름이 무언가를 이미 알려주는 경우, 변수 이름에서 이를 반복하지 마세요.

<br>

**나쁜 예:**

```python
class Car:
    car_make: str
    car_model: str
    car_color: str
```

<br>

**좋은 예**:

```python
class Car:
    make: str
    model: str
    color: str
```

**[⬆ 목차로 이동](#목차)**

<br><br>

### short circuiting(And, Or) 또는 conditionals 대신 기본 매개변수를 사용하세요.

<br>

**다음과 같은 상황에서**:

```python
import hashlib


def create_micro_brewery(name):
    name = "Hipster Brew Co." if name is None else name
    slug = hashlib.sha1(name.encode()).hexdigest()
    # etc.
```

<br>

만약 위와 같이 조건문을 사용하는 것 대신 매개변수만을 사용하더라도 함수의 동작에 아무런 영향이 없다는 것을 알 수 있습니다.

우리는 위 코드를 아래와 같이 수정하고 싶을 것입니다.

<br>

**좋은 예**:

```python
import hashlib


def create_micro_brewery(name: str = "Hipster Brew Co."):
    slug = hashlib.sha1(name.encode()).hexdigest()
    # etc.
```

**[⬆ 목차로 이동](#목차)**

<br><br>

## **함수**

### 함수는 작업의 단위입니다.

함수는 소프트웨어 엔지니어링에서 가장 중요한 rule 중 하나입니다.

함수들이 하나 이상의 작업을 수행한다면 관리, 테스트 및 추론에 어려움을 겪을 것입니다.

<br>

함수를 하나의 작업으로 분리한다면, 리펙토링(refactoring)이 쉬워지고 코드를 훨씬 깨끗하게 만들 수 있습니다.

만약 이 rule를 숙지하고 실천한다면, 여러분은 많은 개발자들을 앞서게 될 것입니다.

<br><br>

**나쁜 예:**

```python
from typing import List


class Client:
    active: bool


def email(client: Client) -> None:
    pass


def email_clients(clients: List[Client]) -> None:
    """Filter active clients and send them an email.
    """
    for client in clients:
        if client.active:
            email(client)
```

<br><br>

**좋은 예**:

```python
from typing import List


class Client:
    active: bool


def email(client: Client) -> None:
    pass


def get_active_clients(clients: List[Client]) -> List[Client]:
    """Filter active clients.
    """
    return [client for client in clients if client.active]


def email_clients(clients: List[Client]) -> None:
    """Send an email to a given list of clients.
    """
    for client in get_active_clients(clients):
        email(client)
```

<br>

위 코드에서 generator를 사용할 수 있는 부분이 보이시나요?

<br><br>

**더 좋은 예:**

```python
from typing import Generator, Iterator


class Client:
    active: bool


def email(client: Client):
    pass


def active_clients(clients: Iterator[Client]) -> Generator[Client, None, None]:
    """Only active clients"""
    return (client for client in clients if client.active)


def email_client(clients: Iterator[Client]) -> None:
    """Send an email to a given list of clients.
    """
    for client in active_clients(clients):
        email(client)
```

**[⬆ 목차로 이동](#목차)**

<br><br>

### 함수의 매개변수 (이상적으로 2개 이하)