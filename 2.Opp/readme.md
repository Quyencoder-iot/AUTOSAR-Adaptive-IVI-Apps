1.Tính đóng gói (Encapsulation)
object 1 có thể khởi tạo tên chứa ký tự đặc biệt (như : Huyen !Trang@13,djfd87&^%)  
và ID có thể giống object 2. Vậy làm sao để giải quyết vấn đề này?
Tính đóng gói (Encapsulation) là ẩn đi các biến property  khỏi người dùng, không thể truy cập trực tiếp tới các property (data member), Bằng cách khai báo dữ liệu thành viên trong quyền truy cập private

2.Tính trừu tượng (Abstraction)
Tính trừu tượng : ẩn đi tính trừu tượng là ẩn đi các hàm, thuật toán, logic, bằng cách khai báo các hàm thành viên (method) trong quyền truy cập private (như tính đóng gói ở trên là ẩn đi biến, thì trừu tượng là ẩn đi hàm tính toán logic)
ứng dụng: ví dụ để đăng nhập 1 trang web yêu cầu mật khẩu, xác thực khuân mặt. thì các hàm kiểm tra (check) mật khẩu và khuân mặt để trong phần private để ẩn đi.
```cpp
#include <iostream>
#include <string>
#include <cmath>

using namespace std;

class GiaiPhuongTrinh{
    private:    // đặt property: a,b,c,x1,x2,delta trong private => tính đóng gói
        double a;
        double b;
        double c;
        double x1;
        double x2;
        double delta;
        void tinhNghiem(){  //đặt hàm trong private => tính trừu tượng
            delta = b*b - 4*a*c;
            if (delta < 0){
                delta = -1;
            }
            else if (delta == 0){
                x1 = x2 = -b/ (2*a);
            }
            else if (delta > 0){
                x1 = (-b + sqrt(delta))/(2*a);
                x2 = (-b - sqrt(delta))/(2*a);
            }
        }
       
    public:
        void enterNumber(double num_a, double num_b, double num_c);
        void printResult();

};

void GiaiPhuongTrinh::enterNumber(double num_a, double num_b, double num_c){
    a = num_a;
    b = num_b;
    c = num_c;
}

void GiaiPhuongTrinh::printResult(){
    tinhNghiem();
    if (delta == -1){
        cout << "PT vo nghiem" << endl;
    }
    else if (delta == 0){
        cout << "PT co nghiem chung: " << x1 << endl;
    }
    else if (delta > 0){
        cout << "PT co 2 nghiem: \n";
        cout << "x1: " << x1 << endl;
        cout << "x2: " << x2 << endl;
    }
}
int main()
{
  GiaiPhuongTrinh phuongtrinh1;
  phuongtrinh1.enterNumber(1,5,4);
  phuongtrinh1.printResult();
  return 0;


```
3.Tính kế thừa (Inheritance)
khi các class có property chung, method chung thì ta đấy chúng vào 1 class cha. các class con sẽ khai báo kế thừa class cha đó
khai báo : class_con : public class_cha (kế thừa cũng có 3 kiểu phạm vị kế thừa public, protected, private)

khai báo inheritance: Nó sẽ copy toàn bộ code của class cha sang class con. Và chỉ thay đổi phạm vi truy cập (public, protected, private) nếu có.
K
3.1 Tinh kế thừa public
Các member protected, private của class cha vẫn sẽ là protected, private khi class con kế thừa.
3.2 Tính kế thừa Protected

Các member public, protected của class cha sẽ là protected trong class con.
Các member private của class cha sẽ vẫn là private trong class con.
public → protected
protected → protected

3.3 Tính kế thừa Private

Các member public, protected của class cha sẽ trở thành private trong class con.
thực tế các biến hàm thường khai báo trong private để bảo vệ dữ liệu, truy cập đọc ghi thông qua các hàm setter, getter. Nhờ vậy có thể chèn thêm hàm kiểm tra điều kiện vào được.
public → private
protected → private

```cpp
/**
 * @brief:
 * @param:
 * @return:
 * @note:
 * inheritance: tính kế thừa
 * khi các class có property chung, method chung thì ta đấy chúng vào 1 class cha. các class con sẽ khai báo kế thừa class cha đó
 * cú pháp: class_con : public class_cha (kế thừa cũng có 3 kiểu phạm vị kế thừa public, protected, private)
 * 
 */

#include <iostream>
#include <string>
#include <cmath>
using namespace std;
// class cha chứa biến hàm chung của các class con.
class Users{
    protected:
        string ten;
        int id;

    public:
    // contractor : gán giá trị ban đầu cho ojbect, không có kiểu trả về, auto gọi ra khi khai báo 1 đối tượng,
        Users(){  
            static int ID = 1; // khai báo static để tất cả Objects.ID dùng chung 1 địa chỉ,
            id = ID;
            ID++;
        }

        void setName(string _ten){
            // check chuỗi nhập vào
            ten = _ten;
        }

        void display(){
            cout << "ten: " << ten << endl;
            cout << "id: " << id << endl;
        }
};
// kế thừa với phạm vi truy cập private
class SinhVien : private Users{
    private:

        string chuyenNganh; // property riêng của class Sinhvien

    public:
        void setChuyenNganh(string _nganh){ // method riêng của class
            chuyenNganh = _nganh;
        }
        // Hàm display() kế thừa từ class cha, có thể mở rộng và thêm tham số nếu cần.
        void display(int x){ // override
            Users::display();
            cout << "chuyen nganh: " << chuyenNganh << endl;
            cout << "x=" << x <<endl;
        }
        // Do kế thừa private nên phải sử dụng 1 hàm trung gian trong public để gọi hàm setName ra.
        // chú ý tên hàm getprivateName có thể đặt trùng tên setName() như trong class cha cũng không sao.
        void getprivateName(string _name)
        {
            setName(_name);
            //Users::setName(_name);
        }
};
// Kế thừa với phạm vi public
class HocSinh : public Users{
    protected:
        string lop;
   
    public:
        void setLop(string _lop){
            lop = _lop;
        }

        void display(){ // override
           Users::display();
            cout << "lop: " << lop << endl;
        }
};
// Kế thừa với phạm vi public
class GiaoVien : public Users{
    protected:
        string chuyenMon; //proprety riêng

    public:
    // method riêng của class giaovien
        void setChuyenMon(string _cmon){ 
            chuyenMon = _cmon;
        }

        void display(){ // override
            Users::display();
            cout << "chuyen mon: " << chuyenMon << endl;
        }
};

int main(int argc, char const *argv[])
{
    // SinhVien sv1;

    // sv1.ten = "Hoang";
    // sv1.id = 1;
    // sv1.chuyenNganh = "DTVT";

    // cout << "ID: " << sv1.id << endl;
    // cout << "Ten: " << sv1.ten << endl;
    // cout << "Chuyen nganh: " << sv1.chuyenNganh << endl;

    SinhVien sv1;
    sv1.setName("Trung"); //Báo lỗi .do kế thừa Sinhvien : Private Users(); nên hàm setName(_name) trong class cha từ public chuyển thành private. Nó không thể truy cập trực tiếp, bắt buộc tạo một 1 hàm trung gian là getprivateNam(_name) để truy cập hàm setName.
    sv1.getprivateName("Trung");
    sv1.setChuyenNganh("TDH");
    sv1.display(999);

    cout << endl;

    HocSinh hs1;
    hs1.setName("Tuan");
    hs1.setLop("12A1");
    hs1.display();

    cout << endl;

    GiaoVien gv1;

```
3.4 Đa kế thừa

Đa kế thừa trong C++ cho phép một class kế thừa từ nhiều class khác.
Đa kế thừa thường dùng để kết hợp các chức năng từ nhiều class.
Class A : class B, class C, class D;
Khi nhiều lớp cha có các phương thức hoặc thuộc tính trùng tên, việc gọi chúng từ lớp con có thể gây ra sự nhầm lẫn.
Khi một lớp con kế thừa từ hai lớp cha, mà hai lớp cha này đều cùng kế thừa từ cùng một lớp khác. Tình huống này tạo ra cấu trúc hình thoi (diamond), do đó được gọi là vấn đề "Diamond".

```cpp
/**
 * @brief:
 * @param:
 * @return:
 * @note:
 * 
 * Đa kế thừa: sẽ gặp vấn đề Dimond
 * B,C kế thừa từ A => trình biên dịch copy toàn bộ code trong A sang B, C.
 * D kế thừa từ B, C => sẽ phần code của class A sẽ được copy trùng lặp 2 lần sang D.
 * => khi gọi các method của A từ class D có thể gây ra trùng lặp (đây là vấn đề dimond)
 * 
 */
#include <iostream>

using namespace std;

class A{
    public:
        A(){ cout << "Constructor A\n"; }

        void hienThiA(){ cout << "Day la lop A\n"; }
};

class B : public A{
    public:
        B(){ cout << "Constructor B\n"; }

        void hienThiB(){ cout << "Day la lop B\n"; }
};

class C : public A {
    public:
        C(){ cout << "Constructor C\n"; }

        void hienThiC(){ cout << "Day la lop C\n"; }
};

class D : public B, public C{
    public:
        D(){ cout << "Constructor D\n"; }

        void hienThiD(){ cout << "Day la lop D\n"; }
};

int main() {
    D d;

    // d.hienThiA(); // wrong

    // Gọi phương thức từ lớp A qua B và C
    d.B::hienThiA(); // Gọi hàm hienThiA từ lớp A thông qua B
    d.C::hienThiA(); // Gọi hàm hienThiA từ lớp A thông qua C

    // d.hienThiB();
    // d.hienThiC();
    // d.hienThiD();

    return 0;
}

```
4.Tính đa hình (Polymorphism)
 
Tính đa hình ( Polymorphism) có nghĩa là "nhiều dạng" , một thành viên thực hiện nhiều nhiệm vụ, chức năng khác nhau ở cả class con và class cha. Thì gọi là đa hinh.
Tính đa hình có thể được chia thành hai loại chính:
Đa hình tại thời điểm biên dịch (Compile-time Polymorphism).
Đa hình tại thời điểm chạy (Run-time Polymorphism).
Cụ thể:
Trong hàm main khai báo nhiều đối tượng. Việc sử dụng cùng 1 con trỏ quản lý nhiều đối tượng khác nhau =>gọi là tính đa hình ở thời điểm run- time.
cùng con trỏ tên this… nhưng cho nó thực hiện thêm nhiều chức năng, nhiệm vụ khác => gọi là đa hình tại thời điểm complier-time.
4.1. Đa hình tại thời điểm chạy hàm main
Trong hàm main khai báo nhiều đối tượng. Việc sử dụng cùng 1 con trỏ quản lý nhiều đối tượng khác nhau =>gọi là tính đa hình ở thời điểm running time.
Cùng tên hàm (function overloading), toán tử cùng tên (operator overloading), cùng con trỏ tên this… nhưng thực hiên nhiều chức năng khác nhau => gọi là đa hình compliter.

4.1.1 Upcasting & Downcasting

-	Upcasting/Downcasting là việc ép kiểu cho con trỏ/tham chiếu từ lớp con lên lớp cha hoặc ngược lại. Con trỏ và tham chiếu trong C++ tương tự trong C (C không có tham chiếu), có thể trỏ đến nhiều địa chỉ của đối tượng. ví dụ thời điểm này nó trỏ đến đối tượng hs1, thời điểm khác trỏ đến sv1. Trước khi ép kiểu, con trỏ/tham chiếu phải được gán đến 1 đối tượng thực tế hợp lệ. nếu không việc ép kiểu sẽ dẫn đến kết quả sai lệch.
-	Việc ép kiểu chỉ có giá trị tức thời tại dòng code đó, ngay sau đó nó sẽ trở về kiểu cũ theo khai báo.
Upcasting:
-	 là việc (ép kiểu) một con trỏ/tham chiếu của (class con) lên (class cha). Upcasting an toàn 100%, thường diễn ra tự động, Do con trỏ/tham chiếu thuộc class con đã chứa toàn bộ dữ liệu của lớp cha, 
-	Con trỏ/tham chiếu của class con phải được gán cho 1 đối tượng hợp lệ trước khi ép kiểu lên class cha. 
Downcasting:
-	 là việc ép kiểu con trỏ/tham chiếu của class cha về kiểu class con. Đây là thao tác không an toàn, có thể sẽ gây lỗi bị treo hoặc lỗi undefined behavior, vì con trỏ class cha không chứa dữ liệu và hàm thành riêng riêng của class con.
-	Con trỏ/tham chiếu của class cha phải được gán cho 1 đối tượng hợp lệ của class con, trước khi ép kiểu xuống class con đó.
```cpp
#include <iostream>
using namespace std;
class Base {
public:
    int baseData = 100; // thành viên dữ liệu của Base
    void func() { std::cout << "Base::func\n"; }
};

class Derived1 : public Base {
public:
    int derivedData = 200; // Thành viên dữ liệu của Derived1
    void func() { std::cout << "Derived1::func\n"; }
};

class Derived2 : public Base {
public:
    int derivedData = 300; // Thành viên dữ liệu của Derived2
    void func() { std::cout << "Derived2::func\n"; }
};

int main() {
    // Tạo một đối tượng, và con tro của các class
    
    Base base1; 
    Derived1 dr1;
    Derived2 dr2;
    Base* basePtr = nullptr; // con trỏ của class Base
    Derived1 *DerPtr1; // Con trỏ của class Derived1

    
    DerPtr1 = &dr1; //Đảm bảo DerPtr1 trỏ tới một đối tượng hợp lệ. ở đây là dr1, không phải bas1 hoặc dr2 (lý do ở trên)
    cout << "Trying to access Base data: " << ((Base*)DerPtr1)->baseData << std::endl;

    DerPtr1->func();
    ((Base*)DerPtr1)->func();
    ((Derived2*)DerPtr1)->func();

    //DOWCASTING: ép kiểu từ class cha xuống class con from Base -> Derived2, Base ->Derived1;
    //Downcasting là ép kiểu không an toàn
    
    basePtr = &dr1;
    cout << "Trying to access Derived1 data: " << ((Derived1*)basePtr)->derivedData << std::endl; 
    basePtr = &dr2;
     cout << "Trying to access Derived2 data: " << ((Derived2*)basePtr)->derivedData << std::endl; 
    
    return 0;
}


```
4.1.2 Hàm ảo (Virtual Function)
Tại sao cần hàm ảo? 
Nếu không có virtual, C++ sẽ dùng early binding (liên kết tĩnh): gọi hàm dựa trên kiểu con trỏ/tham chiếu chứ không dựa trên đối tượng thật. Nếu không dùng virtual, chương trình có thể chạy sai logic mà không báo lỗi biên dịch.

Vấn đề đa hình:  class kế thừa mỗi class kế thừa sẽ có hành vi riêng. => Viết code làm sao để một lần cho lớp cha, các lớp con tự xử lý hành vi riêng. 
Hàm ảo (later biding) được sinh ra để giải quyết vấn đề đa hình (hàm trùng tên như: function overloading, operator overloading). Để làm sao khi sử dung con trỏ lớp cha, trỏ đến lớp nào thì gọi đúng phiên bản hàm lớp đó theo logic thông thường.
 Đặc điểm của override:
-	Override không cho thêm tham số, và nó giữ nguyên tính chất ban đầu, kiểu trả về, vẫn có thể bổ xung mở rộng thêm hành vi cho hàm.
•	Thực chất override không thay đổi cách hoạt động của vtable.
•	Nó chỉ là check của compiler: xác nhận rằng hàm trong derived class thực sự override một virtual function ở base. Nếu không, compile error.
•	Nghĩa là địa chỉ hàm trong vtable không “thay đổi ngay khi thêm override”, mà compiler sẽ ghi đè địa chỉ vào entry trong vtable cho class đó.
```cpp
#include <iostream>
using namespace std;

// Lớp cơ sở
class Shape {
public:
    virtual void draw() {  // Hàm ảo
        cout << "Drawing a shape" << endl;
    }
    virtual ~Shape() {}  // Destructor ảo để tránh lỗi khi xóa qua con trỏ cha
};

// Lớp dẫn xuất 1
class Circle : public Shape {
public:
    void draw() override {  // Ghi đè (override từ C++11)
        cout << "Drawing a circle" << endl;
    }
};

// Lớp dẫn xuất 2
class Rectangle : public Shape {
public:
    void draw() override {
        cout << "Drawing a rectangle" << endl;
    }
};

int main() {
    Shape* shapes[2];  // Mảng con trỏ lớp cha

    shapes[0] = new Circle();
    shapes[1] = new Rectangle();

    for (int i = 0; i < 2; ++i) {
        shapes[i]->draw();  // Gọi runtime: in ra hành vi của lớp con
    }

    // Giải phóng bộ nhớ
    delete shapes[0];
    delete shapes[1];

    return 0;
}

```
4.1.4 vTable
•  Định nghĩa: Vtable là một bảng tĩnh (mảng các con trỏ hàm) được compiler tạo ra cho mỗi lớp có virtual function. Mỗi phần tử trong vtable là địa chỉ của một hàm ảo cụ thể. 
•	Vtable có kích thước bằng số lượng hàm ảo trong lớp (gồm cả hàm thuần ảo nếu có).
•	Thứ tự các phần tử trong vtable khớp với thứ tự khai báo virtual function trong lớp.
•	Đối với lớp con (derived class), vtable sẽ kế thừa và ghi đè (override) các hàm ảo từ lớp cha.

4.1.5 vPointer
•  Định nghĩa: Vptr là một con trỏ ẩn (thường là một trường dữ liệu ngầm định) được compiler thêm vào bên trong mỗi đối tượng của lớp có ít nhất một hàm ảo (virtual function). Nó trỏ đến vtable của lớp đó. 
4.1.4 vTable
•  Định nghĩa: Vtable là một bảng tĩnh (mảng các con trỏ hàm) được compiler tạo ra cho mỗi lớp có virtual function. Mỗi phần tử trong vtable là địa chỉ của một hàm ảo cụ thể. 
4.1.5 vPointer
•  Định nghĩa: Vptr là một con trỏ ẩn (thường là một trường dữ liệu ngầm định) được compiler thêm vào bên trong mỗi đối tượng của lớp có ít nhất một hàm ảo (virtual function). Nó trỏ đến vtable của lớp đó. 

```cpp
#include <iostream>
#include <string>
using namespace std;

// 1 method cùng tên "tong" có thể có nhiều input parameter, return type khác nhau gọi là nạp chồng hàm
class TinhToan{
    private:
        int a;
        int b;
    public:
        int tong(int a, int b){
            return a+b;
        }
        double tong(int a, int b, int c, double d){
            return (double)a+b+c+d;
        }
        double tong(int a, double b){
            return (double)a+b;
        }
};

int main(int argc, char const *argv[])
{
    TinhToan th, th1, th2;
    cout << th.tong(2, 5) << endl;
    cout << th1.tong(2, 5, 7, 6.7) << endl;
    cout << th2.tong(2, 3.5) << endl;
    return 0;
}

```
4.2.2 Operator Overloading
Nạp chồng toán tử (Operator Overloading) là việc định nghĩa mở rộng thêm chức năng, cách hoạt động cho các toán tử (+, -, =, ==, <<, >>,...) cho các kiểu dữ liệu do người dùng định nghĩa ra (class/struct).
Cú pháp: operator” từ khóa giúp C++ hiểu sẽ định nghĩa nạp chồng toán tử, symbol: toán tử sẽ định nghĩa lại, parameter  1 tham số phía sau toán tử.
<return_type> operator symbol (parameter)

```cpp
#include <iostream>
using namespace std;
class Complex
{
    private:
        double realPart;    // phần thực
        double imagPart;    // phần ảo
   
    public:
        //Constructor khởi tạo giá trị =0 cho data member
        Complex(double real = 0, double imag = 0): realPart(real), imagPart(imag){}

        Complex operator + (const Complex other) const
        {
            Complex result;
            result.realPart = Complex::realPart + other.realPart;
            result.imagPart = Complex::imagPart + other.imagPart;
            return result;
        }
        // nạp chồng toán tử so sánh bằng (==) hai số phức bằng nhau
        bool operator == (const Complex other) const
        {
            return (Complex::realPart == other.realPart && Complex::imagPart == other.imagPart);
        }

        // hàm hiển thị
        void display() const
        {
            cout << realPart << " + " << imagPart << "i" << endl;
        }
};
int main()
{
    Complex c1(3,4);
    Complex c2(5,6);
    Complex c3 = c1 + c2;
    c1.display();
    c2.display();
    c3.display();

    if (c1 == c2){
        cout << "Hai số phức bằng nhau" << endl;
    } else {
        cout << "Hai số phức không bằng nhau" << endl;
    }
    return 0;

```
4.2.3 This Pointer
this là một con trỏ ẩn (ẩn danh) có sẵn trong mọi hàm thành viên (method) của class. 
this pointer trỏ đến đối tượng hiện tại mà hàm đang được gọi ra. => con trỏ chứa địa chỉ của đối tượng hiện tại.
class tạo ra các đối tượng obj1(this=&obj1), obj2(this =&obj2), obj3(this=&obj3)… thì mỗi đối tượng sẽ có 1 con trỏ this của riêng nó.
Vd2: trong đoạn code sau
Phanso operator + (Phanso &other)
{
Phanso ketqua;
ketqua.num = this->num * other.den + this->den * other.num;
ketqua.den = this->den * other.den;
return ketqua;
}

```cpp
#include <iostream>
using namespace std;

class Phanso
{
private:
int num; //data member
int den; //data member
public:
//Constructor Phanso
Phanso(int num = 0,int den =1){
this->num = num;
this->den = den;
}
//Nạp chồng toán tử +
Phanso operator + (Phanso &other)
{
Phanso ketqua;
ketqua.num = this->num * other.den + this->den * other.num; //this trỏ đến đối tượng hiện tại đứng trước toán tử +
ketqua.den = this->den * other.den;
return ketqua;
}
Phanso operator * (Phanso const &other)
{
Phanso ketqua;
ketqua.num = this->num * other.num;
ketqua.den = this->den * other.den;
return ketqua;
}
bool operator == (Phanso const &other){
return (this->num == other.num && this->den == other.den);
}
void display()
{
cout << "Tu so: " << num <<" Mau so: "<<den<<endl;
}
};
int main(){
    Phanso p2(2,3); 
    Phanso p4(3,5); 
    Phanso p6(6,7);
    Phanso p1 = p2*p4; // this->p2
    Phanso p3 = p2 + p4 + p6; // 
    Phanso p5 = p2*p6 + p4; // B1: this -> p2 trong p2*p6. B2: this-> biến tạm thời = (p2*p6)
    p1.display();
    p3.display();
    p5.display();
}

```
4.2.4 Tham trị (Pass By Value)
Tham trị là cách truyền một bản sao của biến vào hàm. Mọi thay đổi trong hàm không ảnh hưởng đến biến gốc 
```cpp
#include <iostream>
using namespace std;

void increment(int x) {
    x += 10;
    cout << "Inside function: x = " << x << endl;
}

int main() {
    int a = 5;
    increment(a); // inside function x = a = 15.
    cout << "Outside function: a = " << a << endl; // a không đổi, outside function a = 5.
// x là bản sao của a, thay đổi x không ảnh hưởng đến a.

    return 0;
}
}

```
4.2.5 Tham chiếu (Pass by Reference)
Tham chiếu là cách tạo ra một tên khác để truy cập cùng một vùng nhớ của một biến/đối tượng đã tồn tại.
type& referenceName = variable;
○	type: kiểu dữ liệu
○	&: toán tử address of, ký hiệu cho tham chiếu (khác với con trỏ *)
○	referenceName: tên tham chiếu
○	variable: biến đã khai báo

```cpp
#include <iostream>
using namespace std;

void increment(int &x) {
    x += 10;
    cout << "Inside function: x = " << x << endl;
}

int main() {
    int a = 5;
    increment(a);
    cout << "Outside function: a = " << a << endl; // a bị thay đổi
    return 0;
}
// ket qua: inside function x = 15, outside function a =15.

```