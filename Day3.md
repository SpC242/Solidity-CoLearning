## Day3 2024/09/25
今天是殘酷共學的第三天，大家一起加油吧!!!

### 數據存儲
-   | **數據** | **存儲位置** | **修改性** | **生命週期** | **gas消耗** | **用途** |
    | :---: | :---: | :---: | :---: | :---: | :---: |
    | storage | 鏈上永久存儲 | 可修改 | 合約生命週期內 | 多 | 用於合約的狀態變量，數據存在鏈上 |
    | memory | 臨時存儲 | 可修改 | 函數調用期間 | 少 | 用於函數內部的臨時變量、局部變量 |
    | calldata | 臨時存儲 | 不可修改 | 函數調用期間 | 少 | 用於函數的參數傳遞，適合不可修改的外部傳入數據 |

### 作用域
- **狀態變數(State Variables)**
    - 存儲在鏈上，可被所有合約訪問
    - ```Solidity 
        contract Variables {
            uint public x = 1;
            uint public y;
            string public z;
        }
        ```
- **局部變數(Local Variable)**
    - 只有呼叫此函數時會存在
    - ```Solidity
        function getValue() public view returns(uint){
            uint value = 1
        }
        ```
- **全局變數(Public Variable)**
    - Solidity 的預留關鍵字，可直接使用
        - block.number: (uint) 當前區塊的number
        - block.timestamp: (uint) 當前區塊的時間戳，為unix紀元以來的秒
        - gasleft(): (uint256) 剩餘 gas
        - msg.data: (bytes calldata) 完整call data
        - msg.sender: (address payable) 消息發送者 (當前 caller)
        - msg.value: (uint) 當前交易發送的 wei 值