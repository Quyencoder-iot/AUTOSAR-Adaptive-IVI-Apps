
Generic Programming (Lập trình tổng quát) là một phương pháp lập trình sử dụng các tham số kiểu dữ liệu tổng quát (type parameter) để viết mã có thể tái sử dụng và hoạt động với nhiều kiểu dữ liệu khác nhau. Kỹ thuật này giúp loại bỏ sự trùng lặp code và tăng tính linh hoạt trong thiết kế phần mềm.
Lập trình tổng quát thường được áp dụng trong các ngôn ngữ hỗ trợ Generics (như Java, Rust) hoặc Templates (C++).
C++ sử dụng Templates để triển khai Generic Programming. Templates có hai loại:
Function Templates (Hàm tổng quát)
Class Templates (Lớp tổng quát)
Function Template

Cú pháp:
```cpp

template <typename T>
T func(T a, T b){}

template <typename T1, typename T2, typename T3>
T1 func(T1 a, T2 b, T3 c){}

//VD1
template <typename T>
T sum(T a, T b)
{
    return a + b;
}

int result1 = sum(5, 10);           // Tự động suy luận T là int
double result2 =sum(3.14, 2.71);    // Tự động suy luận T là double
```
VD1:
```cpp
#include <iostream>
using namespace std;

template <typename T>
T maxValue(T a, T b)
{
    return (a > b) ? a : b; 
    //toán tử ternary (rút gọn của if else): 
    //(Điều kiện)? trả về giá_tri_nếu_TRUE : giá_trị_nếu_FALSE
}

int main()
{
    cout << "Max(5, 10) = " << maxValue(5, 10) << endl;
    cout << "Max(3.5, 2.8) = " << maxValue(3.5, 2.8) << endl;
    cout << "Max('A', 'B') = " << maxValue('A', 'B') << endl;
    return 0;
}

```
VD2:
```cpp
#include <iostream>
using namespace std;

// tham số mặc định
template <typename T = int>
T square(T x)
{
    return x * x;
}

int main(int argc, char const *argv[])
{
    cout << square(4) << endl;              
    cout << square<double>(4.5) << endl;    
    return 0;      
}

``` 
Class Template
Class templates trong C++ là một khái niệm tương tự như function templates, nhưng được áp dụng cho class thay vì function. templates cho phép Class có thể làm việc với nhiều kiểu dữ liệu mà không cần viết lại code.

template <typename T>
class Class_Name
{
    private:
        T var; 
}



VD1:
```cpp
#include <iostream>
using namespace std;

template <typename T> 
class Sensor{
    private:
        T value; 
    public:
    //constructor: 
        Sensor(T init): value(init){}
        void readSensor(T newValue){ value = newValue; } 
        T getValue() const { return value; } 
        void display(){ cout << "Gia tri cam bien: " << value << endl; }
};

int main(int argc, char const *argv[]){
// Tạo đối tượng sử dụng
    Sensor tempSensor(36.5); 
    tempSensor.display();

    Sensor lightSensor(512);
    lightSensor.display();

    Sensor stateSensor("OFF");
    stateSensor.display();
    return 0;
}

```
VD2:
```cpp
/
#include <iostream>
#include <string>
using namespace std;

class Sensor
{
    public:
        virtual double getValue() const = 0;

        virtual string getUnit() const = 0;
};

// Class đại diện cho cảm biến nhiệt độ (Temperature Sensor)
class TemperatureSensor : public Sensor
{
    private:
        double temp;

    public:
        double getValue() const override
        {
            // temp = 30.3;
            return 40.5; // Giá trị cảm biến giả định
        }

        string getUnit() const override
        {
            return "Celsius"; // lấy giá trị đơn vị 
        }
};

// Class đại diện cho cảm biến áp suất lốp (Tire Pressure Sensor)
class TirePressureSensor : public Sensor
{
    public:
        double getValue() const override
        {
            return 32; // Giá trị cảm biến giả định
        }

        string getUnit() const override
        {
            return "PSI";
        }
};

// Template class quản lý hai cảm biến khác nhau
template <typename Sensor1, typename Sensor2>
class VehicleSensors
{
    private:
        Sensor1 sensor1;  
        Sensor2 sensor2;  

    public:
        
        VehicleSensors(Sensor1 s1, Sensor2 s2) : sensor1(s1), sensor2(s2) {}

        // Hàm hiển thị thông tin của cả hai cảm biến
        void displaySensorsInfo() const
        {
            cout << "Sensor 1 Value: " << sensor1.getValue() << " " << sensor1.getUnit() << endl;
            cout << "Sensor 2 Value: " << sensor2.getValue() << " " << sensor2.getUnit() << endl;
        }
};

int main()
{
    // Tạo đối tượng cảm biến nhiệt độ
    TemperatureSensor tempSensor;

    // Tạo đối tượng cảm biến áp suất lốp
    TirePressureSensor pressureSensor;

    // Quản lý cả hai cảm biến bằng class VehicleSensors
    VehicleSensors vehicleSensors(tempSensor, pressureSensor);
    vehicleSensors.displaySensorsInfo();

    return 0;
}

```

 
Template Specialization
Template chuyên biệt hóa (Template Specialization) trong C++ cho phép tùy chỉnh hành vi của template cho một kiểu dữ liệu cụ thể.
Khi nào “specialization” cần thiết
-	Khi có 1 hoặc 1 vài kiểu dữ liệu cần logic và cách xử lý khác biệt
-	So sánh số/ký tự khác biệt hoàn toàn so với so sánh chuỗi ký tự

Cú pháp:

template <> 
class name_of_class<data_type> 
{
    private:
        T var;
}


VD1:
```cpp
/*
*Tạo 3 kiểu hàm display với kiểu dữ liệu chuyên biệt hóa riêng.
*/
#include <iostream>
using namespace std;

// Template tổng quát
template <typename T>
void display(T value){
    cout << "Generic: " << value << endl;
}

// Chuyên biệt hóa cho kiểu `int`
template <>
void display<int>(int value){
    cout << "Specialized for int: " << value << endl;
}

// Chuyên biệt hóa cho kiểu `char*`
template <>
void display<char*>(char* value){
    cout << "Specialized for string: " << value << endl;
}

int main(){
    
    display(42);        
    display(3.14);     
    display("Hello");   // Sử dụng chuyên biệt hóa cho char*
    return 0;
}
```
Kết quả:
 
VD2:
```cpp
/**
*@brief: Prog tạo 3 template : 1 tổng quát, 2 chuyên biệt cho class trùng tên Printer.
*/
#include <iostream>
using namespace std;

// Template tổng quát
template <typename T>
class Printer
{
    public:
        static void print(T value)
        {
            cout << "Generic: " << value << endl;
        }
};

//Printer Chuyên biệt hóa cho kiểu `char*`
template <>
class Printer<char*>
{
    public:
        static void print(char* value)
        {
            cout << "String specialization: " << value << endl;
        }
};

// Printer Chuyên biệt hóa cho kiểu `bool`
template <>
class Printer<bool>
{
    public:
        static void print(bool value)
        {
            cout << "Boolean specialization: " << (value ? "true" : "false") << endl;
        }
};

int main()
{
    Printer<int>::print(100);          // Sử dụng template tổng quát
    Printer<double>::print(3.14);      
    Printer<char*>::print("Hello");    
    Printer<bool>::print(true);        // Sử dụng template chuyên biệt hóa
    return 0;
}

```
 
Variadic Template
Sử dụng dấu ba chấm … : Dùng khi muốn truyền lượng tham số tùy ý, không giới hạn.
Cách sử dụng variadic
#include <iostream>
using namespace std;
template<typename... Args>
void count(Args...args) {
cout<< sizeof ...(args) <<endl đếm // số phần tử
}
typename... Args: Định nghĩa ra nhiều tên kiểu dữ liệu tổng quát Args như: Arg1, Arg2..., 
                             Tương ứng: template<typename Arg1, typename Arg2,…>
Args... args : Khai báo nhiều tham số args cho hàm tương ứng với nhiều kiểu dữ liệu Args

VD1:
```cpp
#include <iostream>

using namespace std;

// Function Template khi chỉ có một tham số
template<typename T>
T sum(T value)
{
    return value;
}

template<typename T, typename... Args> // Args: Tên nhiều kiểu dữ liệu. VD dùng 2 tham số thì có 2 kiểu dữ liệu: Arg1, Arg2
auto sum(T first, Args... args) //các tham số args lần lượt có các kiểu dữ liệu Args.
{        
    return first + sum(args...);  
}

int main(int argc, char const *argv[])
{
    cout << sum(1, 2, 3.6, 5.7, 40) << endl;
    return 0;
}

```
VD2:
```cpp
/**
 * brief
 * 
 */
#include <iostream>
using namespace std;

// Class tổng quát sử dụng Variadic Template
template <typename... Args>
class MyClass;

// Định nghĩa class khi không có đối số => đây gọi là chuyên biệt hóa đầy dủ(Full specialization)-điểm dừng
template <>
class MyClass<>
{
    public:
        void display()
        {
            cout << "No arguments" << endl;
        }
};

// Định nghĩa class khi có ít nhất một đối số
//MyClass<T,Args...>: gọi là chuyển biệt hóa một phần (partial specialization)
template <typename T, typename... Args>
class MyClass<T, Args...> : public MyClass<Args...>
{
    private:
        T first_;

    public:
   
 MyClass(T first, Args... args): first_(first),  MyClass<Args...>(args...){} 

        void display()
        {
            cout << first_ << " ";
            MyClass<Args...>::display(); // Gọi hàm display của lớp cơ sở
        }  

};

int main()
{
    //Tạo đối tượng obj
    MyClass<int, double, char> obj(1, 2.5, 'A');
    obj.display();  // Kết quả: 1 2.5 A

    MyClass obj1;
    obj1.display();
    return 0;
}
```
VD2: 
```cpp

#include <iostream>
using namespace std;

// Class tổng quát (chưa định nghĩa), dùng Variadic Template => nói với g++, sẽ có lớp MyClass có thể nhận nhiều kiểu dữ liệu khác nhau
//Dòng này bắt buộc phải có, nếu bạn định nghĩa các chuyên biệt hóa ở dưới
template <typename... Args>
class MyClass;

template <>
class MyClass<>
{
    public:
        void display()
        {
            cout << "No arguments" << endl;
        }
};

// Định nghĩa class khi có ít nhất một đối số
template <typename T, typename... Args>
class MyClass<T, Args...> : public MyClass<Args...>
{
    private:
        T first_;

    public:
   
 MyClass(T first, Args... args): first_(first),  MyClass<Args...>(args...){} 

        void display()
        {
            cout << first_ << " ";
            MyClass<Args...>::display(); // Gọi hàm display của lớp cơ sở
        }  

};

int main()
{
    //Tạo đối tượng obj
    MyClass<int, double, char> obj(1, 2.5, 'A');
    obj.display();  // Kết quả: 1 2.5 A

    MyClass obj1;
    obj1.display();
    return 0;
}
```
