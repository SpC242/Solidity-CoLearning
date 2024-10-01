## Day4 2024/09/26
���ѬO�ݻŦ@�Ǫ��ĥ|�ѡA�j�a�@�_�[�o�a!!!

### Reference Type
- **Array**
    - **�T�w����array[num]** : �n���ɫ��w�Ʋժ��סA�Ʋժ��צb�B��ɤ�����ܡC
        - ```Solidity
            uint[8] array1;       // 8�� uints array
            bytes1[5] array2;     // 5�� bytes array
            address[100] array3;  // 100�� address array
            ```

    - **�ʺA����array[ ]** :  �n���ɤ����w�Ʋժ��סC
        - ```Solidity
            uint[] array4;       // dynamic uints array
            bytes1[] array5;     // dynamic bytes array
            address[] array6;    // dynamic address array
            bytes array7;        // special bytes array�]�`��gas�^
            ```
        - bytes����S��A�Oarray�A�����Υ[[]�C�t�~�A�����byte[]�n����r�`�ƲաA�i�H�ϥ�bytes��bytes1[]�Cbytes �� bytes1[] ��gas�C
    
    - **�Ʋդ�k**
        - length�G�Ω����array���������ƶq�Amemory �Ʋժ����צb�Ыث�O�T�w���C
        - push()�G�ȾA�Ω� dynamic array�A�barray���ݲK�[�@���q�{�ȡ]0�^�����A��^�Ӥ������ޥΡC
        - push(x)�G�barray�����K�[�@�� x �����C
        - pop()�G����array���̫�@�Ӥ���
    
    - **²����:�ʪ���**
        - ```Solidity
            // SPDX-License-Identifier: MIT
            pragma solidity ^0.8.0;

            contract ShoppingCart {
                // �n���@��dynamic array�A�s�x�ʪ��������ӫ~
                string[] public cart;

                // �K�[�ӫ~���ʪ�����
                function addItem(string memory item) public {
                    cart.push(item);  // �ϥ�push�K�[�ӫ~
                }

                // �����ʪ��������̫�@�Ӱӫ~
                function removeLastItem() public {
                    require(cart.length > 0, "�ʪ������šA�L�k����");
                    cart.pop();  // �ϥ�pop�����̫�@�Ӱӫ~
                }

                // ����ʪ������ӫ~���ƶq
                function getItemCount() public view returns (uint) {
                    return cart.length;  // �ϥ�length����ӫ~���ƶq
                }

                // ����ʪ��������w���ު��ӫ~
                function getItem(uint index) public view returns (string memory) {
                    require(index < cart.length, "���޶W�X�d��");
                    return cart[index];  // ��^���w���ު��ӫ~
                }
            }
- **Struct**
    - **�򥻬[�c**
        - ```Solidity
            // �w�qStruct
            struct Student {
                uint256 id;      // �ǥ�ID
                uint256 score;   // �ǥͦ��Z
            }

            // �n���@�� Student �������ܶq
            Student student;
            ```
    - **��Ȥ�k**
        1. **�ϥ� storage �ޥ�**
            - ```Solidity
                // �ϥ�storage�ޥν��
                function initStudent1() external {
                    Student storage _student = student;  // ���student���s�x�ޥ�
                    _student.id = 11;                    // �ק�id
                    _student.score = 100;
                }
                ```
            - storage �ޥΫ��V��W�����A�ܶq�A�����ק�struct�������e�|�v�T�쥻�����A�ܶq�C
        2. **�����ޥΪ��A�ܶq��struct**
            - ```Solidity
                // �����ޥΪ��A�ܶq���
                function initStudent2() external {
                    student.id = 1;
                    student.score = 80;
                }
                ```
        3. **�c�y��Ʀ����**
            - ```Solidity
                // �ϥκc�y��ƽ��
                function initStudent3() external {
                    student = Student(3, 90);  // �c�y��Ʀ����
                }
                ```
        4. **key-value**
            - ```Solidity
                // �ϥ�key-value�Φ����
                function initStudent4() external {
                    student = Student({id: 4, score: 60});  // key-value�����
                    //��ȶ��ǥi���T�w
                }
                ```

    - **²����:�ϮѺ޲z**
        - ```Solidity
            // SPDX-License-Identifier: MIT
            pragma solidity ^0.8.0;

            contract Library {
                // �n��strust�A�s�x�ϮѰT��
                struct Book {
                    uint256 id;      // �Ϯ�ID
                    string title;    // �ϮѼ��D
                    string author;   // �@��
                    uint256 copies;  // �Ѿl�ƥ��ƶq
                }

                // ��struct�s�x�Ҧ��Ϯ�
                Book[] public books;

                // �[�J�s��
                function addBook(uint256 _id, string memory _title, string memory _author, uint256 _copies) public {
                    books.push(Book(_id, _title, _author, _copies));
                }

                // ���ϮѰƥ��ƶq
                function updateCopies(uint index, uint256 _copies) public {
                    require(index < books.length, "���޶W�X�d��");
                    Book storage book = books[index];  // �ϥ�storage�ޥέק�
                    book.copies = _copies;
                }

                // ����ϮѰT��
                function getBook(uint index) public view returns (uint256, string memory, string memory, uint256) {
                    require(index < books.length, "���޶W�X�d��");
                    Book memory book = books[index];
                    return (book.id, book.title, book.author, book.copies);
                }
            }
            ```