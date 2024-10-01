## Day7 2024/10/01
���ѬO�ݻŦ@�Ǫ��ĤC�ѡA�j�a�@�_�[�o�a!!!

### Event (�ƥ�)
- **�T��** : �ƥ�]Events�^ �O�@�ӥΩ�N����X���M�~�����γs��������u��CDApp �q�`�|��ť����X��Ĳ�o���ƥ�A�q�ӹ�{�D�P�B��s�����ζi���L�ާ@�C
- **�g��** : �ƥ�O�@�ج۹�g�٪��ƾڦs�x�覡�C�C�Өƥ󪺰O���j������2,000 gas�A�ۤ񤧤U�A��W�s�x�ܶq�ܤֻݭn20,000 gas�A�]���b�Y�Ǳ��p�U�A�ƥ��s�x�ܶq��`�ٸ귽�C

- **�y�k**
```Solidity
// �ŧi
event name([argument, ...]) [anonymous]; 
event FundTransfer(address to, uint value);

// �I�s
emit name([argument, ...]);
emit FundTransfer(someAddress, 100);
```

- **²��d��:�v�Ь���**
```Solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract SimpleAuction {
    address public highestBidder;  // ��e�̰��X����
    uint256 public highestBid;     // ��e�̰��X�����B
    address public owner;          // �v�Ь��ʪ��o�_��
    bool public ended;             // �v�ЬO�_����

    // �w�q�ƥ�A�Ω�q���s���̰��X��
    event HighestBidIncreased(address indexed bidder, uint256 amount);
    // �w�q�ƥ�A�Ω�q���v�е�����Ĺ�a
    event AuctionEnded(address winner, uint256 amount);

    // �c�y��ơA��l���v�Ъ��֦���
    constructor() {
        owner = msg.sender; // �X�����p�̬O�v�Ъ��֦���
        highestBid = 0;     // ��l�̰��X���]��0
        ended = false;      // �v�Х�����
    }

    // �׹����G����u���֦��̤~�����S�w�ާ@
    modifier onlyOwner() {
        require(msg.sender == owner, "�A���O�v�Ь��ʪ��o�_��");
        _;
    }

    // �ѻP�v�Ъ����
    function bid() public payable {
        require(!ended, "�v�Фw�g����");  // �T�O�v�Х�����
        require(msg.value > highestBid, "�X�����������e�̰��X��");

        // ��s�̰��X���̩M�X�����B
        highestBidder = msg.sender;
        highestBid = msg.value;

        // ����s���̰��X���ƥ�
        emit HighestBidIncreased(msg.sender, msg.value);
    }

    // �����v�СA�ñN�̰��X��������൹�o�_��
    function endAuction() public onlyOwner {
        require(!ended, "�v�Фw�g����");

        // �N�v�маO���w����
        ended = true;

        // �����v�е����ƥ�
        emit AuctionEnded(highestBidder, highestBid);

        // �N����൹�v�Ъ��֦��̡]�o�_�̡^
        payable(owner).transfer(highestBid);
    }
}
```