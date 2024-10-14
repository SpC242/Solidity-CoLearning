## Day16 2024/10/14
今天是殘酷共學的第十六天，大家一起加油吧!!!

`try-catch` 是用來處理智能合約中外部函數調用失敗的異常處理機制。它在 Solidity 0.6 版本中被引入，適合用於外部合約函數的調用或創建合約時的構造函數（constructor）。
```Solidity
try externalContract.f() {
    // 調用成功時運行的代碼
} catch {
    // 調用失敗時運行的代碼
}
```

**使用 this.f() 調用自身函數**
可以使用 this.f() 來調用自身的外部函數，但不能在合約的構造函數中使用，因為此時合約還未完成部署。
```Solidity
try this.f() {
    // 調用成功時運行
} catch {
    // 調用失敗時運行
}
```

**處理帶返回值的外部調用**
```Solidity
try externalContract.f() returns(returnType val) {
    // 調用成功時運行，並且可以使用返回值 val
} catch {
    // 調用失敗時運行
}
```

**捕獲特定異常類型**
```Solidity
// 在創建新合約中使用try-catch （合約創建被視為external call）
// executeNew(0)會失敗並釋放`CatchEvent`
// executeNew(1)會失敗並釋放`CatchByte`
// executeNew(2)會成功並釋放`SuccessEvent`
function executeNew(uint a) external returns (bool success) {
    try new OnlyEven(a) returns(OnlyEven _even){
        // call成功的情況下
        emit SuccessEvent();
        success = _even.onlyEven(a);
    } catch Error(string memory reason) {
        // catch失敗的 revert() 和 require()
        emit CatchEvent(reason);
    } catch (bytes memory reason) {
        // catch失敗的 assert()
        emit CatchByte(reason);
    }
}
```