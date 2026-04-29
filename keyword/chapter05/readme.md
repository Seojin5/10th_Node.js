- 환경 변수
    
    : 코드 밖에서 설정하는 설정값
    
    코드에 데이터베이스 비밀번호와 같은 정보를 올려 버리면, 깃에 올라가 보안이 취약해지고, 환경이 바뀔 때마다 코드를 수정해야 한다. 개발/운영 구분도 어렵기 때문에, 환경 변수를 사용한다
    
    .env 파일에
    
    - DB 연결 정보
    - API 키
    - JWT secret
    - 포트 번호
    - 외부 서비스 URL
    
    와 같은 정보들을 저장한다
    
- CORS
    
    **C**ross-**O**rigin **R**esource **S**haring: 교차 출처 리소스 공유
    
    출처에 있는 리소스에 접근할때 안전한지 판단하기 위해 브라우저가 서버와 직접 통신하는 방법
    
    예시) 브라우저에서 동영상 플랫폼의 동영상을 가져올때
    
    국가 날씨 데이터베이스의 날씨를 표현하는 앱
    
    출처 구성 요소 : 프로토콜, 도메인, 포트
    
    → 셋 중 하나라도 다르면 CORS 에러 발생
    
    SOP (동일 출처 정책): 서로 다른 출처일때 요청과 응답 차단
    
    → 안전하지만 실제 서비스에서는 유연하지 않음
    
    CORS: 출처가 다른 서버 간의 리소스 공유 허용
    
    CORS 정책 생긴 이유
    
    이전에는 프론트/백 구분하지 않고 한번에 구성 → 다른 출처로 요청: 의심
    
    요즘에는 클라이언트가 API를 직접 호출하는게 당연해짐: 클라이언트와 API가 다른 도메인에 존재
    
    → 출처가 달라도 요청/응답을 처리하기 위해 서버에 리소스 호출이 허용된 출처를 명시해줌
    
    CORS 에러 대응
    
    1. 서버에서 Access-Control-Allow-Origin 헤더 설정
        
        'Access-Control-Allow-Origin': <origin> | *
        
        *로 설정하면 보안이 취약해지기 때문에 허용할 출처를 세팅하는 방법이 더 좋음
        
    2. 프락시 서버 사용
        
        애플리케이션이 리소스를 직접 사용하지 않고, 프락시 서버를 이용해 리소스로의 요청을 전달하는 방법
        
    
- DB Connection, DB Connection Pool
    
    **DB Connection**
    
    애플리케이션과 데이터베이스 서버가 통신할 수 있도록 하는 기능
    
    데이터베이스 드라이버와 DB 연결 정보를 포함하는 URL 필요
    
    데이터베이스와 연결 시 Connection 개체를 반환
    
    **DB Connection Pool**
    
    Connection 개체의 반복적인 생성과 해제를 줄이고 효율적인 DB 연결을 위한 데이터베이스 연결의 캐시
    
    애플리케이션 시작 시에 미리 일정 수의 Connection 개체를 만들어 Pool에 보관
    
    이후 DB 작업이 필요할때 Pool에서 Connection 개체 사용하고 작업 종료 시 Pool에 반환
    
    장점
    
    1. 성능 향상
        
        Pool에 연결을 유지하고, 요청 발생 시 재사용으로 응답시간 ⬇️, 에플리케이션 성능 ⬆️
        
    2. 자원 관리
        
        연결 생성/유지 시 필요한 자원을 최적화
        
        : 불필요한 연결 X, 연결 재사용 → 메모리, CPU의 효율적인 관리 가능
        
    3. 동시성 관리
        
        동시에 여러 연결을 처리할 수 있는 연결 제공 → 사용자들이 동시에 접속해도 안정적으로 처리할 수 있음
        
    4. 연결 풀링
        
        연결의 개수를 제한하고, 개수를 초과하는 요청이 들어올 시 대기하게 만들어 서버의 과부하 방지
        
    5. 커넥션 오버헤드 감소
        
        반복적인 DB 연결/해제 작업에 따른 추가적인 비용과 부하가 감소함
        
    
    단점
    
    1. 리소스 사용
        
        연결을 미리 생성하므로 메모리를 일정 부분 소비해야 함
        
    2. 설정/관리 복잡
        
        다양한 환경에서 최적의 성능을 발휘하기 위해서 관리와 모니터링 필요, 초기 설정에 시간을 써야 함
        
    3. 커넥션 누수
        
        애플리케이션게서 올바르지 않은 연결을 반환하거나 예외 발생 시, 커넥션 풀에서 제대로 반환하지 못할 수 있음 → 커넥션 누수 발생
        
    
    커넥션 풀을 크게 설정했을 때와 작게 설정했을 때의 장단점이 다르므로 최적의 성능을 위해서는 커넥션 풀의 크기를 적절하게 조절해야 함
    
- 비동기 (async, await)
    
    비동기?
    
    : asynchronous, 작업 실행 시, 특정 코드의 로직이 완료되지 않았더라도 다음 코드를 실행하는 방식
    
    오래 걸리는 작업을 기다리지 않아 시간을 절약하고, 병렬적인 작업 처리 가능
    
    JavaScript는 싱글 스레드 기반이기 때문에 비동기 처리 방식이 필요하다
    
    **Promise** : 비동기 함수의 결과를 담고 있는 객체
    
    상태
    
    - 대기(Pending): 비동기 함수 시작 전
    - 성공(Fulfilled): 비동기 함수가 성공적으로 완료
    - 실패(Rejected): 비동기 함수가 실패
    
    대기에서 상태가 바뀔때 then(), catch()를 사용해서 각각 성공/실패 상태의 Promise를 처리할 수 있음
    
    → 하지만 then() 체인을 길게 이어나가면 코드 가독성 떨어지고, 에러가 어디서 났는지 확인이 어려움
    
    async&await은 ES2017부터 추가된 JavaScript의 비동기 처리 방식 중 하나이며, 비동기 코드를 동기 코드처럼 작성할 수 있어서 가독성이 좋아지고 에러 처리가 간단해짐
    
    **async**
    
    함수의 앞에 붙여서, 해당 함수가 비동기 함수임을 나타냄
    
    → 반환 값이 Promise 생성 함수가 아니어도, 반환되는 값을 Promise 객체에 담음
    
    **await**
    
    비동기 함수의 실행 결과를 기다리는 키워드
    
    해당 비동기 작업이 완료될때까지 코드 실행을 중지하고 결과를 기다린 다음, 결과를 반환
    
    Promise 객체가 완료될깨까지 코드 실행을 일시중지 → try-catch 블록 안에서 사용해 에러처리 가능
    
    fetch에서 네트워크 에러 발생 시 await 이후의 코드는 실행되지 않으며 catch 블록으로 제어가 넘어가서 에러를 처리할 수 있다
    
    async 함수 안에서만 사용할 수 있는 문법
    
- try/catch/finally
    
    예외 처리
    
    **try…catch 문**
    
    실행할 코드블럭을 표시하고, 예외가 발생할 경우의 응답을 지정함
    
    문법
    
    : try_statements - try 블록에서 실행될 선언들
    
      catch_statements - catch 블록에서 실행될 선언들
    
      exeption_var - 블록과 관련된 예외 객체를 담기 위한 식별자
    
    ‘e’로 사용됨
    
    try-block에서 예외가 발생하면 e가 예외 값을 가짐 → 이 식별자를 이용해서, 발생한 예외에 대한 정보를 얻을 수 있음
    
    catch-block의 스코프 내에서만 사용 가능
    
      finally_block - 선언이 완료된 후 실행될 선언들: 예외 발생 여부와 상관없이 무조건 실행됨
    
    try 블록과 catch 블록 실행 후 실행될 명령문 포함
    
    1. 무조건적 catch문
        
        try-block에서 예외가 발생하면 catch-block이 실행됨
        
        ```jsx
        try {
            throw "myExeption";
        }
        catch {
            logMyErrors(e); // 오류 처리기에 예외 객체를 전달
        }
        ```
        
    2. 조건적 catch문
        
        if…else문과 결합해서 생성
        
        ```jsx
        try {
            ...
        } catch(e) {
            if (...) {
            
            } else if (...) {
            
            } else {
                // 지정되지 않은 예외를 처리하기 위한 상태
            }
        }
        ```
        
    3. 중첩 catch문
        
        ```jsx
        try {
            try {
                ...
            } 
            catch {
                ...
            }
            finally {
                ...
            }
        } 
        catch(e) {
            ...
        }
        ```
        
        내부 try 블록에서 예외를 포착한다면, 외부 catch 블록은 작동하지 않음
        
    
    finally 블록에서 반환
    
- Interface(인터페이스)
    
    → 일반적으로 타입 체크를 위해 사용됨: 변수, 함수, 클래스에 사용 가능
    
    Typescript는 인터페이스를 지원하며, 선언된 프로퍼티나 메소드의 구현을 강제해서 일관성을 유지하게 한다
    
    - 변수의 타입으로 사용
        
        인터페이스를 타입으로 선언한 변수는 해당 인터페이스를 준수해야 한다
        
        ```tsx
        // 인터페이스 정의
        interface Todo {
            id: number;
            content: string;
            completed: boolean;
        }
        
        let study: Todo;
        study = {id: 1, content: typescript, completed: false};
        ```
        
    - 함수의 타입으로 사용
        
        인터페이스를 타입으로 선언한 함수는 선언된 파라미터와 타입 리스트를 준수해야 한다
        
        ```tsx
        interface Func {
            (num: number): number;
        }
        
        const SquareFunc: Func = function (num: number) {
            return num*num;
        }
        ```
        
    - 클래스
        
        클래스 선언 시 implements의 뒤에 인터페이스를 선언하면 해당 클래스는 지정된 인터페이스를 반드시 구현해야 한다
        
        → 일관성 유지 가능
        
        인터페이스는 클래스와 유사하나(프로퍼티와 메소드 보유), 직접 인스턴스를 생성할 수 없다는 점이 다르다
        
    
- Type Assertion(as 키워드)
    
    Typescript에만 존재하는 키워드 (JavaScript X)
    
    타입을 지정해주는 키워드 : (오브젝트) as (타입)
    
    ```tsx
    const num = 10;
    const value = num as number;
    ```
    
    unknown과 같은 타입을 사용했는데 .length를 사용해야 할 때, type assertion을 이용해서 문자열이라고 강제로 지정하고 쓸 수 있다
    
    잘못 사용하면 일어나야 할 버그를 숨겨버려서 좋지 않다
    
    **사용하는 상황**
    
    1. DOM
        
        ```tsx
        const input = document.getElementById("name") as HTMLInputElement;
        input.value = "hello";
        ```
        
    2. union 타입 좁힐 때
        
        ```tsx
        function printLength(value: string | number) {
          console.log((value as string).length);
        }
        ```
        
    3. API 응답 처리 : 서버 응답 타입을 확신할 때