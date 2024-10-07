## Day11 2024/10/07
今天是殘酷共學的第十一天，大家一起加油吧!!!

## 接收ETH
1. **receive() 函數**
    - `receive()` 函數專門用來處理合約接收 ETH 的情況，它只能用於接收 ETH 轉帳，並且有以下特點：
        - 只能有一個 `receive()` 函數
        - 不需要 function 關鍵字
        - 必須用 external 和 payable 修飾
        - 無參數、無返回值
```Solidity
// 定義事件
event Received(address Sender, uint Value);

// 接收ETH時觸發事件
receive() external payable {
    emit Received(msg.sender, msg.value);
}
```

2. **fallback() 函數**
    - `fallback()` 函數用於兩種情況：
        - 合約被調用了一個不存在的函數
        - 接收 ETH 並且沒有 `receive()` 函數可供調用
    - `fallback()` 函數同樣不需要 function 關鍵字，並且通常會加上 `payable` 修飾以便接收 ETH。
    - 它可以用於代理合約（proxy contract）來處理未匹配的函數調用。
```Solidity
// 定義事件
event fallbackCalled(address Sender, uint Value, bytes Data);

// 當被觸發時，記錄調用的信息
fallback() external payable {
    emit fallbackCalled(msg.sender, msg.value, msg.data);
}
```

3. **receive() 和 fallback() 的區別**
這兩個函數都能接收 ETH，但觸發規則不同。
如果合約接收 ETH 時，msg.data 是空的，且存在 `receive()` 函數，那麼將觸發 `receive()` 函數。
如果 msg.data 不是空的，或者合約沒有定義 `receive()` 函數，那麼將觸發 `fallback()` 函數。
```scss
         接收ETH
            |
       msg.data是空？  
          /  \
        是    否
        /      \
  receive()存在?  fallback()
      / \
     是  否
    /     \
receive() fallback()
```

## 發送ETH
1. **使用 `transfer()` 發送 ETH**
    - Gas 限制：2300 gas，足夠轉帳但不能執行複雜邏輯。
    - 失敗處理：自動 `revert` (回滾)交易。

2. **使用 `send()` 發送 ETH**
    - Gas 限制：2300 gas，與 `transfer()` 相同。
    - 失敗處理：不會自動 `revert` 交易，需手動檢查並處理。
    - `send()` 返回型態為 `bool` ，代表轉帳成功或失敗。

3. **使用 `call()` 發送 ETH**
    - Gas 限制：無限制，可用於執行複雜邏輯。
    - 失敗處理：不會自動 `revert` 交易，需手動檢查並處理。
    - `call()` 返回型態為 `(bool,bytes)` ，其中 `bool` 代表轉帳成功或失敗。 


## 簡單範例
```Solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

contract ReceiveEther {
    // Function to receive Ether. msg.data must be empty
    receive() external payable {}

    // Fallback function is called when msg.data is not empty
    fallback() external payable {}

    function getBalance() public view returns (uint256) {
        return address(this).balance;
    }
}

contract SendEther {
    function sendViaTransfer(address payable _to) public payable {
        // This function is no longer recommended for sending Ether.
        _to.transfer(msg.value);
    }

    function sendViaSend(address payable _to) public payable {
        // Send returns a boolean value indicating success or failure.
        // This function is not recommended for sending Ether.
        bool sent = _to.send(msg.value);
        require(sent, "Failed to send Ether");
    }

    function sendViaCall(address payable _to) public payable {
        // Call returns a boolean value indicating success or failure.
        // This is the current recommended method to use.
        (bool sent, bytes memory data) = _to.call{value: msg.value}("");
        require(sent, "Failed to send Ether");
    }
}
```