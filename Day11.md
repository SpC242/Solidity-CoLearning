## Day11 2024/10/07
���ѬO�ݻŦ@�Ǫ��Ģ̤@�ѡA�j�a�@�_�[�o�a!!!

## ����ETH
1. **receive() ���**
    - `receive()` ��ƱM���ΨӳB�z�X������ ETH �����p�A���u��Ω󱵦� ETH ��b�A�åB���H�U�S�I�G
        - �u�঳�@�� `receive()` ���
        - ���ݭn function ����r
        - ������ external �M payable �׹�
        - �L�ѼơB�L��^��
```Solidity
// �w�q�ƥ�
event Received(address Sender, uint Value);

// ����ETH��Ĳ�o�ƥ�
receive() external payable {
    emit Received(msg.sender, msg.value);
}
```

2. **fallback() ���**
    - `fallback()` ��ƥΩ��ر��p�G
        - �X���Q�եΤF�@�Ӥ��s�b�����
        - ���� ETH �åB�S�� `receive()` ��ƥi�ѽե�
    - `fallback()` ��ƦP�ˤ��ݭn function ����r�A�åB�q�`�|�[�W `payable` �׹��H�K���� ETH�C
    - ���i�H�Ω�N�z�X���]proxy contract�^�ӳB�z���ǰt����ƽեΡC
```Solidity
// �w�q�ƥ�
event fallbackCalled(address Sender, uint Value, bytes Data);

// ��QĲ�o�ɡA�O���եΪ��H��
fallback() external payable {
    emit fallbackCalled(msg.sender, msg.value, msg.data);
}
```

3. **receive() �M fallback() ���ϧO**
�o��Ө�Ƴ��౵�� ETH�A��Ĳ�o�W�h���P�C
�p�G�X������ ETH �ɡAmsg.data �O�Ū��A�B�s�b `receive()` ��ơA����NĲ�o `receive()` ��ơC
�p�G msg.data ���O�Ū��A�Ϊ̦X���S���w�q `receive()` ��ơA����NĲ�o `fallback()` ��ơC
```scss
         ����ETH
            |
       msg.data�O�šH  
          /  \
        �O    �_
        /      \
  receive()�s�b?  fallback()
      / \
     �O  �_
    /     \
receive() fallback()
```

## �o�eETH
1. **�ϥ� `transfer()` �o�e ETH**
    - Gas ����G2300 gas�A������b�������������޿�C
    - ���ѳB�z�G�۰� `revert` (�^�u)����C

2. **�ϥ� `send()` �o�e ETH**
    - Gas ����G2300 gas�A�P `transfer()` �ۦP�C
    - ���ѳB�z�G���|�۰� `revert` ����A�ݤ���ˬd�óB�z�C
    - `send()` ��^���A�� `bool` �A�N����b���\�Υ��ѡC

3. **�ϥ� `call()` �o�e ETH**
    - Gas ����G�L����A�i�Ω��������޿�C
    - ���ѳB�z�G���|�۰� `revert` ����A�ݤ���ˬd�óB�z�C
    - `call()` ��^���A�� `(bool,bytes)` �A�䤤 `bool` �N����b���\�Υ��ѡC 


## ²��d��
```Solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

contract ReceiveEther {
    // Function to receive Ether. msg.data must be empty
    receive() external payable {}

    // Fallback function is called when msg.data is not empty
    fallback() external payable {}

    function getBalance() public view returns (uint256) {
        return address(this).balance;
    }
}

contract SendEther {
    function sendViaTransfer(address payable _to) public payable {
        // This function is no longer recommended for sending Ether.
        _to.transfer(msg.value);
    }

    function sendViaSend(address payable _to) public payable {
        // Send returns a boolean value indicating success or failure.
        // This function is not recommended for sending Ether.
        bool sent = _to.send(msg.value);
        require(sent, "Failed to send Ether");
    }

    function sendViaCall(address payable _to) public payable {
        // Call returns a boolean value indicating success or failure.
        // This is the current recommended method to use.
        (bool sent, bytes memory data) = _to.call{value: msg.value}("");
        require(sent, "Failed to send Ether");
    }
}
```