Nguyên tắc thiết kế hướng đối tượng
SOLID là tập hợp 5 nguyên tắc thiết kế phần mềm giúp tạo ra mã nguồn dễ bảo trì, dễ nâng cấp, mở rộng, tăng khả năng tái sử dụng và linh hoạt hơn.
 
1.S - Single Responsibility Principle (SRP) - Nguyên tắc đơn trách nhiệm
Mỗi phần của chương trình chỉ nên làm một việc duy nhất. Nếu một phần có nhiều nhiệm vụ, nó sẽ khó bảo trì và dễ gây lỗi khi thay đổi..
Giúp mã nguồn dễ bảo trì, dễ mở rộng và dễ kiểm thử hơn.

```cpp
//Chương trình sai nguyên tắc đơn trách nhiệm SRP
class Sensor
{
    public:
        // Xử lý dữ liệu
        void processData(){}
       
        // Lưu trữ dữ liệu
        void saveData(){}

        // Gửi dữ liệu đi
        void sendData(){}
};
//-----------------------------------------------------//
//Single Responsbility (nguyên tắc đơn trách nhiệm)
// Class xử lý dữ liệu
class Process
{
    public:
        void processData(){}
};

// Class lưu trữ dữ liệu
class Save
{
    public:
        void saveData(){}
};

// Class gửi dữ liệu
class Send
{
    public:
        void sendData(){}
};

```

VD2:
```cpp
//Sai nguyên tắc đơn trách nhiệm SRC
#include<iostream>
using namespace std;
class SpeedSensor{
public:
    double getSpeed(){
        // đọc dữ liệu cảm biến
        return 80.5;
    }

    void controlEngine(double speed){
        if (speed > 100){
            cout << "Giảm tốc độ động cơ" << endl;
        }
    }
};
//-----------------------------------------------------//
//Đúng nguyên tắc đơn trách nhiệm SRP
#include <iostream>
using namespace std;
class SpeedSensor{
    public:
        double getSpeed(){ return 40; }
};   
class EngineController{
    public:
        void adjustSpeed(double speed){
            if (speed <= 40){
                cout << "Tăng tốc độ động cơ" << endl;
            } else if (speed >= 100){
                cout << "Giảm tốc độ động cơ" << endl;
            }
        }
};   
int main(){
    SpeedSensor speedSensor;
    double speed = speedSensor.getSpeed();
    EngineController engine;
    engine.adjustSpeed(speed);
    return 0;
}  


``` 
2.Nguyên tắc đóng mở Open/Closed Principle (OCP)
- Cho phép mở rộng nhưng không sửa đổi mã cũ. Khi cần thêm tính năng mới, hãy thêm mã mới thay vì chỉnh sửa mã hiện có.
- Tránh sửa đổi mã nguồn cũ, giúp phần mềm ổn định hơn.

VD1:
```cpp
/*
Ví dụ: có một hệ thống cảnh báo tốc độ, ban đầu chỉ hỗ trợ cảnh báo bằng âm thanh. 
Nếu muốn thêm cảnh báo bằng đèn LED,ta phải sửa code trong sendAlert(), vi phạm OCP.

*/
#include<iostream>
using namespace std;
class SpeedAlert
{
    public:
        void sendAlert(double speed)
        {
            if (speed > 100)
            {
                cout << "Cảnh báo: Vượt quá tốc độ!" << endl;
            }
        }
};
   
//---------------------------------------------------------//
/*
Sửa lại code để thỏa mãn nguyên tăc OCP
biến Devide_Alert thành class cha, các cảnh báo LED, Sound thành class con
*/
#include<iostream>
using namespace std;
class Device_Alert
{
    public:
        virtual void sendAlert(double speed) = 0;
};

class Sound_Alert : public Device_Alert
{
    public:
        void sendAlert(double speed) override{
            if (speed > 100){
                cout << "Cảnh báo: Vượt quá tốc độ! (Âm thanh)" << endl;
            }
        }
};

class LED_Alert : public Device_Alert
{
    public:
        void sendAlert(double speed) override{
            if (speed > 100){
                cout << "Cảnh báo: Vượt quá tốc độ! (Đèn LED)" << endl;
            }
        }
};

void handleSpeedAlert(Device_Alert *alert, double speed)
{ 
alert->sendAlert(speed); 
}
   
int main(){
    Sound_Alert sound;
    LED_Alert led;
    handleSpeedAlert(&sound, 80);
    handleSpeedAlert(&led, 120);
    return 0;
}

VD2:
/*
Ví dụ: có một hệ thống thanh toán, có nhiều hình thức thanh toán credit, paypay, crypto.... 
Nếu muốn thêm phương thức payment khác,ta phải sửa code trong payment(), vi phạm OCP.

*/
#include<iostream>
using namespace std;
class Payment
{
    public:
        void processPayment(string type)
        {
            if (type == "CreditCard")  
            {
                /* Xử lý thẻ tín dụng */
            }
            else if (type == "PayPal")
            {
                /* Xử lý PayPal */
            }
        }
};
//-------------------------------------------------------//
/*
Sửa lại code để thỏa mãn nguyên tăc OCP
biến paymentMethod thành class cha, các phương thức thanh toán thành class con
*/
class PaymentMethod
{
    public:
        virtual void pay() = 0;
};
   
class CreditCard : public PaymentMethod
{
    public:
        void pay() override { /* Xử lý thẻ tín dụng */ }
};
   
class PayPal : public PaymentMethod
{
    public:
        void pay() override { /* Xử lý PayPal */ }
};

``` 
3. Nguyên tắc Thay Thế Liskov
L - Liskov Substitution Principle (LSP)
Một lớp con có thể thay thế lớp cha mà không làm thay đổi tính đúng đắn của chương trình.
Đảm bảo tính kế thừa không phá vỡ hành vi của hệ thống.
Ví dụ: class Vehicle có các hành vi như bên dưới, trong đó có hàm fly(). Tuy nhiên trong class con thì car và bycicle thì không thể Fly(). Phải sửa làm sao đó để class con có thể thay thế class cha.
 
VD1:
```cpp
/*
Ví dụ: Class cha là hình chữ nhật có width và height độc lập
Nếu để hình vuông là class con của hình chữ nhật thì vô tình ta set width=height.
=> vô tình làm mất tính độc lập của width và height =>vi phạm LSP.

*/
#include<iostream>
using namespace std;
class Rectangle{
    private:
        int width, height;

    public:
        virtual void setWidth(int w){
            width  = w;
        }

        virtual void setHeight(int h){
            height = h;
        }
};

class Square : public Rectangle{
    public:
        void setWidth(int w)  override{
            width = height = w;
        }

        void setHeight(int h) override{
            width = height = h;
        }
};

//---------------------------------------------------------//
/*
Sửa lại code để thỏa mãn nguyên tăc LSP
Tương tự nguyên tắc đóng mở tách ra làm các class khác nhau,tạo 1 class shape tổng quát lên, 
các class khác là class con của class tổng quát
*/
#include<iostream>
using namespace std;
class Shape{
    public:
        virtual int getArea() = 0; // tạo hàm thuần ảo, để class con sẽ phải định nghĩa lại cách tính của riêng mình
};
   
class Rectangle : public Shape {
    private:
        int width, height;

    public:
        void setDimensions(int w, int h){
            width = w; height = h;
        }

        int getArea() override{
            return width * height;
        }
};

class Square : public Shape {
    private:
        int side;

    public:
        void setSide(int s){
            side = s;
        }
       
        int getArea() override{
            return side * side;
        }
};
   

```
VD2:
```cpp
/*
Ví dụ: Class Car có hàm thành viên đổ xăng refuel, nhung class kế thừa xe điện ElectricCar lại không refuel được
=> vi phạm LSP.

*/
#include<iostream>
using namespace std;
class Car{
    public:
        virtual void refuel(){
            cout << "Đổ xăng" << endl;
        }
};
   
class ElectricCar : public Car{
    public:
        void refuel() override{
            // Xe điện không thể đổ xăng
            cout << "Xe điện không dùng xăng!" << endl;
        }
};
     
//---------------------------------------------------------//
/*
Sửa lại code để thỏa mãn nguyên tăc LSP
Tạo 1 class cha tổng quát với hàm thuần ảo rechargeOrRefuel()=0 để các class con phải tự định nghĩa lại hành vi của mình
tách ra thành 2 class con GasCar và ElectricCar
*/
#include <iostream>
using namespace std;

class Vehicle
{
    public:
        virtual void rechargeOrRefuel() = 0;
};
   
class GasCar : public Vehicle
{
    public:
        void rechargeOrRefuel() override
        {
            cout << "Đổ xăng" << endl;
        }
};

class ElectricCar : public Vehicle
{
    public:
        void rechargeOrRefuel() override
        {
            cout << "Sạc pin" << endl;
        }
};
   
int main()
{
    Vehicle* v1 = new GasCar();
    v1->rechargeOrRefuel();
   
    Vehicle* v2 = new ElectricCar();
    v2->rechargeOrRefuel();
    return 0;
}

``` 
4.Nguyên tắc phân tách giao diện interface
I - Interface Segregation Principle (ISP) 
Một class không nên bị ép buộc triển khai những phương thức mà nó không sử dụng.
Tránh việc các class con phải cài đặt các phương thức không liên quan.
Ví dụ driver() là phương thức của Car, Fly() là method của Plane. => ép class con thực hiện phương thức của class cha. Mà nó không liên quan => vi phạm ISP.
 

