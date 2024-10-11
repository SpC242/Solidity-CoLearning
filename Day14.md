## Day13 2024/10/11
今天是殘酷共學的第十四天，大家一起加油吧!!!

### ABI encode 的 4 個主要函數
1. **abi.encode**
    - 將給定的參數根據ABI規則編碼，每個參數填充為32字節，並拼接在一起。
    - ```Solidity
        uint x = 10;
        address addr = 0x7A58c0Be72BE218B41C608b7Fe7C5bB630736C71;
        string name = "0xAA";
        uint[2] array = [5, 6];

        function encode() public view returns(bytes memory result) {
            result = abi.encode(x, addr, name, array);
        }
        ```

2. **abi.encodePacked**
    - 與abi.encode類似，但它會刪除不必要的填充，使用最少的空間進行編碼。
    -  ```Solidity
            function encodePacked() public view returns(bytes memory result) {
            result = abi.encodePacked(x, addr, name, array);
        }
        ```


3. **abi.encodeWithSignature**
    - 與abi.encode相似，但它的第一個參數為函數簽名，例如 "foo(uint256,address,string,uint256[2])"。
    - ```Solidity
            function encodeWithSignature() public view returns(bytes memory result) {
            result = abi.encodeWithSignature("foo(uint256,address,string,uint256[2])", x, addr, name, array);
        }
        ```

4. **abi.encodeWithSelector**
    - 與abi.encodeWithSignature類似，但第一個參數為函數選擇器，這是函數簽名的Keccak哈希值的前4個字節。
    - ```Solidity
        function encodeWithSelector() public view returns(bytes memory result) {
            result = abi.encodeWithSelector(bytes4(keccak256("foo(uint256,address,string,uint256[2])")), x, addr, name, array);
        }
        ```