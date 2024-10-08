## Day12 2024/10/08
���ѬO�ݻŦ@�Ǫ��Ģ̤G�ѡA�j�a�@�_�[�o�a!!!

## �եΤw���p���X��
### **�ؼЦX���G`OtherContract`**
```Solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract OtherContract {
    uint256 private _x = 0; // ���A�ܶq _x
    event Log(uint amount, uint gas); // ���� ETH ��Ĳ�o���ƥ�

    // ��^�X���l�B
    function getBalance() public view returns (uint) {
        return address(this).balance;
    }

    // �]�m _x ���ȡA�ñ��� ETH
    function setX(uint256 x) external payable {
        _x = x;
        if (msg.value > 0) {
            emit Log(msg.value, gasleft());
        }
    }

    // ��� _x ����
    function getX() external view returns (uint x) {
        x = _x;
    }
}
```

### **�ե� `OtherContract` �X��**
1. **�ǤJ�X���a�}**
    - �o�ؤ�k���\�b��Ƥ��ǤJ�ؼЦX�����a�}�A�åͦ��X�����ޥΨӽեΨ��ơC
    - ```Solidity
        function callSetX(address _Address, uint256 x) external {
            OtherContract(_Address).setX(x);
        }
        ```
2. **�ǤJ�X���ܶq**
    - �i�H�����b��Ƥ��ǤJ�X�����ޥΡA�Ӥ��u�O�a�}�C
    - ```solidity
        function callGetX(OtherContract _Address) external view returns (uint x) {
            x = _Address.getX();
        }
        ```
3. **�ЫئX���ܶq**
    - �i�H�b��Ƥ��ЫئX���ܶq�A�M��ϥθ��ܶq�ӽեΥؼЦX������ơC
    - ```Solidity
        function callGetX2(address _Address) external view returns (uint x) {
            OtherContract oc = OtherContract(_Address);
            x = oc.getX();
        }
        ```
4. **�եΦX���õo�e ETH**
    - �p�G�ؼЦX������ƬO `payable` �A�i�H�q�L����k�V�ؼЦX���o�e ETH�C�ϥ� {value: msg.value} �i�H�b�եήɪ��[ ETH�C
    - ```Solidity
        function setXTransferETH(address otherContract, uint256 x) payable external {
            OtherContract(otherContract).setX{value: msg.value}(x);
        }
        ```

## **�Q�� `call` �եΨ�L�X��**
 `call` �O `address` �������C�Ŧ�����ơA���i�H�ΨӻP��L�X���椬�C
 `call` �O `Solidity` �x����˪��q�LĲ�o `fallback` �� `receive` ��Ƶo�eETH����k�C

1. **�եή榡�G**
    - ```solidity
        �ؼЦX���a�}.call(�r�`�X);
        ```
    - �r�`�X�i�H�ϥ� `abi.encodeWithSignature` �ӥͦ��C
        - ```solidity
            abi.encodeWithSignature("���ñ�W", �r�����j������Ѽ�);
            ```
        - ���ñ�W�� "��ƦW��(�r�����j���Ѽ�����)"
2. **���w ETH �M gas�G**
    - `call` �٥i�H���w�o�e�� ETH �M gas �ƶq�G
    - ```solidity
        �ؼЦX���a�}.call{value: ��J ETH �ƶq, gas: gas �ƶq}(�r�`�X);
        ```
3. **��^�ȡG**
    - `call` ��^�@�Ӥ��� `(bool, bytes memory)`�A�䤤 bool ��ܽեάO�_���\�Abytes memory �O��ƪ���^�ƾڡC

### ²��d��
```Solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

contract Receiver {
    event Received(address caller, uint256 amount, string message);

    fallback() external payable {
        emit Received(msg.sender, msg.value, "Fallback was called");
    }

    function foo(string memory _message, uint256 _x) public payable returns (uint256){
        emit Received(msg.sender, msg.value, _message);
        return _x + 1;
    }
}

contract Caller {
    event Response(bool success, bytes data);

    // Let's imagine that contract Caller does not have the source code for the
    // contract Receiver, but we do know the address of contract Receiver and the function to call.
    function testCallFoo(address payable _addr) public payable {
        // You can send ether and specify a custom gas amount
        (bool success, bytes memory data) = _addr.call{
            value: msg.value,
            gas: 5000
        }(abi.encodeWithSignature("foo(string,uint256)", "call foo", 123));

        emit Response(success, data);
    }

    // Calling a function that does not exist triggers the fallback function.
    function testCallDoesNotExist(address payable _addr) public payable {
        (bool success, bytes memory data) = _addr.call{value: msg.value}(
            abi.encodeWithSignature("doesNotExist()")
        );

        emit Response(success, data);
    }
}
```

## **delegatecall**
`delegatecall` �O `Solidity` ���C�Ŧ�����ơA�P `call` �����A����̤����s�b���n���W�U��t���C�b `delegatecall` ���A���ެO�եΨ�L�X������ơA�����A�ܶq���ܧ�o�@�Φb�եΦX���]�]�N�O�o�_ `delegatecall` ���X���^�W�C�]���A`delegatecall` �����γ����D�n�O�N�z�X���]Proxy Contract�^�M�X���ɯšC

1. `delegatecall` �P `call` ���ϧO
    - `call` ���W�U��G
        ��ϥ� `call` �եΨ�L�X������ƮɡA���檺�O�ؼЦX������ơA�W�U���ݩ�ؼЦX���C�]�N�O���G
        msg.sender �O�o�_ `call` ���X���a�}�C
        ���A�ܶq���ܧ�@�Φb�ؼЦX���W�C
    - `delegatecall` ���W�U��G
        �ϥ� `delegatecall` �ɡA���檺�O�ؼЦX������ơA���W�U�头�M�O�o�_ `delegatecall` ���X���C�]�N�O���G
        ``msg.sender`` �O�̪�եΦX�����~���a�}�C
        ���A�ܶq���ܧ�@�Φb�o�_ `delegatecall` ���X���W�C

2. **�եή榡�G**
    - ```solidity
        �ؼЦX���a�}.delegatecall(�r�`�X);
        ```
    - �r�`�X�i�H�ϥ� `abi.encodeWithSignature` �ӥͦ��C
        - ```solidity
            abi.encodeWithSignature("���ñ�W", �r�����j������Ѽ�);
            ```
        - ���ñ�W�� "��ƦW��(�r�����j���Ѽ�����)"

3. **����:�N�z�X���]Proxy Contract�^**
- �N�z�X�����]�p�z���G
    - �b����X���}�o���A�N�޿�P���A�����O�@�ر`�����]�p�Ҧ��C�o�˪��[�c���\�޿�X���ɯšA�Ӥ��|�v�T�s�x�b�N�z�X���������A�ܶq�C
    - �N�z�X���t�d�s�x�Ҧ����A�ܶq�A���޿�X���t�d��������޿�C��ݭn�ɯ��޿�ɡA�u�ݧ�s�N�z�X�����޿�X�����a�}�C
    - �N�z�X���|�ϥ� `delegatecall` �ӽե��޿�X��������ơA�o�˪��A�ܶq���ܧ�N�@�Ω�N�z�X���A�Ӥ��O�޿�X���C

### ²��d��
```Solidity
 // SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

// NOTE: Deploy this contract first
contract B {
    // NOTE: storage layout must be the same as contract A
    uint256 public num;
    address public sender;
    uint256 public value;

    function setVars(uint256 _num) public payable {
        num = _num;
        sender = msg.sender;
        value = msg.value;
    }
}

contract A {
    uint256 public num;
    address public sender;
    uint256 public value;

    function setVars(address _contract, uint256 _num) public payable {
        // A's storage is set, B is not modified.
        (bool success, bytes memory data) = _contract.delegatecall(
            abi.encodeWithSignature("setVars(uint256)", _num)
        );
    }
}
```