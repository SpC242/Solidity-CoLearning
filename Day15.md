## Day15 2024/10/11
今天是殘酷共學的第十五天，大家一起加油吧!!!

Method ID 定義為函數簽名經 Keccak 哈希後的前 4 個字節。
函數簽名 是函數的名稱加上參數的類型。例如，函數 mint 的簽名為 mint(address)。
```Solidity
function elementaryParamSelector() external pure returns(bytes4) {
    return bytes4(keccak256("elementaryParamSelector(uint256,bool)"));
}
```