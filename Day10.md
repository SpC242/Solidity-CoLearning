## Day10 2024/10/05
今天是殘酷共學的第十天，大家一起加油吧!!!

## Library 庫合約
庫合約是一種特殊的智能合約，旨在提高代碼的可重用性和減少 gas 費用。它可以將常用的功能封裝成一個合約，並在不同的合約中進行復用。庫合約通常由經驗豐富的開發者或團隊創建，為開發者提供現成的工具。

### 庫合約與普通合約的區別
- **無狀態變量**：庫合約不能有狀態變量，這使得它更加輕量且不會保存任何狀態。
- **無法繼承或被繼承**：庫合約不能繼承其他合約，亦無法被其他合約繼承。
- **無法接收以太幣**：庫合約無法接收以太幣，這是由於其設計目的僅限於提供功能，而非進行資產管理。
- **無法被銷毀**：庫合約部署後無法被銷毀，這樣確保了其函數的持久可用性。
- **函數的可見性影響調用方式**：庫合約中的 `public` 或 `external` 函數會觸發一次 `delegatecall`，而 `internal` 函數不會。

### 使用方式
1. 利用 `using for` 指令
    - `using A for B;` 指令將庫 A 附加到類型 B 上，這使得庫中的函數成為該類型變量的成員函數，從而可以直接調用。
    - ```Solidity
        using Strings for uint256;
        function getString1(uint256 _number) public pure returns (string memory) {
            // 庫合約中的函數會自動添加為 uint256 型變量的成員
            return _number.toHexString();
        }
        ```
2. 接通過庫合約名稱調用函數
    - ```Solidity
        function getString2(uint256 _number) public pure returns (string memory) {
            return Strings.toHexString(_number);
        }
        ```
    - 直接調用了 `Strings` 庫合約的 `toHexString` 函數，並傳入參數 `_number`。

### import
1. **通過文件的相對路徑進行導入**
    -  當合約文件位於同一目錄或相關目錄中時，可以使用文件的相對路徑進行引用。
    - ```Solidity
        // 通過文件相對位置進行import
        import './Yeye.sol';
        ```
2. **通過網址引用網上的合約**
    - ```Solidity
        // 通過網址引用網上開源合約
        import 'https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/utils/Address.sol';
        ```
3. **通過 npm 目錄導入**
    - ```Solidity
        // 從 npm 目錄導入
        import '@openzeppelin/contracts/access/Ownable.sol';
        ```
4. **導入特定的全局符號**
    -  ```Solidity
        // 只導入指定的合約或符號
        import {Yeye} from './Yeye.sol';
        ```