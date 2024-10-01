## Day1 2024/09/23
今天是殘酷共學的第一天，大家一起加油吧!!!

### Solidity 基本格式
``` Solidity
// SPDX-License-Identifier: MIT  //說明程式碼的Liencese
pragma solidity ^0.8.21;         //聲明Solidity的版本，^0.8.21指的是最低可接受用 0.8.21 版來編譯
contract HelloWeb3{              //合約主體，創建一個合約名為HelloWeb3
    string public _string = "Hello Web3!";
}
```

### Value Type
1. **整數類型**
    - 1.1 **有符號整數(int)**
        - int 可用來表示正數、負數、零。Solidity 默認的有符號整數的類別是 int256
            - 範圍：
                - int256 的取值範圍：-2^255 到 2^255 - 1
                - Solidity 也支持 int8, int16, int32 等類型
            - ```Solidity
                int public signedInt = -42;
                int256 public specificSignedInt = -100; // 明确指定 256 位
                ```

    - 1.2 **無符號整數(uint)**
        - uint 只能用來表示非負整數，默認的型態是 uint256 
        - 範圍：
            - uint256 的取值範圍：0 到 2^256 - 1
            - 同樣支持 uint8, uint16, uint32 等類型(定長字節數組)
        - ```Solidity
            uint public unsignedInt = 42;
            uint256 public specificUnsignedInt = 100; // 明确指定 256 位
            ```
    
2. **bool類型**
    - 取值為 true / false
    - ```Solidity
        // bool 運算
        bool public _bool1 = !_bool; // 取非
        bool public _bool2 = _bool && _bool1; // 與
        bool public _bool3 = _bool || _bool1; // 或
        bool public _bool4 = _bool == _bool1; // 相等
        bool public _bool5 = _bool != _bool1; // 不相等
        ```

3. **常用基本運算符**
    - 3.1 **算術運算符**
    - ```Solidity
        uint public a = 10;
        uint public b = 3;
        uint public sum = a + b;  // 13
        uint public diff = a - b; // 7
        uint public prod = a * b; // 30
        uint public quot = a / b; // 3
        uint public mod = a % b;  // 1
        uint public pow = a ** b; // 1000 (10^3)
        ```
    - 3.2 **比較運算符**
        - 用於比較兩個數值，返回bool：
        - "<"：小于
        - "<="：小于等于
        - ">"：大于
        - ">="：大于等于
        - "=="：等于
        - "!="：不等于
    - 3.3 **位移算符**
        - "&"：按位and
        - "|"：按位or
        - "^"：按位xor
        - "~"：按位取反
        - "<<"：左移
        - ">>"：右移
        - ```Solidity
            uint public bitAnd = a & b;  // 2
            uint public bitOr = a | b;   // 11
            uint public bitXor = a ^ b;  // 9
            uint public bitNot = ~a;     // 0xffff...fff5 (反轉所有位)
            uint public leftShift = a << 1; // 20 (10 * 2)
            uint public rightShift = a >> 1; // 5 (10 / 2)
        ```