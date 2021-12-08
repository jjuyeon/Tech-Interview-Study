# Cryptography - 암호화 알고리즘

암호화 알고리즘은 암호화/복호화에 사용되는 키, 그리고 양방향 가능 유무를 기준으로 분류할 수 있다.

<br>

## Symmetric-Key Algorithm

대칭 키 암호는 암호화 알고리즘의 한 종류로, 암호화와 복호화에 같은 암호 키를 사용하는 알고리즘을 의미한다.

암호화를 하는 송신자가 평문을 암호화 하고, 해당 암호 키를 공유해서 복호화를 하는 수신자는 암호문을 복원한다. 공유되는 암호 키를 **대칭 키**라고 한다.

공개 키 암호화 방식과 비교해서 계산 속도가 빠르다는 장점이 있지만, 같은 키를 공유해야 하기 때문에 언젠가 한 번은 키를 전달해야 하는데, 전송 과정에서 탈취당할 수 있다는 위험성이 있다.

<br>

## Public-Key Algorithm

![image](https://user-images.githubusercontent.com/30489264/140643139-8248db2f-da1c-4cb1-9836-390596a47194.png)

암호화와 복호화에 이용하는 키가 다른 방식을 말한다. 이 알고리즘은 공개 키 암호 또는 비대칭 암호라고 부른다.

두 개의 키를 사용하는데 전체에서 공유하는 암호 키를 **공개 키**, 특정 사용자만이 가지고 있는 암호 키를 **개인 키**라고 한다. 어느 하나의 키로 암호화한 암호문은 다른 쪽의 키로 복호화할 수 있다. 대칭 키 암호보다 복잡한 계산을 사용하기 때문에, 실제에서는 대칭 키 방식과 혼합해서 사용한다.

이 때 사용되는 방식은 두 가지가 있다.

### 공개 키 암호

공개 키를 이용해서 암호화하고, 특정 개인 키를 가진 사용자만이 해당 암호문을 복호화할 수 있다. 대칭 키를 이용했을 때의 단점인 키의 공유가 발생하지 않기 때문에 두 단말 간에 정보를 안전하게 공유할 수 있다.

> 1. A는 공개 키를 공개한다.  
> 2. B는 공개 키로 전달하려는 평문을 암호화 한다.
> 3. B가 A에게 암호문을 전달한다.
> 4. A는 개인 키를 이용해서 암호문을 복호화한다.

### 공개 키 서명

개인 키를 이용해서 암호화하고, 공개 키를 이용해서 해당 암호문을 복호화할 수 있다. 누구나 해당 암호문을 복호화할 수 있으므로, 개인 키를 가진 사용자로부터의 신뢰할 수 있는 정보인지 검증하는 용도로 쓰인다.

> 1. A는 공개 키를 공개한다.
> 2. A는 평문을 자신의 개인 키로 암호화 한다.
> 3. 다른 사용자는 해당 암호문을 A의 공개 키를 이용해서 복호화한다.
> 4. 해당 문서가 변조되었다면 원래의 평문을 복원할 수 없으므로, 문서의 발행 출처와 변조 여부를 확인할 수 있다.

<br>

## 대칭 키 + 비대칭 키

대칭 키 방식과 공개 키 암호 방식의 장단점이 명확하기 때문에 두 방식을 혼합해서 사용한다.

1. 송신자 측에서는 대칭 키로 사용할 암호 키를 공개 키로 암호화한다.
2. 암호화된 대칭 키를 전송한다. 이 암호문은 수신자 측만 복호화할 수 있기에 탈취당해도 안전하다.
3. 수신자는 암호문을 복호화 해서 공개 키를 얻는다.
4. 송신자와 수신자가 공유한 대칭 키를 이용해서 이후에는 해당 대칭 키를 이용한 암호화 방식을 사용해서 통신한다.

<br>

## One-Way Encryption

![image](https://user-images.githubusercontent.com/30489264/140644145-735e0093-c662-4b42-8538-f04bc0cec6a2.png)

위의 두 암호화 알고리즘은 암호 키를 사용해서 평문을 암호문으로 암호화, 암호문을 평문으로 복호화할 수 있었다. 하지만 **Cryptographic hash function(암호화 해시 함수)를 사용한 단방향 암호화 방식**은 평문을 암호화한 뒤로는 복호화할 수 없는 암호화 방식이다. 이 암호화 방식은 패스워드 저장과 같이, 반드시 원본 데이터를 복원해내지 않아도 되는 경우에 사용한다(ex. 서버의 데이터베이스에 저장하는 패스워드는 복호화 될 필요가 없고, 사용자가 패스워드를 입력할 때 마다 암호화 한 값을 비교하면 된다).