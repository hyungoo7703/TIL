# 비트마스크(Bitmask)

비트마스크(Bitmask)는 비트연산을 이용하여 정수들의 이진수 표현을 자료구조로 활용하는 방법이다.

## 비트와(Bit) 비트연산(Bit Operation)
> 참고: [[자료구조 알고리즘] 비트연산 완전정복 - Bit Operation](https://www.youtube.com/watch?v=yHBYeguDR0A&list=PLjSkJdbr_gFa4z1kC3pAqYaoryrENCAhf)

비트마스크 기법을 활용하기 위해서는 당연히 사용되는 비트와, 비트의 연산방식에 대한 이해가 필요하다.

### 비트(bit)

어떤 데이터든 컴퓨터에 저장될 경우는 0과 1로서 이루어진 데이터로 저장된다. <br>
이를 다시 말하면 0과 1로 이루어진 데이터가 컴퓨터의 데이터의 최소 단위라고도 표현할 수 있는데, <br>
이렇게 배타적인 상태 값을 가질 수 있는 데이터의 최소 단위를 비트라고 한다. <br>

-----
### 비트연산(Bit Operation)

정수를 이진수로 표현하는 기법을 이해하기 전, 양의 정수와 음의 정수에서 이진수를 표현하는 방식이 다름을 이해해야한다. <br>

Java에서 integer값은 4bytes의 값을 가진다. <br>
여기서 4bytes의 값은 32bit를 의미하는데, (1bytes = 8bit) 32개의 공간을 가지고 있다고 할 수 있다. <br>
이 32개의 공간으로 표현할 수 있는 숫자는 모든 공간이 0부터 모든 공간이 1이 되는 경우가 된다. (2<sup>32</sup>개) <br>
그러나 숫자는 0부터 시작하기에 주어진 공간의 최댓값은 32bit 기준 2<sup>32</sup> - 1이 된다. <br>

실제로 정수는 양의정수, 음의정수로 표현되기에 구분을 두기 위해 맨 앞 공간은 부호로서 사용하기로 한다. (0일때 양수, 1일때 음수) <br>
아래 그림은 부호로 구분을 두고 양의 정수와 음의 정수를 표현해본 그림이다.

<img alt="이진수표현법" src="https://user-images.githubusercontent.com/93297109/198815325-0183fa8e-93b1-42ad-a801-1303878f5fa8.png">

음의정수에서(부호 절대값으로 표시하지 않을 경우) 전부 1인 경우는 -1이 된다. <br>
이는 음의정수에서 가장 큰 값이 -1이고, 양수에서 0을 표현 했기 때문에 음수에서는 0을 표현하지 않아도 된다. <br>
이 말은 주어진 공간에서의 음의정수의 최솟값은 그대로 표현 가능하다.

이진수 표현 방식을 알았다면, 이제 비트연산을 하기 위한 <b>비트연산자</b>를 알아보자. <br>

#### 비트연산자
> AND, OR, XOR은 두 정수 변수를 연산하여 새로운 정수 변수를 생성한다는 가정

+ AND 연산 <b>&</b>: 비교한 각각 하나의 비트가 둘다 1인 경우에만 1
+ OR 연산 <b>|</b>: 비교한 각각 하나의 비트가 둘다 혹은 하나가 1이면 1
+ XOR 연산 <b>^</b>: 비교한 각각 하나의 비트가 둘이 다르면 1 둘이 같으면 0
+ NOT 연산 <b>~</b>: 0과 1이 반대가 되는 경우이다. 
+ Shift 연산 <b>>>, <<</b>
    + <b>>></b> 정수의 비트들을 오른쪽으로 원하는 만큼 움직인다는 의미
    + <b><<</b> 정수의 비트들을 왼쪽으로 원하는 만큼 움직인다는 의미

-----
## 비트연산을 이용한 비트마스크 알고리즘

### CASE1 정수 하나가 주어질 때 i번째 인덱스 비트가 1인지 0인지 판별하는 방법

```java
/*
getBit 함수 (return i번째 인덱스의 bit 값이 1이면 true 반환)
1<<i 를 통해 비교할 수 있도록 만들어 준뒤 (i가 3인 경우 1000), 
& 연산자를 통해 찾고자하는 인덱스의 bit값이 1인 경우만 값이 되도록 (0이면 0000이 되서 무조건 0 -> false 반환)
*/
public static boolean getBit(int num, int i) {
    return (num & (1 << i)) != 0;
}

public static void main(String[] args)
{
    int num = 9; // 주어진 숫자
    System.out.println(Integer.toBinaryString(num)); // java에서 숫자를 이진수로 변환해서 String으로 표현해주는 method, 출력값: 1001
    int i = 3; // 3번째 인덱스의 비트가 무엇인지 판별
    System.out.println(getBit(num, i)); // true
}
```

### CASE2 정수 하나가 주어질 때 i번째 인덱스 비트를 1, 0로 만들어 주는 방법

```java
/*
1. setOnBit 함수 (i번째 인덱스 비트를 1)
1<<i 를 통해 비교할 수 있도록 만들어 준뒤 (i가 3인 경우 1000),
| 연산자를 사용하면 i번째를 제외하곤 동일하게 나오며 i번째에는 무조건 1이 들어간다.
*/
public static int setOnBit(int num, int i) {
    return num | (1 << i);
}
/*
2. setOffBit 함수 (i번째 인덱스 비트를 0)
i번째에 0을 만들어 주기 위해서는 (i가 3인 경우 0111에 & 연산의 과정이 필요하다.),
*/
public static int setOffBit(int num, int i) {
    return num & ~(1 << i);
}

public static void main(String[] args)
{
    // 1. 5(101)를 변환하자 0101 -> 1010 (13이 출력 되어야 한다.)
    System.out.println(setOnBit(5,3)); // 13
    // 2. 9(1001)를 변환하자 1001 -> 0001 (1이 출력 되어야 한다.)
    System.out.println(setOffBit(9,3)); // 1
}
```

### CASE3 정수 하나가 주어질 때 i번째 인덱스 비트를 포함해서 왼쪽, 오른쪽으로 0으로 clear 해주는 방법

169의 경우 이진수로 표현하면 10101001이다. 3번째 인덱스를 기준으로 전부 클리어 한다는 의미는 왼쪽의 경우 00000001(1), 오른쪽의 경우 10100000(160)이 된다. <br>
그럼 어떻게 표현 할 수 잇을까?

```java
/*
1. clearLeftBit 함수
169를 기준으로 생각해보면 10101001 에서 0~2번 인덱스의 비트는 동일하게 나와야 하고, 앞은 전부 0이 되야 하기에 
00000111과 & 연산이 필요하다. 즉 1<<i의 NOT 모양과 비슷해야 하는데 NOT을 해버리면 11110111이 되기에 1000 에서 1을 빼주면 된다.
*/
public static int clearLeftBit(int num, int i) {
    return num & ((1 << i) - 1);
}
/*
2. clearRightBit 함수
위 clearLeftBit와 반대로 11110000과 & 연산의 과정이 들어간다.
모든 숫자가 1인 bit는 -1 이므로 (int로 선언한 경우 11111111111111111111111111111111)
이를 index + 1 만큼 밀어주면 11110000 형태가 나온다.
*/
public static int clearRightBit(int num, int i) {
    return num & (-1 << i + 1);
}

public static void main(String[] args)
{
    System.out.println(clearLeftBit(169,3)); // 1
    System.out.println(clearRightBit(169,3)); // 160
}
```
