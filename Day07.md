## Day7 2024/10/01
今天是殘酷共學的第七天，大家一起加油吧!!!

### Event (事件)
- **響應** : 事件（Events） 是一個用於將智能合約和外部應用連接的關鍵工具。DApp 通常會監聽智能合約觸發的事件，從而實現非同步更新頁面或進行其他操作。
- **經濟** : 事件是一種相對經濟的數據存儲方式。每個事件的記錄大約消耗2,000 gas，相比之下，鏈上存儲變量至少需要20,000 gas，因此在某些情況下，事件比存儲變量更節省資源。

- **語法**
```Solidity
// 宣告
event name([argument, ...]) [anonymous]; 
event FundTransfer(address to, uint value);

// 呼叫
emit name([argument, ...]);
emit FundTransfer(someAddress, 100);
```

- **簡單範例:競標活動**
```Solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract SimpleAuction {
    address public highestBidder;  // 當前最高出價者
    uint256 public highestBid;     // 當前最高出價金額
    address public owner;          // 競標活動的發起者
    bool public ended;             // 競標是否結束

    // 定義事件，用於通知新的最高出價
    event HighestBidIncreased(address indexed bidder, uint256 amount);
    // 定義事件，用於通知競標結束及贏家
    event AuctionEnded(address winner, uint256 amount);

    // 構造函數，初始化競標的擁有者
    constructor() {
        owner = msg.sender; // 合約部署者是競標的擁有者
        highestBid = 0;     // 初始最高出價設為0
        ended = false;      // 競標未結束
    }

    // 修飾器：限制只有擁有者才能執行特定操作
    modifier onlyOwner() {
        require(msg.sender == owner, "你不是競標活動的發起者");
        _;
    }

    // 參與競標的函數
    function bid() public payable {
        require(!ended, "競標已經結束");  // 確保競標未結束
        require(msg.value > highestBid, "出價必須高於當前最高出價");

        // 更新最高出價者和出價金額
        highestBidder = msg.sender;
        highestBid = msg.value;

        // 釋放新的最高出價事件
        emit HighestBidIncreased(msg.sender, msg.value);
    }

    // 結束競標，並將最高出價的資金轉給發起者
    function endAuction() public onlyOwner {
        require(!ended, "競標已經結束");

        // 將競標標記為已結束
        ended = true;

        // 釋放競標結束事件
        emit AuctionEnded(highestBidder, highestBid);

        // 將資金轉給競標的擁有者（發起者）
        payable(owner).transfer(highestBid);
    }
}
```