### 블로킹/논블로킹

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/97157677-f491-4f35-bb2d-b9de77f8c96c/Untitled.png)

블록 논블록은 함수호출 이야기이다. 

예를들어 A라는 함수를 호출했을때, A라는 함수가 리턴할 때까지, 기대하는 행위를 마칠 때까지 기다리는 것을 블로킹이라고한다. 

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/bf666675-146d-4839-adc2-a36860dcf29b/Untitled.png)

반면에 A라는 함수를 호출하고, 기대하는 행위를 부탁하고 바로 제어권을 리턴받는 것을 논블로킹이러고 한다. 

### 동기/비동기

동기와 비동기는 행위에 대한 이야기이다. 

행위는 던순하게 우리가 생각했던 프로젝트나 서로 다른 쓰레드나 프로세스나 서버에서 일어나는 일련의 동작들이라고 대입해서 생각하면 편하다. 

동시성(concurent)의 개념이지 병행성과는 관련이 없는 내용이라고 한다. 이부분은 다시 살펴보는것이 좋겠다. 

A라는 행위와 별개로 B라는 행위가 있다고 가정한다   

A라는 행위와 B라는 행위가 동시에 동작한다면 비동기라고 한다. 

### 유형

### 동기&블로킹

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/584480c4-fdfe-4322-9969-9e7a6182a52f/Untitled.png)

A함수는 B함수를 호출한다. 이때 A함수는 B함수에게 제어권을 주지 않고, 자신의 코드는 계속 실행한다.(논블로킹)

A함수는 B함수의 리턴값이 필요하기 때문에 중간중간 B함수의 완료 여부를 중간 중간 확인한다(동기)

### 동기&논블로킹

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/97948da4-28a3-4115-be81-b348af9da4ae/Untitled.png)

동기는 

### 비동기-논블로킹

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/02aa5f4c-ed90-4bdf-b00b-6e5d54fc8a16/Untitled.png)

비동기 논블로킹은 

### 비동기&블로킹

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/743df02d-8a87-47ce-a72f-e2b311c1ab6c/Untitled.png)