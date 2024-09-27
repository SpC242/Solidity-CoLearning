## Day5 2024/09/27
���ѬO�ݻŦ@�Ǫ��Ĥ��ѡA�j�a�@�_�[�o�a!!!

### Mapping�M�g
- **�榡** : `mapping(**key** => **value**) public myMapping;`
- **�W�h** : 
    - �z�Lkey�Ӭd�߹�����value(�Ҧp�z�Luint�Ӭd�߱b��address)
    - mapping ��_KeyType�u����Solidity���m���������A��puint�Aaddress���A**����Φ۩w�q��struct**�C
        - �H�U�覡�|����:
            ```Solidity
                // �n�� Struct
                struct Student{
                    uint256 id;
                    uint256 score; 
                }
                mapping(Student => uint) public testVar;
            ```
    - �p�Gmapping�n����public�ASolidity�|�۰ʳЫؤ@��getter��ơA�i�H�q�LKey?�d�߹�����Value�C
    - mapping���x�s���� Key ����T�A�]?��length����T�C
    - �s�Wkey-value pair ���y�k : `_Var[_Key] = _Value`
        ```Solidity
            function writeMap (uint _Key, address _Value) public{
                idToAddress[_Key] = _Value;
            }
        ```
- **Nested Mapping**
    - ```Solidity
        mapping(address => mapping(address => address)) public getPair;
        ```
        - **����**
            - �~�h�M�g�ϥΤ@�� `address` �@����]�ڭ̥i�H�٤��� `tokenA`�^�C
            - �������ȬO�@�Ӥ��h�M�g�]`mapping(address => address)`�^�C
            - ���h�M�g�P�˨ϥΤ@�� `address` �@����]�٬� `tokenB`�^�A��������ȬO�@�� `address`�A�Y `tokenA` �M `tokenB` ������ `Pair` �X���a�}�C
            - public�G�N�o�ӬM�g�n���� public�A�N���� Solidity �۰ʥͦ��F�@�ӥ~���i�եΪ��d�ߨ�ơC�o�Ө�Ƥ��\�~���X���ΥΤ�d�� `getPair` �M�g�A����ӥN������������ `Pair` �X���a�}�C
        - **�\��**
            - �ӬM�g���ت��O���\����H�q�L��ӥN���a�}�Ӭd�ߥ��̹����� `Pair` �X���a�}�C
            - ���N��ӥN���a�}�M�g������� `Pair` �X���a�}�C
            - ���T�O�L�ץN�����Ǧp��]`tokenA` �M `tokenB`�^�A����d��ۦP�� `Pair` �X���a�}�]�Y `getPair[tokenA][tokenB]` �M `getPair[tokenB][tokenA]` ����^�ۦP���a�}�^�C

    - **ERC20 �����N�X**
        - ```Solidity
            ...
            mapping(address => mapping(address => uint256)) public override allowance;
            ...
            function approve(address spender, uint amount) public override returns (bool) {
                allowance[msg.sender][spender] = amount;
                emit Approval(msg.sender, spender, amount);
                return true;
            }
            ```
        - **����**
            - �~�h�M�g��key�O `address`�]�N��N�������̪��a�}�^�C
            - ���h�M�g�P�ˬO�H `address` ��key�]�N��Q���v���a�}�A�Y�Q���v�i�H�N����b���H�^�C
            - ���h�M�g��valueType�O uint256�A��ܫ����̱��v���Q���v�H�i�H�ާ@���N���ƶq�C
        - **�\��**
            - �ӬM�g���@�άO�O���Y�Ӧa�}�]owner�^���v���t�@�Ӧa�}�]spender�^�i�H��t���N���ƶq�A�o�� spender �i�H�b���v�d�򤺥N�� owner ��O�N���C
            - ���]�Τ� A�]�N�������̡^���v�Τ� B �i�H�ϥΥL���@�����N���A���� allowance[A][B] �N�|�O���Τ� B �Q���v�i�H�q�Τ� A ���b�ᤤ��t���N���ƶq�C
                - `allowance[A][B] = 100`�G��ܥΤ� A ���v�Τ� B �̦h�i�H�ಾ 100 �ӥN���C