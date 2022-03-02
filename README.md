# 1-1. Overview

  *라쿠텐에서 제공한 API를 사용하여  구현한 파일 저장소이며 4인 1조 팀 프로젝트 입니다.*

   파일 저장소의 핵심은 `유효 기간`입니다. 유효 기간이 남아 있는 경우에는 상세 페이지로 이동 가능하고, URL을 복사할 수 있는 등 다양한 기능을 체험할 수 있습니다. 반면 유효기간이 만료된 파일의 경우 더이상 URL에 접근할 수 없도, 파일을 다운로드 할 수도 없도록 하였습니다. 유효기간은 실시간 API로 받아오고 있습니다.
   
   그 중 여기저기에서 많이 사용되는 함수들은 `utils 함수`로 분리하였습니다. 데이터 용량을 계산하는 함수, 유효기간을 계산하는 함수, 이미지를 받아오는 함수 등이 모두 `utils 함수` 에 포함됩니다. 메인 페이지와 상세페이지에서는 이러한 함수를 통해 데이터를 관리하고 있으며, `중복 코드를 최소화`하였습니다.

 

🔗 [소스 코드 확인하기](https://github.com/tae100k/rakuten-symphony-transfer)

🔗 간단하게 gif로 보기 👇

- [리스트 페이지]

    ![메인 시연 영상](https://user-images.githubusercontent.com/78027252/156385291-fad4c565-a3fc-42c5-9446-f78b0e48d7b9.gif)

- [상세 페이지]

    ![Untitled](https://user-images.githubusercontent.com/78027252/156385316-9e80a1d2-b5f9-4db3-a151-c8ea7ab503ea.png)


# 1-2. 기술 스택

- React, TypeScript, Styled-Components

# 2-1. 맡은 역할과 과정

1. **데이터 바인딩**
    1. 목표 :  axios를 사용해 api를 받아와서 데이터 바인딩
    2. `문제`: CORS 에러가 발생 → [해결⭕](https://www.notion.so/React-TS-2cf7651ecb474f48ac8341f6f707782c)
    3. 과정 : axios를 통해 라쿠텐의 데이터를 받아온 후, utils함수를 사용하여 적재적소에 바인딩
    4. utils함수를 사용하여 파일 크기, 개수, 유효 기간 등 표시
    
2. **페이지 이동**
    1. 목표 : 아이템 클릭시 상세페이지로 이동 
    2. `문제`: 
        1. `<Link to>`를 사용하면 a태그라서 스타일이 달라짐
            - `navigate`를 사용하여 [해결⭕](https://www.notion.so/React-TS-2cf7651ecb474f48ac8341f6f707782c)
        2. URL링크를 클릭하면 클립보드로 복사되어야 하는데 페이지가 바뀜 
            - 이벤트 버블링 해제로 [해결⭕](https://www.notion.so/React-TS-2cf7651ecb474f48ac8341f6f707782c)
    3. 과정 : URL클릭 시 navigate로 상세페이지를 표시

1. **정보 표시/보호**
    1. 목표 : 유효 기간에 따라 정보를 표시하기도 하고, 감추기도 함 
    2. 과정 
        - 팀원이 만든 utils 함수를 사용하여 유효기간이 만료됐는 지 여부 확인. 유효기간이 남았다면 제목 아래 url의 도메인과 상세페이지 경로 표시. 반면, 유효기간이 만료되었다면 ‘만료됨’이라고 표시하여 정보 숨김.
    
2. **클립보드 복사**
    1. 목표 : 유효기간이 남아있는 url 클릭 시 클립보드에 해당 내용을 복사하고, alert창 표시
    2. 과정 : `document.execCommand('copy')` 를 사용하여 url을 클릭 시 해당 내용을 복사. 
    
3. **표 스타일링**
    1. 목표 : 표 안에 있는 URL주소가 너무 길어져도 표 스타일링 유지
    2. 과정
    - 길어질 수 있는 칸의 너비를 고정 시킨 후 `word-break:break-all`; 속성을 부여. 너비를 초과하면 다음 줄로 내용이 넘어가도록 함.

# 2-2. 프로젝트하면서 마주한 에러들

## CORS 에러

- *CORS에러는 무엇인가?*
    
     기본적으로 CORS 정책을 위반했기 때문에 생기는 에러이다. CORS는 `Cross-Origin Resource Sharing`의 약자이며, ‘`Origin`’은 서버의 protocol, host부터 포트번호까지 합친 것을 의미한다. 두 개의 출처가 서로 다르다고 판단하면 CORS 정책을 위반했다고 하여 CORS에러가 발생한다. 참고로 서버는 CORS를 위반하더라도 정상적인 응답을 준다. 응답의 파기는 브라우저에서 발생한다.
    

- *CORS에는 왜 있는가?*
    
    출처가 다른 두 개의 어플리케이션이 마음대로 소통할 수 있는 환경은 위험한 것이다. 다른 출처의 어플리케이션이 서로 통신을 하게 둔다면 악의를 가진 사용자가 소스 코드를 구경한 후, [CSRF(Cross-Site Request Forgery)](https://ko.wikipedia.org/wiki/%EC%82%AC%EC%9D%B4%ED%8A%B8_%EA%B0%84_%EC%9A%94%EC%B2%AD_%EC%9C%84%EC%A1%B0)나 [XSS(Cross-Site Scripting)](https://ko.wikipedia.org/wiki/%EC%82%AC%EC%9D%B4%ED%8A%B8_%EA%B0%84_%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8C%85)를 통해 중간에 개입하여 해당 어플리케이션에서 코드가 실행된 척하며 사용자의 정보를 탈취할 수 있기 때문이다. 
    
- *Same origin의 기준은?*
    

    ![Untitled (1)](https://user-images.githubusercontent.com/78027252/156385453-8f932bb5-67f8-45a5-bdaf-d52976dc76bc.png)



- *CORS에러를 어떻게 해결하는가?*
    
    프론트엔드 개발자는 대부분 웹팩과 **`webpack-dev-server`**
    를 사용하기 때문에 해당 라이브러리가 제공하는 프록시 기능을 사용하면 된다. 브라우저가 같은 출처로 인식하도록 웹팩이 요청을 프록싱하여 해결해주는 것이다.
    
    [package.json]
    
    ```html
    {	
    	(...),
    	"proxy": "http://localhost:4000"
    }
    ```
    


## Event Bubbling

- *Event Bubbling/capture는 무엇인가?*
    
     이벤트가 발생했을 때 해당 이벤트가 더 상위의 화면 요소들로 전달되어 가는 특성을 의미한다. 여기서 상위의 화면 요소란 트리 구조상 한 단계 위에 있는 HTML요소를 의미한다.  반대로 특정 이벤트가 발생했을 대 최상위 요소인 body태그에서 해당 태그를 찾아가는 것을 `event capture`라고 한다.
    
    - 이벤트 버블링
    
    ![Untitled (2)](https://user-images.githubusercontent.com/78027252/156385784-9af47b78-faf7-4127-a769-30c1bff4744e.png)

    - 이벤트 캡처
    
   ![Untitled (3)](https://user-images.githubusercontent.com/78027252/156385793-b695fd32-3a55-4109-8fac-2ee0d32b0d78.png)


- *Event Bubbling은 어떻게 해결하는가?*
    
    `event.stopPropagation()` 을 적어주면 해당 이벤트가 전파되는 것을 막아준다. 이벤트 버블링의 경우에는 상위 요소로 전달되지 않고, 이벤트 캡처의 경우 하위 요소로 전달되지 않는다.
    
    ```jsx
    const logEvent = (event) => {	event.stopPropagation(); }
    ```
    
    하지만 매번 새롭게 아이템이 추가될 때마다 `event.stopPropagation()`을 적어주는 것이 번거로울 수 있다. 이럴 때 사용하는 것이 `이벤트 위임`이다.  `이벤트 위임`이란 요소마다 각각 이벤트 리스너를 달지않고, 요소들의 공통 조상에 리스너를 달아 여러 요소를 한꺼번에 다룰 수 있는 방법이며, 예시는 다음과 같다.
    
    ```jsx
    const itemList = document.querySelector('.itemList');
    itemList.addEventListener('click', function(event) {
    	alert('clicked');
    });
    ```
    

# 2-3. 회고

- **코끼리를 삼킨 보아뱀인가, 모자인가?**
    
    현 팀원들과 같이 하는 마지막 그룹 프로젝트였다. 우리 팀원들은 다들 훌륭하셔서 옆에서 배울  게 정말 많았다. 또한, 팀프로젝트의 특성 상 페어 프로그래밍, 코드 리뷰 등을 하면서 다른 사람들의 코드를 읽을 기회가 많아서 견문을 넓힐 수 있는 기회였다. 
    
    수십개의 장점 중 가장 좋았던 것은 협업을 연습할 수 있었던 것이다. 사용하는 용어가 달라서, 인식이나 개념이 달라서 개발자들은 **서로의 생각을 동기화**하는 것이 필요하다.  프로젝트 하면서 잘 듣고, 동료들의 피드백에 마음을 열고, 때로는 용기를 가지고 나의 생각, 의문점 등을 말하며, 한 뼘 성장할 수 있었다. 
    
    선배 개발자들이 협업에 대해 쓴 글을 많이 읽었지만, 하나같이 하는 말이 ‘결국 정답은 없다’는 것이었다. 잘 듣고, 잘 말하다 보면 어느샌가 경험치가 쌓인다고 말한다. 나도 더 많은 사람들과 협업하고, 배우며, 어제보다 더 나은 김태희가 되고 싶다.
    

- **어제보다 더 나은 김태희**
    
    좌우명은 ‘The only person I want to be better than is the person I were yesterday’이다. 기라성 같은 개발자들 사이에서 코딩을 하고, 배워도 계속 모르는 게 생길 때면 가끔씩 스트레스를 받기도 한다. 조금의 스트레스는 긴장을 주어 좋지만, 과잉되면 나와 프로그래밍의 관계가 나빠질 수도 있다.
    
    나는 오래 오래 개발 하고 싶고, 지금처럼 개발을 좋아하고 싶다. 그래서 다른 사람과 나를 비교하기 보다는 ‘어제의 나’와 비교하여 더 나은 사람이고 싶다. 그런 나에게 이번 위코드 프로그램은 프로젝트 하나 끝날 때마다 새로운 것을 한 보따리씩 얻어갈 수 있는 행복한 기회였다. 
    

# 3-1. 참고문헌

- **CORS는 왜 이렇게 우리를 힘들게 하는걸까? -** [https://evan-moon.github.io/2020/05/21/about-cors/](https://evan-moon.github.io/2020/05/21/about-cors/)
- **타입스크립트 관련 에러** [https://stackoverflow.com/questions/54884488/how-can-i-solve-the-error-ts2532-object-is-possibly-undefined](https://stackoverflow.com/questions/54884488/how-can-i-solve-the-error-ts2532-object-is-possibly-undefined)
- ****React Router: router, link -**** [https://velog.io/@bigbrothershin/React-Router](https://velog.io/@bigbrothershin/React-Router)
- ****react-router-dom -**** [https://velog.io/@devstone/react-router-dom-이해하고-활용하기](https://velog.io/@devstone/react-router-dom-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B3%A0-%ED%99%9C%EC%9A%A9%ED%95%98%EA%B8%B0)
- ****React Router v6 업데이트 정리 -**** [https://velog.io/@ksmfou98/React-Router-v6-업데이트-정리](https://velog.io/@ksmfou98/React-Router-v6-%EC%97%85%EB%8D%B0%EC%9D%B4%ED%8A%B8-%EC%A0%95%EB%A6%AC)
- **React Router 6 - UseNavigate -** [https://www.reddit.com/r/reactjs/comments/k7asnz/react_router_6_usenavigate_how_to_get_current_url/](https://www.reddit.com/r/reactjs/comments/k7asnz/react_router_6_usenavigate_how_to_get_current_url/)
- **jQuery event.stopPropagation() Method -** [https://www.w3schools.com/jquery/event_stoppropagation.asp](https://www.w3schools.com/jquery/event_stoppropagation.asp)
- **[ 리액트 ] React 문자열 클립보드 복사하기 -** [https://tttap.tistory.com/126#:~:text=함수 안에 el의 이름,클립보드에 복사합니다](https://tttap.tistory.com/126#:~:text=%ED%95%A8%EC%88%98%20%EC%95%88%EC%97%90%20el%EC%9D%98%20%EC%9D%B4%EB%A6%84,%ED%81%B4%EB%A6%BD%EB%B3%B4%EB%93%9C%EC%97%90%20%EB%B3%B5%EC%82%AC%ED%95%A9%EB%8B%88%EB%8B%A4) , [https://kyounghwan01.github.io/blog/React/clipboard-copy/#서론](https://kyounghwan01.github.io/blog/React/clipboard-copy/#%E1%84%89%E1%85%A5%E1%84%85%E1%85%A9%E1%86%AB), [https://stackoverflow.com/questions/400212/how-do-i-copy-to-the-clipboard-in-javascript](https://stackoverflow.com/questions/400212/how-do-i-copy-to-the-clipboard-in-javascript)
- ****Page Not Found on Netlify with React Router -**** [https://www.sung.codes/blog/2018/page-not-found-on-netlify-with-react-router](https://www.sung.codes/blog/2018/page-not-found-on-netlify-with-react-router)

# 3-2. 알아두면 좋은 사이트

- **JSON을 타입스크립트로 자동 변환해주는 사이트** - [JSON to TypeScript (transform.tools)](https://transform.tools/json-to-typescript)





## 📌 팀원별 역할 및 회고

### 김태희

- 역할
    - api 데이터 바인딩
    - 유효 기간 남아있을 시, 상세페이지로 이동, 링크 복사 기능 구현
    - Avatar 컴포넌트 사용
    - 유틸함수를 사용하여 파일 크기, 개수, 유효 기간 등 표시
- 회고
    - ****📜**** [React, TS로 구현한 라쿠텐 파일 저장소](https://fallacious-smash-138.notion.site/React-TS-2cf7651ecb474f48ac8341f6f707782c)
    

### 문현돈

- 역할
    - 링크 상세 페이지 구현
    - 파일 썸네일 구현 위한 유틸 함수 구현
- 회고
    - CSS 처리가 이미 되어있었기 때문에 빠르게 작업을 진행할 수 있었다. 하지만 내가 맡은 페이지에서 컴포넌트로 분리할 수 있음에도 불구하고 분리되어 있지 않은 코드가 꽤 많이 있어서 따로 분리시켜주는 작업을 먼저 진행했다. 또한, 페이지 파일 안에 styled-components로 구현된 스타일 코드도 함께 있었는데 SoC 원칙에 따라 컴포넌트와 스타일을 각각 별개의 파일에 두는 것이 맞다고 생각해 분리해주었다.
    - 주어진 thumbnailUrl로 svg 파일에 접근이 안 되는 문제가 생겨, 썸네일을 구현하는데 어려움을 겪었다. api 명세에 주어진 thumbnailUrl을 보니 불러오는 파일 형식이 매번 url 끝에 오는 패턴을 확인했다. 따라서 우선, svg 파일을 불러오는 링크인지 확인하고, svg 파일을 불러오는 링크라면 public 폴더 안에 저장해둔 svg 파일들을 thumbnailUrl 대신 활용하고, svg 외의 다른 파일 형식을 불러오는 링크는 thumbnailUrl을 활용하는 방식으로 문제를 해결했다.

### 이경은

- 역할
    - 숫자 콤마 함수, 유효기간 계산, 생성 날짜 함수 등 유틸함수 구현
- 회고
    - 기간 계산, 날짜 규격 계산을 진행하면서 date 생성자에 대해 깊게 공부해볼 수 있었다.

### 이욱창

- 역할
    - 404 페이지 구현
    - react-router로 라우팅 구현
    - 파일 용량 계산 함수 구현
- 회고
    - 이번 과제는 한사람이 할 수 있을정도로 양이 적었기 때문에 맡은 부분에서 뭘 더 할수 있을까 생각을 해보다 404 페이지를 구현했습니다. 구현하며 react-router v6에 조금 더 익숙해 질 수 있었습니다.
    - 이번 과제에서 준 api는 cors 에러가 걸려있었는데 맡은 팀원이 혼자 해결하는것이 아니라, 이런 문제가 있다고 공유하고 같이 해결 방법을 논의했기에 모두 다 같이 성장할 수 있었습니다☺️
