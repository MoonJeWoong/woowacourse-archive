# 나의 늘어만 가는 기술 부채 목록...

# LV1

- 자바의 JVM / 상수풀
## 자바의 문자열 강의 재수강
- 레퍼런스
    - [String.intern() 개념](https://simple-ing.tistory.com/3)
    - [String.intern in native (어렵당...)](https://www.latera.kr/blog/2019-02-09-java-string-intern/)
    - [String 생성 방식 별 소요 시간 실험 결과](https://blog.ggaman.com/918)

  - 바이트 코드를 뜯어보며 해당 코드들의 동작 방식 차이를 알아보자
      - LDC는 JVM 명령어 set 중 하나로 아이템을 런타임 상수풀에 push 한다는 명령어이다.
          - [공식문서](https://docs.oracle.com/javase/specs/jvms/se8/html/jvms-6.html#jvms-6.5.ldc)
      - bipush는 즉시 바이트가 int 값으로 부호 확장된 이후 operand stack으로 push된다.
      - invokestatic 은 클래스의 static 메서드를 invoke한다.
      - 정수형 데이터에서 byte, short 범위에 해당하는 정수 리터럴들은 런타임 상수풀에 push되지 않는다.
          - short 범위를 벗어나는 정수 리터럴 값들이 사용될 경우에만 LDC 명령어를 통해 상수풀에 할당된다.
          - [레퍼런스](https://stackoverflow.com/questions/29875430/why-in-jvm-integer-is-stored-as-byte-and-short)
          - [byte,short,Integer가 precached된 이유?](https://stackoverflow.com/questions/20394116/java-why-is-constant-pool-maintained-only-for-string-values)
          - byte와 short는 미리 캐싱이 되어있기 때문에 바로 operand stack에 push된다.(bipush, sipush)
  - chat GPT 센세에게 물어본 내용들 정리
      - constant pool
          - Constant pool은 Java 클래스 파일에서 상수를 저장하는 테이블입니다. 이 테이블에는 클래스의 이름, 상수 필드 및 메소드의 이름, 타입, 문자열 값 등이 저장됩니다.
          - 예를 들어, 클래스의 static final 상수들은 이 constant pool에 저장됩니다. 이를 통해 클래스 파일에서 상수들을 빠르게 참조할 수 있습니다.
          -  Constant pool은 클래스 파일에서 상수를 저장하는 테이블입니다.
      - String pool
          - String pool은 Java에서 문자열 리터럴을 저장하는 특별한 영역입니다. 문자열 리터럴은 프로그램에서 동일한 문자열이 여러 번 사용될 경우 메모리를 절약하기 위해 하나의 인스턴스만을 유지합니다.
          - 즉, String pool은 문자열 리터럴의 중복을 제거하여 메모리 사용을 최적화합니다.
          - String pool은 문자열 리터럴을 저장하는 특별한 영역입니다.
      - 자바 바이트 코드에서 bipush, sipush, LDC를 이용한 정수 저장 방식에는 어떤 차이가 있어?
          - 자바 바이트 코드에서 bipush, sipush, LDC는 모두 상수를 저장하는 데 사용되는 명령어입니다. 이들 명령어는 정수를 저장하는 데 사용됩니다.
          - bipush는 1바이트 정수 상수를 스택에 직접 push하는 명령어입니다. 즉, -128에서 127까지의 값을 직접 스택에 push할 수 있습니다. bipush는 바이트 코드 크기를 줄일 수 있으며, 작은 정수 상수를 push하는 데 유용합니다.
          - sipush는 2바이트 정수 상수를 스택에 직접 push하는 명령어입니다. 즉, -32768에서 32767까지의 값을 직접 스택에 push할 수 있습니다. bipush와 달리 더 큰 범위의 정수를 push할 수 있습니다.
          - LDC는 상수 풀(constant pool)에서 정수 상수를 로드하여 스택에 push하는 명령어입니다. 상수 풀에 저장된 값은 4바이트 또는 8바이트 정수일 수 있습니다. LDC는 모든 정수 상수를 push할 수 있습니다. 또한, LDC는 문자열, 클래스, 인터페이스 등 다양한 상수 타입을 로드할 수 있습니다.
          - 따라서, bipush는 작은 정수 상수를 push할 때 사용하고, sipush는 더 큰 범위의 정수 상수를 push할 때 사용합니다. LDC는 모든 정수 상수를 push할 수 있으며, 다른 상수 타입도 로드할 수 있습니다.

      - 자바 Integer pool에는 어떤 값이 저장돼?
          - 자바 Integer pool은 자주 사용되는 int 값들을 캐시하는 영역입니다. 이를 통해 메모리를 절약하고, 같은 값을 가진 Integer 객체를 공유하여 성능을 개선합니다.
          - Integer pool에는 -128부터 127까지의 int 값들이 저장됩니다. 이 범위 내에서 Integer.valueOf() 메서드를 사용하여 Integer 객체를 생성하면, 이 값들에 대해서는 이미 생성된 객체를 반환합니다.
          - 그 외 리터럴은 Integer pool에 저장되지 않습니다. 이 값은 해당 코드가 실행될 때 메모리의 스택 영역에 임시적으로 저장됩니다. 따라서 동일한 값을 가지는 Integer 객체를 생성하려면 Integer.valueOf(4000)을 사용해야 합니다
          - [레퍼런스](https://escapefromcoding.tistory.com/127)

      - 자바에서 string pool과 integer pool 작동 방식 차이는 뭐야?
          - 자바에서 String Pool과 Integer Pool은 각각 다른 방식으로 작동합니다.
          - String Pool은 String 객체를 생성할 때 생성자에 전달된 문자열 값이 이미 String Pool에 존재하는 경우, 새로운 객체를 생성하지 않고 기존 객체의 참조를 반환합니다. 즉, 문자열 리터럴이 이미 String Pool에 존재하는 경우, 해당 리터럴을 참조하는 String 객체를 생성하지 않고, 이미 존재하는 String 객체를 참조하게 됩니다.
          - Integer Pool은 Integer 객체를 생성할 때, 생성자에 전달된 값이 -128에서 127사이의 정수인 경우, 새로운 객체를 생성하지 않고 기존 객체의 참조를 반환합니다. 이는 Integer 객체의 생성 비용을 줄이고, 성능 향상을 도모하기 위한 것입니다.
          - 따라서, String Pool과 Integer Pool의 작동 방식은 유사하지만, 사용되는 데이터 타입과 값의 범위 등에 따라 차이가 있습니다.
      - Integer pool에는 String Pool 과 다르게 새로운 값을 추가할 수 없는거네?
          - 네, 맞습니다. Integer pool에는 런타임 시점에 값을 추가하거나 수정할 수 없습니다. Integer pool은 자바에서 미리 정의된 일부 정수 값만 저장할 수 있습니다.
          - 따라서 Integer pool에 존재하지 않는 int 값은 항상 새로운 객체로 생성됩니다.

      - 자바7 ~ 8 사이에서 변경된 메모리 영역에 관련해서 알아봐야 할 듯 하다.
          - [레퍼런스1](https://8iggy.tistory.com/230)
          - [레퍼런스2](https://8iggy.tistory.com/229)
          - [레퍼런스3](https://www.nakjunizm.com/2017/07/25/String_Pool/)

  - 문자열에서 intern 메서드가 왜 있지? -> 스트링풀이 왜 있지? -> 없으면 어떻게 되지?
      - gc가 뭐지? -> 왜 예전에는 String이 gc 대상이 아니었지?

  - 자바에서 문자열 덧셈 연산에서 java 9 이전까지는 StringBuilder를 사용해 진행됐다면, 이후부터는 StringConcatFactory.makeConcatWithConstants를 사용한다.
    - 왜 최적화를 진행했을까?
    - StringBuilder로 했는데 왜 이젠 아니지? -> StringBuilder가 매번 생성 되는구나 -> GC가 돌 수 있구나(성능이슈)
    - 내부적으로 buffer로 처리하고 StringBuilder가 생성되지 않는구나 -> 어느정도 차이가 있을까?
    - 동시성 이슈를 해결하려고 했다는 것 같은데... -> 뭘 해결하려 한거지? -> StringBuilder는 가변상태를 만든다. -> 그러면 StringBuffer를 사용하면 되는거 아닌가?
      - [레퍼런스](https://12bme.tistory.com/42)

## Constant Pool

- 자바 클래스 파일 내부의 Constant Pool?
    - [oracle 레퍼런스](https://blogs.oracle.com/javamagazine/post/java-class-file-constant-pool)
    - [string pool vs constant pool](https://stackoverflow.com/questions/23252767/string-pool-vs-constant-pool)
    - [JVM stack & frames](https://sanghoonly.tistory.com/62)
    - [오라클 JVM Frames](https://docs.oracle.com/javase/specs/jvms/se8/html/jvms-2.html#jvms-2.6)
    - [오라클 JVM Instruction set](https://docs.oracle.com/javase/specs/jvms/se8/html/jvms-6.html#jvms-6.5.dup)
    - [런타임 constant pool](https://docs.oracle.com/javase/specs/jvms/se7/html/jvms-2.html#jvms-2.5.5)
    - 특정 클래스 파일을 실행할 때가 오면 JVM은 클래스 파일을 찾아내고 load 한다.
    - loading 프로세스는 클래스 파일 내 다양한 필드들을 Parsing 하는 것과 Parsing된 데이터들을 JVM의 Method 영역에 형식을 맞춰서 저장하는 것을 수반한다.
    - 메서드 영역은 변수, 메서드, 다른 아이템들에 대해 전 쓰레드가 공유하는 영역이다.
    - 클래스 파일 format은 자바 릴리즈 버전에 따라 계속 변화해왔다.
        - 파일 헤더 : 자바 클래스 파일이 생성된 자바 버전을 나타내는 바이트
        - 상수풀 : constant pool이라 불리는 중요한 영역
        - 추가적인 데이터 필드, 메서드, 일련의 속성들 등으로 이루어져 있다.
    - Introducing the Constant Pool
        - constant pool : 클래스 파일의 가장 중요한 영역 중 하나로 클래스에 대한 Symbol 분류표로서 제공되는 항목들의 집합이다.
        - constant pool은 참조되는 클래스들의 이름, 문자열, 숫자 상수들의 초기값, 그리고 적절한 명령 수행에 필수적인 기타 데이터들을 포함한다.
        - Constant Pool을 보기 위해서는 JDK에 포함된 javap 클래스 파일 디셈블러를 사용해라.
            - 결과 값을 보면 첫번째 index는 단순히 항목들의 entry 숫자이다.
            - 두번째 컬럼은 entry의 타입을 명시한다.
            - 세번째 컬럼은 entry의 값을 포함하고 있다.
            - // 문자 이후 내용들은 javap가 무엇이 사용되고 있는지 표시하기 위해 얘기하고 있는 내용이다.
        - 아마도 이 결과 리스팅에 대해서 가장 충격적인 내용은 문자열 데이터들을 표시하기 위해 자바 클래스 내부에서만 사용되는 UTF-8타입의 entry 빈도수일 것이다.
        - 클래스 이름, 메서드 이름, 메서드 시그니처 등을 모두 utf-8 문자열로 저장한다.
    - 개인적인 정리
        - 자바에서 constant pool은 기본적으로 클래스 파일 내부에 존재하는 클래스 이름, **문자열 / 숫자 상수 초기값**, 그리고 기타 데이터들을 symbol 테이블로 저장한다.
        - 여기에서 문자열 리터럴들은 모두 constant pool에 utf-8로 타입으로 저장된다.
        - 정수형 리터럴 중 byte, short에 해당되는 값들은 미리 상수풀에(상수풀에 저장되는지는 아직 몰루?) 캐싱되어 있다. (자바버전 업그레이드에 따라 과거에 비해 변경된 사항임)
            - 그래서 byte, short에 해당하는 리터럴 값들은 바이트 코드에서 bipush/sipush 를 사용해 값이 operand stack에 push된다.
            - 그 이외 범위의 정수 리터럴들은 LDC를 사용해 런타임 상수풀에 저장되어 있는 리터럴 값을 operand stack에 push한다.(?몰루)
            - 40000 이라는 정수 리터럴은 미리 캐싱되어 있지 않기 때문에 컴파일 시 생성되는 constant pool에 등록된다.
            - 이후 LDC 컴파일 명령어를 통해 runtime constant pool에 저장되어 있는 40000이라는 값을 operand stack에 넣어준다.
        - runtime constant pool은 기존 클래스와 인터페이스 별 constant pool로부터 derived 된다.
        - runtime constant pool은 클래스 혹은 인터페이스의 constant pool 내 모든 symbolic reference들을 resolving하여 얻어지기 때문에
            - runtime constant pool은 정확하게 클래스 / 인터페이스의 constant pool이 가지지 않고 있는 일부 상수들도 포함하고 있다.
        - DUP 명령어는 operand stack에 최상단에 위치한 value를 복사해 다시 operand stack에 push 한다.
        - new 명령어는 새로운 객체를 생성하고 객체에 대한 레퍼런스를 operand stack에 push 한다.

- LinkedList 구현 코드 내부 clear Method 달린 주석 중
  - // Clearing all of the links between nodes is "unnecessary", but:
    // - helps a generational GC if the discarded nodes inhabit
    //   more than one generation
    // - is sure to free memory even if there is a reachable Iterator
  - Iterator가 접근 가능한 경우가 뭘까...?
  - 그 경우에는 왜 삭제해도 되는 거지?


## 블랙잭 미션 구현 중 Enum 내부 메서드 구현은 어떤식으로 하는게 좋은지 의문이 듦
- [레퍼런스](https://velog.io/@nawhew/%EC%9E%90%EB%B0%94-Enum%EC%9D%84-%EB%8D%94-%EC%9E%98%EC%93%B0%EA%B8%B0-%EC%9C%84%ED%95%9C-%EB%B0%A9%EB%B2%95)
- 객체 캐싱의 개념도 한 번 공부해보자.
- assertj vs junit : assertj gradle dependencies 설정 안되어 있을 수도 있으니 추가해주기.
- 이번 미션에서 왜 객체의 인스턴스 변수 개수를 2개로 제한했을까?
  - [레퍼런스](https://limdingdong.tistory.com/13)
- 그냥 new ArrayList<>() 해주는 거랑 Collections.emptyList()를 사용하는 것이랑 무슨 차이가 있나?
- 한 객체에서 여러 개의 생성자를 여는 것이 괜찮을까? 이번 블랙잭 학습 자료에 나와있으니 알아보자...
- Hand 객체 내부에서 score 합산 메서드 구현시 사용한 stream reduce 에서 함수형 인터페이스 사용하는데 나중에 정리해보기
- 
