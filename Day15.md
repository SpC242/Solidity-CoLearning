## Day15 2024/10/11
���ѬO�ݻŦ@�Ǫ��Ģ̤��ѡA�j�a�@�_�[�o�a!!!

Method ID �w�q�����ñ�W�g Keccak ���ƫ᪺�e 4 �Ӧr�`�C
���ñ�W �O��ƪ��W�٥[�W�Ѽƪ������C�Ҧp�A��� mint ��ñ�W�� mint(address)�C
```Solidity
function elementaryParamSelector() external pure returns(bytes4) {
    return bytes4(keccak256("elementaryParamSelector(uint256,bool)"));
}
```