## Day2 2024/09/24
���ѬO�ݻŦ@�Ǫ��ĤG�ѡA�j�a�@�_�[�o�a!!!

1. **Solidity ����Ƶ��c**
    - ```Solidity
        function <function name>(<parameter types>) {internal|external|public|private} [pure|view|payable] [returns (<return types>)]
        ```
    - 1.1 **<parameter types>�G�Ѽ�����**
    - 1.2 **��ƥi���ʡGpublic�Bprivate�Bexternal �M internal**
        * public�G�Ө�ƥi�H�Q�����M�~���X�ݡC�Y�i�H�q�L�X���~�����եΡA�ΦX��?������ƶi��եΡC
        * private�G�ȭ����X�������ϥΡA�~���L�k�եΡA�~�Ӫ��X���]�L�k�X�ݡC
        * external�G�Ө�ƥu��Q�X���~���եΡA�X���������ઽ���եΦ���ơA���i�H�q�L this.functionName() �Ӷ����եΡC
        * internal�G�ȭ��X�������եΡA�~�Ӫ��X���]�i�H�X�ݸӨ�ơC
        * ```Solidity
            function publicFunc() public { 
                // ����H���i�H�ե�
            }

            function privateFunc() private {
                // �u�����X�������i�H�ե�
            }

            function externalFunc() external {
                // �u��q�L�~���ե�
            }

            function internalFunc() internal {
                // �X�������M�~�ӦX���i�H�ե�
            }
            ```

2. **pure�Bview�Bpayable**
    - 2.1 **pure�Bview**
        - ```Solidity
            // pure��ơA�u�i��B��
            function pureFunc(uint a, uint b) public pure returns (uint) {
                return a + b;
            }

            // view��ơAŪ����W�ƾ�
            uint public storedData;
            function viewFunc() public view returns (uint) {
                return storedData;
            }
            ```
    - 2.2 **payable** 
        - payable ����r��ܸӨ�Ư������ ETH�C��Τ�ե� payable ��ƮɡA�i�H���a ETH ��b�A�Ω󴼯�X�������ľާ@
        - ```Solidity
            // ����ETH�����
            function deposit() public payable {
                // �X���i�H������ETH�|�W�[
            }

            function getBalance() public view returns (uint) {
                return address(this).balance; // ��^��e�X�����l�B
            }
            ```

3. **��ƪ�^(��X)** 
    - 3.1 **returns�Breturn**
        - returns : �i�H���T���w��^�������A�i��^��@�ȩΦh�Ӫ�^�ȡC
        - return : ��^���w���ܶq
        - ```Solidity
            function returnMultiple() public pure returns(uint256, bool, uint256[3] memory) {
                return (1, true, [uint256(1), 2, 5]);
            }
            ```
        * �Ʋ���������^�Ȼݭn�� memory �׹��Ũ��n���A��ܳo�O�@���{���x�s����
    - 3.2 **�R�W����^**
        - �b returns ���A�i�H���e�R�W��^���ܶq�A�o�˦b��Ƥ��u�ݱN�o�Ǥw�R�W���ܶq��ȡASolidity �|�۰ʱN�o�ǭȪ�^�A���ΦA�ϥ� return 
        - ```Solidity
            // �R�W����^
            function returnNamed() public pure returns(uint256 _number, bool _bool, uint256[3] memory _array){
                _number = 2;
                _bool = false;
                _array = [uint256(3),2,1];
            }
            ```
    - 3.3**�Ѻc�����**
        - ```Solidity
            //Case1
            uint256 _number;
            bool _bool;
            uint256[3] memory _array;
            (_number, _bool, _array) = returnNamed();

            //Case2
            bool _bool2;
            (, _bool2, ) = returnNamed();  // �uŪ�� _bool2�A������L��^��
            ```