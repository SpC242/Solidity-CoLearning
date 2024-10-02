## Day8 2024/10/02
���ѬO�ݻŦ@�Ǫ��ĤK�ѡA�j�a�@�_�[�o�a!!!

## Inheritance(�~��)�BAbstract Contracts(��H�X��)�BInterfaces(���f)

1. **Inheritance(�~��)**
    - **����**
        - ���X���]Parent Contract�^�G�]�t�򥻥\����޿�A��L�X���i�H�~�ӳo�ǥ\��C
        - �l�X���]Child Contract�^�G�~�Ӥ��X�����\��öi���X�i�έק�C
    - **�W�h**
        - virtual: ���X��������ơA�Y�Ʊ�l�X�����g�A�ݥ[�W`virtual`����r�C
        - override: �l�X���Y�n���g���X������ơA�ݥ[�W`override`����r�C

2. **Abstract Contracts(��H�X��)**
    - ��H�X���O�㦳�ܤ֤@�ӥ���{��ƪ��X���A�o�ǥ���{����ƨS�����骺��{�A�u�w�q�F��ƪ������C��H�X���L�k�������p�A�������Q��L�X���~�Өù�{�Ҧ�����{����ƫ�A�~�i�Q���p�C
    - �w�q�@�P���޿赲�c�A�����骺��{�浹�~�Ӫ��X���h�����C

3. **Interfaces(���f)**
    - �Ҧ�����Ƴ��O�S����{���¨�ơA���u��ŧi��ơA�L�k�w�q���A�ܼƩι�{��ƪ��޿�C
    - ���f����Ƴ��O `external` �B�S�������{�C
    - �w�q��L�X��������u���欰�W�d�A�ϱo�X�������i�H�̿౵�f�Ӷi�椬�ʡC

- **²��d��**
    - **contract A : ��H�X��**
        ```Solidity
            // SPDX-License-Identifier: MIT
            pragma solidity ^0.8.20;

            // ��H�X���A�w�q�F������{�����
            abstract contract A {
                uint public storedData;

                // ��H��ơA�S�������{
                function set(uint x) public virtual;

                // �w��{���
                function get() public view returns (uint) {
                    return storedData;
                }
            }
        ```

    - **contract B : �~�ӻP��{(�i�Q�G�p)**
        ```Solidity
            // SPDX-License-Identifier: MIT
            pragma solidity ^0.8.20;

            import "./A.sol";       //import�\�ध��|�Ǩ�

            // �X�� B �~�Ӧ۩�H�X�� A �ù�{��H���
            contract B is A {
                // ��{��H���
                function set(uint x) public override {
                    storedData = x;
                }
            }
        ```

    - **interface IC : �w�q���f**
        ```Solidity
            // SPDX-License-Identifier: MIT
            pragma solidity ^0.8.20;

            // �w�q���f IC�A�W�d�Y�Ǧ欰
            interface IC {
                function set(uint x) external;
                function get() external view returns (uint);
            }
        ```

    - **contract D : ��{���f���~��**
        ```Solidity
            // SPDX-License-Identifier: MIT
            pragma solidity ^0.8.20;

            import "./IC.sol";

            // �X�� D ��{���f IC �å[�J�B�~�޿�
            contract D is IC {
                uint public storedData;

                // ��{ set ���
                function set(uint x) public override {
                    storedData = x;
                }

                // ��{ get ���
                function get() public view override returns (uint) {
                    return storedData;
                }
            }
        ```

- **modifier �~��**
    - **contract A : ���X���w�qmodifier**
        ```Solidity
            // SPDX-License-Identifier: MIT
            pragma solidity ^0.8.20;

            contract A {
                address public owner;

                // �w�q�@�ӭ׹����A�����ƥu���owner�ե�
                modifier onlyOwner() virtual {
                    require(msg.sender == owner, "Not the owner");
                    _; // �~�����׹������
                }

                // �c�y��ơA��l��owner���X�����p��
                constructor() {
                    owner = msg.sender;
                }

                // �u��owner�~��եΦ����
                function setOwner(address _newOwner) public onlyOwner {
                    owner = _newOwner;
                }
            }
        ```

    - **contract B : �l�X���~�ӻP���gmodifier**
        ```Solidity
            // SPDX-License-Identifier: MIT
            pragma solidity ^0.8.20;

            import "./A.sol";

            contract B is A {
                // ���g�׹����A�K�[���P�����~�T��
                modifier onlyOwner() override {
                    require(msg.sender == owner, "You are not authorized in B");
                    _; // �~�����׹������
                }

                function specialFunction() public onlyOwner {
                    // �u��owner�i�H����o�ӯS��\��
                }
            }
        ```
- **constructor �~��**
    - **contract A : ���X���w�qconstructor**
        ```Solidity
            // SPDX-License-Identifier: MIT
            pragma solidity ^0.8.20;

            contract A {
                uint public value;

                // ���X�����c�y��ơA��l��value�ܼ�
                constructor(uint _value) {
                    value = _value;
                }
            }
        ```
    - **contract B : �l�X���~��constructor**
        ```Solidity
            // SPDX-License-Identifier: MIT
            pragma solidity ^0.8.20;

            import "./A.sol";

            contract B is A {
                // �l�X�����c�y��ơA�~�ӨýեΤ��X��A���c�y���
                constructor(uint _value) A(_value * 2) {
                    // �o�̥i�H�[�JB�X������l���޿�
                }
            }
        ```