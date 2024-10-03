## Day9 2024/10/03
���ѬO�ݻŦ@�Ǫ��ĤE�ѡA�j�a�@�_�[�o�a!!!

## ���`�B�z
### Error
- `error` �����P `revert` �@�_�ϥΡC
- ```Solidity
    error TransferNotOwner(); // �۩w�qerror
    function transferOwner1(uint256 tokenId, address newOwner) public {
        if (_owners[tokenId] != msg.sender) {
            revert TransferNotOwner(); // �ߥX���~
        }
        _owners[tokenId] = newOwner; // �����ಾ�Ҧ��v
    }
    ```
-  `transferOwner1()` ��Ʒ|�ˬd�N�����֦��̬O�_����e�o�e�̡A�p�G���O�A�h�ߥX `TransferNotOwner` ���~�F�p�G�O�A�h���Q������b�C

### Require
- `require` �O�������������`�B�z�覡�A���Q�s�x�ϥΡC���ˬd���󤣦��߮ɡA`require` �|�ߥX���`�A�ê��W���~�y�z�C�ۤ� `error`�A`require` ��gas���Ӹ����A�ר�O�y�z��r�����ɡC
- ```Solidity
    function transferOwner2(uint256 tokenId, address newOwner) public {
        require(_owners[tokenId] == msg.sender, "Transfer Not Owner"); // �T�{�֦���
        _owners[tokenId] = newOwner; // �����ಾ�Ҧ��v
    }
    ```

### Assert
- `assert` �q�`�Ω�}�o�̽ոեN�X�A���ˬd���󤣦��߮ɡA`assert` �|�ߥX���`�C���P `require` ���P�A`assert` ���|���Ѩ��骺���~�H���A�L�k���U�z�ѿ��~��]�C
- ```Solidity
    function transferOwner3(uint256 tokenId, address newOwner) public {
        assert(_owners[tokenId] == msg.sender); // �T�{�֦���
        _owners[tokenId] = newOwner; // �����ಾ�Ҧ��v
    }
    ```

### ����
- Error + revert�G�`��gas�A�B���\�ǻ��ѼơA�A�X��T�ǹF���~�C


## ��ƭ���(overloading)
- ```Solidity
    // �S���Ѽƪ�saySomething���
    function saySomething() public pure returns (string memory) {
        return "Nothing";
    }

    // ����string�Ѽƪ�saySomething���
    function saySomething(string memory something) public pure returns (string memory) {
        return something;
    }
    ```
- �b�o�ӽd�Ҥ��A���M��Ө�ƦW�٬ۦP�A���ѩ�ѼƤ��P�A�sĶ������Ϥ����̡C�b�եήɡA�p�G���ǤJ����ѼơA�h��^ "Nothing"�F�p�G�ǤJ�@�� `string` �ѼơA�h��^�ӰѼƪ����e�C

## Argument Matching
- �b�եέ�����ƮɡA�sĶ���|�ھڶǤJ����ڰѼ������P��Ʃw�q�����Ѽ������i��ǰt�C�p�G���h�Ө�Ư�ǰt�A�h�|�X�{�sĶ���~�C
- ```Solidity
    // �ѼƬ�uint8��f���
    function f(uint8 _in) public pure returns (uint8 out) {
        out = _in;
    }

    // �ѼƬ�uint256��f���
    function f(uint256 _in) public pure returns (uint256 out) {
        out = _in;
    }
    ```
- �p�G�ե� f(50)�A�ѩ� 50 �i�H�Q�۰��ഫ�� `uint8` �M `uint256`�A�sĶ���L�k�T�w�ӽեέ��@�Ө�ơA�]���|�����C