> 참고: [http://www.tcpschool.com/javascript/js_object_prototype](http://www.tcpschool.com/javascript/js_object_prototype)
# 프로토타입

JavaScript는 클래스기반 언어가 아니기 때문에, 상속의 개념이 다른 언어와 다르다. <br>
결론적으로 이야기하면 프로토타입기반 객체지향 언어로서, <br>
**모든 객체는 그들의 프로토타입으로부터 프로퍼티와 메소드를 상속 받는다.** <br>
> #### 프로토타입 상속의 특징 [차별적 상속]
> 프로토타입의 속성들은 실제로 복사되지 않고 링크로 연결되는데, 이는 메모리를 효율적으로 사용할 수 있게 해준다.

코드를 보면서 이해해 보자.

```js
const date = new Date(); // 새로운 Date 객체를 생성할 때
date.getDate(); // 변수 date는 객체를 생성해주는 Date의 메소드들을 사용할 수 있다.
```

바로 위 코드처럼 변수 date는 Date.prototype 에서의 상속을 받아 메소드를 사용 할 수 있는데 <br>
가상의 연결 고리를 프로토타입 체인(prototype chain)이라 한다. <br><br>
조금 더 자세히 알아보자.

```js
/*
ES6 전 class 문법이 없을 때, 객체 생성 함수를 통해 생성하곤 했다.
*/
function Dog(color, name, age) { // 개에 관한 생성자 함수를 작성함.
    this.color = color;  // 색에 관한 프로퍼티
    this.name = name;    // 이름에 관한 프로퍼티
    this.age = age;      // 나이에 관한 프로퍼티
}

const myDog = new Dog("흰색", "마루", 1); // 이 객체는 Dog라는 프로토타입을 가짐.

console.log(myDog); // 출력결과: {color: '흰색', name: '마루', age: 1}
console.log(myDog.name); // 출력결과: 마루

/*
Dog라는 프로토타입을 가지기에 프로토타입에 원하는 프로퍼티나 메소드의 추가가 가능하다.
*/
Dog.prototype.breed = '말티즈'; // 프로토타입에 종 = '말티즈' 정보 추가
Dog.prototype.say = () => console.log('왈왈'); // 프로토타입에 함수정보 추가

console.log(myDog); // 출력결과: {color: '흰색', name: '마루', age: 1}
console.log(myDog.breed); // 출력결과: 말티즈
myDog.say(); // 출력결과: 왈왈
```

myDog가 생성될 때, 프로토타입만의 정보는 본인이 가지고 있지 않고, 상속 받아서 사용하기에 <br>
아래와 같이 프로토타입에 새로운 프로퍼티 추가 후에도 객체는 달라지지 않는다.

```
{color: '흰색', name: '마루', age: 1}
```

하지만 myDog.bread 나 myDog.say() 처럼 <br>
프로토타입의 추가한 값을 출력하면 잘 나오는데, <br>

이를 통해 객체는 프로토타입으로부터 프로퍼티와 메소드를 상속 받는다는 사실을 알 수 있다.
