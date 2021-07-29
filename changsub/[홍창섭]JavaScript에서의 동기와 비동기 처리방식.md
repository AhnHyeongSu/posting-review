# \<JavaScript\> 동기/비동기와 CallBack,Promise and Async/Await
## 동기와 비동기(Sync,Async)
- 동기적 : 말 그대로 어떠한 일들이 동시에 일어난다는 뜻이다. 일상에서 접할 수 있는 동기화의 어원이라 볼 수 있으며, 요청과 동시에 그 응답이 요청을 요구한 자리에 시간과 관계없이 주어져야 한다. 즉, 요청과 응답을 서로 확인하는게 중요해서 일의 순서가 보장이 되어야한다고 볼 수 있다. 따라서 응답이 올때까지 기다려야 한다는 단점이 존재한다.(TCP 통신 방식과 비슷한점이 있다고 이해해도 좋다!)
- 비동기적 : 요청과 응답이 동시에 일어나지 않는다. 응답이 오지 않아도 새로운 요청을 보낼 수 있다는 장점이 있다.
```js
//비동기식 프로그래밍 1->3->2 순으로 콘솔에 찍힌다.
console.log('1')
setTimeout(function() {
    console.log('2')
},1000)
console.log('3')
```
- 수많은 데이터를 전송해야하는 웹 환경에서 동기적인 방식은 사용자에게 너무 많은 대기 시간을 부여하기에 한번에 여러개의 일을 수행하는 비동기식 방식이 채택되어 많이 사용되고 있습니다.

## 자바 스크립트에서의 비동기 구현 방식 3가지
### 콜백함수 사용
- 특정함수에 매개변수로 전달된 함수를 의미합니다.즉, 나중에 호출될 함수들이라고 볼 수 있습니다.
- 음식점을 예약하는 행위와 빗대어 설명이 가능하다.대기자가 있는 맛집을 찾아갔다고 가정해보자. 사람이 많아서 음식점을 예약하고 나면 실제 예약시간에 도달하기 전까지는 다양한 일들을 할 수 있다.(근처에서 쇼핑을 하거나 카페를 가거나 등등)이 때 음식점으로부터 내 차례가 되었다고 전화가 옵니다. 이때를 콜백함수가 호출되는 시기라 볼 수 있습니다.자리가 났다는 조건을 만족할때 저희는 밥을먹는 행위를 수행할 수 있으며, 조건이 만족되기 전까지는 자유롭게 다른 일들을 할 수 있습니다.즉 데이터가 준비된 시점에서만 저희가 원하는 동작을 수행할 수 있습니다.
- 위의 setTimeout 메서드를 사용한 방식도 콜백이라 볼 수 있습니다.
```js
function Callback(callback){
    console.log('callback function');
    callback();
}
Callback(function(){
    console.log('callback 호출');
})
```
- 하지만 아래와 같이 가독성이 좋지않다는 단점이 존재한다.
```js
function Callback(callback){
    function Callback2(callback){
        function Callback3(callback){
            console.log('무한콜백');
        }
    }
}
```
### Promise
- Promise 객체는 자바스크립트에서 비동기적인 프로그래밍을 위해 사용된다.
- promise는 기본적으로 콜백과 하는일 동일하다.하지만 아래와 같이 .then()을 호출한다.이는 연속적으로 메소드들을 호출할 수 있다는 장점이 존재한다.따라서 콜백과 기능은 동일하지만 훨씬 간결하고 가독성이 좋아진다.
#### Promise의 세가지 상태(States)
1. Pending(대기) : 비동기 처리 로직이 아직 완료되지 않은 상태
2. Fulfilled(이행) : 비동기 처리 로직이 수행완료되어 결과 값을 반환한 상태
3. Rejected(실패) : 비동기 처리가 실패하거나 에러가 발생한 상태
#### Pending 상태 예시
- 아래와 같이 Promise 객체를 생성하는 시점부터 Pending상태이다.
- 또한 콜백함수의 인자로서 resolve,reject를 받을 수 있다.
```js
new Promise(function(resolve, reject) {
  // ...
}); 
```
####  Fulfilled 상태 예시
- 위의 콜백함수 인자 중 Resolve를 아래와 같이 실행하면 Fulfilled 상태로 들어간다.
- 또한 Fulfilled 상태가되면 아래의 then()메서드를 이용한 체이닝이 가능하다.
```js
new Promise(function(resolve, reject) {
	resolve(); 
}); 
```
#### Rejected 상태 예시
- 콜백함수의 인자 중 reject()를 인자로 받으면 Rejected 상태로 들어간다.
- 어떠한 비동기 로직이 거절이 된 상태로, 에러에 대한 예외처리가 가능해진다.
- 두번째 예시처럼 then()메서드의 결과값을 보고, reject가 나오면 발생한 에러를 catch 메서드로 받고 이후 동작을 수행 할 수 있다.
```js
new Promise(function(resolve, reject) {
	rejected(); 
}); 
```

```js
function getData() {
  return new Promise(function(resolve, reject) {
    reject(new Error("Request is failed"));
  });
}

// reject()의 결과 값 Error를 err에 받음
getData().then().catch(function(err) {
  console.log(err); // Error: Request is failed
});
```
#### 기본적인 Promise 구조
```js
//1. Producer
//When new Promise is created, the executor runs automatically
const promise = new Promise((resolve,reject) =>{
    //doing someting heavy work (network or data)
    console.log("Doing Something")
    setTimeout(()=>{
        resolve('brido');
        //reject(new Error("network error"));
    },2000)
})

//2. Consumer: then, catch, finally
promise
.then((value) => {
    console.log(value);
})
.catch(error => {
    console.lot(error);
})
.finally(()=>{
    console.log("finally")
})
```
#### Promise Chaining
```js
function add10(a) {
  return new Promise(resolve => setTimeout(() => resolve(a + 10), 100));
} //Promise사용 시 작업이 끝났음을 알려주는 resolve를 인자로 받아들임.
add10(10)
  .then(add10)
  .then(add10)
  .then(add10)
  .then((res) => console.log(res))
```
### Async/Await
- 가장 최근 자바스크립트 문법에 추가된 비동기 처리 패턴입니다.기존의 콜백과 Promise의 단점을 보완하기위해 태어났다고 볼 수 있습니다.
- 특히 가독성 부분을 집중적으로 보완한 문법입니다. 즉, 깔끔하게 Promise를 사용할수 있게 해주는 역할을 수행합니다.
- Async와 Await을 사용하려면 우선 사용할 함수 앞에 async라는 키워드를 붙여 사용해야 하며 선언된 async 함수 안에서만 await 키워드를 사용할 수 있다.
#### async & await 기본 문법
- 먼저 함수의 앞에 async라는 예약어를 씁니다.그리고 해당 함수 내에서 비동기 처리 로직을 수행하는 메서드 앞에 await 예약어를 붙여줍니다.
- 여기서 비동기 처리 메서드는 꼭 Promise객체를 반환해야 합니다.
```js
async function 함수명() {
  await 비동기_처리_메서드_명();
}
```
#### Async/Await 간단한 예제 1
```js
function add10(a){
    return new Promise(resolve => setTimeout(() => resolve(a + 10),100));
}

async function f1() {
  const a = await add10(10);
  const b = await add10(a);
  console.log(a, b)
}
f1();
```
#### Async/Await 간단한 예제 2
```js
function delay(ms) {
    return new Promise(resolve => setTimeout(resolve, ms));
  }
  //2초동안 기다리게 하고 사과를 리턴하는 메서드
  async function getApple() {
    await delay(2000);
    return '🍎';
  }
  //1초동안 기다라게 하고 사과를 리턴하는 메서드
  async function getBanana() {
    await delay(1000);
    return '🍌';
  }
getApple().then(console.log);
getBanana().then(console.log);
```
### Reference
- [https://joshua1988.github.io/web-development/javascript/promise-for-beginners/](https://joshua1988.github.io/web-development/javascript/promise-for-beginners/ "캡핀판교-자바스크립트 Promise 쉽게 이해하기")
