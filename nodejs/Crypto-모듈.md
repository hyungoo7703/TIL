# Crypto 모듈

다양한 암호화 기능을 제공하는 내장 모듈로, 외부 의존성 없이 Node.js 설치 시 기본적으로 포함된다.
<br>
주요 기능은 아래와 같다.
+ 데이터 해싱
+ <b>암호화 및 복호화</b>
+ 메시지 인증 코드(HMAC) 생성
+ 디지털 서명 생성 및 검증
<br>
실제 실무에서 적용해 본 암호화 및 복호화를 위주로 설명한다.

## 사용방법
require('crypto')를 통해 모듈을 가져온 뒤 사용한다.

```js
const crypto = require('crypto');
```
crypto는 다양한 메서드를 가지고 있는데, 주요 메서드를 정리하면 아래와 같다.

+ createCipheriv(): 특정 알고리즘과 키, 초기화 벡터(IV)를 사용하여 Cipher 객체를 생성
+ <b>createDecipheriv()</b>: 특정 알고리즘과 키, IV를 사용하여 Decipher 객체를 생성
  > Decipher: 암호화된 메시지를 해독하거나 이해하기 어려운 내용을 해석하는 과정을 담은 객체
+ createHash(): 지정된 알고리즘을 사용하여 Hash 객체를 생성
+ createHmac(): 지정된 알고리즘과 키를 사용하여 HMAC 객체를 생성
+ <b>randomBytes()</b>: 임의의 데이터를 생성

## 실무 적용 코드를 통해 사용방식 파악

(상황)
<br>
"DB 커넥션 계정 암호화가 되어있지 않아서 보안 취약점 개선을 진행해야 합니다. <br>
최소 256비트 길이의 키를 사용하여 데이터 관리 암호화를 진행해주세요."

(접근)
<br>
1. crypto 모듈로 암호화 진행 하는데, 요구사항에 맞춰 알고리즘 채택 <b>AES-256-CBC 알고리즘</b>
```js
const crypto = require('crypto');

const algorithm = 'aes-256-cbc'; //AES-256-CBC 알고리즘
```
2. AES-256-CBC 알고리즘 사용을 위해 키, IV쌍을 발급한다. (crypto의 임의의 데이터 생성 메서드를 이용)
```js
const key = crypto.randomBytes(32); // 256-bit key
const iv = crypto.randomBytes(16); // 128-bit IV
```
키와 IV는 독립적으로 사용되지만, 동일 키와 IV로 암호화 복호화를 진행해 주어야 한다. <br>
실제 코드에서는 환경변수로 관리하고 있다.

3. 우선 .env파일 내 DB_PASSWORD를 암호화 하기 위해, 메서드를 만들어 준다. 그 후 암호화를 진행한다.
```js
/**
 * 암호화
 * @param {*} text 
 * @returns 
 */
const encrypt = (text) => {
  let cipher = crypto.createCipheriv(algorithm, Buffer.from(key), iv);
  let encrypted = cipher.update(text);
  encrypted = Buffer.concat([encrypted, cipher.final()]);
  return encrypted.toString('hex'); //최종 암호문을 16진수 문자열로 변환하여 반환
};

```
4. 암호화된 DB_PASSWORD를 복호화 매서드를 이용해 복호화를 하면서 DB커넥션과 연결되도록 설정한다.
```js
const encryptedPassword = process.env.DB_PASSWORD; //암호화된 DB_PASSWORD

/**
 * 복호화
 * @param {*} text 
 * @returns 
 */
const decrypt = (text) => {
  let encryptedText = Buffer.from(text, 'hex');
  let decipher = crypto.createDecipheriv(algorithm, Buffer.from(key), iv);
  let decrypted = decipher.update(encryptedText);
  decrypted = Buffer.concat([decrypted, decipher.final()]);
  return decrypted.toString(); //최종적으로 완전한 복호화 결과를 문자열로 변환하여 반환
};

const decryptedPassword = decrypt(encryptedPassword); // 실제 사용 커넥션 시 사용.
```
