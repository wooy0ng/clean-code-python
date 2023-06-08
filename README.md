# clean-code-python

[![Build Status](https://travis-ci.com/zedr/clean-code-python.svg?branch=master)](https://travis-ci.com/zedr/clean-code-python)
[![](https://img.shields.io/badge/python-3.8+-blue.svg)](https://www.python.org/download/releases/3.8.3/)


<br><br>

## Author

[@zedr](https://github.com/zedr), Thank you for making a great document!!!

<br><br>

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
5. [반복은 지양합시다. (Don't repeat yourself; DRY)](#dont-repeat-yourself-dry)
6. [Translations](#translations)

<br><br>

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

**참고하면 좋은 예:**  
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

### short circuiting(회로 연산) 또는 conditionals(조건부) 대신 기본 매개변수(default parameter)를 사용하세요.

여기서 `short circuiting`은 논리 연산(and, or)를 의미합니다.

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

**좋은 예 1**:

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

**좋은 예 2:**

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

매개변수의 수가 많다는 것은 일반적으로 함수가 너무 많은 일을 수행한다는 것을 의미합니다. (has more than one responsibility)

때문에 매개변수의 개수를 제한한다면 함수를 더 쉽게 테스트 할 수 있습니다. 

매개변수가 많은 함수를 매개변수가 적은 함수로 분해할 수 있다면 해보세요. 이상적으로는 3개 미만입니다.

<br>

함수에 단일 책임(single responsibility)이 있는 경우 여러 개의 매개변수를 하나의 특수한 개체로 묶을 수 있는지도 살펴보세요.

프로그램에서 다른 곳에 매개변수를 재사용해야 하는 상황이 온다면 이 개체를 요긴하게 사용할 수 있습니다.

<br>

또한 이 방법이 여러 개의 매개변수를 갖는 것 보다 더 나은 이유는

함수 내부의 매개변수를 사용하여 수행되는 연산들을 또 하나의 함수로 만들어 복잡성을 줄일 수 있기 때문입니다.


<br>

**나쁜 예:**

```python
def create_menu(title, body, button_text, cancellable):
    pass
```

<br>

**java-esque (자바 표현법)**:

```python
class Menu:
    def __init__(self, config: dict):
        self.title = config["title"]
        self.body = config["body"]
        # ...


menu = Menu(
    {
        "title": "My Menu",
        "body": "Something about my menu",
        "button_text": "OK",
        "cancellable": False
    }
)
```

<br>

**좋은 예 1:**

```python
class MenuConfig:
    """A configuration for the Menu.

    Attributes:
        title: The title of the Menu.
        body: The body of the Menu.
        button_text: The text for the button label.
        cancellable: Can it be cancelled?
    """
    title: str
    body: str
    button_text: str
    cancellable: bool = False


def create_menu(config: MenuConfig) -> None:
    title = config.title
    body = config.body
    # ...


config = MenuConfig()
config.title = "My delicious menu"
config.body = "A description of the various items on the menu"
config.button_text = "Order now!"
# The instance attribute overrides the default class attribute.
config.cancellable = True

create_menu(config)
```

<br>

**좋은 예 2:**

```python
from typing import NamedTuple


class MenuConfig(NamedTuple):
    """A configuration for the Menu.

    Attributes:
        title: The title of the Menu.
        body: The body of the Menu.
        button_text: The text for the button label.
        cancellable: Can it be cancelled?
    """
    title: str
    body: str
    button_text: str
    cancellable: bool = False


def create_menu(config: MenuConfig):
    title, body, button_text, cancellable = config
    # ...


create_menu(
    MenuConfig(
        title="My delicious menu",
        body="A description of the various items on the menu",
        button_text="Order now!"
    )
)
```

<br>

**좋은 예 3:**

```python
from dataclasses import astuple, dataclass


@dataclass
class MenuConfig:
    """A configuration for the Menu.

    Attributes:
        title: The title of the Menu.
        body: The body of the Menu.
        button_text: The text for the button label.
        cancellable: Can it be cancelled?
    """
    title: str
    body: str
    button_text: str
    cancellable: bool = False


def create_menu(config: MenuConfig):
    title, body, button_text, cancellable = astuple(config)
    # ...


create_menu(
    MenuConfig(
        title="My delicious menu",
        body="A description of the various items on the menu",
        button_text="Order now!"
    )
)
```

<br>

**좋은 예 4 (Python3.8+ only)**

```python
from typing import TypedDict


class MenuConfig(TypedDict):
    """A configuration for the Menu.

    Attributes:
        title: The title of the Menu.
        body: The body of the Menu.
        button_text: The text for the button label.
        cancellable: Can it be cancelled?
    """
    title: str
    body: str
    button_text: str
    cancellable: bool


def create_menu(config: MenuConfig):
    title = config["title"]
    # ...


create_menu(
    # You need to supply all the parameters
    MenuConfig(
        title="My delicious menu",
        body="A description of the various items on the menu",
        button_text="Order now!",
        cancellable=True
    )
)
```

**[⬆ 목차로 이동](#목차)**

<br><br>


### 함수의 이름은 함수가 수행하는 작업을 나타내야 합니다.

<br>

**나쁜 예:**

```python
class Email:
    def handle(self) -> None:
        pass


message = Email()
# What is this supposed to do again?
message.handle()
```

<br>

**좋은 예:**

```python
class Email:
    def send(self) -> None:
        """Send this message"""


message = Email()
message.send()
```

**[⬆ 목차로 이동](#목차)**

<br><br>

### 함수에는 추상화(abstraction)가 한 층만 있어야 합니다.

만약 함수에 추상적인 층이 하나 이상 있다면, 함수가 너무 복잡해집니다.

추상층이 여러 개 있다면, 그것들을 함수로 분해하여 재사용성을 높이고 테스트에 용이하도록 하는 것이 좋습니다.

<br>

**나쁜 예:**

```python
# type: ignore

def parse_better_js_alternative(code: str) -> None:
    regexes = [
        # ...
    ]

    statements = code.split('\n')
    tokens = []
    for regex in regexes:
        for statement in statements:
            pass

    ast = []
    for token in tokens:
        pass

    for node in ast:
        pass
```

<br>

**좋은 예:**

```python
from typing import Tuple, List, Dict

REGEXES: Tuple = (
    # ...
)


def parse_better_js_alternative(code: str) -> None:
    tokens: List = tokenize(code)
    syntax_tree: List = parse(tokens)

    for node in syntax_tree:
        pass


def tokenize(code: str) -> List:
    statements = code.split()
    tokens: List[Dict] = []
    for regex in REGEXES:
        for statement in statements:
            pass

    return tokens


def parse(tokens: List) -> List:
    syntax_tree: List[Dict] = []
    for token in tokens:
        pass

    return syntax_tree
```

**[⬆ 목차로 이동](#목차)**

<br><br>

### 함수의 매개변수로 flag를 사용하지 마세요.

flag는 사용자로 하여금 이 함수가 두가지 이상의 기능을 수행한다는 것으로 보여질 수 있습니다.

함수는 한가지 일을 해야합니다. bool을 기준으로 함수의 기능이 완전히 바뀐다면 함수를 분할해보세요.

<br>

**나쁜 예:**

```python
from tempfile import gettempdir
from pathlib import Path


def create_file(name: str, temp: bool) -> None:
    if temp:
        (Path(gettempdir()) / name).touch()
    else:
        Path(name).touch()
```

<br>

**좋은 예:**

```python
from tempfile import gettempdir
from pathlib import Path


def create_file(name: str) -> None:
    Path(name).touch()


def create_temp_file(name: str) -> None:
    (Path(gettempdir()) / name).touch()
```

**[⬆ 목차로 이동](#목차)**

<br><br>

### 함수는 부작용(side effect)을 피해야 합니다. 

여기서 말하는 부작용(side effect)은 부정적인 의미가 아닙니다. 

함수는 일반적으로 매개변수를 받은 후 일련의 작업을 거쳐 값을 반환합니다. 

만약 값을 반환하는 것 이외에 다른 작업을 추가로 수행하는 경우 이 행위를 부작용이라 부릅니다.

<br>

예를 들어 부작용으로 파일에 글을 쓸 수도 있으며, 파일의 특정 변수를 수정할 수도 있고, 실수로 모든 돈을 낯선 사람에게 송금할 수도 있습니다.

만약 부작용을 꼭 필요로 한다면, 부작용이 유발되는 위치를 표시해주는 것이 좋습니다. 

또한 다른 함수나 클래스가 동시에 동일한 파일을 조작하지 않도록 하고 특정 함수를 통해 파일을 이 파일을 조작하도록 합시다.

<br>

주요 요점은 개체 간 상태 공유, 가변 데이터 등을 사용하여 모든 함수 또는 변수가 이러한 데이터(파일 혹은 파일 내 데이터)를 조작할 수 있게 되는 일반적인 함정은 피할 필요가 있습니다. 

만약 이것을 잘 지킨다면, 다른 프로그래머들보다 오류를 찾기 더 수월해질 것입니다.

<br>

**나쁜 예:**

```python
# type: ignore

# This is a module-level name.
# It's good practice to define these as immutable values, such as a string.
# However...
fullname = "Ryan McDermott"


def split_into_first_and_last_name() -> None:
    # The use of the global keyword here is changing the meaning of the
    # the following line. This function is now mutating the module-level
    # state and introducing a side-effect!
    global fullname
    fullname = fullname.split()


split_into_first_and_last_name()

# MyPy will spot the problem, complaining about 'Incompatible types in
# assignment: (expression has type "List[str]", variable has type "str")'
print(fullname)  # ["Ryan", "McDermott"]

# OK. It worked the first time, but what will happen if we call the
# function again?
```

<br>

**좋은 예 1:**

```python
from typing import List, AnyStr


def split_into_first_and_last_name(name: AnyStr) -> List[AnyStr]:
    return name.split()


fullname = "Ryan McDermott"
name, surname = split_into_first_and_last_name(fullname)

print(name, surname)  # => Ryan McDermott
```

<br>

**좋은 예 2:**

```python
from dataclasses import dataclass


@dataclass
class Person:
    name: str

    @property
    def name_as_first_and_last(self) -> list:
        return self.name.split()


# The reason why we create instances of classes is to manage state!
person = Person("Ryan McDermott")
print(person.name)  # => "Ryan McDermott"
print(person.name_as_first_and_last)  # => ["Ryan", "McDermott"]
```

**[⬆ 목차로 이동](#목차)**

<br><br>