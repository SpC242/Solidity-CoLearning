## Day3 2024/09/25
���ѬO�ݻŦ@�Ǫ��ĤT�ѡA�j�a�@�_�[�o�a!!!

### �ƾڦs�x
-   | **�ƾ�** | **�s�x��m** | **�ק��** | **�ͩR�g��** | **gas����** | **�γ~** |
    | :---: | :---: | :---: | :---: | :---: | :---: |
    | storage | ��W�ä[�s�x | �i�ק� | �X���ͩR�g���� | �h | �Ω�X�������A�ܶq�A�ƾڦs�b��W |
    | memory | �{�ɦs�x | �i�ק� | ��ƽեδ��� | �� | �Ω��Ƥ������{���ܶq�B�����ܶq |
    | calldata | �{�ɦs�x | ���i�ק� | ��ƽեδ��� | �� | �Ω��ƪ��Ѽƶǻ��A�A�X���i�ק諸�~���ǤJ�ƾ� |

### �@�ΰ�
- **���A�ܼ�(State Variables)**
    - �s�x�b��W�A�i�Q�Ҧ��X���X��
    - ```Solidity 
        contract Variables {
            uint public x = 1;
            uint public y;
            string public z;
        }
        ```
- **�����ܼ�(Local Variable)**
    - �u���I�s����Ʈɷ|�s�b
    - ```Solidity
        function getValue() public view returns(uint){
            uint value = 1
        }
        ```
- **�����ܼ�(Public Variable)**
    - Solidity ���w�d����r�A�i�����ϥ�
        - block.number: (uint) ��e�϶���number
        - block.timestamp: (uint) ��e�϶����ɶ��W�A��unix�����H�Ӫ���
        - gasleft(): (uint256) �Ѿl gas
        - msg.data: (bytes calldata) ����call data
        - msg.sender: (address payable) �����o�e�� (��e caller)
        - msg.value: (uint) ��e����o�e�� wei ��