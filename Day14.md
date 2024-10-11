## Day13 2024/10/11
���ѬO�ݻŦ@�Ǫ��Ģ̥|�ѡA�j�a�@�_�[�o�a!!!

### ABI encode �� 4 �ӥD�n���
1. **abi.encode**
    - �N���w���ѼƮھ�ABI�W�h�s�X�A�C�ӰѼƶ�R��32�r�`�A�ë����b�@�_�C
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
    - �Pabi.encode�����A�����|�R�������n����R�A�ϥγ̤֪��Ŷ��i��s�X�C
    -  ```Solidity
            function encodePacked() public view returns(bytes memory result) {
            result = abi.encodePacked(x, addr, name, array);
        }
        ```


3. **abi.encodeWithSignature**
    - �Pabi.encode�ۦ��A�������Ĥ@�ӰѼƬ����ñ�W�A�Ҧp "foo(uint256,address,string,uint256[2])"�C
    - ```Solidity
            function encodeWithSignature() public view returns(bytes memory result) {
            result = abi.encodeWithSignature("foo(uint256,address,string,uint256[2])", x, addr, name, array);
        }
        ```

4. **abi.encodeWithSelector**
    - �Pabi.encodeWithSignature�����A���Ĥ@�ӰѼƬ���ƿ�ܾ��A�o�O���ñ�W��Keccak���ƭȪ��e4�Ӧr�`�C
    - ```Solidity
        function encodeWithSelector() public view returns(bytes memory result) {
            result = abi.encodeWithSelector(bytes4(keccak256("foo(uint256,address,string,uint256[2])")), x, addr, name, array);
        }
        ```