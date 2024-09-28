## Day6 2024/09/28
今天是殘酷共學的第六天，大家一起加油吧!!!

### Constructor
- 建構函數只會在合約部署時運行一次，之後不會再被調用，因此它適合用來執行一些只需運行一次的初始化工作。
- **語法**
    - ```Solidity
        contract MyContract {
            address public owner;

            // 使用 constructor 定義建構函數
            constructor(address initialOwner) public {
                owner = initialOwner;
            }
        }
        ```
- **合約的繼承**： 在繼承結構中，父合約的建構函數會在子合約的建構函數之前被調用。可以使用 super() 顯式調用父合約的建構函數並傳遞參數。
- **建構函數只能執行一次**：合約部署完成後，建構函數將永遠不會再次被調用。

### Modifier
- **用途**
    - 在執行函數前進行預檢查。
    - 控制函數執行的條件。
    - 減少代碼重複，提升合約的安全性和可讀性。
- **定義 onlyOwner modifier**
    - ```Solidity
        modifier onlyOwner {
            require(msg.sender == owner); // 檢查調用者是否為擁有者
            _; // 如果檢查通過，繼續執行函數主體；否則 revert 交易
        }
        ```
    - `require(msg.sender == owner)` 用於檢查調用該函數的人（`msg.sender`）是否是合約的擁有者（`owner`）。
    - `_` 表示函數的主體將在這裡執行，當檢查通過時，繼續執行函數內容。

- **帶有 onlyOwner modifier 的函數**
    - ```Solidity
        function changeOwner(address _newOwner) external onlyOwner {
            owner = _newOwner; // 只有當前的擁有者可以更改擁有者
        }
        ```

### 簡單實例:房屋租賃系統
```Solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract RentHouse {
    address public owner;  // 房東的地址
    address public tenant; // 房客的地址
    uint256 public rent;   // 租金
    bool public isRented;  // 租賃狀態

    // constructor，初始化房東地址和租金
    constructor(uint256 initialRent) {
        owner = msg.sender; // 部署合約的人是房東
        rent = initialRent; // 設定初始租金
        isRented = false;   // 初始狀態為未租賃
    }

    // modifier：限制只有房東才能調用的函數
    modifier onlyOwner() {
        require(msg.sender == owner, "你不是房東");
        _;
    }

    // modifier：限制只有租客才能調用的函數
    modifier onlyTenant() {
        require(msg.sender == tenant, "你不是房客");
        _;
    }

    // 房東設置租金，只能房東調用
    function setRent(uint256 newRent) public onlyOwner {
        rent = newRent;
    }

    // 房東批准租客入住，並設定租客地址
    function approveTenant(address _tenant) public onlyOwner {
        tenant = _tenant;
        isRented = true; // 設定為已租賃
    }

    // 房客支付租金
    function payRent() public payable onlyTenant {
        require(msg.value == rent, "請支付正確的租金金額");
        // 將租金轉給房東
        payable(owner).transfer(msg.value);
    }

    // 房東驅逐房客（結束租約），只能房東調用
    function evictTenant() public onlyOwner {
        require(isRented == true, "目前沒有租客");
        tenant = address(0); // 清空租客地址
        isRented = false;    // 設定為未租賃
    }
}
```