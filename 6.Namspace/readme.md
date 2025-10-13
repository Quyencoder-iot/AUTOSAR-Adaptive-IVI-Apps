6.NAMESPACE
6.1 Định nghĩa 
Trong C không cho phép khai báo tên biến, hàm giống nhau => nó gây lỗi trình biên dịch. Tuy nhiên trong C++ thì cho phép dung tên biến, hàm, class giống nhau, nó được sử dụng trong Function Overloading, Template đệ quy, Namespace.
Tình huống thường xảy ra trong lập trình C/C++. Ví dụ: Code bạn đang viết có hàm tên là Test() và có thư viện khác có sẵn mà cũng có hàm Test(). Bây giờ, trình biên dịch không biết phiên bản nào của hàm mà bạn muốn sử dụng trong code của mình.
Làm sao để giải quyết vấn đề trên?
Namespace là cách nhóm các đối tượng như biến, hàm, class, struct,... vào một không gian tách biệt.
Namespace được sử dụng với mục đích là để tránh xung đột tên khi có các Tên giống nhau được khai báo trong các phần của chương trình hoặc các thư viện khác nhau.
VD1:
```cpp
#include <iostream>
using namespace std;
namespace A{
    char *name = (char*)"Anh 20";

    void display(){
        cout << "Name: " << name << endl;
    }
}
namespace B{
    char *name = (char*)"Anh 21";

    void display(){
        cout << "Name: " << name << endl;
    }
}

int main(){
    cout << "Name: " << A::name << endl; // A::name --> gọi biến name trong namspace A.
    cout << "Name: " << B::name << endl;
    A::display(); // gọi hàm display() trong namespace A.
    B::display();
    return 0;
}


```
VD2:
Lcd1.hpp
```cpp
#ifndef _LCD1_HPP
#define _LCD1_HPP

namespace LCD{
    int temp;
    class lcd{
        public:
            lcd(/* args */);
            ~lcd();
    };

    lcd::lcd(){}
    lcd::~lcd(){}
}

#endif

```
Lcd2.hpp
```cpp
#ifndef _LCD2_HPP
#define _LCD2_HPP

namespace LCD{
    char *text;
}

#endif

```
Lcd.cpp
```cpp
#include <iostream>
#include "lcd1.hpp"
#include "lcd2.hpp"

using namespace std;

namespace A{
    char *name = (char*)"Trung 20";
}
// trong namespace A chứa namespace C.
namespace A{
    namespace C{
        char *str = (char*)"Nguyen Hoang";
    }
}

int main(int argc, char const *argv[])
{
    cout << "Name: " << A::name << endl;
    cout << "Name: " << A::C::str << endl;

    LCD::lcd;
    LCD::temp;
    LCD::text;

    return 0;
}


```
6.2.Name Mangling (cơ chế mã hóa tên trong C++, không có trong C)
Biến đổi tên (Name Mangling) là một cơ chế của trình biên dịch g++ nhằm mã hóa tên hàm, biến, class, namespace,... thành tên khác, để tránh xung đột trong quá trình biên dịch (giai đoạn compiler).
 

```cpp
#include <iostream>
using namespace std;
namespace A{
    char *name = (char*)"Anh 20";
}

namespace B{
    char *name = (char*)"Anh 21";
}

using namespace A;
// using namespace B; // error: ambigious

int main()
{
    cout << "Name: " << name << endl;
    cout << "Name: " << B::name << endl;
    return 0;
}
```
Namespace tiêu chuẩn (std)
Một trong những namespace quan trọng và phổ biến nhất trong C++ là std. Tất cả các thành phần của thư viện chuẩn C++ (như cout, cin, vector, string) đều được định nghĩa bên trong namespace std.
using namespace std;
cout
cin
vector<int>
```cpp
#include <iostream>
using namespace std;

namespace std{
    struct{
        int x;
        int y;
    } point;

    void display(){
        cout << "x = " << point.x << endl;
        cout << "y = " << point.y << endl;
    }
}

int main()
{  
    point.x = 10;
    point.y = 20;
    display();
    return 0;
}

```
