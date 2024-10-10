## Day13 2024/10/10
今天是殘酷共學的第十三天，大家一起加油吧!!!

## Create

`create` 是 `Solidity` 中使用 **`new`** 關鍵字創建新合約的標準方法。當使用 `create` 時，合約會在區塊鏈上部署一個新的合約實例，並返回新合約的地址。
```Solidity
Contract x = new Contract{value: _value}(params);
```
- `Contract` ：表示要創建的合約名，該合約已經在 Solidity 中定義。
- `x` ：新創建的合約對象（地址）。
- `{value: _value}`（可選） ：如果新合約的構造函數是 `payable`，可以在創建時轉入 `_value` 數量的 ETH。
- `params` ：新合約構造函數所需的參數。
- ```Solidity
        新地址 = hash(創建者地址, nonce)
    ```
    - `nonce` 隨著交易次數變化，因此合約地址難以預測。

## Create2
使用 `Create2` 創建合約與 `Create` 類似，只是多了一個 `salt` 參數。
```Solidity
Contract x = new Contract{salt: _salt, value: _value}(params);
```
- `Contract` ：表示要創建的合約名。
- `x` ：新創建的合約對象（地址）。
- `{salt: _salt, value: _value}` ：指定的 `salt` 值和創建時要轉入的 ETH 數量（_value 可選）。
- `params` ：新合約構造函數的參數。
- ```Solidity
        新地址 = hash(0xFF, 創建者地址, salt, initcode)
    ```
    - `0xFF` 是固定常數，避免與 `Create` 發生衝突。
    - `salt` 是一個創建者指定的 `bytes32` 值，用來影響新合約的地址。
    - `initcode` 是新合約的初始字節碼（包括合約創建時的代碼和構造函數參數）。

## 簡單範例
- **Simple Contract**
    ```Solidity
        // SPDX-License-Identifier: MIT
        pragma solidity ^0.8.0;

        contract SimpleContract {
            uint256 public value;
            
            // payable 構造函數，可以在創建合約時轉入ETH
            constructor(uint256 _value) payable {
                value = _value;
            }

            // 返回合約 ETH 餘額
            function getBalance() public view returns (uint256) {
                return address(this).balance;
            }
        }
    ```
- **FactoryContract**
    ```Solidity
        // SPDX-License-Identifier: MIT
        pragma solidity ^0.8.0;

        contract FactoryContract {
            // 使用 CREATE2 創建 SimpleContract 的函數
            function createSimpleContract(uint256 _value, bytes32 _salt) public payable returns (address) {
                // 使用 salt 和 msg.value 創建新合約
                SimpleContract newContract = new SimpleContract{salt: _salt, value: msg.value}(_value);
                return address(newContract);
            }

            // 預測合約地址
            function calculateAddress(bytes32 _salt, bytes memory _initCode) public view returns (address predicted) {
                return address(uint160(uint(keccak256(abi.encodePacked(
                    bytes1(0xff),
                    address(this),
                    _salt,
                    keccak256(_initCode)
                )))));
            }
        }
    ```