Bài 1_CLASS
Trong C++, từ khóa "class" được sử dụng để định nghĩa một cấu trúc dữ liệu có thể chứa dữ liệu và các hàm thành viên liên quan. Class là nền tảng của lập trình hướng đối tượng (OOP) trong C++.
 
Nếu so với ngôn ngữ C:
Biến được khai báo trong class gọi là Property (Thuộc tính), hoặc gọi là data member (thành viên dữ liệu). khi được sử dụng ngoài class được gọi là Object (Đối tượng) (Object có kiểu Class)
Hàm khai báo trong Class thì gọi là phương thức(method) hoặc hàm thành viên (member Function)

Access Specifier: Khác với Struct trong C, thì trong class phải khai báo thêm Acess Speifier (Phạm vi truy cập)
•  public: Thành viên có thể truy cập từ bên ngoài class. 
•  private: Chỉ truy cập được bên trong class, để ẩn dữ liệu
•  protected: Dùng trong kế thừa, truy cập được trong class và các class con. Thành viên bên ngoài 2 class thì không truy cập được

```cpp
/**
 * @brief:
 * @param:
 * @return:
 * @note:
 * 
 * Constructor:
 * mục đích: khi bạn bật nguồn cho 1 thiết bị, máy tính, động cơ... cần các thông số ban đầu, để thiết bị làm việc với cấu hình tối thiểu. Hàm constructor sinh ra để giá trị ban đầu cho các biến trong class
 * 1.tên hàm trùng tên class
 * 2.hàm không có kiểu trả về
 * 3.Mục đích Để khởi tạo các giá trị ban đầu cho object
 * 4.tự động gọi khi 1 đối tượng được khởi tạo
 * 5.Dạng hàm
 *      5.1 không có tham số truyền vào
 *      5.2 Có tham số, nhưng không gán giá trị mặc đinh -> bắt buộc khi tạo object phải truyền vào giá trị cho từng tham số
 *      5.3 Có tham số, được gán luôn giá trị mặc định -> khai báo object không bắt buộc truyền vào giá trị tham số.
 * nhược điểm của constructor không có tham số là khi đối tượng tao ra nó sẽ sử dụng chung 1 tham số nào đó
 * Destructor:
 * mục đích: khi tắt nguồn, đóng chương trình của thiết bị, máy tính, động cơ... cần reset giá trị các biến về 0 hoặc giá trị mặc định nào đó
 * 1. Là 1 hàm không có kiểu trả về, không có tham số
 * 2. Tên hàm trùng tên class và thêm ký tự ~
 * 3. Xóa dữ liệu object sau khi kết thúc sử dụng
 * 4. Tự động gọi ra trước khi một đối tượng bị thu hồi
 * Static property:
 * 1. Tất cả các object sẽ dùng chung địa chỉ của property này
 * 2. Địa chỉ biến static bắt buộc phải khởi tạo trước khi khơi tạo object, bằng cách gán giá trị cho biến static đó
 * Static Method:
 * 1. Có thể gọi ra mà không cần thông qua đối tượng. bình thường để gọi hàm hay biến cần khái báo 1 đối tượng class sau đó gọi hàm biến đó thông qua đối tượng
 * 2. Gọi ra bằng cách sử dụng tên class thông qua toán tử ::
 * 3. Chỉ tương tác được với các biến static
 * 
 */

#include <iostream>
#include <string>
using namespace std;
class SinhVien
{
    public:                //  sv1                 sv2
        string name;       //0x01-0x0f             0xa1-0xaf
        int ID;            //0x10-0x14             0xb0-0xb4               
        int age;           //0x15-0x19             0xb5-0xb9
        double gpa;        //0x1a-0x1f             0xba-0xbf
        // method hiển thị
        void display(); // method
        // method creat
        void creat();

        //Tạo hàm Constructor,hàm này để gán giá trị ban đầu cho biến hàm trong class
        //// constructor có thể khai báo theo 3 cách: 1. tên hàm không có tham số, 2. tên hàm có tham số truyền vào, 3. tên hàm có tham số và được gán luôn giá trị ban đầu
        
        //SinhVien(); //1.khai báo constructor không truyền vào tham số
        SinhVien(string newName, int newID, int newage, double newgpa); // 2.khai báo construtor có tham số truyền vào, nhưng không truyền vào giá trị, ưu điểm: khi tạo đối tượng có thể truyền vào giá trị cho từng object riêng biệt.
        //SinhVien(string newName ="", int newID =1, int newage=1, double newgpa=1); //3. constructor có tham số, có gán giá trị ban đầu

        //Tạo hàm Destructor()
        ~SinhVien();
        // static property
        static int count;    //0x20-0x23,static sẽ cấp pháp riêng vùng nhớ tĩnh, các đối tương sv1, sv2 khi truy cập đến biến cout, đều phải sử dụng chung vùng nhớ này  
        // static method
        static int getcount()
        {
            // ID=100; // trong 1 hàm static method không thể sử dụng 1 biến thông thường, bắt buộc phải sử dụng 1 biến static, không sẽ báo lỗi
            return count;
        }
        ;

};
void SinhVien::creat(){
    // SinhVien sv;
    // sv.ID = 9;
    // sv.name = "thuong";
    // sv.age = 18;
    // sv.gpa = 99;
    SinhVien sv("Hung",11,15,50);
}
void SinhVien::display(){
    cout<< "id:" << ID << endl;
    cout<<"tên:" << name <<endl;
    cout<<"tuổi:" << age <<endl;
    cout<<"gpa:" << gpa <<endl<< endl;

}
// khi mot doi tuong duoc tao ra, constructor sẽ tự động được gọi ra để gán giá trị ban đầu cho object đó
// method định nghĩa cho constructor khai báo ở trên

//SinhVien::SinhVien()      // không có tham số truyền vào
SinhVien::SinhVien(string newName, int newID, int newage, double newgpa) // truyền vào tham số nhưng không gán giá trị, => có thể gán giá trị ban đầu cho từng object riêng biệt, ưu điểm cho phép mở rộng chương trình
{
    cout << "this is constructor: "<< endl;
    // name = "david";
    // ID = 18;
    // age = 18;
    // gpa = 99;
    
    name = newName;
    ID = newID;
    age = newage;
    gpa = newgpa;

    display();
    count++; // đặt count trong constructor do khi tạo 1 đối tượng mới thì constructor được gọi ra ngay lập tức.

};
// truoc khi 1 doi tuong bi huy thi destructor se duoc goi ra de xoa dữ liệu liên quan đến object đó.
SinhVien::~SinhVien()
{
    cout <<"this is Destructor: "<< endl;
    name = "";
    ID = 0;
    age = 0;
    gpa = 0;
    display();
    count--;
};
//count là biến static. Nên Địa chỉ biến static bắt buộc phải khởi tạo trước khi khơi tạo object, bằng cách gán giá trị cho biến static đó
int SinhVien::count = 0; // 0x20- 0x23

int main(int argc, char const *argv[])
{
    //SinhVien sv1,sv2;
    SinhVien sv1("kien",16,25,99), sv2("Vinh",17,18,100);
   
    //sv1.display();
  
    // cout << "địa chỉ biến sv1: " << &(sv1.count)<< endl; 
    // cout << "địa chỉ biến sv2: " << &(sv2.count)<< endl; 

    // cout << "Số Sinh vien: " << sv1.count<< endl;    // gọi biến count thông qua 1 đối tượng sv1.
    // cout << "Số Sinh vien: " << sv2.getcount<< endl; // gọi biến count thông qua 1 đối tượng sv2 và hàm getcount
    // cout << "Số Sinh vien: " << SinhVien::count<< endl; // gọi trực tiếp biến count mà không cần thông qua đối tượng sv1, sv2
    //  cout << "Số Sinh vien: " << SinhVien::getcount<< endl;

    return 0;
}

```
