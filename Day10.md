## Day10 2024/10/05
���ѬO�ݻŦ@�Ǫ��Ģ̤ѡA�j�a�@�_�[�o�a!!!

## Library �w�X��
�w�X���O�@�دS������X���A���b�����N�X���i���ΩʩM��� gas �O�ΡC���i�H�N�`�Ϊ��\��ʸ˦��@�ӦX���A�æb���P���X�����i��_�ΡC�w�X���q�`�Ѹg���״I���}�o�̩ιζ��ЫءA���}�o�̴��Ѳ{�����u��C

### �w�X���P���q�X�����ϧO
- **�L���A�ܶq**�G�w�X�����঳���A�ܶq�A�o�ϱo����[���q�B���|�O�s���󪬺A�C
- **�L�k�~�өγQ�~��**�G�w�X�������~�Ө�L�X���A��L�k�Q��L�X���~�ӡC
- **�L�k�����H�ӹ�**�G�w�X���L�k�����H�ӹ��A�o�O�ѩ��]�p�ت��ȭ��󴣨ѥ\��A�ӫD�i��겣�޲z�C
- **�L�k�Q�P��**�G�w�X�����p��L�k�Q�P���A�o�˽T�O�F���ƪ����[�i�ΩʡC
- **��ƪ��i���ʼv�T�եΤ覡**�G�w�X������ `public` �� `external` ��Ʒ|Ĳ�o�@�� `delegatecall`�A�� `internal` ��Ƥ��|�C

### �ϥΤ覡
1. �Q�� `using for` ���O
    - `using A for B;` ���O�N�w A ���[������ B �W�A�o�ϱo�w������Ʀ����������ܶq��������ơA�q�ӥi�H�����եΡC
    - ```Solidity
        using Strings for uint256;
        function getString1(uint256 _number) public pure returns (string memory) {
            // �w�X��������Ʒ|�۰ʲK�[�� uint256 ���ܶq������
            return _number.toHexString();
        }
        ```
2. ���q�L�w�X���W�ٽեΨ��
    - ```Solidity
        function getString2(uint256 _number) public pure returns (string memory) {
            return Strings.toHexString(_number);
        }
        ```
    - �����եΤF `Strings` �w�X���� `toHexString` ��ơA�öǤJ�Ѽ� `_number`�C

### import
1. **�q�L��󪺬۹���|�i��ɤJ**
    -  ��X�������P�@�ؿ��ά����ؿ����ɡA�i�H�ϥΤ�󪺬۹���|�i��ޥΡC
    - ```Solidity
        // �q�L���۹��m�i��import
        import './Yeye.sol';
        ```
2. **�q�L���}�ޥκ��W���X��**
    - ```Solidity
        // �q�L���}�ޥκ��W�}���X��
        import 'https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/utils/Address.sol';
        ```
3. **�q�L npm �ؿ��ɤJ**
    - ```Solidity
        // �q npm �ؿ��ɤJ
        import '@openzeppelin/contracts/access/Ownable.sol';
        ```
4. **�ɤJ�S�w�������Ÿ�**
    -  ```Solidity
        // �u�ɤJ���w���X���βŸ�
        import {Yeye} from './Yeye.sol';
        ```