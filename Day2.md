## Day2 2024/09/24
今天是殘酷共學的第二天，大家一起加油吧!!!

1. **Solidity 的函數結構**
    - ```Solidity
        function <function name>(<parameter types>) {internal|external|public|private} [pure|view|payable] [returns (<return types>)]
        ```
    - 1.1 **<parameter types>：參數類型**
    - 1.2 **函數可見性：public、private、external 和 internal**
        * public：該函數可以被內部和外部訪問。即可以通過合約外部的調用，或合約?部的函數進行調用。
        * private：僅限本合約內部使用，外部無法調用，繼承的合約也無法訪問。
        * external：該函數只能被合約外部調用，合約內部不能直接調用此函數，但可以通過 this.functionName() 來間接調用。
        * internal：僅限合約內部調用，繼承的合約也可以訪問該函數。
        * ```Solidity
            function publicFunc() public { 
                // 任何人都可以調用
            }

            function privateFunc() private {
                // 只有本合約內部可以調用
            }

            function externalFunc() external {
                // 只能通過外部調用
            }

            function internalFunc() internal {
                // 合約內部和繼承合約可以調用
            }
            ```

2. **pure、view、payable**
    - 2.1 **pure、view**
        - ```Solidity
            // pure函數，只進行運算
            function pureFunc(uint a, uint b) public pure returns (uint) {
                return a + b;
            }

            // view函數，讀取鏈上數據
            uint public storedData;
            function viewFunc() public view returns (uint) {
                return storedData;
            }
            ```
    - 2.2 **payable** 
        - payable 關鍵字表示該函數能夠接收 ETH。當用戶調用 payable 函數時，可以附帶 ETH 轉帳，用於智能合約的金融操作
        - ```Solidity
            // 接收ETH的函數
            function deposit() public payable {
                // 合約可以接收的ETH會增加
            }

            function getBalance() public view returns (uint) {
                return address(this).balance; // 返回當前合約的餘額
            }
            ```

3. **函數返回(輸出)** 
    - 3.1 **returns、return**
        - returns : 可以明確指定返回的類型，可返回單一值或多個返回值。
        - return : 返回指定的變量
        - ```Solidity
            function returnMultiple() public pure returns(uint256, bool, uint256[3] memory) {
                return (1, true, [uint256(1), 2, 5]);
            }
            ```
        * 數組類型的返回值需要用 memory 修飾符來聲明，表示這是一個臨時儲存的值
    - 3.2 **命名式返回**
        - 在 returns 中，可以提前命名返回的變量，這樣在函數中只需將這些已命名的變量賦值，Solidity 會自動將這些值返回，不用再使用 return 
        - ```Solidity
            // 命名式返回
            function returnNamed() public pure returns(uint256 _number, bool _bool, uint256[3] memory _array){
                _number = 2;
                _bool = false;
                _array = [uint256(3),2,1];
            }
            ```
    - 3.3**解構式賦值**
        - ```Solidity
            //Case1
            uint256 _number;
            bool _bool;
            uint256[3] memory _array;
            (_number, _bool, _array) = returnNamed();

            //Case2
            bool _bool2;
            (, _bool2, ) = returnNamed();  // 只讀取 _bool2，忽略其他返回值
            ```