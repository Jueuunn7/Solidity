
# Simple smart contract

### Storage
```solidity
pragma solidity ^0.8.0;

contract simple {
    uint storedData;

    function set(uint x) public {
        storedData = x;
    }

    function get() public view returns (uint) {
        return storedData;
    }
}
```
컨트렉트란 무수한 코드(함수)와 상태들이 이더리움의 특정 주소에서 존재하는것을 말한다.

`uint Type`의 `storedData`변수를 선언한다.     
이건 데이터베이스에서 함수룰 호출하면서 값을 조회하거나 변경할 수 있다.
`get`, `set`으로 변수의 값을 조회, 변경 가능하다.

---

### Subcurrency
```solidity
pragma solidity ^0.5.0;

contract Coin {
    address public minter;
    mapping (address => uint) public balances;

    event Sent(address from, address to, uint amount);

    constructor() public {
        minter = msg.sender;
    }

    function mint(address receiver, uint amount) public {
        require(msg.sender == minter);
        require(amount < 1e60);
        balances[receiver] += amount;
    }

    function send(address receiver, uint amount) public {
        require(amount <= balances[msg.sender], "Insufficient balance.");
        balances[msg.sender] -= amount;
        balances[receiver] += amount;
        emit Sent(msg.sender, receiver, amount);
    }
}
```

`address public minter;`     
> address 는 20바이트의 크기를 갖으며 이더리움의 주소를 나타낸다. 이 주소는 불변이다.    

address 형식으로 모두 접근가능한 minter 변수를 생성한다.     
public 키워드 없이는 다른 컨트렉트가 이 변수에 접근이 불가능하다.    

`mapping (address => uint) public balances;`
> mapping 은 솔리디티의 Key-value 형태로 저장되는 데이터이다.     

balances 이름으로 address => uint 형식으로 저장된다.

매핑은 가상으로 초기화되는 해시테이블이다. 값이 없는게 아니라 0에 해당하는 해시값이 저장된다.

`emit Sent(msg.sender, receiver, amount);`
> 이벤트를 실행한다. 이벤트는 중요한 활동이 기록되면 블록체인에 기록한다.    
> 인자를 함께 받으며 이벤트의 트랜젝션을 파악하는데 도움된다.
