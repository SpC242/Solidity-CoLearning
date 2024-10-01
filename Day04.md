## Day4 2024/09/26
今天是殘酷共學的第四天，大家一起加油吧!!!

### Reference Type
- **Array**
    - **固定長度array[num]** : 聲明時指定數組長度，數組長度在運行時不能改變。
        - ```Solidity
            uint[8] array1;       // 8個 uints array
            bytes1[5] array2;     // 5個 bytes array
            address[100] array3;  // 100個 address array
            ```

    - **動態長度array[ ]** :  聲明時不指定數組長度。
        - ```Solidity
            uint[] array4;       // dynamic uints array
            bytes1[] array5;     // dynamic bytes array
            address[] array6;    // dynamic address array
            bytes array7;        // special bytes array（節省gas）
            ```
        - bytes比較特殊，是array，但不用加[]。另外，不能用byte[]聲明單字節數組，可以使用bytes或bytes1[]。bytes 比 bytes1[] 省gas。
    
    - **數組方法**
        - length：用於獲取array中的元素數量，memory 數組的長度在創建後是固定的。
        - push()：僅適用於 dynamic array，在array末端添加一個默認值（0）元素，返回該元素的引用。
        - push(x)：在array末尾添加一個 x 元素。
        - pop()：移除array的最後一個元素
    
    - **簡單實例:購物車**
        - ```Solidity
            // SPDX-License-Identifier: MIT
            pragma solidity ^0.8.0;

            contract ShoppingCart {
                // 聲明一個dynamic array，存儲購物車中的商品
                string[] public cart;

                // 添加商品到購物車中
                function addItem(string memory item) public {
                    cart.push(item);  // 使用push添加商品
                }

                // 移除購物車中的最後一個商品
                function removeLastItem() public {
                    require(cart.length > 0, "購物車為空，無法移除");
                    cart.pop();  // 使用pop移除最後一個商品
                }

                // 獲取購物車中商品的數量
                function getItemCount() public view returns (uint) {
                    return cart.length;  // 使用length獲取商品的數量
                }

                // 獲取購物車中指定索引的商品
                function getItem(uint index) public view returns (string memory) {
                    require(index < cart.length, "索引超出範圍");
                    return cart[index];  // 返回指定索引的商品
                }
            }
- **Struct**
    - **基本架構**
        - ```Solidity
            // 定義Struct
            struct Student {
                uint256 id;      // 學生ID
                uint256 score;   // 學生成績
            }

            // 聲明一個 Student 類型的變量
            Student student;
            ```
    - **賦值方法**
        1. **使用 storage 引用**
            - ```Solidity
                // 使用storage引用賦值
                function initStudent1() external {
                    Student storage _student = student;  // 獲取student的存儲引用
                    _student.id = 11;                    // 修改id
                    _student.score = 100;
                }
                ```
            - storage 引用指向鏈上的狀態變量，直接修改struct中的內容會影響原本的狀態變量。
        2. **直接引用狀態變量的struct**
            - ```Solidity
                // 直接引用狀態變量賦值
                function initStudent2() external {
                    student.id = 1;
                    student.score = 80;
                }
                ```
        3. **構造函數式賦值**
            - ```Solidity
                // 使用構造函數賦值
                function initStudent3() external {
                    student = Student(3, 90);  // 構造函數式賦值
                }
                ```
        4. **key-value**
            - ```Solidity
                // 使用key-value形式賦值
                function initStudent4() external {
                    student = Student({id: 4, score: 60});  // key-value式賦值
                    //賦值順序可不固定
                }
                ```

    - **簡單實例:圖書管理**
        - ```Solidity
            // SPDX-License-Identifier: MIT
            pragma solidity ^0.8.0;

            contract Library {
                // 聲明strust，存儲圖書訊息
                struct Book {
                    uint256 id;      // 圖書ID
                    string title;    // 圖書標題
                    string author;   // 作者
                    uint256 copies;  // 剩餘副本數量
                }

                // 用struct存儲所有圖書
                Book[] public books;

                // 加入新書
                function addBook(uint256 _id, string memory _title, string memory _author, uint256 _copies) public {
                    books.push(Book(_id, _title, _author, _copies));
                }

                // 更改圖書副本數量
                function updateCopies(uint index, uint256 _copies) public {
                    require(index < books.length, "索引超出範圍");
                    Book storage book = books[index];  // 使用storage引用修改
                    book.copies = _copies;
                }

                // 獲取圖書訊息
                function getBook(uint index) public view returns (uint256, string memory, string memory, uint256) {
                    require(index < books.length, "索引超出範圍");
                    Book memory book = books[index];
                    return (book.id, book.title, book.author, book.copies);
                }
            }
            ```