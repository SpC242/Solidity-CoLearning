## Day5 2024/09/27
今天是殘酷共學的第五天，大家一起加油吧!!!

### Mapping映射
- **格式** : `mapping(**key** => **value**) public myMapping;`
- **規則** : 
    - 透過key來查詢對應的value(例如透過uint來查詢帳戶address)
    - mapping 的_KeyType只能選擇Solidity內置的值類型，比如uint，address等，**不能用自定義的struct**。
        - 以下方式會報錯:
            ```Solidity
                // 聲明 Struct
                struct Student{
                    uint256 id;
                    uint256 score; 
                }
                mapping(Student => uint) public testVar;
            ```
    - 如果mapping聲明為public，Solidity會自動創建一個getter函數，可以通過Key?查詢對應的Value。
    - mapping不儲存任何 Key 的資訊，也?有length的資訊。
    - 新增key-value pair 的語法 : `_Var[_Key] = _Value`
        ```Solidity
            function writeMap (uint _Key, address _Value) public{
                idToAddress[_Key] = _Value;
            }
        ```
- **Nested Mapping**
    - ```Solidity
        mapping(address => mapping(address => address)) public getPair;
        ```
        - **解釋**
            - 外層映射使用一個 `address` 作為鍵（我們可以稱之為 `tokenA`）。
            - 對應的值是一個內層映射（`mapping(address => address)`）。
            - 內層映射同樣使用一個 `address` 作為鍵（稱為 `tokenB`），其對應的值是一個 `address`，即 `tokenA` 和 `tokenB` 對應的 `Pair` 合約地址。
            - public：將這個映射聲明為 public，意味著 Solidity 自動生成了一個外部可調用的查詢函數。這個函數允許外部合約或用戶查詢 `getPair` 映射，找到兩個代幣之間對應的 `Pair` 合約地址。
        - **功能**
            - 該映射的目的是允許任何人通過兩個代幣地址來查詢它們對應的 `Pair` 合約地址。
            - 它將兩個代幣地址映射到相應的 `Pair` 合約地址。
            - 它確保無論代幣順序如何（`tokenA` 和 `tokenB`），都能查到相同的 `Pair` 合約地址（即 `getPair[tokenA][tokenB]` 和 `getPair[tokenB][tokenA]` 都返回相同的地址）。

    - **ERC20 部分代碼**
        - ```Solidity
            ...
            mapping(address => mapping(address => uint256)) public override allowance;
            ...
            function approve(address spender, uint amount) public override returns (bool) {
                allowance[msg.sender][spender] = amount;
                emit Approval(msg.sender, spender, amount);
                return true;
            }
            ```
        - **解釋**
            - 外層映射的key是 `address`（代表代幣持有者的地址）。
            - 內層映射同樣是以 `address` 為key（代表被授權的地址，即被授權可以代幣轉帳的人）。
            - 內層映射的valueType是 uint256，表示持有者授權給被授權人可以操作的代幣數量。
        - **功能**
            - 該映射的作用是記錄某個地址（owner）授權給另一個地址（spender）可以支配的代幣數量，這樣 spender 可以在授權範圍內代替 owner 花費代幣。
            - 假設用戶 A（代幣持有者）授權用戶 B 可以使用他的一部分代幣，那麼 allowance[A][B] 就會記錄用戶 B 被授權可以從用戶 A 的帳戶中支配的代幣數量。
                - `allowance[A][B] = 100`：表示用戶 A 授權用戶 B 最多可以轉移 100 個代幣。