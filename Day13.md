## Day13 2024/10/10
���ѬO�ݻŦ@�Ǫ��Ģ̤T�ѡA�j�a�@�_�[�o�a!!!

## Create

`create` �O `Solidity` ���ϥ� **`new`** ����r�Ыطs�X�����зǤ�k�C��ϥ� `create` �ɡA�X���|�b�϶���W���p�@�ӷs���X����ҡA�ê�^�s�X�����a�}�C
```Solidity
Contract x = new Contract{value: _value}(params);
```
- `Contract` �G��ܭn�Ыت��X���W�A�ӦX���w�g�b Solidity ���w�q�C
- `x` �G�s�Ыت��X����H�]�a�}�^�C
- `{value: _value}`�]�i��^ �G�p�G�s�X�����c�y��ƬO `payable`�A�i�H�b�Ыخ���J `_value` �ƶq�� ETH�C
- `params` �G�s�X���c�y��Ʃһݪ��ѼơC
- ```Solidity
        �s�a�} = hash(�Ыت̦a�}, nonce)
    ```
    - `nonce` �H�ۥ�������ܤơA�]���X���a�}���H�w���C

## Create2
�ϥ� `Create2` �ЫئX���P `Create` �����A�u�O�h�F�@�� `salt` �ѼơC
```Solidity
Contract x = new Contract{salt: _salt, value: _value}(params);
```
- `Contract` �G��ܭn�Ыت��X���W�C
- `x` �G�s�Ыت��X����H�]�a�}�^�C
- `{salt: _salt, value: _value}` �G���w�� `salt` �ȩM�Ыخɭn��J�� ETH �ƶq�]_value �i��^�C
- `params` �G�s�X���c�y��ƪ��ѼơC
- ```Solidity
        �s�a�} = hash(0xFF, �Ыت̦a�}, salt, initcode)
    ```
    - `0xFF` �O�T�w�`�ơA�קK�P `Create` �o�ͽĬ�C
    - `salt` �O�@�ӳЫت̫��w�� `bytes32` �ȡA�ΨӼv�T�s�X�����a�}�C
    - `initcode` �O�s�X������l�r�`�X�]�]�A�X���Ыخɪ��N�X�M�c�y��ưѼơ^�C

## ²��d��
- **Simple Contract**
    ```Solidity
        // SPDX-License-Identifier: MIT
        pragma solidity ^0.8.0;

        contract SimpleContract {
            uint256 public value;
            
            // payable �c�y��ơA�i�H�b�ЫئX������JETH
            constructor(uint256 _value) payable {
                value = _value;
            }

            // ��^�X�� ETH �l�B
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
            // �ϥ� CREATE2 �Ы� SimpleContract �����
            function createSimpleContract(uint256 _value, bytes32 _salt) public payable returns (address) {
                // �ϥ� salt �M msg.value �Ыطs�X��
                SimpleContract newContract = new SimpleContract{salt: _salt, value: msg.value}(_value);
                return address(newContract);
            }

            // �w���X���a�}
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