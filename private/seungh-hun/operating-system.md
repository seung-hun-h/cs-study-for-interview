# Operating System
:open_book: Contents
1. [운영체제란?](#-운영체제란)
2. [운영체제 구조](#-운영체제-구조)
  
---

## 운영체제란?
---

운영체제는 사용자에게 편리한 **인터페이스 환경**을 제공하고 **컴퓨터 시스템의 자원을 효율적으로 관리**하는 **소프트웨어**이다.

### 운영체제의 필요성
초기 컴퓨터는 정해진 계산만 수행하면 됐다. 하지만 메모리, CPU의 성능이 향상되고, 여러 작업을 동시에 할 수 있는 컴퓨팅 환경이 조성되면서 사용 규칙이 필요해졌다. 이로 인해 운영체제가 나타났다.

### 운영체제의 역할
1. **자원 관리**
\
운영체제는 한정된 컴퓨팅 자원을 응용 프로그램에게 나누고 회수하며 사용자가 원활하게 작업할 수 있도록 돕는다.

2. **자원 보호**
\
운영체제는 미숙한 사용자나 악의적인 사용자로부터 컴퓨터 자원을 보호한다 

3. **하드웨어 인터페이스 제공**
\
운영체제는 복잡한 과정 없이 다양한 장치를 사용할 수 있도록 해주는 하드웨어 인터페이스를 제공한다. 운영체제는 다양한 장치를 일관된 방법으로 사용할수 있도록 지원하여 사용자는 하드웨어의 구체적인 동작 방식에 대해서 알 필요가 없다. 하드웨어 장치와 상호작용하기 위해 만들어진 컴퓨터 프로그램(하드웨어 인터페이스)을 **드라이버**라고한다.

4. **사용자 인터페이스 제공**
\
사용자 인터페이스는 사용자가 운영체제를 쉽게 사용하도록 지원한다. 운영체제는 사용자에게 GUI를 제공하여 사용자가 쉽게 작업을 수행할 수 있도록 한다.

<p align=middle>
    <img src=https://user-images.githubusercontent.com/60502370/147912120-3f760a8e-18c1-44a4-826f-4da45a33d92d.png width=500>
</p>

## 운영체제 구조
---

### 커널과 인터페이스

운영체제는 커널과 인터페이스로 나눌 수 있다.

<p align=middle>
    <img src=https://user-images.githubusercontent.com/60502370/147912449-12a72bf5-8e90-4ae6-8764-e4182ad1248a.png width=500>
</p>

**커널은** 프로세서 관리, 메모리 관리, 저장장치 관리와 같은 운영체제의 핵심적인 기능을 모아놓은 것을 말한다.

**인터페이스는** 커널에 사용자 명령을 전달하고 실행 결과를 사용자에게 알려주는 역할을 한다.

운영체제는 커널과 인터페이스를 분리하여 같은 커널을 사용하더라도 다른 인터페이스를 가진 형태로 제작할 수 있다. 다른 인터페이스를 사용하면 사용자에게 다른 운영체제처럼 느껴질 수 있다.

### 시스템 호출과 디바이스 드라이버

**시스템 호출**은 커널이 제공하는 시스템 관련 서비스를 모아놓은 것이며 함수 형태로 제공된다. 커널은 컴퓨터 자원을 보호하기 위하여 자원에 직접 접근하는 것을 차단한다. 따라서 자원을 이용하기 위해서는 커널이 제공하는 시스템 호출을 사용해야한다.

```txt
API(Application Programming Interface)

응용 프로그램이 자신과 연관된 프로그램을 만들 수 있도록 제공하는 인터페이스이다. 운영체제의 API를 시스템 호출이라할 수 있다.

SDK(System Developer's Kit)

프로그램 개발자를 위해 API 및 API 사용 매뉴얼, 프로그램 개발에 필요한 코드 편집기 같은 각종 개발용 응용 프로그램까지 묶어서 배포하는 개발 툴을 말한다(ex: android studio).

```

**드라이버**는 커널과 하드웨어 사이의 인터페이스이다. 커널이 모든 하드웨어에 맞는 인터페이스를 모두 개발하기 어렵다. 따라서 커널은 입출력의 기본적인 부분만 제작하고 하드웨어 특성을 반영한 소프트웨어를 하드웨어 제작자에게 받아 커널이 실행될 때 함께 실행하도록한다. 이때 하드웨어 제작자가 만든 소프트웨어를 디바이스 드라이버라고한다.

### 커널의 구성

커널은 운영체제의 핵심 기능을 모아놓은 것이다. 커널이 주로 하는 일은 아래와 같다

- 프로세스 관리
- 메모리 관리
- 파일 시스템 관리
- 입출력 관리
- 프로세스간 통신관리

커널은 이러한 기능을 구현하는 방법에 따라 단일형, 계층형, 마이크로 구조 커널로 구분된다.

- 단일형 구조 커널
  - 커널의 핵심 기능을 구현하는 모듈들이 구분 없이 하나로 구성되어있다.
  - 모듈이 거의 분리되어 있지 않아 모듈간 통신 비용이 적다
  - 모듈이 분리되어있지 않아 버그나 오류를 처리하기 어렵다
  - 상호 의존성이 높아 작은 결함이 시스템 전체로 번질 수 있다

- 계층형 구조 커널
  - 비슷한 기능을 가진 모듈을 묶어서 하나의 계층으로 만들고 계층간 통신을 통해 운영체제를 구현하는 방식이다.
  - 모듈이 계층적으로 분리되어 있어 단일형 보다 오류를 처리하거나 디버깅하기 편리하다

- 마이크로 구조 커널
  - 프로세스 관리, 메모리 관리, 프로세스 간 통신 관리 등 가장 기본적인 기능만 제공하는 커널
  - 다양한 요구를 수용하기 위해 커널이 방대해짐에 따라 나타난 구조
  - 다른 커널에 비해 운영체제의 많은 부분이 사용자 영역에 구현되어 있다.
  - 각 모듈이 독립적으로 작동하기 때문에 한 모듈의 결함이 전체로 퍼지지 않는다.

### 가상 머신
가상머신은 운영체제와 응용 프로그램 사이에서 작동하는 프로그램으로, 가상머신을 설치하면 응용 프로그램이 모두 동일한 환경에서 작동하는 것처럼 보인다. 자바 가상머신(JVM)이 여기에 해당한다.


