## Day16 2024/10/14
���ѬO�ݻŦ@�Ǫ��Ģ̤��ѡA�j�a�@�_�[�o�a!!!

`try-catch` �O�ΨӳB�z����X�����~����ƽեΥ��Ѫ����`�B�z����C���b Solidity 0.6 �������Q�ޤJ�A�A�X�Ω�~���X����ƪ��եΩγЫئX���ɪ��c�y��ơ]constructor�^�C
```Solidity
try externalContract.f() {
    // �եΦ��\�ɹB�檺�N�X
} catch {
    // �եΥ��ѮɹB�檺�N�X
}
```

**�ϥ� this.f() �եΦۨ����**
�i�H�ϥ� this.f() �ӽեΦۨ����~����ơA������b�X�����c�y��Ƥ��ϥΡA�]�����ɦX���٥��������p�C
```Solidity
try this.f() {
    // �եΦ��\�ɹB��
} catch {
    // �եΥ��ѮɹB��
}
```

**�B�z�a��^�Ȫ��~���ե�**
```Solidity
try externalContract.f() returns(returnType val) {
    // �եΦ��\�ɹB��A�åB�i�H�ϥΪ�^�� val
} catch {
    // �եΥ��ѮɹB��
}
```

**����S�w���`����**
```Solidity
// �b�Ыطs�X�����ϥ�try-catch �]�X���ЫسQ����external call�^
// executeNew(0)�|���Ѩ�����`CatchEvent`
// executeNew(1)�|���Ѩ�����`CatchByte`
// executeNew(2)�|���\������`SuccessEvent`
function executeNew(uint a) external returns (bool success) {
    try new OnlyEven(a) returns(OnlyEven _even){
        // call���\�����p�U
        emit SuccessEvent();
        success = _even.onlyEven(a);
    } catch Error(string memory reason) {
        // catch���Ѫ� revert() �M require()
        emit CatchEvent(reason);
    } catch (bytes memory reason) {
        // catch���Ѫ� assert()
        emit CatchByte(reason);
    }
}
```