## Day8 2024/10/02
今天是殘酷共學的第八天，大家一起加油吧!!!

## Inheritance(繼承)、Abstract Contracts(抽象合約)、Interfaces(接口)

1. **Inheritance(繼承)**
    - **概念**
        - 父合約（Parent Contract）：包含基本功能或邏輯，其他合約可以繼承這些功能。
        - 子合約（Child Contract）：繼承父合約的功能並進行擴展或修改。
    - **規則**
        - virtual: 父合約中的函數，若希望子合約重寫，需加上`virtual`關鍵字。
        - override: 子合約若要重寫父合約的函數，需加上`override`關鍵字。

2. **Abstract Contracts(抽象合約)**
    - 抽象合約是具有至少一個未實現函數的合約，這些未實現的函數沒有具體的實現，只定義了函數的介面。抽象合約無法直接部署，它必須被其他合約繼承並實現所有未實現的函數後，才可被部署。
    - 定義共同的邏輯結構，但具體的實現交給繼承的合約去完成。

3. **Interfaces(接口)**
    - 所有的函數都是沒有實現的純函數，它只能宣告函數，無法定義狀態變數或實現函數的邏輯。
    - 接口的函數都是 `external` 且沒有具體實現。
    - 定義其他合約必須遵守的行為規範，使得合約之間可以依賴接口來進行互動。

- **簡單範例**
    - **contract A : 抽象合約**
        ```Solidity
            // SPDX-License-Identifier: MIT
            pragma solidity ^0.8.20;

            // 抽象合約，定義了但未實現的函數
            abstract contract A {
                uint public storedData;

                // 抽象函數，沒有具體實現
                function set(uint x) public virtual;

                // 已實現函數
                function get() public view returns (uint) {
                    return storedData;
                }
            }
        ```

    - **contract B : 繼承與實現(可被佈署)**
        ```Solidity
            // SPDX-License-Identifier: MIT
            pragma solidity ^0.8.20;

            import "./A.sol";       //import功能之後會學到

            // 合約 B 繼承自抽象合約 A 並實現抽象函數
            contract B is A {
                // 實現抽象函數
                function set(uint x) public override {
                    storedData = x;
                }
            }
        ```

    - **interface IC : 定義接口**
        ```Solidity
            // SPDX-License-Identifier: MIT
            pragma solidity ^0.8.20;

            // 定義接口 IC，規範某些行為
            interface IC {
                function set(uint x) external;
                function get() external view returns (uint);
            }
        ```

    - **contract D : 實現接口並繼承**
        ```Solidity
            // SPDX-License-Identifier: MIT
            pragma solidity ^0.8.20;

            import "./IC.sol";

            // 合約 D 實現接口 IC 並加入額外邏輯
            contract D is IC {
                uint public storedData;

                // 實現 set 函數
                function set(uint x) public override {
                    storedData = x;
                }

                // 實現 get 函數
                function get() public view override returns (uint) {
                    return storedData;
                }
            }
        ```

- **modifier 繼承**
    - **contract A : 父合約定義modifier**
        ```Solidity
            // SPDX-License-Identifier: MIT
            pragma solidity ^0.8.20;

            contract A {
                address public owner;

                // 定義一個修飾器，限制函數只能由owner調用
                modifier onlyOwner() virtual {
                    require(msg.sender == owner, "Not the owner");
                    _; // 繼續執行修飾的函數
                }

                // 構造函數，初始化owner為合約部署者
                constructor() {
                    owner = msg.sender;
                }

                // 只有owner才能調用此函數
                function setOwner(address _newOwner) public onlyOwner {
                    owner = _newOwner;
                }
            }
        ```

    - **contract B : 子合約繼承與重寫modifier**
        ```Solidity
            // SPDX-License-Identifier: MIT
            pragma solidity ^0.8.20;

            import "./A.sol";

            contract B is A {
                // 重寫修飾器，添加不同的錯誤訊息
                modifier onlyOwner() override {
                    require(msg.sender == owner, "You are not authorized in B");
                    _; // 繼續執行修飾的函數
                }

                function specialFunction() public onlyOwner {
                    // 只有owner可以執行這個特殊功能
                }
            }
        ```
- **constructor 繼承**
    - **contract A : 父合約定義constructor**
        ```Solidity
            // SPDX-License-Identifier: MIT
            pragma solidity ^0.8.20;

            contract A {
                uint public value;

                // 父合約的構造函數，初始化value變數
                constructor(uint _value) {
                    value = _value;
                }
            }
        ```
    - **contract B : 子合約繼承constructor**
        ```Solidity
            // SPDX-License-Identifier: MIT
            pragma solidity ^0.8.20;

            import "./A.sol";

            contract B is A {
                // 子合約的構造函數，繼承並調用父合約A的構造函數
                constructor(uint _value) A(_value * 2) {
                    // 這裡可以加入B合約的初始化邏輯
                }
            }
        ```