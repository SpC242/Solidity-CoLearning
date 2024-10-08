## Day12 2024/10/08
今天是殘酷共學的第十二天，大家一起加油吧!!!

## 調用已部署的合約
### **目標合約：`OtherContract`**
```Solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract OtherContract {
    uint256 private _x = 0; // 狀態變量 _x
    event Log(uint amount, uint gas); // 收到 ETH 時觸發的事件

    // 返回合約餘額
    function getBalance() public view returns (uint) {
        return address(this).balance;
    }

    // 設置 _x 的值，並接收 ETH
    function setX(uint256 x) external payable {
        _x = x;
        if (msg.value > 0) {
            emit Log(msg.value, gasleft());
        }
    }

    // 獲取 _x 的值
    function getX() external view returns (uint x) {
        x = _x;
    }
}
```

### **調用 `OtherContract` 合約**
1. **傳入合約地址**
    - 這種方法允許在函數內傳入目標合約的地址，並生成合約的引用來調用其函數。
    - ```Solidity
        function callSetX(address _Address, uint256 x) external {
            OtherContract(_Address).setX(x);
        }
        ```
2. **傳入合約變量**
    - 可以直接在函數中傳入合約的引用，而不只是地址。
    - ```solidity
        function callGetX(OtherContract _Address) external view returns (uint x) {
            x = _Address.getX();
        }
        ```
3. **創建合約變量**
    - 可以在函數內創建合約變量，然後使用該變量來調用目標合約的函數。
    - ```Solidity
        function callGetX2(address _Address) external view returns (uint x) {
            OtherContract oc = OtherContract(_Address);
            x = oc.getX();
        }
        ```
4. **調用合約並發送 ETH**
    - 如果目標合約的函數是 `payable` ，可以通過此方法向目標合約發送 ETH。使用 {value: msg.value} 可以在調用時附加 ETH。
    - ```Solidity
        function setXTransferETH(address otherContract, uint256 x) payable external {
            OtherContract(otherContract).setX{value: msg.value}(x);
        }
        ```

## **利用 `call` 調用其他合約**
 `call` 是 `address` 類型的低級成員函數，它可以用來與其他合約交互。
 `call` 是 `Solidity` 官方推薦的通過觸發 `fallback` 或 `receive` 函數發送ETH的方法。

1. **調用格式：**
    - ```solidity
        目標合約地址.call(字節碼);
        ```
    - 字節碼可以使用 `abi.encodeWithSignature` 來生成。
        - ```solidity
            abi.encodeWithSignature("函數簽名", 逗號分隔的具體參數);
            ```
        - 函數簽名為 "函數名稱(逗號分隔的參數類型)"
2. **指定 ETH 和 gas：**
    - `call` 還可以指定發送的 ETH 和 gas 數量：
    - ```solidity
        目標合約地址.call{value: 轉入 ETH 數量, gas: gas 數量}(字節碼);
        ```
3. **返回值：**
    - `call` 返回一個元組 `(bool, bytes memory)`，其中 bool 表示調用是否成功，bytes memory 是函數的返回數據。

### 簡單範例
```Solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

contract Receiver {
    event Received(address caller, uint256 amount, string message);

    fallback() external payable {
        emit Received(msg.sender, msg.value, "Fallback was called");
    }

    function foo(string memory _message, uint256 _x) public payable returns (uint256){
        emit Received(msg.sender, msg.value, _message);
        return _x + 1;
    }
}

contract Caller {
    event Response(bool success, bytes data);

    // Let's imagine that contract Caller does not have the source code for the
    // contract Receiver, but we do know the address of contract Receiver and the function to call.
    function testCallFoo(address payable _addr) public payable {
        // You can send ether and specify a custom gas amount
        (bool success, bytes memory data) = _addr.call{
            value: msg.value,
            gas: 5000
        }(abi.encodeWithSignature("foo(string,uint256)", "call foo", 123));

        emit Response(success, data);
    }

    // Calling a function that does not exist triggers the fallback function.
    function testCallDoesNotExist(address payable _addr) public payable {
        (bool success, bytes memory data) = _addr.call{value: msg.value}(
            abi.encodeWithSignature("doesNotExist()")
        );

        emit Response(success, data);
    }
}
```

## **delegatecall**
`delegatecall` 是 `Solidity` 中低級成員函數，與 `call` 類似，但兩者之間存在重要的上下文差異。在 `delegatecall` 中，儘管是調用其他合約的函數，但狀態變量的變更卻作用在調用合約（也就是發起 `delegatecall` 的合約）上。因此，`delegatecall` 的應用場景主要是代理合約（Proxy Contract）和合約升級。

1. `delegatecall` 與 `call` 的區別
    - `call` 的上下文：
        當使用 `call` 調用其他合約的函數時，執行的是目標合約的函數，上下文屬於目標合約。也就是說：
        msg.sender 是發起 `call` 的合約地址。
        狀態變量的變更作用在目標合約上。
    - `delegatecall` 的上下文：
        使用 `delegatecall` 時，執行的是目標合約的函數，但上下文仍然是發起 `delegatecall` 的合約。也就是說：
        ``msg.sender`` 是最初調用合約的外部地址。
        狀態變量的變更作用在發起 `delegatecall` 的合約上。

2. **調用格式：**
    - ```solidity
        目標合約地址.delegatecall(字節碼);
        ```
    - 字節碼可以使用 `abi.encodeWithSignature` 來生成。
        - ```solidity
            abi.encodeWithSignature("函數簽名", 逗號分隔的具體參數);
            ```
        - 函數簽名為 "函數名稱(逗號分隔的參數類型)"

3. **應用:代理合約（Proxy Contract）**
- 代理合約的設計理念：
    - 在智能合約開發中，將邏輯與狀態分離是一種常見的設計模式。這樣的架構允許邏輯合約升級，而不會影響存儲在代理合約中的狀態變量。
    - 代理合約負責存儲所有狀態變量，而邏輯合約負責執行具體邏輯。當需要升級邏輯時，只需更新代理合約中邏輯合約的地址。
    - 代理合約會使用 `delegatecall` 來調用邏輯合約中的函數，這樣狀態變量的變更將作用於代理合約，而不是邏輯合約。

### 簡單範例
```Solidity
 // SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

// NOTE: Deploy this contract first
contract B {
    // NOTE: storage layout must be the same as contract A
    uint256 public num;
    address public sender;
    uint256 public value;

    function setVars(uint256 _num) public payable {
        num = _num;
        sender = msg.sender;
        value = msg.value;
    }
}

contract A {
    uint256 public num;
    address public sender;
    uint256 public value;

    function setVars(address _contract, uint256 _num) public payable {
        // A's storage is set, B is not modified.
        (bool success, bytes memory data) = _contract.delegatecall(
            abi.encodeWithSignature("setVars(uint256)", _num)
        );
    }
}
```