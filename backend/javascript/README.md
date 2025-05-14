## Table of content

[1. Kiểu dữ liệu trong JavaScript](#1-hooks)   
[2. Hoisting](#2-hoisting) 
[2. Closure trong Javascript](#2-closure-trong-javascript) 
[3. this trong Javascirpt](#2-this-trong-javascript) 
[3. call-apply-bind](#2-call-apply-bind) 



### 1. Kiểu dữ liệu trong JavaScript


### 2. Hoisting

### 3. Closure trong Javascript

Closure là một hàm mà có thể truy cập biến thuộc scope chứa nó (hàm closure), ngay cả khi scope chứa nó đã được thực thi xong. Cụ thể, closure có thể truy cập biến mà được khai báo trong hàm mà tạo ra closure đó, ngay cả khi hàm tạo ra nó đã hoàn tất việc thực thi.

Ví dụ:
```tsx
export default function Counter () {
  const [count, setCount] = useState<number>(0);
  
  const handleCount = () => {
	  setCount(count => count + 1)
  }
  // Ở đây hàm handleCount có thể truy cập vào biến 'count' nằm bên ngoài scope của nó

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
};
```

Closure được tạo ra khi một hàm được khai báo trong một hàm khác. Hàm bên trong có thể truy cập tất cả biến mà được khai báo ở hàm bên ngoài, ngay cả khi hàm bên ngoài đã hoàn thành việc thực thi.

Closure có 3 scope chain, đó là:

- Có thể truy cập đến biến của chính nó (biến được định nghĩa trong dấu ngoặc nhọn của nó).
- Có thể truy cập biến của hàm bên ngoài.
- Có thể truy cập biến toàn cục (global).

**Các quy tắc của closures và Side Effects của nó:**

1. **Closures có thể truy cập biến của hàm bên ngoài ngay cả hàm bên ngoài đã return:**
    
    Một trong những "tính năng" hay ho quan trọng của closures đó là hàm bên trong vẫn có thể truy cập đến các biến số của hàm bên ngoài ngay cả khi hàm bên ngoài đã trả về. Khi các hàm trong Javascript thực thi, chúng sử dụng cùng scope chain. Điều này có nghĩa là sau khi hàm bên ngoài trả về, hàm bên trong vẫn có thể truy cập đến các biến của hàm bên ngoài. Do đó, ta có thể gọi hàm bên trong này trong chương trình sau đó. Ví dụ:
    ```ts
    function celebrityName (firstName) {
        var nameIntro = "This celebrity is ";
        // Đây là hàm bên trong mà có thể truy cập đến biến của hàm bên ngoài, truy cập được tham số của hàm ngoài.
        function lastName (theLastName) {
            return nameIntro + firstName + " " + theLastName;
        }
    return lastName;
    }

    var mjName = celebrityName ("Michael"); celebrityName (bên ngoài) đã trả về.

    // Closure (lastName) được goi ở đây sau khi hàm ngoài đã trả về.
    // Closure vẫn có thể truy cập được biến và tham số của hàm bên ngoài.
    mjName ("Jackson"); // This celebrity is Michael Jackson.
    ```
2. **Closures lưu tham chiếu đến biến của hàm bên ngoài:**
    
    Closure không lưu giá trị. Closures trở nên thú vị khi giá trị của biến của hàm bên ngoài thay đổi trươc khi closures được gọi. Đây là một "tính năng" mạnh mẽ có thể được khai thác theo nhiều cách sáng tạo, ví dụ:   

    ```js
    function celebrityID () {
        var celebrityID = 999;
        // Ta đang trả về một object với các hàm bên trong.
        // Tất cả các hàm bên trong có thể truy cập đến biến của hàm ngoài (celebrityID).
        return {
            getID: function ()  {
                // Hàm này sẽ trả về celebrityID đã được cập nhật.
                // Nó sẽ trả về giá trị hiện tại của celebrityID, sau khi setID thay đổi nó.
                return celebrityID;
        },
            setID: function (theNewID)  {
            // Hàm này sẽ thay đổi biến của hàm ngoài khi gọi.
                celebrityID = theNewID;
            }
        }

    }

    var mjID = celebrityID (); //Lúc này, celebrityID đã trả về
    mjID.getID(); // 999
    mjID.setID(567); // Thay đổi biến của hàm ngoài
    mjID.getID(); // 567: Tả về biến celebrityID đã được cập nhật.
    ```
3. **Closures đôi khi trở nên không như ý:**
    
    Bởi vì closures có thể truy cập đến các giá trị đã được cập nhật của các biến của hàm bên ngoài, chúng có thể gây ra bugs khi biến của hàm bên ngoài thay đổi với vòng lặp for, ví dụ:

    ```js
    // Ví dụ này sẽ được giải thích bên dưới.
    function celebrityIDCreator (theCelebrities) {
        var i;
        var uniqueID = 100;
        for (i = 0; i < theCelebrities.length; i++) {
            theCelebrities[i]["id"] = function ()  {
            return uniqueID + i;
            }
        }
    
    return theCelebrities;
    }

    var actionCelebs = [{name:"Stallone", id:0}, {name:"Cruise", id:0}, {name:"Willis", id:0}];

    var createIdForActionCelebs = celebrityIDCreator (actionCelebs);

    var stalloneID = createIdForActionCelebs [0];
    console.log(stalloneID.id()); // 103
    ```
    Trong ví dụ trên, trước khi hàm anonymous được gọi, giá trị của i là 3. Con số 3 được cộng vào uniqueID để tạo thành 103 cho tất cả celebritiesID. Vì vậy ở mỗi lúc trả về, thì giá trị nhận được là 103 thay vì 100, 101, 102 như mong muốn.

    Như đã giải thích ở ví dụ trước, closure (hàm anonymous trong ví dụ) đã truy cập đến biến của hàm bên ngoài bằng tham chiếu, không phải truy cập giá trị. Vì vậy như ví dụ trước đã chỉ ra, chúng ta có thể truy cập các biến đã được cập nhật với closure, ví dụ này truy cập biến i khi nó đã bị thay đổi, kết quả là hàm bên ngoài chạy toàn bộ vòng lặp và trả về giá trị cuối cùng của i, là 103.

    Để sửa bug này trong closures, ta có thể sử dụng Immediately Invoked Function Expression (IIFE), ví dụ như sau:

    ```js
    function celebrityIDCreator (theCelebrities) {
        var i;
        var uniqueID = 100;
        for (i = 0; i < theCelebrities.length; i++) {
            theCelebrities[i]["id"] = function (j)  {
            return function () {
                return uniqueID + j;
                } ()
            } (i); // Chạy ngay khi hàm được gọ với tham số i
        }

        return theCelebrities;
    }

    var actionCelebs = [{name:"Stallone", id:0}, {name:"Cruise", id:0}, {name:"Willis", id:0}];

    var createIdForActionCelebs = celebrityIDCreator (actionCelebs);

    var stalloneID = createIdForActionCelebs [0];
    console.log(stalloneID.id); // 100

    var cruiseID = createIdForActionCelebs [1];
    console.log(cruiseID.id); // 101
    ```

### 3. This trong Javascript


### 4. Call - Apply - Bind

**Call**
Giả sử bạn muốn gọi 1 hàm và truyền cho nó một đối tượng cụ thể để gán cho `this`. Bạn có thể dùng phương thức call.

```js
var sayName = function() {
    console.log('My name is ' , this.name);  // My name is Stacey
   };

var stacey = {
   name: 'Stacey', 
   age: 34
};

sayName.call(stacey) // Chúng ta dùng đối tượng stacey để gọi hàm sayName

```

**Apply**
Hàm apply hoạt động như hàm call nhưng chúng có thể truyền đối số là collection hoặc array.
```js
var sayName = function(lang1, lang2) {
    console.log('My name is ' + this.name + ', I love '+ lang1 + ' and ' + lang2);  
   };

var stacey = {
   name: 'Stacey', 
   age: 34
};

var languages = ['JavaScript', 'Ios'];

sayName.apply(stacey, languages); // My name is Stacey, I love JavaScript and Ios
```
