## Day1 2024/09/23
���ѬO�ݻŦ@�Ǫ��Ĥ@�ѡA�j�a�@�_�[�o�a!!!

### Solidity �򥻮榡
``` Solidity
// SPDX-License-Identifier: MIT  //�����{���X��Liencese
pragma solidity ^0.8.21;         //�n��Solidity�������A^0.8.21�����O�̧C�i������ 0.8.21 ���ӽsĶ
contract HelloWeb3{              //�X���D��A�Ыؤ@�ӦX���W��HelloWeb3
    string public _string = "Hello Web3!";
}
```

### Value Type
1. **�������**
    - 1.1 **���Ÿ����(int)**
        - int �i�ΨӪ�ܥ��ơB�t�ơB�s�CSolidity �q�{�����Ÿ���ƪ����O�O int256
            - �d��G
                - int256 �����Ƚd��G-2^255 �� 2^255 - 1
                - Solidity �]��� int8, int16, int32 ������
            - ```Solidity
                int public signedInt = -42;
                int256 public specificSignedInt = -100; // ���̫��w 256 ��
                ```

    - 1.2 **�L�Ÿ����(uint)**
        - uint �u��ΨӪ�ܫD�t��ơA�q�{�����A�O uint256 
        - �d��G
            - uint256 �����Ƚd��G0 �� 2^256 - 1
            - �P�ˤ�� uint8, uint16, uint32 ������(�w���r�`�Ʋ�)
        - ```Solidity
            uint public unsignedInt = 42;
            uint256 public specificUnsignedInt = 100; // ���̫��w 256 ��
            ```
    
2. **bool����**
    - ���Ȭ� true / false
    - ```Solidity
        // bool �B��
        bool public _bool1 = !_bool; // ���D
        bool public _bool2 = _bool && _bool1; // �P
        bool public _bool3 = _bool || _bool1; // ��
        bool public _bool4 = _bool == _bool1; // �۵�
        bool public _bool5 = _bool != _bool1; // ���۵�
        ```

3. **�`�ΰ򥻹B���**
    - 3.1 **��N�B���**
    - ```Solidity
        uint public a = 10;
        uint public b = 3;
        uint public sum = a + b;  // 13
        uint public diff = a - b; // 7
        uint public prod = a * b; // 30
        uint public quot = a / b; // 3
        uint public mod = a % b;  // 1
        uint public pow = a ** b; // 1000 (10^3)
        ```
    - 3.2 **����B���**
        - �Ω�����ӼƭȡA��^bool�G
        - "<"�G�p�_
        - "<="�G�p�_���_
        - ">"�G�j�_
        - ">="�G�j�_���_
        - "=="�G���_
        - "!="�G�����_
    - 3.3 **�첾���**
        - "&"�G����and
        - "|"�G����or
        - "^"�G����xor
        - "~"�G�������
        - "<<"�G����
        - ">>"�G�k��
        - ```Solidity
            uint public bitAnd = a & b;  // 2
            uint public bitOr = a | b;   // 11
            uint public bitXor = a ^ b;  // 9
            uint public bitNot = ~a;     // 0xffff...fff5 (����Ҧ���)
            uint public leftShift = a << 1; // 20 (10 * 2)
            uint public rightShift = a >> 1; // 5 (10 / 2)
        ```