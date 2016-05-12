---
layout: default
title: "Essential Lisp in Twelve Lessons"
permalink: /external/successful_lisp_ch03_kor/

---

- latest update: 20160513
- 원문: https://www.psg.com/~dlamkins/sl/chapter03.html
- 주의: 미숙한 번역이라 오역이 난무할지도 모릅니다.


# Chapter 3 - Essential Lisp in Twelve Lessons

 이번 장은 여러분에게 Lisp를 시작하기에 앞서 알아야 할 모든것을 가르칠 것입니다. 저는 언어의 코어 기능을 모두 다루었습니다. 저는 여러분이 이 코어를 Lisp 자체라 생각하고, 그렇지 않은 것은 하나의 거대한 표준 라이브러리라 생각하셨으면 합니다. 이러한 배경을 바탕으로, 책을 마져 읽고 Common Lisp : The Language, 2nd Edition과 같은 참조 메뉴얼을 통해, Lisp의 남은 부분들을 배우기가 더욱 편해질것입니다.

 여러분은 이번 장을 쭉 읽어야 합니다. 이따금 다음 단락이나 다음 장을을 언급하겠지만, 이번장을 이해하기 위해서 반드시 그러한 참고들을 따를 필요는 없습니다. 이번 장을 끝냈을때, Lisp 시스템의 키보드에 앉아있으면서 여러분은 4장을 진행하고 있을것입니다.

* Lesson 1 [p 39] - Essential Syntax
* Lesson 2 [p 41] - Essential Evaluation
* Lesson 3 [p 46] - Some Examples of Special Forms and Macros
* Lesson 4 [p 51] - Putting things together, and taking them apart
* Lesson 5 [p 53] - Naming and Identity
* Lesson 6 [p 56] - Binding versus Assignment
* Lesson 7 [p 59] - Essential Function Definition
* Lesson 8 [p 61] - Essential Macro Definition
* Lesson 9 [p 63] - Essential Multiple Values
* Lesson 10 [p 65] - A Preview of Other Data Type
* Lesson 11 [p 77] - Essential Input and Output
* Lesson 12 [p 82] - Essential Reader Macros


## Lesson 1 - Essential Syntax

### `list`는 괄호로 감싸져있다.
 이것이 Lisp에 대해 여러분이 우선적으로 알아할 것입니다: 괄호로 둘러싸인 모든것은 list이다. 여기 예가 나와있습니다:

```lisp
(1 2 3 4 5)
(a b c)
(cat 77 dog 89)
```

 말씀드린대로, 괄호로 둘러싸인 것은 `list`입니다. 이와 같은 문장을 들었을 때, 여러분은 아마도 다음과 같은 질문을 하고자 할 것입니다:

1. 아무것도 없는것을 괄호로 둘러싼다면?
2. 다른 list를 괄호로 둘러싼다면?

 두 경우 모두 대답은 같습니다. 여전히 `list`입니다. 따라서 다음 나오는 것들 또한 list입니다:

```lisp
()
(())
((()))
((a b c))
((1 2) 3 4)
(mouse (monitor 512 342) (keyboard US))
(defun factorial (x) (if (eql x 0) 1 (* x (factorial (- x 1)))))
```

 list가 아닌 유일한 경우는 다음 4개의 예제에 나온 것처럼 좌측 괄호와 매칭하는 우측괄호가 없거나 역인 경우입니다:

```lisp
(a b c(
((25 g) 34
((())
(()))
```

 이것은 크게 신경쓸 부분이 아닙니다 - Lisp는 괄호가 일치하지 않는게 있으면 여러분에게 알려 줄 것입니다. 또한 Lisp 프로그램을 작성하기 위해 사용하는 편집기는, 대부분 자동적으로 일치하는 괄호를 찾는 방법을 제공해 줄 것입니다. 27장에서 편집기를 살펴볼 수 있습니다 [p 227].

 list는 Lisp에서 많은 것이 될 수 있습니다. 가장 일반적인 경우, list는 프로그램이나 데이터가 될 수 있습니다. 그리고 list가 스스로 다른 list를 만들수 있기에, 여러분은 재멋대로된 데이터 조합과 다른 레벨의 list구조로된 프로그램을 지닐 수 있습니다. 이것이 이를 잘 이해하는 자에게 Lisp가 매우 유연함을 주는 것이며, 그렇지 않은 자에게 많은 혼란을 주는 것입니다. 저희는 이번장을 계속해서 나아가면서 이러한 혼란을 없에도록 노력할 것입니다.


### Atom은 공백이나 괄호로 분리된 것입니다

 이제 여러분은 list를 인지할 수 있으며, 괄호 사이에 나타나지만(list 가 아닌 것) 단어나 숫자인 것들에 대한 이름을 알고자 할 것입니다. 이러한 것들을 `atom`이라 합니다.

 그러므로, 다음 단어와 숫자들은 모두 atom입니다:

```lisp
1
25
342
mouse
factorial
x
```

 Lisp는 거의 모든 문자를 atom을 구성하는데 이용하는 것을 허용합니다. 이제, 이 이전과 이후에 공백이나 괄호가 있으면(이는 시작과 끝라인을 포함합니다), 여러분은 어떠한 문자들, 숫자들, 마침표의 sequence가 atom이라 말할 수 있습니다. 다음 나오는 것들은 모두 atom입니다:

```lisp
-
*
@comport
funny%stuff
9^
case-2
```

 여러분이 다른 프로그래밍에 대한 경험이 있다면, 한가지 여러분이 기억해야 할 것은, 일반적으로 연산자로 예약된 문자들이 Lisp atom으로 보여질 때에는 특별한 의미를 지니지 않는다는 것입니다. 예를들어, case-2는 atom이며, 수학arithmetic 표현식이 아닙니다.

 atom이 공백이나 괄호로 구분할수 있기에, atom과 괄호 사이나 두 괄호 사이에 어떠한 공백도 소거시킬 수 있습니다. 그러므로, 다음 두 예제는 동일한 것입니다:

```lisp
(defun factorial (x) (if (eql x 0) 1 (* x (factorial (- x 1)))))
(defun factorial(x)(if(eql x 0)1(* x(factorial(- x 1)))))
```

 실제로, 여러분은 절대로 두번째 list를 작성해서는 안됩니다. 실제로, 여러분은 가독성을 향상시키기 위해 list를 적절하게 다수의 라인으로 분리시키고 각 라인마다 들여쓰기를 해야합니다. 이 list는 사실 작은 프로그램이며, 다음과 같이 들여쓰면 Lisper 프로그래머가 읽기 수월해 집니다:

```lisp
(defun factorial (x)
  (if (eql x 0)
    1
    (* x (factorial (- x 1)))))
```

 지금, 여러분은 이것이 무얼의미하는지, 이러한 종류의 indentation을 어떻게 알아야하는지에 대해 걱정할 필요가 없습니다. 이번장을 통해, 여러분은 많은 indentation의 예제를 보게될 것입니다.


 Subsequent 장에서 추가 예제를 보여드릴 것이며, 많은 새로운 구조에 대한 가독성을 향상시키기 위한 indentation의 사용법을 알려드릴 것입니다. 28장 [p 230]에서 적절한 indentation의 사용을 포함하여, Lisp의 스타일에 대해 다룰 것입니다.



## Lesson 2 - Essential Evaluation

### A form is meant to be evaluated

 `form`은 `atom`이나 `list`가 될 수 있습니다. 중요한 것은 form이 평가된다는 것입니다. 평가evaluation는 이번 section에서 서서히 나타날 상당한 기술적 정의입니다.

 form이 atom이라면 평가는 단순합니다. Lisp는 atom을 이름처럼 다루고, (값이 존재한다면)이름에 대한 값을 받습니다. 여러분은 아마도 왜 제가 atom이라 불리는 변수에 대한 더욱 자세한 설명을 피하는지에 대해 혼란스러울 것입니다. 이유는 atom은 변수 값이나 상수 값을 지닐 수 있기 때문입니다. 그리고 atom의 값은 두가지 경우에 대해 상수가 될 수 있습니다.

 숫자는 atom입니다(이 값은 명백한 이유로 상수입니다). Lisp는 숫자에 값을 저장 할 수 없습니다 - 숫자는 setf-evaluation이라 부릅니다.

 저희는 완벽한 정의없이 새로운 용어를 도입할 것입니다. 이제, symbol을 값을 가질수 있는 atom이라고 여기기 바랍니다. Lesson 5[p 53]에서 symbol에 대해 더욱 자세히 살펴볼 것입니다.

 defconstant에서 정의된 symbol은 상수 값을 지닙니다. Lisp는 atom이 변수 값을 지녔을때 값을 저장할 것이며, 그리고 값을 바꿀 수 없다는 기록을 추가할 것입니다.

 KEYWORD package에 있는 symbol은 self-evaluation입니다. package에대해 31장[p 247]에서 자세히 살펴볼 것입니다. 지금, 여러분이 알아야 할것은 (package prefix 라 불리는) `:` 문자로 시작하는 symbol은 keyword symbol이라는 것입니다. keyword symbol은 그 자신을 값으로 갖습니다.

 symbol은 다양한 방식으로 변수 값을 얻을 수 있습니다. Lisp는 실제로 symbol에 대한 다양한 값을 지닙니다. 하나는 변수처럼 다뤄지는 symbol의 값이라는 전통적인 의미를 지닙니다. 다른 하나는 symbol의 함수라는 의미를 지닙니다. 다른 것들은 the symbol’s documentation, its printed representation, symbol과 연관짓기 위해 프로그래머가 선택한 properties를 지닙니다. 이러한 것들에 대해 Lesson 5 [p 53], Lesson 6 [p 56], Lesson 7 [p 59]에서 더욱 자세히 살펴볼 것입니다.

 form이 list라면, 첫번째 요소는 symbol이거나 lambda 표현식이라 불리는 특별한 form일 것입니다.(lambda 표현식을 잠시동안 살펴보지 않겠습니다.) symbol은 함수의 이름이여야 합니다. Lisp에서 symbol +, -, *, / 이름은 일반적인 산술 연산자입니다: 덧셈, 빨셈, 곱셈, 나누기. 이러한 각 symbol은 산술 연산을 수행하는 함수와 관계되어있습니다.

 따라서 Lisp가 form (+ 2 3)을 평가하면, 이는 덧셈 함수에 인자 2와 3을 적용하여, 예상되는 결과 5를 줄것입니다. 함수 symbol +이 인자들 앞에 있습니다. 이는 prefix notation입니다. list를 볼때마다, Lisp가 list를 form으로 평가하기 위해 무얼할 것인지 이해하기 위해선 첫번째 요소를 살펴보시기 바랍니다.


### A function is applied to its arguments

 주어진 list를 평가할때 Lisp는 form을 함수 호출로 다룹니다. 지금부터 저희는 수많은 Lisp 평가를 보게될 것이며, Lisp의 input과 이의 반응을 구분하기 위해 시각적 지원aids를 이용할 것입니다:

    @  Lisp의 input 앞에 있는 Lisp prompt
    >  Lisp 평가의 결과

 예:

```lisp
@  (+ 4 9)
>  13
@  (- 5 7)
>  -2
@  (* 3 9)
>  27
@  (/ 15.0 2)
>  7.5
```

 위의 각 경우에, 평가된 form은 list입니다. 이의 첫번째 요소는 함수라는 symbol입니다. 남아있는 요소는 함수의 인자argument입니다. 여기서, 인자는 모두 숫자이며, 저희는 숫자가 self-evaluating이란것을 알고 있습니다.

 여기 몇몇 예제가 더 있습니다:

```lisp
@  (atom 123)
>  T
@  (numberp 123)
>  T
@  (atom :foo)
>  T
@  (numberp :foo)
>  NIL
```

 ATOM과 NUMBERP는 predicate(술부)입니다. Predicates 는 true나 false 값을 반환합니다. NIL은 Lisp에서 유일한 false 값입니다. -- 아닌 모든 것은 true. predicate가 유용한 값을 반환하지 않는한, 관습적으로 true를 의미하는 T를 반환합니다. 인자가 Lisp atom이라면 ATOM은 T를 반환합니다. 인자가 숫자이면 NUMBERP는 T를 반환합니다.

 위의 form들을 각각 평가하기 위해, Lisp는 우선 인자를 (좌측에서 우측으로) 평가하고, 그런 다음 함수를 얻기 위해 첫번째 요소를 평가하고나서, 인자에 함수를 적용합니다. 몇몇 예외가 있으며, 이번 lesson의 끝에서 이에 대해 배울 것이며, Lisp는 list form을 평가하기 위해 항상 동일한 것을 수행합니다:
1.  좌측에서 우측으로 인자를 평가한다.
2.  첫번째 요소와 관계된 함수를 얻는다.
3.  함수를 인자에 적용한다.

 atom도 또한 Lisp form이라는 것을 명심하시기 바랍니다. 주어진 atom이 평가되면, Lisp는 단순히 이의 값을 반환합니다:

```lisp
@  17.95
>  17.95
@  :A-KEYWORD
>  :A-KEYWORD
@  *FEATURES*
>  (:ANSI-CL :CLOS :COMMON-LISP)
@  "Hello, world!"
>  "Hello, world!"
@  WHAT-IS-THIS?
|  Error: Unbound variable
```

 숫자와 keyword는 self-evaluating입니다. 문자열도 그러합니다. `*FEATURES*` 변수는 Lisp에 의해 미리정의된 것입니다. 여러분의 system은 아마도 다른 값을 반환할 것입니다.

 symbol WHAT-IS-THIS?는 Lisp에 의해 미리 정의되지 않았으므로, 값을 지니지 않으며, 이의 값을 줄 수 없습니다. 시스템은 값대신 에러메시지로 응답할 것입니다. 저희는 이러한 메시지를 성공적인 평가를 위해 사용하는 > 표지가 아니라, |로 표시하였습니다. 여러분의 system은 아마도 다른 메시지를 출력할 것입니다.


### A function can return any number of values
 종종 여러분은 다수의 값을 반환하는 함수를 지니기를 원합니다. 예를들어, 데이터베이스 전체를 살펴보는 함수는 요구하는 값과 완료상태코드를 동시에 반환해야 합니다. 이를 행할 방법은 함수를  하나의 결과를 위한 위치에 넣는 것입니다; 이는 가능하지만, Lisp 프로그램에 매우 드믄 일입니다.

 또 다른 접근법은 결과와 상태코드를 묶어 하나의 반환 값을 생성하는 것입니다.
Lisp는 구조체[p 72]를 포함하여 여러분에게 이를 수행할 다양한 방식을 제공합니다. 숙련된 Lisp 프로그래머는, 생성된 값이 이의 구성물과 멀어저 잊혀지게될때, 값을 구성하고나선 잠재적으로 프로그램의 작동을 느리게 하는 쓰래기garbage가 되기에 (29장 [p 238] 참조) 이를 하지 않습니다

 함수에서 다수의 값을 반환하는 올바른 법은 VALUES form을 이용하는 것입니다. 함수 context에서 VALUES의 사용을 [p 63]에서 조금 살펴볼 것입니다. 지금은, Lisp가 VALUES form을 평가할때 무슨 일이 벌어지는지 살펴봅시다:

```lisp
@  (values 1 2 3 :hi "Hello")
>  1
>  2
>  3
>  :HI
>  "Hello"
```

 어떻게 Lisp가 VALUES form에서 각 인자에 대한 값을 반환하는지(> 지시자 다음에 오는) 주목하시기 바랍니다. 제 Lisp 시스템은 이를 새로운 라인마다 각 값을 출력함으로써 나타냈습니다; 여러분의 것은 아마 다른 방식으로 값을 분리시킬 것입니다.


### Arguments are usually not modified by a function

 장소에 함수를 통과시킬 수 있으며, 함수가 그 장소의 값을 변화 시킬 수 있다고, 이전에 언급하였습니다. 비록 다른 언어들이 이를 표준 repertoire의 일부로 만들지라도,  Lisp 프로그램에서 이는 매우 평범치 않은 일입니다.

 여러분은 non-keyword symbol이나 composite value처럼 수정될 장소을 지정할 수 있습니다 - 상수는 수정할 수 없습니다. symbol을 제공provide하면, 함수는 symbol에 새로운 값을 주는 코드를 실행할 것입니다. composite data 구조를 제공하면, 함수는 적절한 composite의 값을 바꾸는 코드를 실행할 것입니다. 이러한 Lisp 코드를 작성하는 것이 어렵기에,이러한 방식으로 쓰여진 프로그램을 이해하는 것도 어렵습니다. 따라서 Lisp 프로그래머는 주로 인자로부터 입력을 받는 함수를 작성하며, 함수를 결과로 하여 출력물을 생성합니다.


### Arguments are usually evaluated before function application
 Lisp가 함수를 평가하면, 이전에 봤던것처럼[p 42], 이는 항상 모든 인자를 우선적으로 평가합니다. 불행히도, 모든 규칙에는 예외가 있으며, (곧 보게될 것처럼)이 규칙도 예외는 아닙니다... 문제는 Lisp가 항상 함수의 인자를 평가하지 않는것이 아니라, 모든 list form은 함수 호출이 아니라는 것입니다.

### Arguments are evaluated in left-to-right order
 list form이 함수 호출하면, 이의 인자는 항상 좌측에서 우측으로 순서대로 평가됩니다. 다른 프로그래밍 언어처럼, 이에 의존하는 것은 볼품없지만, 전적으로 이러한 순서에 의존한다면,  Lisp가 정의한 것을 알기에 좋을 것입니다.


### Special forms and macros change argument evaluation
 그러면 list form이 항상 함수 호출이 아니라면, 무엇이 될 수 있을까요?
2가지 경우가 있지만, 결과는 같습니다: 몇몇 인자가 평가되며 몇몇은 평가가 안됩니다. 각각은 form과 form이 아닌 것에 달려있습니다. 여러분은 예외를 배울 것입니다. 다행스럽게도, 대다수의 Lisp 시스템은 하나 혹은 두개의 keystrok로 어떠한 form에 대한 온라인 문서를 여러분에게 보여줄 것입니다.

 모든 인자가 평가되지 않는 2가지 종류의 form이 있습니다: special form과 macro. Lisp는 적은 수의 special form을 미리 정의하였습니다. 여러분은 여러분만의 special form을 추가할 수 없습니다 - 언어 자체의 주요기능입니다. Lisp는 또한 매우 적은수의 macro를 정의하였습니다. 여러분은 여러분의 macro를 정의할 수 있습니다. Lisp의 macro는 언어의 강력한 힘을 이용해 여러분만의 기능을 추가 가능하게 합니다. 이 장의 나중부분에서 저희는 간략하게 간단한 macro를 어떻게 작성하는지 살펴볼 것입니다 [p 61]. 20장에서[p 188] 복잡한 macro들에 관한 주제를 다룰 것입니다.


## Lesson 3 - Some Examples of Special Forms and Macros
 이제 몇몇 special form과 macro를 살펴볼 것입니다. 다음 4 lesson을 거치며, 여러분은 가장 기본적인 Lisp 데이터 형식 list를 이용하여 간단한 함수를 작성케 해주는 repertoire를 구축할 것입니다. 나중 장에서 더욱 복잡한 프로그램 구조와 데이터형식을 다룰 것입니다.

### SETQ
 이전에, 저는 여러분에게 Lisp가 이 변수 값을 받아 symbol form을 평가한다고 말했습니다. SETQ는 값을 설정하는 방식을 제공합니다:

```lisp
@  (setq my-name "David")
>  "David"
@  my-name
>  "David"
@  (setq a-variable 57)
>  57
@  a-variable
>  57
@  (setq a-variable :a-keyword)
>  :A-KEYWORD
```

 SETQ의 첫번째 인자는 symbol입니다. 이는 평가되지 않습니다. 두번째 인자는 변수의 값으로 할당됩니다. SETQ는 마지막 인자의 값을 반환합니다. SETQ는 symbol 그 자체를 값으로 할당하고자 하기에 첫번째 인자를 평가하지 않습니다. SETQ가 첫번째 인자를 평가한다면, 인자가 갖게될 값은 symbol입니다. SET form은 이러한 일을 합니다:

```lisp
@  (setq var-1 'var-2)
>  VAR-2
@  var-1
>  VAR-2
@  var-2
>  Error: Unbound variable
@  (set var-1 99)
>  99
@  var-1
>  VAR-2
@  VAR-2
>  99
```

 첫번째 form에 있는 '을 발견하셨나요? 이는 다음 form var-2 가 평가되는 것을 방지합니다. 이번 lesson 후반에, QUOTE[p 50]를 살펴볼때, 더욱 자세히 이를 설명할 것입니다.

 이번 예제에서, 우선 VAR-1의 값을 symbol VAR-2로 하였습니다. VAR-2의 값을 확인하였고, 이것이 아무것도 가지지 않았다는 것을 확인하였습니다. 다음으로, VAR-1의 값인 symbol VAR-2에 값 99를 할당하기 위해 (SETQ가 아닌)SET을 사용하였습니다.

 SETQ form은 사실 symbol과 value가 번갈아 나오게하여 짝수개의 인자를 취할 수 있습니다:

```lisp
@  (setq month "June" day 8 year 1954)
>  1954
@  month
>  "June"
@  day
>  8
@  year
>  1954
```

 SETQ는 좌측에서 우측으로 할당을 수행하고, 가장 오른쪽에 있는 값을 반환합니다.


### LET
 LET form은 이전에 봐왔던 것보다 좀 더 복잡해 보입니다. LET form은 중첩된 list를 이용하지만, special form이 아니기에, 특정 요소만 평가됩니다.

```lisp
@  (let ((a 3)
        (b 4)
        (c 5))
    (* (+ a b) c))
>  35
@  a
>  Error: Unbound variable
@  b
>  Error: Unbound variable
@  c
>  Error: Unbound variable
```

 위에 있는 LET form은 symbol A, B, C의 값을 정의한 다음, 수학적 계산에서 이를 변수로 이용하였습니다. 또한 이 계산의 결과가 LET form의 결과입니다. LET에서 정의된 변수가 Lisp가 form을 평가한후에 어떠한 값도 지니지 않는다는 것을 주목하시기 바랍니다.

 대게, LET은 이처럼 생겼습니다:

```lisp
(let (bindings) forms)
```

 bindings에는 어떠한 수의 두-요소의 list가 있으며 (각 list는 symbol과 값을 포함합니다), forms에는 어떠한 수의 Lisp form이 있습니다. form의 평가를 위해, bindings에 의해 수립된 값을 이용합니다. LET은 마지막 form에 의해 반환된 값(들)을 반환합니다.

 Indentation은 LET의 동작에 영향을 미치진 않지만, 적절한 indentation은 가독성을 향상시킵니다. 이 동일한 form들을 살펴보시기 바랍니다:

```lisp
(let ((p 52.8)
      (q 35.9)
      (r (f 12.07)))
  (g 18.3)
  (f p)
  (f q)
  (g r t))
(let ((p 52.8) (q 35.9) (r (f 12.07))) (g 18.3) (f p) (f q) (g r t))
```

 첫번째 경우, indentation은 어떤게 bindings며 어떤게 forms인지 명백하게 합니다. 독자가 LET form의 두 부분에서 수행된 서로 다른 roles에 관해 알지 못할지라도, indentation은 차이를 나타냅니다.

 두번째경우, 여러분은 어디에서 bindings가 끝나며 forms가 시작되는지 알기위해 괄호를 세어야 합니다. 더욱 안좋은 것은, indentation의 부제는 LET form의 두 부분에 의해 수행되는 역활의 차이점에 대해 시각적 단서(visual cues)를 파괴합니다.

 SETQ를 이용하여 변수를 정의하고 동일한 변수를 LET form에서 이름지었다면, LET에 의해 정의된 값은 LET의 평가 중에는 다른 값으로 대처될 것입니다:

```lisp
@  (setq a 89)
>  89
@  a
>  89
@  (let ((a 3))
    (+ a 2))
>  5
@  a
>  89
```

 값의 할당이 좌에서-우의 순서로 되는 SETQ와 달리, LET은 모두 동일한 시간에 변수를 bind합니다.

```lisp
@  (setq w 77)
>  77
@  (let ((w 8)
        (x w))
    (+ w x))
>  85
```

 LET은 W를 8로 X는 W로 bind 하였습니다. 동일한 시각에 이러한 binding이 발생하므로, W는 여전히 값 77을 지닙니다.

 Lisp는 순서대로 binding을 수행하는 LET*이라는 LET의 변종을 지녔습니다.

```lisp
@  (setq u 37)
>  37
@  (let* ((v 4)
         (u v))
    (+ u v))
>  8
```

### COND
 COND macro는 조건적으로 Lisp form을 평가하도록 합니다. LET처럼, COND는 form의 다양한 부분의 경계를 긋기위해 괄호를 이용합니다. 이 예제를 살펴보시기 바랍니다:

```lisp
@  (let ((a 1)
        (b 2)
        (c 1)
        (d 1))
    (cond ((eql a b) 1)
          ((eql a c) "First form" 2)
          ((eql a d) 3)))
>  2
```

 COND form 위에서 저희는 3개의 절을 정의하였습니다. 각 절은 test form으로 시작하는 list이며 원하는 만큼의 body form이 뒷따라 나옵니다. body form은 단순히 test가 성공이라면 실행하기 원하는 코드 입니다. 절은 순서대로 선택됩니다 - 하나의 test가 성공하자마자, 대응하는 body form이 평가되고 마지막 body form의 값이 COND form의 값이 됩니다.


 COND는, 하나의 test와 then과 else 부분을 위한 각각 하나의 form만을 허용하는 special form, IF에 비해 더욱 general합니다

 예제에서 어떤일이 발생하는지 살펴보겠습니다. 두 인자가 동일하거나, 동일한 숫자면 EQL 은 T를 반환합니다(여기에는  17장[p 174]에서 다루게될 미묘한 다름이 있습니다). 3개의 테스트중 두개만 실행되었습니다. 첫번째 (EQL AB)는 NIL을 반환합니다. 그러므로, 나머지 절 (1 을 포함하는 유일한 form)은 넘어갑니다. 두번째 절은 (EQL AC)를 테스트하며 이는 true입니다. 이 test가 NIL-이아닌 값을 반환하기에, 나머지 절이 평가되어, 마지막 form의 값이 COND의 값으로 반환된 다음, LET에 둘러싸인 값으로 반환됩니다. 이전 절이 이미 선택되었기에, 세번째 절은 평가되지 않습니다 - 절들은 순서대로 선택됩니다.

 CONS의 전통적인 사용은 마지막 절에 test form으로 T를 이용합니다. 이는 다른 절들이 모두 test에 실패하면 마지막 절의 body forms이 평가된다는 것을 보증합니다. 여러분은 기본 값을 반환하거나 적절한 작업을 수행하기 위해 마지막 절을 이용할 수 있습니다. 여기 예제가 있습니다:


```lisp
@  (let ((a 32))
    (cond ((eql a 13) "An unlucky number")
          ((eql a 99) "A lucky number")
          (t "Nothing special about this number")))
>  "Nothing special about this number"
```

### QUOTE
 종종 여러분은 Lisp의 평범한normal 평가 규칙을 막고자 할 것입니다. 이러한 경우 중 하나는 symbol이 함수 호출의 인자로써 나타날때 이의 값보다 그 자체를 나타내고 싶을 때입니다:

```lisp
@  (setq a 97)
>  97
@  a
>  97
@  (setq b 23)
>  23
@  (setq a b)
>  23
@  a
>  23
@  (setq a (quote b))
>  B
@  a
>  B
```

 차이점은 (SETQ A B)에서 사용된 B의 값과 (SETQ A (QUOTE B))에서 그 자신을 나타낸 B입니다.

 QUOTE form은 매우 자주 사용되어 Lisp는 약칭 표기를 제공합니다:

    (QUOTE form) 	\equiv 'form

 \equiv 표시는 두 Lisp form이 동일하다는 것을 의미합니다. Lisp는 reader macro를 통해 '와 QUOTE를 동일하게 처리합니다. Lesson 12 [p 82]에서 어떻게 여러분만의 reader macro를 정의할 수 있는지 간략하게 살펴볼 것입니다.




## Lesson 4 - Putting things together, and taking them apart
### CONS
 CONS느 가장 기본적인 list의 구성자입니다. 이는 함수므로 이의 인자를 동시에 평가합니다. 두번째 인자는 list나 NIL이여야 합니다.

```lisp
@  (cons 1 nil)
>  (1)
@  (cons 2 (cons 1 nil))
>  (2 1)
@  (cons 3 (cons 2 (cons 1 nil)))
>  (3 2 1)
```

 CONS는 새로운 항목을 list의 시작부분에 추가합니다. 비어있는 list는 NIL과 동일하며,
 () \equiv  NIL
따라서 저희는 또한 이렇게 작성할 수 있습니다:

```lisp
@  (cons 1 ())
>  (1)
@  (cons 2 (cons 1 ()))
>  (2 1)
@  (cons 3 (cons 2 (cons 1 ())))
>  (3 2 1)
```

 여러분이 혼란스럽다면, 맞습니다, NIL에 관한 특별한 무언가가 있습니다. NIL은 keyword가 아니지만 상수 값으로 자기 자신을 지닌 Lisp의 두가지 symbol중 하나입니다. T는 이와 동일한 일을 하는 또 다른 symbol입니다.

 () \equiv NIL이란 것과 NIL이 스스로 평가된다는 것을 종합하자면 이는 (QUOTE ())가 아닌 ()로 쓸 수 있다는 것을 의미합니다. 그렇지 않았다면,  Lisp는 비어있는 list를 다루기 위해 평가 규칙에 예외를 생성했을 것입니다.


### LIST
 여러분이 아마 눈치챗다면, 중첩된 CONS form으로 list가 만들어진다는 것은 조금 지루할 수 도 있습니다. LIST form은 좀더 명료한 방법으로 동일한 일을 수행합니다:

```lisp
@  (list 1 2 3)
>  (1 2 3)
```

 LIST는 어떠한 수의 인자도 취할 수 있습니다. LIST는 함수이기에, 이는 인자를 평가합니다:

```lisp
@  (list 1 2 :hello "there" 3)
>  (1 2 :HELLO "there" 3)
@  (let ((a :this)
        (b :and)
        (c :that))
     (list a 1 b c 2))
>  (:THIS 1 :AND :THAT 2)
```

### FIRST and REST
 list가 (첫번째 요소와 나머지 것들)두 부분으로 만들어졌다고 가정한다면, 여러분은 FIRST와 REST 두 연산자를 이용하여 list의 개별 원소를 받을 수 있습니다.

```lisp
@  (setq my-list (quote (1 2 3 4 5)))
>  (1 2 3 4 5)
@  (first my-list)
>  1
@  (rest my-list)
>  (2 3 4 5)
@  (first (rest my-list))
>  2
@  (rest (rest my-list))
>  (3 4 5)
@  (first (rest (rest my-list)))
>  3
@  (rest (rest (rest my-list)))
>  (4 5)
@  (first (rest (rest (rest my-list))))
>  4
```

 분명, FIRST와 REST 함수를 엮는 것은 지루해질 수 있습니다. 또한, 이 접근 법은 프로그램이 실행되고 특정 요소를 선택하고자 할때나 무한한 길이의 리스트일때 동작하지 않습니다. 4장[p 84] 에서 재귀 함수를 정의함으로써 이러한 문제를 어떻게 해결하는지 살펴볼 것입니다. 나중에 13장에서 [p 150], 저희는 list나 다른 sequence의 요소의 선택을 수행하기 위해 Lisp가 제공하는 함수를 살펴 볼 것입니다


 FIRST와 REST는 상당히 최근에 동일한 CAR와 CDR 함수 각각을 이름바꾸어 Lisp에 추가된 것입니다. CAR와 CDR은 초기 Lisp 구현제중 하나의 세부 구현에서 그 이름을 얻었으며, 그 이름은 기반한 구현체가 오래전에 바뀌었다는 사실에 불구하고 수십년간 고수되었습니다


## Lesson 5 - Naming and Identity

### A symbol is just a name
 symbol은 단지 이름입니다. 이는 그 자신을 나타낼 수 있습니다. 이는 Lisp에서 특정 종류의 프로그램을 작성하는 것을 쉽게 만들어 줍니다. 예를들어, 여러분의 프로그램으로 여러분의 가족 관계를 나타내고자 한다면, 이와 같은 관계를 유지하는 데이터베이스를 만들 수 있습니다:

```lisp
(father John Barry)
(son John Harold)
(father John Susan)
(mother Edith Barry)
(mother Edith Susan)
...
```

 각 관계는 하나의 list입니다. (father John Barry)는 John은 Barry의 아버지라는 것을 의미합니다. 데이터베이스안의 list의 모든 요소는 symbol입니다. 여러분의 Lisp 프로그램은, 예를들어 Harold는 Barry의 할아버지라는 것을 판별하기 위해, 이 데이터베이스안에 있는 symbol을 비교할 수 있습니다

 여러분이 다른 언어(symbol이 없는 언어)로 이와 같은 프로그램을 작성하고자 한다면, 여러분은 가족 구성원의 이름과 관계를 어떻게 표현해야할지 결정해야할 것이며, 모든 필요한 동작을 수행하는 코드를 만들어야 할 것입니다(읽고, 출력하고, 비교하고, 할당하고, 기타등등). symbol이 이름으로 사용했어야만 했던 object와 구분되는 데이터 형식이기에, 이는 모두 Lisp에 내장되었습니다( This is all built into Lisp, because symbols are a data type distinct from the objects they might be used to name.)


### A symbol is always unique

 항상 여러분의 프로그램은, 이름이 같은 다른 모든 symbol과 동일한, symbol을 이용합니다. symbol을 비교하는데 EQ test를 이용할 수 있습니다:

```lisp
@  (eq 'a 'a)
>  T
@  (eq 'david 'a)
>  NIL
@  (eq 'David 'DAVID)
>  T
@  (setq zzz 'sleeper)
>  SLEEPER
@  (eq zzz 'sleeper)
>  T
```

 symbol이름으로 대문자나 소문자를 사용하는 것은 문제가 안된다는 것을 알립니다. 내부적으로, Lisp는 symbol이름에 있는 모든 알파벳 문자를 common case로 바꾸지만 (보통 대문자), Lisp reader에 있는 flag를 설정하여 이를 제어할 수 있습니다.
  Lesson 10 [p 65](또한 31장 [p 247]참조)에서 pakcage에 대해 배운다면, 동일한 스펠이 주어졌지만 동일하지는 않은 symbol 이름을 생성할 수 있습니다. 지금, 여러분이 알아야 할것은 :로 기술된 symbol은 특별한 취급을 받는다는 것입니다.


### A symbol can name a value
 Lisp symbol이 자기 자신을 나타내는 능력은 종종 유용하지만, 더욱 일반적으로 사용하는 것은 값의 이름을 위한 symbol입니다. 이는 다른 프로그래밍 언어에서 변수와 함수 이름의 역활을 맏습니다. 가장 일반적인 Lisp symbol은 값이나 (function call form의 첫번째 요소로 사용할 때)함수를 이름짓습니다.

 Lisp에 관해 특출한unusual 것은 symbol은 동시에 함수나 변수를 값처럼 가질 수 있다는 것입니다:

```lisp
@  (setq first 'number-one)
>  NUMBER-ONE
@  (first (list 3 2 1))
>  3
@  first
>  NUMBER-ONE
```

 첫번째와 마지막 경우에서의 FIRST의 쓰임은 변수고 , 두번째인 경우는 (이 예제에서, lisp에 의해 미리 정의된)함수라는 것을 주의하시기 바랍니다. Lisp는 symbol이 보이는 곳에 기반한걸 이용하여 이 값이 무엇인지 결정합니다. 평가규칙(evaluation rule)이 값을 요구하면, Lisp는 symbol의 변수 값을 찾습니다. 함수를 요청받으면, Lisp는 symbol의 함수를 찾습니다.

 symbol은 이것이 지닌 것 외에 다른 값들을 변수나 함수로써 가질 수 있습니다. symbol은 또한 이의 documentation, property list, print name의 값을 지닐 수 있습니다. symbol의 documentation은 symbol을 기술하기 위해 만든 text입니다. 이를 DOCUMENTATION form을 이용하거나 symbol의 값을 정의하는 특정 form의 일부로 만들 수 있습니다. symbol은 다양한 의미를 지닐수 있기에, documentation에 예를들어 함수나 변수처럼 각각의 개별적인 의미를 할당해야합니다

 property list는 전체에 대해 하나의 키를 지닌 자그마한 데이터 베이스와 같습니다. Lesson 10 [p 65]에서 이러한 symbol의 사용법을 살펴볼 것입니다.

 print name은 symbol을 출력하기 위해 Lisp가 사용하는 것입니다. 여러분은 보통 이를 바꾸지 않고자 할 것입니다; 이를 한다면, Lisp는, 나중에 Lisp가 읽을때read 생성되는 다양한 symbol을 읽기 위해, 원래부터 사용했던 것과는 다른 이름의 symbol을 출력할 것입니다

### A value can have more than one name
 값은value 하나보다 많은 이름을 지닐 수 있습니다. 이는 하나 이상의 symbol이 값을 공유할 수 있다는 것입니다. 다른 언어를 이러한 방식으로 동작하는 pointer를 갖습니다. Lisp는 프로그래머에게 pointer를 노출하지 않으며, objects를 공유합니다. EQ test를 통과할때 object는 동일하다고 여겨집니다. 다음 나오는 것을 살펴보시기 바랍니다:

```lisp
@  (setq L1 (list 'a 'b 'c))
>  (A B C)
@  (setq L2 L1)
>  (A B C)
@  (eq L1 L2)
>  T
@  (setq L3 (list 'a 'b 'c))
>  (A B C)
@  (eq L3 L1)
>  NIL
```

 여기서, L1가 동일한 값을 L2라 이름지엇기에, L1은 L2와 EQ합니다. 다시 말하자면 (LIST 'A 'B 'C) form에 의해 생성된 값은 L1과 L2 두개의 이름을 지닙니다. (SETQ L2 L1) form은  "L2의 값이 L1의 값이 되도록 해라" 라고 말합니다. 값의 복사가 아니라, 값입니다. 따라서 L1과 L2는 동일한 값(처음에 L1의 값으로 할당된 list (A B C))을 공유합니다.

 또한 L3도 list (A B C)를 값으로 갖지만, 이는 L1과 L2가 공유하는 것과는 다른 list 입니다. 비록 L3의 값이 L1과 L2의 값과 동일한 것처럼 보이지만, 다른 LIST form에 의해 생성되었기에, 이는 다른 list입니다. 따라서, symbol A, B, C로 구성된 list이지만 값이 다른 list이기에, (EQ L3 L1) > NIL입니다.





## Lesson 6 - Binding versus Assignment

### Binding creates a new place to hold a value
 Lisp는 종종, 변수의 값을 담기 위해 저장 조각을 할당하여 값을 새로이 할당된 메모리에 넣어, 변수를 위한 "binding을 생성"합니다. Binding은 변수의 lexical scope의 구현을 위한 매우 general한 mechanism이지만, binding의 lifetime에 의존하는 다른 쓰임새를 지녔습니다. 8장[p 126]에서 lifetime과 visibility를 공부할때 이를 다시 논할 것입니다.

 예, Lisp는 새로운 binding을 위한 저장공간을 할당합니다. 이것이 끔찍하게 비효율적인 것처럼 들릴지라도, Lisp가 어디에 저장공간을 할당하는지에 대해 아직은 아무것도 말할 수 없습니다. 예를들어, Lisp는 함수 인자를 실제 값으로 bind하지만, 다른 프로그래밍 처럼 stack에 저장공간을 할당합니다. Lisp는, binding form의 실행을 마치면 종료되는, lifetime을 binding이 지녔는지를 판단치 못하면 binding을 heap에 생성합니다


### Bindings have names
 Lisp는 각 binding에 이름을 부여합니다. 그렇지 않다면, 여러분의 프로그램은 어떻게 binding을 참조할 수 있을까요? 단순하게, 어? 잠시만요...


### A binding can have different values at the same time
 동일한 이름을 공유하는 것은 multiple binding에서 매우 일반적인 것입니다. 예를들어:

```lisp
(let ((a 1))
   (let ((a 2))
      (let ((a 3))
         ...)))
```

 여기서, A는, innermost LET에 (...으로 표시된) body에 도착할때쯤, 3개의 다른 binding을 지녔습니다. 그러나, 위 예제가 전통적인 Lisp 코드를 대표하는 것이라고 말하는 것은 아닙니다.


### One binding is innermost

```lisp
;; 여기서, A는 binding을 갖지 않습니다.
(let ((a 1))
   ;; 여기서 A의 innermost binding은 값 1을 갖습니다.
   (let ((a 2))
      ;; 여기서 A의 innermost binding은 값 2을 갖습니다.
      (let ((a 3))
         ;; 여기서 A의 innermost binding은 값 3을 갖습니다.
         ...)))
```

 보시다시피, innermost binding의 notion은 프로그램의 특정 binding을 구성하는 form 코드의 상대적인 위치에 의존합니다. 어떻게 binding form이 중첩되었는지 살펴보았다면 (위에서 보인것처럼 여러분의 코드를 indent한다면 이를 하기에 쉬울 것입니다), 프로그램은 생성?怜킬?코드에 동봉된 bindings에 대한 접근권을 지닐 것입니다.

 한가지 더 여러분이 알아야 할것은, 내부 binding form이 동일한 symbol을 bind하지 않는 동안, 내부 binding form에서도 외부 binding가 여전히 visible한다는 것입니다:


```lisp
;; 여기서, A와 B는 binding을 갖지 않습니다.
(let ((a 1)
      (b 9))
   ;; 여기서, A의 innermost binding은 값 1을 지니며,
   ;; B의 binding은 값 9를 지닙니다.
   (let ((a 2))
      ;; 여기서, A의 innermost binding은 값 2를 지닙니다.
      ;; B의 binding은 여전히 값 9를 지닙니다.
      (let ((a 3))
         ;; 여기서, A의 innermost binding은 값 2를 지닙니다.
         ;; B는 여전히 outermost LET form에서 값 9를 지닙니다.
         ...)))
```

### The program can only access bindings it creates
 binding form이 새로운 값을 존제하는 symbol에 bind할때, 이전 값은 가려지게됩니다. 프로그램이 안쪽의 내부 binding form을 실행하는 동안에, 외부 binding의 값이 감춰집니다 (그렇지만 잊혀지지는 않습니다). 그러나, 프로그램이 내부 binding form을 빠저나가면, 외부 binding의 값이 복구됩니다. 예를들어:


```lisp
(let ((z 1))
   ;; 여기서, Z의 innermost binding은 값 1을 지닙니다.
   (let ((z 2))
      ;; 여기서, Z의 innermost binding은 값 2을 지닙니다.
      ...)
   ;; 이제 여러분은 내부 binding form을 빠져나왔으며,
   ;; 다시 binding의 값 1을 보게됩니다.
   ...)
```

### Assignment gives an old place a new value
 SETQ form은 이미 존재하는 binding의 값을 바꿉니다:


```lisp
(let ((z 1))
   ;; 여기서, Z의 innermost binding은 값 1을 지닙니다.
   (setq z 9)
   ;; 이제 값 Z는 9입니다.
   (let ((z 2))
      ;; 여기서, Z의 innermost binding은 값 2을 지닙니다.
      ...)
   ;; 이제 여러분은 내부 binding form을 빠져나왔으며,
   ;; 다시 Z의 외부 binding의 값 9을 보게됩니다.
   ...)
```

 위의 SETQ form은, 바깥쪽 LET form의 나머지에서, Z의 외부 binding값을 바꿉니다. 이는 종종 해서는 안되는 일입니다. 문제는 Z값을 밝혀내기 위해 살펴봐야 할 곳이 이제 두 곳이라는 것입니다 - 첫번째는 binding form, 그 다음이 SETQ와 같은 할당을 위한 코드. 관습에 의해 indent되는 binding form과 달리 (많은 Lisp 편집기들이 여러분의 취향에 맞게 이를 수행합니다), 프로그램의 body 코드의 일부인 할당assignment form은 특별한 indentation을 가지지 않습니다; 프로그램을 읽을 시에 이는 찾아내는것을 어렵게 만듭니다.


 저희는 이전 예제에 새로운 binding을 도입하여 매우 쉽게 assignment를 피할 수 있습니다.


```lisp
(let ((z 1))
   ;; 여기서, Z의 innermost binding은 값 1을 지닙니다.
   (let ((z 9))
      ;; Now the value of Z is 9.
      (let ((z 2))
         ;; 여기서, Z의 innermost binding은 값 2을 지닙니다.
         ...)
      ;; 이제 여러분은 내부 binding form을 빠져나왔으며,
      ;; 다시 Z의 middle binding의 값 9을 보게됩니다
      ...)
   ;; 여기서, Z의 outermost binding의 값이 1인 것을 보게됩니다.
   ...)
```

 이제 Z의 모든 binding은 LET form의 상대적인 indentation으로 명백해졌습니다. 프로그램을 읽는 동안, 프로그램의 어느 지점에서(예제에서는 ...에서) Z의 올바른 binding을 찾기위해 여러분이 해야할 일은 수직으로 indentation의 상위outer level에 있는 LET form을 찾아보는 것입니다.

 SETQ form이 LET form으로 둘러싸여진 테두리에 있지 않는 변수를 참조한다면, 이는 값을 symbol의 global이나 special 값으로 할당합니다.

 global 값는 감춰지지 않는 어느곳에서나 접근이 가능하며, Lisp 시스템이 동작하는 동안 이용가능한 상태로 남아있습니다. 8장[p 126]에서 special 변수를 살펴볼 것입니다.


```lisp
(setq a 987)
;; 여기서, A는 전역 값 987를 지녔습니다.
(let ((a 1))
   ;; 여기서, A의 binding 값 1은 전역 변수를 가립니다.
   ...)
;; 이제 A의 전역 값이 다시 보입니다visible.
...
```


## Lesson 7 - Essential Function Definition

### DEFUN defines named functions
 DEFUN form을 이용하여 이름있는 함수를 정의할 수 있습니다:

```lisp
@  (defun secret-number (the-number)
     (let ((the-secret 37))
        (cond ((= the-number the-secret) 'that-is-the-secret-number)
              ((< the-number the-secret) 'too-low)
              ((> the-number the-secret) 'too-high))))
>  SECRET-NUMBER
```

 Lesson 3에서 LET, COND, '(QUOTE의 별칭)를 다루었습니다. 숫자 비교 함수들은 명백한 의미를 지녔습니다.

 DEFUN form은 3개의 인자를 지녔습니다:
1.  함수의 이름: SECRET-NUMBER,
2.  인자 이름의 list: 이것이 호출되엇을시 함수의 인자가 될 (THE-NUMBER), 그리고
3.  함수의 본체: (LET ...).

 3개 모두 그 자신을 나타내므로, DEFUN은 인자를 평가하지 않습니다 (만일 이를 수행한다면, 각 인자에 quote를 지닌 불편함을 맞이할 것입니다).

 DEFUN은 정의된 함수 이름을 반환하고, 여러분이 제공한 이름, 인자 list, 본체를 이용하여 global 정의를 install합니다. 일단 DEFUN을 이용하여 함수를 만들면, 바로 이를 이용할 수 있습니다:

```lisp
@  (secret-number 11)
>  TOO-LOW
@  (secret-number 99)
>  TOO-HIGH
@  (secret-number 37)
>  THAT-IS-THE-SECRET-NUMBER
```

 함수를 호출할때, 이것의 인자는(예. 두번쩨 예제에서 99) 정의에서 사용한 인자 이름(즉. THE-NUMBER)에 bind 됩니다. 그런 다음, 함수의 본체(즉. (LET ...))은 parameter binding의 context로 평가됩니다. 다른 말로 하자면, (SECRET-NUMBER 99)를 평가하는 것은 SECRET-NUMBER 함수 본체가 99로 bind된 변수 THE-NUMBER로 실행되도록 합니다.

 물론, 여러분은 하나의 인자가 더 많은 함수를 정의 할 수 있습니다.

```lisp
@  (defun my-calculation (a b c x)
     (+ (* a (* x x)) (* b x) c))
>  MY-CALCULATION
@  (my-calculation 3 2 7 5)
>  92
```

 함수를 호출할때, 인자는 순서대로 인자 이름에 bind됩니다. Lisp는 인자 이름의 list에 대한 몇몇 선택적인 변형variations을 지닙니다. 공식적으로 이러한 list를 lambda list라 부릅니다 - 21장[p 198]에서 이것의 다른 기능들에 대해 조사examine할 것입니다.


### LAMBDA defines anonymous functions
 때로는 프로그램에서 단 한곳에서만 쓰이는 함수가 필요할 것입니다. DEFUN으로 함수를 만들어 이를 한번 호출할 수 있습니다. 때때로, 함수에 나중에 프로그램을 읽을때 도움을 주는 descriptive 이름을 줄 수 있으므로, 이는 할 수 있는 최선이 됩니다. 그러나 종종 여러분이 필요한 함수는 아주 사소하거나 아주 명백하여, 이름을 짓고싶지 않거나 이전에 이름이 쓰였는제 대해 걱정하지 않고자 할 것입니다. 이와 같은 상황에서, Lisp는 LAMBDA form을 이용하여 이름이 붙여지지 않는 함수를 만들 수 있게 합니다. LAMBDA form은 이름을 갖지 않는 DEFUM form 처럼 보입니다:

```lisp
(lambda (a b c x)
   (+ (* a (* x x)) (* b x) c))
```

 LAMBDA form을 평가할 수 없습니다; 이는 Lisp가 함수를 찾을 수 있다고 예상하는 곳에서 보여야 합니다 - 보통 form의 첫번째 오소로;

```lisp
@  (lambda (a b c x)
     (+ (* a (* x x)) (* b x) c))
>  Error
@  ((lambda (a b c x)
      (+ (* a (* x x)) (* b x) c))
   3 2 7 5)
>  92
```


## Lesson 8 - Essential Macro Definition

### DEFMACRO defines named macros
 DEFMACRO form은 DEFUN form과 많이 유사합니다 - 이는 이름, 인자 list, 본체를 지녔습니다:

```lisp
(defmacro name (argument ...)
   body)
```

### Macros return a form, not values
 macro body는 평가될 form을 반환합니다. 다른말로 하자면, 값이 아닌 form을 반환하는 macro의 body를 작성해야 합니다. Lisp가 macor의 호출을 평가할때, 이는 우선 macro 본체 정의를 평가하고, 첫번째 평가의 결과를 평가합니다 (비교하여, 함수의 본체는 값을 반환하도록 평가됩니다) 여기 여러분이 가장 알고자 하는 것을 설명해주는 간단한 macro 쌍이 있습니다:

```lisp
>  (defmacro setq-literal (place literal)
     ‘(setq ,place ',literal))
@  SETQ-LITERAL
>  (setq-literal a b)
@  B
>  a
@  B
>  (defmacro reverse-cons (rest first)
     `(cons ,first ,rest))
@  REVERSE-CONS
>  (reverse-cons nil A)
@  (B)
```

 SETQ-LITERAL는 인자가 평가되지 않는다는 점을 제외하면 SETQ처럼 동작합니다(SETQ가 이의 두번째 인자를 평가한다는 것을 명심하시기 바랍니다). SETQ-LITERAL의 본체는 ("역따옴표"라 말하는) `로 시작하는 form을 지녔습니다. 역따옴표는 역따옴표된 form에서 쉼표가 보인다는 점을 제외하면 (form으로 둘러싸인 모든 것의 평가를 숨기는)따옴표처럼 행동합니다.

 따라서 위의 (SETQ-LITERAL A B)를 호출함에 있어, 무슨 일이 벌어지는지가 여기 나와 있습니다:
1.  PLACE를 symbol A에 bind한다.
2.  LITERAL를 symbol B에 bind한다.
3. 다음 단계를 거처 `(SETQ ,PLACE ',LITERAL) 본체를 평가한다:
  1.  symbol A얻기 위해 PLACE를 평가한다.
  2.  symbol B얻기 위해 LITERAL를 평가한다.
  3.  form (SETQ A 'B)를 반환한다.
4.  form (SETQ A 'B)를 평가한다.

 반환된 form에서 역따옴표나 쉼표는 보이지 않습니다. SETQ-LITERAL 호출에서 A나 B가 평가되지 않지만 서로 다른 이유 때문입니다. A는 SETQ의 첫번째 인자처럼 보이기에 평가되지 않습니다. B는 macro에 의해 반환된 form앞에 따옴표가 보이기에 평가되지 않습니다

 (REVERSE-CONS NIL A)의 동작은 유사합니다:
1.  bind REST를 symbol NIL에 bind한다.
2.  bind FIRST를 symbol A에 bind한다.
3.  다음 단계를 거처, `(CONS ,FIRST ,REST) 본체를 평가한다:
  1.  symbol A를 얻기 위해 FIRST를 평가한다.
  2.  symbol NIL를 얻기 위해 REST를 평가한다.
  3.  form (CONS A NIL)를 반환한다.
4.  form (CONS A NIL)을 평가한다.

 CONS가 이의 인자를 평가하며, macro 본체는 두 인자에 따옴표를 붙이지 않았기에, REVERSE-CONS의 두 인자는 평가됩니다. A는 symbol B로 평가되며, NIL은 자기자신으로 평가됩니다.

 여러분이 평가되기전에 macro 본체가 어떻게 보여지는지 확인하고자 한다면, MACROEXPAND 함수를 이용할 수 있습니다:

```lisp
  (macroexpand '(setq-literal a b))
  (SETQ A 'B)
  (macroexpand '(reverse-cons nil a))
  (CONS A NIL)
```

 MACROEXPAND가 함수이기에, 이는 인자를 평가합니다. 이는 확장expand하고자 하는 form에 따옴표가 있는지에 대한 이유입니다.

 일부러 이번 lesson의 예제를 매우 간단한 걸로 놓았으므로, 여러분은 매우 간단 기본 매커니즘을 이해할 수 있을 것입니다. 일반적으로, macro는 함수보다 작성하기 힘듭니다 - 20장[p 188]에서 이유와 복잡한 상황을 다룰수 있는 적절한 기법을 살펴볼 것입니다.


## Lesson 9 - Essential Multiple Values

### Most forms create only one value
 form은 보통 하나의 값을 반환합니다. Lisp는 적은 숫자의 다수의 값을 생성하거나 받는 form을 지녔습니다

### VALUES creates multiple (or no) values
 VALUES form은 0이나 더 많은 값을 생성합니다:

```lisp
@  (values)
>
@  (values :this)
>  :THIS
@  (values :this :that)
>  :THIS
>  :THAT
```

 form의 평가에 의해 생성된 여러 라인이으로, 얼마나 많은 값이 반환되었는지 보았습니다. 위의 예제에서 3개의 VALUES form에서 0개, 1개, 2개의 값을 각각 생성하였습니다.

 VALUES는 함수이며, 따라서 인자를 평가합니다.


### A few special forms receive multiple values
 하나의 프로그램에서 다수의 값으로 무얼 하길 원하십니까? 가장 기본적인 동작은 :
1.  각 값들을 개별 symbol로 bind한다, 혹은
2.  값들을 list로 모은다.

 각 값들을 개별 symbol로 bind하기 위해 MULTIPLE-VALUE-BIND를 이용합니다:

```lisp
@  (multiple-value-bind (a b c) (values 2 3 5)
     (+ a b c))
>  10
```

 symbol보다 더 많은 값을 넣으면, 초과된 값은 무시됩니다:

```lisp
@  (multiple-value-bind (a b c) (values 2 3 5 'x 'y)
     (+ a b c))
>  10
```

 symbol보다 더 적은 값을 넣으면, 초과된 symbol은 NIL로 bind됩니다:

```lisp
@  (multiple-value-bind (w x y z) (values :left :right)
     (list w x y z))
>  (:LEFT :RIGHT NIL NIL)
```

### Some forms pass along multiple values
 몇몇 form은 새로운 값을 생성하는 것 대신에, 마지막 값을 본체로 보냅니다. 예제는 LET, COND, DEFUN, LAMBDA의 본체를 포함합니다.

```lisp
@  (let ((a 1)
        (b 2))
     (values a b))
>  1
>  2
@  (cond (nil 97)
        (t (values 3 4)))
>  3
>  4
@  (defun foo (p q)
     (values (list :p p) (list :q q)))
>  FOO
@  (foo 5 6)
>  (:P 5)
>  (:Q 6)
@  ((lambda (r s)
      (values r s))
     7 8)
>  7
>  8
```

 함수와 lambda 본체의 경우엔, "암묵적인 PROGN"라는 것으로 부터 다수의 값이 반환되었습니다. 이는, 본체가 다수의 form을 포함할 수 있으며, 오직 마지막 form의 값이 반환될거라고, 멋지게 말하는 방법입니다.

 여러분은 이러한 행동을 원할때 PROGN special form을 이용할 수 있습니다. (PROGN form1 form2 ... formN) 은 순서대로 form1부터 formN까지 평가하고, formN의 값을 반환합니다.


## Lesson 10 - A Preview of Other Data Types

### Lisp almost always does the right thing with numbers
 이는 말하기에 이상한 것처럼 들립니다. 컴퓨터는 항상 숫자로 일을 수행하지 않나요?  음, 아니요... 보통은 그렇지 않습니다.

 수치 계산은 수많은 방식으로 나뉠 수 있습니다. 가장 큰 문제가 되는 점 중 하나는 소수의 계산이 있습니다 (여러분의 언어가 이를 실수라 부른다면, 그것은 거짓말 입니다). visual-어쩌고 혹은 객체-지향의-무언가에 대한 많은 책들중 거의 절반정도가 소수의 계산의 적절한 사용에 대해 쓰여졌습니다.

 소수의 문제는 수학적으로 실수가 아니지만, 종종 그것인양 (잘못)사용됩니다.
중요한 문제는 그러한 소수가 정확도의 한계를 지녔다는 것입니다 - only so many digits to the right of the decimal point. 이제, 계산의 모든 숫자가 거의 같은 규모라면, 계산은 정확도를 잃지 않을 것입니다. 그러나, 숫자가 매우 다양한 규모라면, 소수 계산은 정확도를 희생합니다.

 여러분의 컴퓨터의 소수가 정확하게 소숫점 7자리를 표현할수 있다고 가정해 봅시다. 그러면, 여러분은 1897482.0에 2973225.0를 더하여 완벽히 정확한 답을 얻을 수 있을 것입니다. 그러나, 1897482.0에 0.2973225를 더하고자 한다면, 정확한 답은 14자리의 숫자이지만, 여러분의 컴퓨터는 1897482.0을 답으로 할 것입니다.

 소수에 간한 또다른 문제는 더욱 미묘subtle합니다. 프로그램을 작성할때, 10진수에 기반하여 수를 쓸 것입니다. 그러나 컴퓨터는 모든 연산을 2진수에 기반하여 수행합니다. 10진수에서 2진수로의 변환은 "명백히 정확한" 특정 숫자에 대해 재미난 일을 수행합니다. 예를들어, 소수 0.1은 2진수로 변환하면 순환 분수입니다. 컴퓨터는 순환분수에 의해 요구되는 무한한 숫자를 저장하지 못하기에, 정확하게 0.1을 저장할 수 없습니다.

 대부분의 컴퓨터 언어에서 정수(자연수) 연산은 또다른 문제를 지닙니다

- 하나의 정수를 담을 수 있는 양수나 음수의 최대치에 재한을 가하는 경향이 있습니다. 따라서, 숫자 하나를 여러분의 언어에 대해 컴퓨터를 다룰 수 있는 가장 큰 정수에 더하고자 한다면, 둘 중 하나가 발생합니다:

1.  에러로 프로그램이 종료된다, 혹은
2.  굉장히 부정확한 답을 얻는다.(가장 큰 양수에 1을 더한 것은 가장 큰 음수중에 적어도 하나를 만들어 냅니다.)

 그러면 숫자에 관해 올바른 일을 수행하기 위해 Lisp는 어떻게 처리할까요? 무엇보다도, 이 문제가 컴퓨터 연산에서 나온 것처럼 보입니다. 답은 Lisp는 내장 컴퓨터 산술 연산을 바로 사용하지 않는다는 것입니다 - 수학적으로 정확한 특정한 수치 데이터 형식을 추가하였습니다:
* bignums은 무한한 정수이다 (컴퓨터의 메모리의 한계에 달려있다)
* 유리수는, 대략적인 머신 나눗셈 알고리즘에서 나온 결과인 소수가 아니라,
두 정수의 정확한 몫이다.

 물론, Lisp 또한 machine-기반 정수와 소수를 지닙니다. Machine integers를 Lisp에서 fixnums라 부릅니다. fixnum의 범위에서 자연수가 떨어지면, Lisp는 이를 machine integer로 저장할 것입니다. 그러나, 이게 매우 커지면, Lisp는 자동으로 이를 bignum으로 승격promote시킵니다.


 제가 Lisp는 거의 항상 수에 관해 올바른 일을 수행한다고 했을때, 저는 이것이 거의 항상 수학적으로 옳은 수의 표현을 고른다는 것을 의미한다는 뜻으로 말했었습니다:

```lisp
@ (/ 1 3)
> 1/3
@ (+ (/ 7 11) (/ 13 31))
> 360/341
@ (defun factorial (n)
    (cond ((= n 0) 1)
          (t (* n (factorial (- n 1))))))
> FACTORIAL
@ (factorial 100)
> 933262154439441526816992388562667004907159682643816214685
  929638952175999932299156089414639761565182862536979208272
  23758251185210916864000000000000000000000000
```

 소수를 이용한 계산을 작성할 수 있지만, Lisp는 정확한 수치 결과를 부정확한 소수로 전환할수 없기에(여러분은 이에게 소수는 전염된다라고 말해야 합니다) 일단 이를 계산에 도입하면, 전체 계산 결과는 소수를 유지할 것입니다:

```lisp
@ (float (/ 1 3))
> 0.3333333333333333
@ (* (float (/ 1 10)) (float (/ 1 10)))
> 0.010000000000000002
@ (+ 1/100 (* (float (/ 1 10)) (float (/ 1 10))))
> 0.020000000000000004
@ (+ 1/100 1/100) ; 이전 계산과 비교하세요
> 1/50
@ (* 3 7 10.0)
> 210.0
@ (- 1.0 1)
> 0.0
@ (+ 1/3 2/3 0.0)
> 1.0
@ (+ 1/3 2/3)
> 1 ; 이전 계산과 비교하세요
```

 Lisp는 소수를 소수점과 같이, 정수에는 없이 출력합니다.


### Characters give Lisp something to read and write
 기본 Lisp I/O는 문자를 이용합니다. READ와 WRITE 함수는 문자를 Lisp object로 그리고 역으로 변환시킵니다. READ-CHAR와 WRITE_CHAR는 단일 문자를 읽고 씁니다.

```lisp
@ (read)
< a \newline
> A
@ (read)
< #\a \newline
> a
@ (read-char)
< a
> #\a
@ (write 'a)
= A
> A
@ (write #\a)
= #\a
> #\a
@ (write-char #\a)
= a
> #\a
@ (write-char 'a)
| Error: Not a character
```

 위 예에서 새로운 표기를 추가했습니다. < 표시는 Lisp는 READ와 같은 입력함수에 대한 input을 기다린다는 것을 의미합니다. 이는 평가되고 출력되는 입력을 받는 @와 다릅니다. \newline은 return이나 enter 키에 의해 생성되는 newline 문자를 나타냅니다.

 =는 값을 반환한 것이 아니라 출력을 나타냅니다.

 여러분은 newline이 READ input을 종료한다는 것을 알아채셨을 것입니다. READ는 완벽한 Lisp 표현식을 구성하고자하는 문자들을 모으기 때문입니다. Lesson 11[p 77]에서 이것에 대해 좀 더 살펴볼 것입니다. 이번 예제에서, READ는 새로운라인newline으로 종료되는 symbol을 모읍니다. symbol은 또한 공백, 괄호, symbol의 일부가 아닌 문자로 종료됩니다.

 대조적으로, READ-CHAR는 input에서 정확히 하나의 문자만 읽습니다. 문자를 받자마자, READ-CHAR는 실행을 완료하고 문자를 반환합니다.

 몇몇 Lisp 시스템은 어떠한 입력을 받아들이기 전에 return 키를 누르도록 요구할 것입니다. 이는 일상적인게 아니며, 환경설정 인자로 수정할 수 있습니다 - 여러분의 Lisp vender와 상담을 하시기 바랍니다.

 WRITE와 WRITE-CHAR는 둘다 받았던 값을 반환합니다. 값을 출력하는 방식은 다릅니다. WRITE는 값을 출력하므로, 동일한 값을 생성하는 READ로 표현할 수 있습니다. WRITE-CHAR는, 이것이 READ에서의 문자라는 것을 나타내는 추가 Lisp syntax (#\)가 없는, 읽을 수 있는 문자를 출력합니다.

 Lisp는 단일 문자를 #\문자 표기법을 이용하여 나타내며, 그러한 문자로는 literal 문자나 출력가능한 상형문자가 아닌 문자의 이름이 있습니다.

    Character           Hex Value   Lisp           Standard?
    --------------------------------------------------------
      space                 20      #\Space          yes
      newline               --      #\Newline        yes
      backspace             08      #\Backspace      semi
      tab                   09      #\Tab            semi
      linefeed              0A      #\Linefeed       semi
      formfeed              0C      #\Page           semi
      carriage return       0D      #\Return         semi
      rubout or DEL         7F      #\Rubout         semi

 #\Space와 #\Newline만이 모든 Lisp 시스템에서의 요구사항입니다. ASCII 문자셋를 이용하는 시스템은 아마 위에 나온 나머지 문자코드를 구현할 것입니다. \#Newline 문자는 호스트 시스템에 맞는 출력 라인의 끝을 표시하는 convention을 따릅니다. 예로:

    System        Newline     Hex Value
    -----------------------------------
    Macintosh       CR          0D
    MS-DOS          CR LF       0D 0A
    Unix            LF          0A

     94개의 출력가능한 표준 문자들이 \#char로 표현됩니다:
      ! " # $ % & ' ( ) * + , - . /
    0 1 2 3 4 5 6 7 8 9 : ; < = > ?
    @ A B C D E F G H I J K L M N O
    P Q R S T U V W X Y Z [ \ ] ^ _
    ‘ a b c d e f g h i j k l m n o
    p q r s t u v w x y z { | } ~


### Arrays organize data into tables
 여러분이 데이터를 2, 3, 더 많은 규모의 테이블에 역으려한다면, 배열array을 만들 수 있습니다:

```lisp
@ (setq a1 (make-array '(3 4)))
> #2A((NIL NIL NIL NIL)
  (NIL NIL NIL NIL)
  (NIL NIL NIL NIL))
@ (setf (aref a1 0 0) (list 'element 0 0))
> (ELEMENT 0 0)
@ (setf (aref a1 1 0) (list 'element 1 0))
> (ELEMENT 1 0)
@ (setf (aref a1 2 0) (list 'element 2 0))
> (ELEMENT 2 0)
@ a1
> #2A(((ELEMENT 0 0) NIL NIL NIL)
  ((ELEMENT 1 0) NIL NIL NIL)
  ((ELEMENT 2 0) NIL NIL NIL))
@ (aref a1 0 0)
> (ELEMENT 0 0)
@ (setf (aref a1 0 1) pi)
> 3.141592653589793
@ (setf (aref a1 0 2) "hello")
> "hello"
@ (aref a1 0 2)
> "hello"
```

 dimensions의 list를 취하여 배열을 반환하는 MAKE-ARRAY를 이용하여 배열을 만듭니다. 기본적으로,  배열은 어떠한 종류의 데이터라도 담을 수 있습니다; 효율성을 위해 추가 인자로 데이터형식의 요소를 제한할 수 있습니다.

 배열의 rank는 dimension의 수와 동일합니다. 위 예제에서 저희는 2-차원rank-2 배열을 만들었습니다. Lisp는 #rankA(...)표기법을 이용하여 배열을 출력합니다. 배열의 내용물은, 첫번째 dimension이 최상단 그룹의 요소로 보여지고, 마지막 dimension이 최하단 그룹의 요소로 보여지는, 중첩된 list로 보여집니다.

 여러분의 Lisp 시스템은 아마도 여기서 보여졌던것 처럼 라인을 넘기면서 배열을 출력하지 않을 것입니다. 저는 배열의 구조를 강조하기 위해 라인을 넘겼습니다.

 배열의 요소를 받기 위해선, AREF를 이용합니다. AREF의 첫번째 요소는 배열입니다; 나머지 요소는 각 dimension의 index를 지정합니다. index의 수는 배열의 rank와 맞아야만 합니다.

 배열의 요소를 설정하기 위해선, 이 예제에서 보인것 처럼 SETF form내부에서 AREF를 사용합니다. SETF는 SETQ와 유사하지만, SETQ가 값을 symbol에게 할당하고, SETF는 값을 place에 할당합니다. 예제에서 AREF form은 배열의 요소의 place를 지정합니다.


### Vectors are one-dimensional arrays
 Vector는 일-차원one-dimensional 배열입니다. MAKE-ARRAY를 이용하여 벡터를 만들 수 있으며, AREF를 이용하여 이의 요소에 접근 할 수 있습니다.

```lisp
@ (setq v1 (make-array '(3)))
> #(NIL NIL NIL)
@ (make-array 3)
> #(NIL NIL NIL)
@ (setf (aref v1 0) :zero)
> :ZERO
@ (setf (aref v1 1) :one)
> :ONE
@ (aref v1 0)
> :ZERO
@ v1
> #(:ZERO :ONE NIL)

 Lisp는 #1A(...)이 아닌 조금 간소화된 form #(...)을 이용하여 벡터를 출력합니다.

 MAKE-ARRAY에서 단일-원소 리스트나 벡터의 차원dimenion을 지정할 수 있습니다.

 VECTOR form을 이용하여 값을 나열하여 벡터를 만들 수 있습니다:
@ (vector 34 22 30)
> #(34 22 30)
```

 결과값이 list가 아닌 vector라는 점을 제외하면, 이는 LIST form과 유사합니다. list와 vector간에는 또 다른 닮음이 있습니다: 모두 sequence입니다. sequence는 13장[p 150]에서 보게될 함수에 의해 다뤄집니다.

 벡터의 요소에 접근하기 위해 AREF를 이용하거나, sequence-특화 함수 ELT를 이용할 수 있습니다:

```lisp
@ (setf v2 (vector 34 22 30 99 66 77))
> #(34 22 30 99 66 77)
@ (setf (elt v2 3) :radio)
> :RADIO
@ v2
> #(34 22 30 :RADIO 66 77)
```

### Strings are vectors that contain only characters
 여러분은 이미 "..." syntax를 이용하여 문자열string을 작성하는 법을 알고 있습니다. string은 vector이기에, 문자열의 요소에 접근하기 위해 array와 vector 함수를 적용할 수 있습니다. 또한 MAKE-STRING 함수를 이용하여 string을 만들거나, STRING 함수를 이용하여 문자나 symbol을 string으로 바꿀 수 있습니다.

```lisp
@ (setq s1 "hello, there.")
> "hello, there."
@ (setf (elt s1 0) #\H))
> #\H
@ (setf (elt s1 12) #\!)
> #\!
@ s1
> "Hello, there!"
@ (string 'a-symbol)
> "A-SYMBOL"
@ (string #\G)
> "G"
```

### Symbols are unique, but they have many values
 Lesson 5에서 symbol은 고유한 독자성identity을 지닌다고 보았지만, 이는 반복성을 지닙니다: symbol은 같은 철자의 다른 symbol과 동일합니다(이번 lesson 끝부분에서 좀더 배우게될 package designation를 포함하여). 이는 Lisp로 하여금 프로그램이나 데이터를 읽어올 수 있게 만들 수 있다는 것을 의미하며, 같은 철자로 나타낸 모든 symbol은 동일한 symbol입니다. Lisp가 이를 수행하는 매커니즘을 제공하기에, symbolic 정보를 다루는 프로그램을 작성하는 것에 대한 걱정을 하나 덜게되었습니다.

 저희는 또한 Lesson 5에서 symbol은 변수와 함수, 그리고 documentation, print name, properties를 위한 값을 가질수 있다고 배웠습니다. symbol의 property list는 다수의 키/값 쌍이 symbol과 연결된 아주 작은 데이터베이스와 같습니다. 예를들어, 여러분의 프로그램이 object를 표현하고 다룬다면, object에 관한 정보를 property list에 저장할 수 있습니다:

```lisp
@ (setf (get 'object-1 'color) 'red)
> RED
@ (setf (get 'object-1 'size) 'large)
> LARGE
@ (setf (get 'object-1 'shape) 'round)
> ROUND
@ (setf (get 'object-1 'position) '(on table))
> (ON TABLE)
@ (setf (get 'object-1 'weight) 15)
> 15
@ (symbol-plist 'object-1)
> (WEIGHT 15 POSITION (ON TABLE) SHAPE ROUND SIZE LARGE
COLOR RED)
@ (get 'object-1 'color)
> RED
@ object-1
> Error: no value
```

 OBJECT-1은 값을 갖지 않는다는 점을 주목하시기 바랍니다 - 모든 유용한 정보는 두 곳에 있습니다: symbol의 identify와 symbol의 properties.

 이러한 property의 이용은 현대 object programming보다 몇 십년이나 앞서 나왔습니다. 이는 object의 필수적인 3가지 매커니즘중 2가지를 제공합니다: 식별자identify와 캡슐화encapsulation (property 값 역시 함수가 될 수 있다는 것을 명심하시기 바랍니다). 3번째 매커니즘, 상속inheritance은 다른 "object"를 link하여 simulate합니다.

 Property는 현대 Lisp 프로그램에서 거의 사용되지 않습니다. 해쉬테이블Hashtables (아래 참조) [p 73] , (다음 section에서 기술된) 구조체structures , CLOS objects(7장 [p 117]과 14장 [p 157] 참조)는 사용하기에 더 편리하며 더욱 효율적인 방식으로 property lists의 모든 기능을 제공합니다. 현대 Lisp 개발 시스템은 property에 파일과 symbol을 정의하는 form의 파일 위치, 함수의 인자 list의 정의와 같은 (프로그래밍 환경에서 유용한 도구로 이용하기 위한) 특정 정보를 기록하여 프로그램에 주석을 다는 목적으로 주로 사용합니다.


### Structures let you store related data
 Lisp 구조체는, 명명된 slot과 관계된 데이터를 저장하는, object를 생성하는 방법을 여러분에게 제시합니다.

```lisp
@ (defstruct struct-1 color size shape position weight)
> STRUCT-1
@ (setq object-2 (make-struct-1
  :size 'small
  :color 'green
  :weight 10
  :shape 'square))
> #S(STRUCT-1 :COLOR GREEN :SIZE SMALL :SHAPE SQUARE
  :POSITION NIL :WEIGHT 10)
@ (struct-1-shape object-2)
> SQUARE
@ (struct-1-position object-2)
> NIL
@ (setf (struct-1-position object-2) '(under table))
> (UNDER TABLE)
@ (struct-1-position object-2)
> (UNDER-TABLE)
```

 이 예에서, 저희는 COLOR, SHAPE, SIZE, WEIGHT의 slot을 지닌 STRUCT-1 구조체를 정의하였습니다. 그런 다음 STRUCT-1의 instance를 만들고, 그 instance에 변수 OBJECT-2를 할당하였습니다. 예제의 나머지 부분은, struct-slot 이름으로된 접근자assessor 함수를 이용하여, 어떻게 구조체 instance의 slot에 접근하는지를 보여줍니다. Lisp는 DEFSTRUCT를 이용하여 구조체를 정의할때 make-structname과 structname-slotname 함수를 생성합니다.

 6장[p 112]에서 DEFSTRUCT의 optional 기능에 대해 살펴볼 것입니다.


### Type information is apparent at runtime
 symbol은 runtime시 어떠한 변수 타입과도 연관지을 수 있습니다. 이것이 문제되는 경우에 대해, Lisp는 저희에게 값의 타입을 묻는것을 허용합니다

```lisp
@ (type-of 123)
> FIXNUM
@ (type-of 123456789000)
> BIGNUM
@ (type-of "hello, world")
> (SIMPLE-BASE-STRING 12)
@ (type-of 'fubar)
> SYMBOL
@ (type-of '(a b c))
> CONS
```

 TYPE-OF 는 인자의 종류type를 나타내는 symbol이나 list를 반환합니다. 게다가 이 정보는 인자의 종류의 기반하는 프로그램의 행동을 설명하는데 이용할 수 있습니다. TYPECASE 함수는 type 질의inquiry와 COND-와같은 분기dispatch를 결합한 것입니다.

 CLOS(14장 [p 157] 참조)S의 제네릭 함수의 도입으로, TYPE-OF는 예전만큼 중요하지는 않습니다.


### Hash Tables provide quick data access from a lookup key
 해쉬 테이블은 값과 고유키unique key와 관련있습니다. property list완 다르게, 해쉬 테이블은 엄청나게 많은 키/값쌍에 매우 적합하지만, 작은 연관 set에 대해 엄청난 overhead를 감수해야합니다.

```lisp
@ (setq ht1 (make-hash-table))
> #<HASH-TABLE>
@ (gethash 'quux ht1)
> NIL
> NIL
@ (setf (gethash 'baz ht1) 'baz-value)
> BAZ-VALUE
@ (gethash 'baz ht1)
> BAZ-VALUE
> T
@ (setf (gethash 'gronk ht1) nil)
> NIL
@ (gethash 'gronk ht1)
> NIL
> T
```

 MAKE-HASH-TABLE을 이용하여 해쉬 테이블을 생성하며, GETHASH를 이용하여 값에 접근합니다. GETHASH는 두 값을 반환합니다. 첫번째는 키와 관련있는 값입니다. 두번째는 키를 찾으면 T, 아니면 NIL입니다. 위 예제에서 처음과 마지막 GETHASH form의 차이를 주목하시기 바랍니다.

 기본적으로, 해쉬테이블은 EQ(이는 숫자나 list가 아닌 symbol에 대해서만 동작합니다)를 이용하여 이의 키를 비교하기 위해 만들어졌습니다. 17장[p 174]에서 동일성equality 판단predicates에 대해 더 자세히 배울 것입니다. 지금은, 다수의 키를 이용하고자 한다면, 이 form을 이용하여 해쉬테이블을 만들어야한다는 것을 명심하시기 바랍니다:
  (make-hash-table :test #'eql)

 list를 키로 사용하고자 한다면, 이렇게 해쉬테이블을 만듭니다:
  (make-hash-table :test #'equal)

 키를 없에고자한다면, form (REMHASH key hash-table)를 이용합니다. 그리고 키에 해당하는 값을 바꾸고자한다면, 키/값쌍을 추가했던것처럼 GETHASH와 SETF를 이용합니다.


### Packages keep names from colliding
 프로그램을 작성하는것에 관한 어려운 일중 하나는 프로그램의 이름을 짓는 부분입니다. 한편으론, 기억하기 쉽고 명명된 object의 규칙과 목적을 환기evocative시켜주는 이름을 이용하기 원할 것입니다. 다른 한편으론, 언젠가 여러분의 프로그램과 작업하게될, 다른 프로그램의 어딘가에 이미 사용한 (혹은 사용될 것 같은)이름을 이용하지 않길 원할 것입니다.

 이름충돌naming conflict을 피하는 법 중 하나는 프로그램에 있는 모든 이름에 누구도 사용하지 않는 고유 접두사prefix를 붙이는 것입니다. 여러분은 이것이 라이브러리에 매번 필요하다고 생각할 것입니다 - 보통 하나에서 세개정도 문자의 접두사가 있다. 불행히도, 여전히 두 소프트웨어 개발자가 동일한 prefix를 선택하기 위한 많은 관문이 남아있습니다; 유달히 몇몇 prefix들이 다른 것보다 더욱 evocative하기 때문입니다. 여러분이 생산한 모든 소프트웨어를 제어하고자 한다면, 모든 prefix를 선택하고 문제를 피할 수 있습니다. prefix naming scheme을 이용하는 third-parfy 소프트웨어를 사고자 한다면, 제조사에 의해 선택된 이름에 관련된 일을 해야할 것이며, 두 제조사가 우연히 동일한 prefix를 사용하지 않기를 희망할 것입니다.
 
![prefixed-names.gif]

 naming conflicts 피하는 또 하나의 방법은 qualified name을 이용하는 것입니다. 이를 하기 위해선, 언어가 프로그래머에 의해 정의되고 제어되는 분리된 이름공간의 지원을 제공해야 합니다. 어떻게 이것이 동작하는지 이해하기 위해, 프로그램을 위해 여러분이 만든 모든 이름이 여러분의 이름이 타이틀 상단에 쓰여진 한장에 종이에 작성되었다고 상상해보시기 바랍니다. 이름이 이용하기에 안전한지 확인하기 위해선, 여러분이 이 페이지에 작성한 이름 목록을 확인하기만 하면 됩니다. 누군가의 소프트웨어가 여러분의 프로그램의 서비스를 필요로 할때, 그것은 여러분의 qualifier와 name을 이용하여 여러분의 이름을 참조합니다. 다른이의 소프트웨어는 다른 qualifier를 지니고 그들의 qualifier는 그들만의 name을 암시하기에(이것을 작성할 필요는 없습니다), 이름 충돌을 피할 방도가 없습니다.

 여러분은 qualifier는 이름에 prefix를 붙이기 위한 복잡한 방법에 지나지 않는다고 생각할 것입니다. 그러나, 거기엔 미묘하며 중대한 차이점이 있습니다. prefix는 이름의 일부입니다; 이는 한번 작성되면 바뀔 수 없습니다. qualifier는 이가 qualify한 이름에서 분리되었으며, 정확히 한 장소에서 "작성"되었습니다. 더욱이, 여러분은 이름들이 쓰여진 "종이"를 가리켜 이를 "그러한 이름들"이라 말할 수 있습니다. 다른 프로그래머와 동일한 qualifier를 선택하게 된다면, 여러분은 여전히 여러분이 선택한 qualifier에 의해 "그 이름"을 참조할 수 있습니다 - 다른 말로 하자면, 여러분이 사용할 소프트웨어가 출고된 후에도, 여러분은 qualifier를 바꿀 수 있습니다.
 
![rename-package.gif]

 위 예제에는, 파일 LIB1과 LIB2에서 가져온 두 라이브러리가 있습니다. 두 라이브러리 설계자는, Lisp에서 package 이름으로 알려진, 이름 공간namespace을 위해 UTIL이란 이름을 사용하였습니다. 각 라이브러리는 client에게 보여지는 이름들을 나열list했습니다. 두 라이브러리를 이용하는 프로그래머는 MY-PACKAGE란 package 이름에서 코드를 작성합니다. 각 라이브러리를 로드한후, 프로그래머는 이의 package의 이름을 다른 이름으로 바꿉니다. 그러면, UTIL-1:INITIALIZE와 UTIL-2:INITIALIZE의 호출에서 봤던 것처럼, 라이브러리에 있는 이름들은 이름이 바뀐 qualifier를 이용하여 참조됩니다. 프로그래머는 여전히 qualify form이 아닌 INITIALIZE 이름은 이용할 수 있다는 것을 주목하시기 바랍니다 - 이는 MY-PACKAGE:INITIALIZE와 동일합니다.

 Lisp는 package facility라 알려진 함수와 매크로를 통해 이러한 기능을 제공합니다. DEFPACKAGE 매크로는 간편하게 대부분의 package 연산을 제공하는데 반해, IN-PACKAGE 매크로는 현재 package를 설정합니다:

```lisp
;;;; ---- File 1 ----
(defpackage util1
  (:export init func1 func2)
  (:use common-lisp))
(in-package util1)
(defun init () 'util1-init)
(defun func1 () 'util1-func1)
(defun func2 () 'util1-func2)
;;;; ---- File 2 ----
(defpackage util2
  (:export init func1 func2)
  (:use common-lisp))
(in-package util2)
(defun init () 'util2-init)
(defun func1 () 'util2-func1)
(defun func2 () 'util2-func2)
;;;; ---- File 3 ----
(defpackage client
  (:use common-lisp)
  (:import-from util1 func1)
  (:import-from util2 func2))
(in-package client)
(defun init () 'client-init)
(util1:init)
(util2:init)
(init)
(func1)
(func2)
```

 예제는 세개의 파일의 내용물을 나열했습니다. File 1과 File 2는 모두 동일한 이름으로 3개의 함수를 정의하였습니다. File 1은 이름을 UTIL1 package에 넣었으며, File 2는 UTIL2 package를 이용합니다. DEFPACKAGE form은 package의 이름을 지어줍니다. :USE 옵션은 다른 pakcage에서 qualification없이 사용될 이름을 지정하는 반면, :EXPORT 옵션은 package에서 client에게 노출될 이름을 지정합니다.

 DEFPACKAGE form은 단지 pakcage를 생성합니다. USE-PACKAGE form은 package를 current로 만듭니다 -  qualify가 되지 않은 모든 이름들은 current입니다. COMMON-LISP:*PACKAGE* 변수는 항상 current package를 포함합니다.

 File 3은 CLIENT package를 만듭니다. :INFORT-FROM 옵션은 UTIL1과 UTIL2 package로부터 특정 이름을 가져옵니다 - 이러한 이름들은 CLIENT package에서 qualification 없이 사용되어집니다. UTIL1나 UTIL2에서 export되었지만 CLIENT에서 import된 이름들은, form pakcage:name 의 명시적explicit qualifier를 이용하여 CLIENT에 의해 참조될 수 있습니다.

 이번 section에선 아주 기초적인 package 연산만을 다루었습니다. 31장[p 247]에서, 큰 규모의 소프트웨어 시스템을 구성에서의 package를 살펴볼때, 좀더 세부적인걸 다룰 것입니다.


## Lesson 11 - Essential Input and Output

### READ accepts Lisp data
 Lesson 10에서 봤던것처럼, READ는 문자를 Lisp 데이터로 변환시킵니다. 지금까지, 여러분은 몇몇 종류의 Lisp 데이터가 출력된 표현을 보았습니다:
* symbols과 numbers,
* strings, characters, lists, arrays, vectors, structures,
* hash tables.

 Lisp reader는 문자의 분류법classifications를 따라 이의 일을 수행합니다. 표준 분류법classifications은 아래 나와있습니다. Lesson 12에서 보게될 것처럼, 여러분은 필요에 따라 이러한 분류법을 바꿀 수 있습니다.

    Standard Constituent Characters(표준 구성문자)
    -------------------------------
     a b c d e f g h i j k l m n o p q r s t u v w x y z
     A B C D E F G H I J K L M N O P Q R S T U V W X Y Z
     0 1 2 3 4 5 6 7 8 9
     ! $ % & * + - . / : < = > ? @ [ ] ^ _ { } ~
     <backspace> <rubout>

    Standard Terminating Macro Characters(표준 종료매크로문자)
    -------------------------------------
     " ’ ( ) , ; ‘

    Standard Non-Terminating Macro Characters(표준 비-종료매크로문자)
    -----------------------------------------
     #

    Standard Single Escape Characters(표준 단일예외문자)
    ---------------------------------
     \

    Standard Multiple Escape Characters(표준 다수예외문자)
    -----------------------------------
     |

    Standard Whitespace Characters(표준 공백문자)
    ------------------------------
     <tab> <space> <page> <newline> <return> <linefeed>

 READ가 구성문자(constituent character)로 시작한다면, 이는 symbol이나 number를 축적합니다. READ가 종료매크로문자(terminating macro character)나 공백문자(whitespace character)를 만나면, 이는 모아진 구성문자를 우선 숫자로, 그 다음 symbol로 이해interpret하려 시도합니다. 수치화 해석이 가능하다면, READ는 숫자를 반환합니다. 그렇지 않으면, READ는 영문자를 standard case(보통 대문자)로 바꾸어, symbol로써 이름을 intern하여, symbol을 반환합니다.

 예외문자(Escape characters)는 특별한 역활을 수행합니다. 하나의 예외문자는 다음에 나오는 문자를 마치 constituent character처럼 다루도록 합니다. 보통 공백이나 종료매크로문자로 취급되는 문자는 symbol의 일부가 될 수 있습니다. READ가 escape character를 만나면, 숫자들로만 된것이 escape될지라도, constituents 결과를 숫자로 interpret 하려 하지 않을 것입니다

 READ가 매크로문자(macro character)로 시작한다면, 문자는 다음 경우를 판별determine할 것입니다.

    "
    문자열을 읽는다.
    ’
    form을 읽는다.
    (
    list를 읽는다.
    ;
    새로운 라인을 만나기 전까지 모든 것을 무시한다.
    #
    다음 나오는 문자에 기반하여 무엇인지 결정한다.

 마지막으로, 몇몇 Lisp 데이터는 읽을 수 없습니다. 예를들어, 해쉬테이블이 출력되어 보여지는 것은 #<HASH-TABLE>와 같이 보여집니다. #< 문자로 시작하는 어떠한 것을 읽으려할때 READ는 에러를 낼 것입니다.


### PRINT writes Lisp data for you and for READ
 PRINT 함수는 Lisp object를, READ가 이를 재구성해야만 하는, 문자열(sequence of characters)로 바꿉니다:

```lisp
@  (print ’abc)
=  \newline ABC \space    > ABC
@  (print (list 1 2 3))
=  \newline (1 2 3) \space   > (1 2 3)
@  (print "A String")
=  \newline "A string" \space   > "A string"
@  (print 387.9532)
=  \newline 387.9532 \space   > 387.9532
@  (print (make-hash-table))
=  \newline #<HASH-TABLE> \space   > #<HASH-TABLE>
```
 PRINT는 개행문자(\newline)로 시작하고 띄어쓰기(\space)으로 끝나는 결과물을 출력합니다. newline과 space 모두 공백(whitespace)으로 취급되므로, (escape가 아닌)Lisp object가 출력된 표현의 일부가 될수 없기에, 이는 PRINT 결과물이 다른 결과물과는 다르다는 것을 보증합니다.

 PRINT의 다른 변종들은 다양한 쓰임세를 지녔습니다. PRIN1은 PRINT처럼 행동하지만, whitespace로 감싸지 않습니다. 예를들어 연이은 조각들로부터 이름을 만들고자 할때, 이는 유용할 것입니다. PRINC는 PRIN1처럼 행동하지만, 보여는것 보단 READ를 위한 결과물을 생성합니다; 예를들어, PRINC는 문자열을 둘러싸는 쌍따옴표(quotes)를 생략하며, escape character를 출력하지 않습니다.

```lisp
@  (print ’a\ bc)
=  \newline  |A BC| \space  >  |A BC|
@  (prin1 ’a\ bc)
=  |A BC|
>  |A BC|
@  (princ ’|A BC|)
=  \newline  A BC \space   > |A BC|
```

### OPEN and CLOSE let you work with files
 보통, READ는 키보드를 읽고 PRINT는 화면에 출력합니다. 이러한 함수 모두 optional 인자를 취합니다; 인자는 READ를 위한 input stream과 PRINT를 위한 output stream을 지정합니다. stream은 무엇일까요? stream은 데이터의 근원(source)나 밑바닥(sink)이며, 보통 (그치만 절대적이지는 않은) 문자(characters)입니다. 이제부터, 저희는 tex file이 어떻게 문자 stream의 source나 sink가 될 수 있는지를 살펴볼 것입니다. 19장 [p 183]에서 저희는 몇몇 다른 가능성들을 살펴볼 것입니다.

 여러분은, 파일 이름을 인자로 취하고 stream의 방향(input이나 output)을 결정하는 keyword 인자를 취하는, OPEN 함수를 이용하여 stream을 파일로 붙일 수 있습니다. stream에 관한 작업을 종료하고 관련된 file을 닫기 위해선, CLOSE 함수를 사용합니다.

```lisp
@  (setq out-stream (open "my-temp-file" :direction :output))
>  #<OUTPUT-STREAM "my-temp-file">
@  (print 'abc out-stream)
>  ABC
@  (close out-stream)
>  T
@  (setq in-stream (open "my-temp-file" :direction :input))
>  #<INPUT-STREAM "my-temp-file">
@  (read in-stream)
>  ABC
@  (close in-stream)
>  T
```

 이 예제에서, 저희는 my-temp-file에 대한 output stream을 만들고, symbol ABC를 그 stream에 출력하였습니다. 인자는 평상시처럼 반환하지만 출력을 하지 않는 다는 것을 주목하시기 바랍니다 - 대신, 출력된 결과는 파일로 갑니다.

 다음으로, output stream을 닫고 동일한 파일에 input stream을 열었습니다. 그런 다음 저희가 file에 출력한 symbol을 읽어온 다음, input stream을 닫음으로써 끝을 맺습니다.


### Variations on a PRINT theme
 Lisp는 또한, 이러한 옵션을 제어하는 keyword 인자를 이용하여, 여러분에게 출력에 대해 더욱 세부적인 것에 관한 제어권을 주는 WRITE 함수를 제공합니다:

    Keyword Argument   Default Value           Action
    ----------------   -------------           ------
     :stream           t                       set output stream
     :escape           *print-escape*          include escape characters
     :radix            *print-radix*           include radix (base) prefix
     :base             *print-base*            set number base (rationals)
     :circle           *print-circle*          print circular structures
     :pretty           *print-pretty*          add whitespace for readability
     :level            *print-level*           limit nesting depth
     :length           *print-length*          limit items per nesting level
     :case             *print-case*            :upper, :lower, or :mixed
     :gensym           *print-gensym*          prefix uninterned symbols
     :array            *print-array*           print arrays readably
     :readably         *print-readably*        force printing to be readable
     :right-margin     *print-right-margin*    controls pretty-printing
     :miser-width      *print-miser-width*                "
     :lines            *print-lines*                      "
     :pprint-dispatch  *print-pprint-dispatch*            "

 우연하게도, 위에 keyword argument의 default value로 나온 변수들은 또한 PRINT의 연산을 제어합니다. 여러분은, PRIN1을 감싸는 LET form에서 이러한 변수들을 binding 함으로써, non-default keyword 인자로 WRITE의 효과를 얻을 수 있습니다:

    (write foo                    (let ((*print-pretty* t)
       :pretty t                        (*print-right-margin* 60)
       :right-margin 60                 (*print-case* :downcase))
       :case :downcase)              (prin1 foo))

 PRINT가 추가하는 앞에 newline이 있고 뒤에 blank가 오는걸 원치 않기에, PRINT보다 PRIN1을 사용하였습니다.

 여러분의 프로그램이 *PRINT-...* 변수를 바꾸지만, 프로그램의 어느 지점에서 default values을 보증해야할때, WITH-STANDARD-IO-SYNTAX form으로 프로그램의 일부를 감쌀 수 있습니다:

```lisp
;프로그램의 printer control을 정의한다.
(setq *print-circle* t)
(setq *print-array* nil)
(setq *print-escape* nil)
...
;위에서 설정한 걸 출력한다.
(print ...)
...
;default printer control 설정으로 되돌린다.
(with-standard-io-syntax
   ...
   ;standard setting으로 출력한다음,
   ;위에서 설정한 이러한 것들을 덮어쓴다.
   (print ...)   
   ...)
;WITH-STANDARD-IO-SYNTAX form 밖에선,
;예제 상단에 있는 SETQ form에 의해
;구축establish된 print 설정을 또 다시 해야합니다.
```

## Lesson 12 - Essential Reader Macros

### The reader turns characters into data
 저희는 Lesson 11에서 Lisp reader가 constituent characters를 symbol과 number로 모으gather고 macro characters가 lists, strings, quoted forms, comments를 다루기 위해 reader를 제어하는 것을 보았습니다. 이 모든 경우에, reader는 문자(characters)를 데이터로 바꿉니다 (좀 더 분명히 하자면, comment는 "데이터가 아닙니다".)


### Standard reader macros handle built-in data types
 지금까지, 저희는 Lisp의 standard syntax만을 보았습니다. 이는 reader에 의해 구현되며, readtable에 의해 제어됩니다. reader는 readtable에 저장되어있는 정보를 따라 characters를 처리합니다.


### User programs can define reader macros
 Lisp는 readtable를 *readtable* 변수를 통해 노출expose시키며, readtable에 있는 항목을 다루는 몇몇 함수를 제공합니다. 여러분은 이를 이용하여 Lisp reader의 행동을 바꿀 수 있습니다. 다음 예제에서, 저희는 syntax를 변화시킬 것이며 [와 ]를 이용하여 quoted(즉, 평가되지 않는) list를 작성할 수 있을 것입니다.

```lisp
;이는 틀렸습니다:
@  (1 2 3 4 5 6)
>  Error: 1 is not a function
;대신에 이렇게 해야 합니다:
@  '(1 2 3 4 5 6)
>  (1 2 3 4 5 6)

;새로운 synex를 정의하여
; '(1 2 3 4 5 6)
;대신에
; [1 2 3 4 5 6]
;이처럼 작성할 수 있게 합시다.
@  (defun open-bracket-macro-character (stream char)
     `',(read-delimited-list #\] stream t))
>  OPEN-BRACKET-MACRO-CHARACTER
@  (set-macro-character #\[ #'open-bracket-macro-character)
>  T
@  (set-macro-character #\] (get-macro-character #\)))
>  T
;이제 테스트 해봅시다:
@  [1 2 3 4 5 6]
>  (1 2 3 4 5 6)
```

 처음 저희는 (1 2 3 4 5 6)을 평가하려 했습니다 - 1은 함수가 아니기에, 이는 옳지않습니다. 저희가 실제로 해야하는 것은 list를 quote하는 것입니다. 그러나 저희가 이를 자주 해야한다면, 더욱 편리한 syntax를 원할 것입니다. 특히, '(...)과 같은 행동을 하는 [...]를 원합니다.

 이를 수행하기 위해, 이의 인자를 평가하지 않는 specialized list reader macro 함수를 정의해야 합니다. 저희는 reader가 [ 문자를 만나면 함수가 호출되도록 할 것입니다; 함수는 ] 문자를 만나면 list를 반환할 것입니다. 모든 reader macro function은 두개의 인자에 의해 호출됩니다: input stream과 macro를 작동시키는 문자.

 Lisp가 한정된delimited list를 읽도록 설계된 함수를 지녔기에, 저희 reader macro는 매우 단순합니다. READ-DELIMITED-LIST는 하나의 인자를 기다립니다 - 읽고있는 list를 종료시키는 문자. 다른 두개의 인자는 optional입니다 - input stream과 (reader macro 함수에서 사용될땐 보통 T로 설정되는)flag. READ-DELIMITED-LIST는 terminating character를 만나기 전까지 input stream에서 objects를 읽은다음, 모든 objects를 list로 반환합니다. 자체적으로, 이는 평가를 막는것suppressing을 제외한 저희가 필요한 모든것을 수행합니다.

 Lesson 3에서 봤던것 처럼, QUOTE (혹은  ')는 평가를 막습니다. 그러나 저희는 '(READ-DELIMITED-LIST ...)를 사용할 수 없습니다; 이는, 저희가 quote하고자 하는 form을 얻기 위해 저희가 평가하고자 하는, form의 평가를 막습니다... 대신, quoted form에 대해 선택적인 평가를 요구하는 `(Lesson 8 참조)를 이용하였습니다.

 저희 OPEN-BRACKET-MACRO-CHARACTER의 정의는 form을 평가하고 quoted된 결과를 반환하기 위해
`',form
을 이용하였습니다.

 Lisp는 프로그래머를 위해 6개의 문자를 예약하였습니다:
 [ ] { } ! ?

 여러분은 이중 일부나 전체를 Lisp의 중재없이 macro characters처럼 정의할 수 있습니다. 그러나, 다른 프로그래머와 코드를 공유한다면, 분쟁을 조심해야 합니다.

 
 [prefixed-names.gif]: ./prefixed-names.gif
 [rename-package.gif]: ./rename-package.gif