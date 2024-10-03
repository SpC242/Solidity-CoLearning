## Day9 2024/10/03
今天是殘酷共學的第九天，大家一起加油吧!!!

## 異常處理
### Error
- `error` 必須與 `revert` 一起使用。
- ```Solidity
    error TransferNotOwner(); // 自定義error
    function transferOwner1(uint256 tokenId, address newOwner) public {
        if (_owners[tokenId] != msg.sender) {
            revert TransferNotOwner(); // 拋出錯誤
        }
        _owners[tokenId] = newOwner; // 完成轉移所有權
    }
    ```
-  `transferOwner1()` 函數會檢查代幣的擁有者是否為當前發送者，如果不是，則拋出 `TransferNotOwner` 錯誤；如果是，則順利完成轉帳。

### Require
- `require` 是早期版本的異常處理方式，仍被廣泛使用。當檢查條件不成立時，`require` 會拋出異常，並附上錯誤描述。相比 `error`，`require` 的gas消耗較高，尤其是描述文字較長時。
- ```Solidity
    function transferOwner2(uint256 tokenId, address newOwner) public {
        require(_owners[tokenId] == msg.sender, "Transfer Not Owner"); // 確認擁有者
        _owners[tokenId] = newOwner; // 完成轉移所有權
    }
    ```

### Assert
- `assert` 通常用於開發者調試代碼，當檢查條件不成立時，`assert` 會拋出異常。但與 `require` 不同，`assert` 不會提供具體的錯誤信息，無法幫助理解錯誤原因。
- ```Solidity
    function transferOwner3(uint256 tokenId, address newOwner) public {
        assert(_owners[tokenId] == msg.sender); // 確認擁有者
        _owners[tokenId] = newOwner; // 完成轉移所有權
    }
    ```

### 結論
- Error + revert：節省gas，且允許傳遞參數，適合精確傳達錯誤。


## 函數重載(overloading)
- ```Solidity
    // 沒有參數的saySomething函數
    function saySomething() public pure returns (string memory) {
        return "Nothing";
    }

    // 接收string參數的saySomething函數
    function saySomething(string memory something) public pure returns (string memory) {
        return something;
    }
    ```
- 在這個範例中，雖然兩個函數名稱相同，但由於參數不同，編譯器能夠區分它們。在調用時，如果不傳入任何參數，則返回 "Nothing"；如果傳入一個 `string` 參數，則返回該參數的內容。

## Argument Matching
- 在調用重載函數時，編譯器會根據傳入的實際參數類型與函數定義中的參數類型進行匹配。如果有多個函數能匹配，則會出現編譯錯誤。
- ```Solidity
    // 參數為uint8的f函數
    function f(uint8 _in) public pure returns (uint8 out) {
        out = _in;
    }

    // 參數為uint256的f函數
    function f(uint256 _in) public pure returns (uint256 out) {
        out = _in;
    }
    ```
- 如果調用 f(50)，由於 50 可以被自動轉換為 `uint8` 和 `uint256`，編譯器無法確定該調用哪一個函數，因此會報錯。