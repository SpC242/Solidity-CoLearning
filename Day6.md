## Day6 2024/09/28
���ѬO�ݻŦ@�Ǫ��Ĥ��ѡA�j�a�@�_�[�o�a!!!

### Constructor
- �غc��ƥu�|�b�X�����p�ɹB��@���A���ᤣ�|�A�Q�եΡA�]�����A�X�ΨӰ���@�ǥu�ݹB��@������l�Ƥu�@�C
- **�y�k**
    - ```Solidity
        contract MyContract {
            address public owner;

            // �ϥ� constructor �w�q�غc���
            constructor(address initialOwner) public {
                owner = initialOwner;
            }
        }
        ```
- **�X�����~��**�G �b�~�ӵ��c���A���X�����غc��Ʒ|�b�l�X�����غc��Ƥ��e�Q�եΡC�i�H�ϥ� super() �㦡�եΤ��X�����غc��ƨöǻ��ѼơC
- **�غc��ƥu�����@��**�G�X�����p������A�غc��ƱN�û����|�A���Q�եΡC

### Modifier
- **�γ~**
    - �b�����ƫe�i��w�ˬd�C
    - �����ư��檺����C
    - ��֥N�X���ơA���ɦX�����w���ʩM�iŪ�ʡC
- **�w�q onlyOwner modifier**
    - ```Solidity
        modifier onlyOwner {
            require(msg.sender == owner); // �ˬd�եΪ̬O�_���֦���
            _; // �p�G�ˬd�q�L�A�~������ƥD��F�_�h revert ���
        }
        ```
    - `require(msg.sender == owner)` �Ω��ˬd�եθӨ�ƪ��H�]`msg.sender`�^�O�_�O�X�����֦��̡]`owner`�^�C
    - `_` ��ܨ�ƪ��D��N�b�o�̰���A���ˬd�q�L�ɡA�~������Ƥ��e�C

- **�a�� onlyOwner modifier �����**
    - ```Solidity
        function changeOwner(address _newOwner) external onlyOwner {
            owner = _newOwner; // �u����e���֦��̥i�H���֦���
        }
        ```

### ²����:�Ыί���t��
```Solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract RentHouse {
    address public owner;  // �ЪF���a�}
    address public tenant; // �ЫȪ��a�}
    uint256 public rent;   // ����
    bool public isRented;  // ����A

    // constructor�A��l�ƩЪF�a�}�M����
    constructor(uint256 initialRent) {
        owner = msg.sender; // ���p�X�����H�O�ЪF
        rent = initialRent; // �]�w��l����
        isRented = false;   // ��l���A��������
    }

    // modifier�G����u���ЪF�~��եΪ����
    modifier onlyOwner() {
        require(msg.sender == owner, "�A���O�ЪF");
        _;
    }

    // modifier�G����u�����Ȥ~��եΪ����
    modifier onlyTenant() {
        require(msg.sender == tenant, "�A���O�Ы�");
        _;
    }

    // �ЪF�]�m�����A�u��ЪF�ե�
    function setRent(uint256 newRent) public onlyOwner {
        rent = newRent;
    }

    // �ЪF��㯲�ȤJ��A�ó]�w���Ȧa�}
    function approveTenant(address _tenant) public onlyOwner {
        tenant = _tenant;
        isRented = true; // �]�w���w����
    }

    // �ЫȤ�I����
    function payRent() public payable onlyTenant {
        require(msg.value == rent, "�Ф�I���T���������B");
        // �N�����൹�ЪF
        payable(owner).transfer(msg.value);
    }

    // �ЪF�X�v�Ыȡ]���������^�A�u��ЪF�ե�
    function evictTenant() public onlyOwner {
        require(isRented == true, "�ثe�S������");
        tenant = address(0); // �M�ů��Ȧa�}
        isRented = false;    // �]�w��������
    }
}
```