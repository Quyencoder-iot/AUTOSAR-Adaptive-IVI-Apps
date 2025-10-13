7-12_Standard Template Library (STL)
7.1 Giới Thiệu
Standard Template Library ( STL) là một tập hợp các thư viện thiết kế để hỗ trợ lập trình tổng quát (generic programming). 
STL C++ cung cấp một tập hợp các template classes và functions để thực hiện nhiều loại cấu trúc dữ liệu và các thuật toán phổ biến. 
STL đã trở thành một phần quan trọng của ngôn ngữ C++ và làm cho việc lập trình trở nên mạnh mẽ, linh hoạt và hiệu quả.
Một số thành phần chính của STL:
-	Containers (Cấu trúc dữ liệu)
-	Iterators (Bộ lặp)
-	Algorithms (Thuật toán)
-	Functors & Lambda
Container
Một container là một cấu trúc dữ liệu chứa nhiều phần tử theo một cách cụ thể. STL (Standard Template Library) cung cấp một số container tiêu biểu giúp lưu trữ và quản lý dữ liệu. 
Một vài containers phổ biến:
-	Vector
-	List
-	Map
-	Array
-	stack, queue
7.2. Vector
Method	Chức năng
push_back()	thêm phần tử vào vị trí cuối của vector.
pop_back()	xóa phần tử ở vị trí cuối của vector.
insert()	thêm phần tử vào vị trí bất kỳ.
erase()	xóa phần tử ở vị trí bất kỳ hoặc xóa các phần tử trong phạm vi được chỉ định.
clear()	xóa toàn bộ phần tử của vector.
```cpp
#include<iostream>
#include<vector>
using namespace std;
//CÚ PHÁP KHAI BÁO VECTOR
vector<data_type> name;  

vector<data_type> name(size);  

vector<data_type> name(size, value);  

vector<data_type> name = {1, 2, 3, 4, 5}; 

//DUYỆT VECTOR
    for (int i = 0; i < v.size(); ++i)
        cout << v[i] << " ";
 
    for (int x : v)   // for cải tiến - C++11
        cout << x << " ";
   
    for (auto it = v.begin(); it != v.end(); ++it) 
        cout << *it << " ";


```
VD1:
```cpp
#include<iostream>
#include<vector>
using namespace std;

int main(){


vector<int> vec = {1, 2, 3, 4, 5}; // khởi tạo vector 5 phần tử, 20 byte

vec.at(0) = 33;
vec.at(3) = 66;
vec.resize (9); // resize kích thước lên 9 phần tử 
//DUYỆT VECTOR

    for (int i = 0; i < vec.size(); ++i)
        cout << vec[i] << " ";
        cout << endl;
 
    for (int x : vec)   // for cải tiến - C++11
        cout << x << " ";
        cout << endl;
    //vector<int>::iterator it;
    for (auto it = vec.begin(); it != vec.end(); ++it) 
        cout << *it << " ";
        cout << endl;

}
```
VD3:
```cpp

#include<iostream>
#include<vector>
using namespace std;

int main(){

    vector<int> vec;  // vector rỗng
    vec.push_back(3);
    vec.push_back(6);
    vec.push_back(9);
    vec.insert(vec.begin(),10); //push_front
    vec.insert(vec.end(),66); // push_end
    vec.insert(vec.begin() + 1, 99); //insert vào vị trí số 1 sau begin().
    vec.insert(vec.begin() + 3, 666);
    

    for (auto it = vec.begin(); it != vec.end(); ++it) 
        cout << *it << " ";
        cout << endl;

    vec.pop_back(); // xóa vị trí cuối
    vec.erase(vec.begin()+3); // xóa vị trí bất kỳ
    vec.erase(vec.begin(), vec.begin()+2); // xóa các phần tử nàm trong dải địa chỉ từ begin() đến begin() + 2.

    for(auto x : vec)
    cout << x <<" ";

}
```
Result:
 

VD4:
```cpp
#include<iostream>
#include<vector>
using namespace std;
int main() {
    vector<vector<double>> arr2 { 
        {3,3,3},
        {6,6,6},
        {3.3, 6.6, 9.9, 3.33}
    };
    for(const auto item : arr2) { /
        for(const auto item1 : item) 
        cout<< item1 << " ";
        cout << endl;

    };
    return 0;
}
```
VD5:
```cpp
#include<iostream>
#include<vector>
#include<string>
using namespace std;
typedef struct{
 string name;
 double score;   
}Student;

int main() {
    vector<Student> Students;
    int n;
    cout <<"nhap so luong hoc sinh: ";
    cin >> n; // mỗi lần dùng cin>> nó sẽ xuống dòng.
    cin.ignore(); 

    for(int i=0; i<n; i++){
        Student sv;
        cout << "nhap ten sv thu "<< i+1<< ": ";
        getline(cin,sv.name); //Đọc nguyên cả dòng ký tự (bao gồm cả khoảng trắng) từ luồng nhập (cin) cho đến khi gặp ký tự xuống dòng (\n)

        cout << "nhap diem sv thu "<< i+1<< ": ";
        cin >> sv.score;
        cin.ignore();
        Students.push_back(sv);

    }
    cout<<"Danh sach sinh vien";
    for(const auto &s : Students)
    cout<< s.name << ": "<< s.score <<endl;

    return 0;
}
``` 
7.3 List
List là một container trong STL  <list>của C++, triển khai dưới dạng danh sách liên kết đôi (double-linked list). nghĩa là mỗi phần tử có con trỏ (pre, next) trỏ đến phần tử trước và sau nó
Single Linked List:
-	Duyệt một chiều, node đầu đến node cuối
Double-Linked List
-	Duyệt xuôi : từ node đầu  node cuối : con trỏ next
-	Duyệt ngược: node cuối  node đầu: con trỏ pre
-	Node đầu tiên : con trỏ pre =NULL, Node cuối cùng con trỏ Next = NULL
-	Next là trỏ đến địa chỉ node kế tiếp, pre trỏ đến địa chỉ node trước đó.
Một số đặc điểm quan trọng của list:
-	Truy cập tuần tự: Truy cập các phần tử của list chỉ có thể thực hiện tuần tự, không hỗ trợ truy cập ngẫu nhiên.
-	Hiệu suất chèn và xóa: Chèn và xóa ở bất kỳ vị trí nào trong danh sách(khi đã có iterator) là O(1). Có hiệu suất tốt hơn so với vector. Điều này đặc biệt đúng khi thêm/xóa ở giữa danh sách.
-	Iterator chỉ là bidirectional (không có random access, không có operator[]).
-	Đổi chỗ phần tử bằng liên kết → các thuật toán như splice, merge, remove, unique, sort hoạt động hiệu quả mà không cần copy/move phần tử (đa số là thao tác liên kết). tránh di chuyển phần tử
-	Tính ổn định iterator: chỉ iterator đến phần tử bị xóa là invalid; các iterator khác vẫn hợp lệ sau chèn/xóa ở vị trí khác.

 ```cpp
#include <list>
#include <iostream>
using namespace std;

int main() {
    list<int> L = {10, 20, 30, 40};
    int index=0;
    cout << "Địa chỉ từng node:\n";
    // iterator duyệt xuôi từ node begin() đến khi gặp node end() thì dừng
    //++it : đây là con trỏ đặc biệt sẽ là overloading qua các địa chỉ Next, chứ không phải + 4byte (int) như dùng chỉ số.
    for (auto it = L.begin(); it != L.end(); ++it, ++index)
        cout <<"node "<< index << ": "<< *it << " -> " << &(*it) << '\n';
        cout<<endl;
    /**
     * iterator duyệt ngược, sẽ xảy ra trường hợp gặp value rác ở vị trí end(), và mất dữ liệu vị trí begin().
     * Lý do: begin() --> địa chỉ node đầu tiên, 
     * end(): địa chỉ node đứng sau node cuối cùng.
     * như hàm in bên dưới gặp kết quả = {4, 40, 30, 20} : bị 1 giá trị rác =4, mất giá trị 10 ở node đầu tiên.
     */
    int index2 =0;
    for (auto it = L.end(); it != L.begin(); --it, ++index2)
        cout <<"node "<< index2 << ": "<< *it << " -> " << &(*it) << '\n';
        cout<<endl;

     /**
     * iterator duyệt ngược dùng reverse_iterator,
     * rbegin() = node cuối cùng.
     * rend() = node đầu tiên
     * rit++ = --it : dùng cho duyệt ngược.
     */
    list<int>::reverse_iterator rit; //
     int index3 =0;
    for (auto rit = L.rbegin(); rit != L.rend(); ++rit, ++index3)
        cout <<"node "<< index3 << ": "<< *rit << " -> " << &(*rit) << '\n';
}


```
Result
 

VD1:
```cpp
#include <iostream>
#include <list>
using namespace std;

int main(int argc, char const *argv[])
{
    list<int> lst; // khai báo list, phần tử kiểu int.

    lst.push_back(1); // thêm node vào phía sau list
    lst.push_back(3);
    lst.push_back(2);
    lst.push_back(7);
    lst.push_back(5);
    lst.push_front(99); // thêm node vào đầu list
    lst.push_front(66);
    //lst.resize(7);

    cout << "Số lượng node trong list: " << lst.size() << endl;

    list<int>::iterator it; // it: con trỏ của class con iterator dùng duyệt node
    // In ra list ban đầu
    int index = 0;
    //it duyệt xuôi node từ begin(), đến khi gặp end() thì dừng.
    for (it = lst.begin(); it != lst.end(); it++){
        cout << "node " << index++ << ": value: " << *it << endl;
    }
    cout << "------------------\n";

      auto pos = next(lst.begin(), 1); // pos là con trỏ trỏ vào vị trí node 2 từ node begin()
    lst.insert(pos,222);
    // Cach 2.Thêm node vào giữa list bằng dùng for
    int i = 0;
    for (it = lst.begin(); it != lst.end(); ++it){ //dùng con tro 'it' duyệt từng node trong list
       
        // thêm node vào vị trị thứ 2 trong list
        if (i == 3){
            lst.insert(it, 333);
        }
        
        if (i == 5){
            lst.insert(it, 555);
        }

        // xóa node ở vị trị thứ 4 trong list
        if (i == 4){
            lst.erase(it);
        }
    ++i;
    }
    // In ra list sau khi thêm node
     cout << "Số lượng node sau khi them/xoa: " << lst.size() << endl;
    i = 0;
    //Dùng iterator Duyệt từ node begin() đến khi gặp node end().
    for (it = lst.begin(); it != lst.end(); it++){
        cout << "node: " << i++ << ", value: " << *it << endl;
    }
    cout << "----------------\n";

    //Xóa node theo giá trị 
    lst.remove(7);
    // Tìm vi trí node thứ 5 và xóa node đó
    auto pos2 = next(lst.begin(), 6);
    lst.erase(pos);
   
    // Tim và xóa dải node 2-4
    auto start = next(lst.begin(),1); // start là iterator của node thứ 2.
    auto end = next(lst.begin(),3);
    lst.erase(start,end);
    // In ra list sau khi txóa node
    i = 0;
    for (it = lst.begin(); it != lst.end(); it++){
        cout << "node: " << i++ << ", value: " << *it << endl;
    }

    return 0;
}

```
 
 
7.3 Map
Map là một container trong STL của C++, cung cấp một cấu trúc dữ liệu ánh xạ key-value (tương tự JSON). Dữ liệu trong map được lưu trữ theo thứ tự tăng dần hoặc giảm dần của key, và nó được triển khai dưới dạng cây tìm kiếm nhị phân cân bằng (thường là Red-Black Tree)
Mỗi phần tử trong std::map là một std::pair<const Key, T>:
 
VD1:
```cpp
/
#include <iostream>
#include <string>
#include <map>
using namespace std;
int main() {

map<int, string> m{
    {10, "Hanh"}, //1.thêm trực tiếp cặp key-value khi khai báo map
    {10, "Kien"}
};
//2.dùng insert để thêm key-value
m.insert({3, "Nhan"});
m.insert({1, "Quyen"});
m.insert({2, "Quang"});
m.insert({6, "Ana"});

//3.dùng toán tư [] để thêm cặp key-value
m[15] = "aaaa";
m[16] = "bbbb";

//Để truy cập in dữ liệu cũng có 3 cách
//1. dung tham chiếu
 for (const auto &item : m)
 cout<< "Key: "<< item.first <<", Value:  "<<item.second <<endl;
//2. vẫn dùng for cải tiến, nhưng dùng cặp [k,v] là phần tử kế thừa từ map => k = key, v=value
 for (const auto& [k,v] : m)
 cout<< "Key: "<< k <<", Value:  "<<v <<endl;
 //3.
 map<int,string>::iterator it;
 for(it = m.begin(); it != m.end(); ++it){
    cout <<"key " <<(*it).first << ", value: " <<(*it).second <<endl;
 }

 return 0;

}

``` 
VD2: 
```cpp
#include <iostream>
#include <map>
#include <string>
#include <vector>
using namespace std;

class SinhVien
{
    private:
    string mssv;
    string name;
    int age;
    public:
    SinhVien(const string &mssv,const string &name, int age):mssv(mssv), name(name), age(age){}
    //do khai bao private cho data member nen cần tạo các hàm setter, getter truy xuất tt sinh viên
    string getMssv() const {return mssv;}
    string getName() const {return name;}
    int getAge() const {return age;}
    void display()const{
        cout<< mssv <<"-"<< name << "-" << age << "\n";
    }
};

int main(int argc, char const *argv[])
{
   vector<SinhVien> danhsach{
    SinhVien("sv001", "Hoang", 23),
    SinhVien("sv006", "Tien", 23),
    SinhVien("sv003", "Tung", 23),
    SinhVien("sv002", "Khoi", 22),
    SinhVien("sv008", "Lam", 23),
    SinhVien("sv009", "Long", 23),
    };

    //Sắp xếp sinh viên theo tên do map tự động sắp xếp theo key. nên chỉ cần truyền vào key kiểu dữ liệu của name là nó tự động sắp sếp theo tên 
    map<string,SinhVien> sortbyname;
    for(const auto sv: danhsach) //duyệt từng phần tử của vector để thêm vào map
    {
        //sortbyname[sv.getName()] = sv; // để key=name, map sẽ tự động sắp xếp theo tên.
        sortbyname.insert({sv.getName(), sv}); // cặp {key,value} : value = vector sv.
    }
    cout << "danh sach sinh vien sap sep theo ten" << endl;
    for (const auto& [k, sv] : sortbyname){
        sv.display();
    }
    return 0;
}

```
VD3:
```cpp
#include <iostream>
#include <map>
#include <string>
using namespace std;

typedef struct
{
    string name;
    int age;
    string lop;
} SinhVien;

int main(int argc, char const *argv[])
{
    map<string, SinhVien> Database =
    {
        {
            "SV100", {
                "Hoang",
                20,
                "DDT"
            }
        },
        {
            "SV101", {
                "Tuan",
                21,
                "CDT"
            }
        },
        {
            "SV102", {
                "Anh",
                22,
                "KTMT"
            }
        }
    };

    for (auto item : Database)
    {
        cout << "ID: " << item.first << " - Ten: " << item.second.name << " - Tuoi: " << item.second.age << " - Lop: " << item.second.lop << endl;
    }
    return 0;
}

```
7.4.Lamda
Lambda trong C++ là “hàm vô danh” có thể tạo ngay tại chỗ, truyền như đối tượng, bắt (capture) biến bên ngoài theo giá trị hoặc tham chiếu, và dùng làm callback/predicate cho STL
Đặc điểm:
-	Là hàm không có tên
-	Hàm bình thường khai phạm vi toàn cục (tức là không khai báo trong main()): mục đích sử dụng nhiều lần (ít nhất 2 lần)=> hàm tốn bộ nhớ lưu trên RAM.
-	Hàm lamda dùng cho mục đích sử dụng 1 lần=> không tốn bộ nhớ RAM. Sẽ biến mất sau khi thực hiện xong công việc. Tuy nhiên nó vẫn có thể sử dụng được nhiều lần bằng cách lưu lamda vào một biến hàm (nếu cần). Và sử dụng trong phạm vi cho phép.
-	Được định nghĩa tại chỗ để thực hiện tính năng nào đó. Có thể khai báo cục bộ (khai báo trong hàm khác như hàm main(){…lamda},… sort(…,lamda-condition). Nó cũng có thể khai báo toàn cục như hàm bình thường (khai báo ngoài hàm main()).
Cách khai báo lamda
[captures] (params list) specifiers -> return_type { body };
Hoặc
[captures] (params list) { body };


VD1:
```cpp

#include <iostream>
#include <vector>
#include<algorithm>
using namespace std;
//bool comp(int i, int j) {return i>j; // true nếu i>j, ngược lại false} //chỉ sử dụng 1 lần cho hàm sort, khai báo toàn cục sẽ tốn tài nguyên RAM=> thay nó bằng lamda trong main
int main(){
int a = 5;
int b = 6;
double c = 6.9;
//Cách 1: gọi lamda dùng toán tử () đặt sau lamda
    [&a, b, &c]() -> void {
    cout << "hello world"<<endl;
    a =9;   // Read-Write
    cout << "a = "<< a << endl;
    //b = 16; // Read-only (báo lỗi ghi giá trị vào biến read only (variable read-only))
    cout << "b = "<< b << endl;
    c = 99; // Read-Write
    cout << "c = "<< c << endl;
}();
// Cach 2: Lưu lambda vào biến hàm rồi gọi như bình thường.
auto func = []() -> void {

    cout << "hello world"<<endl;
};
func();

auto func2 = [](int x, double y) -> void {

    cout << "hello world"<<endl;
    cout << "x = " << x << " ,y = " << y <<endl;
};
func2(2,3.66);

vector<int> vec {1, 8, 10, 35, 4, 6, 123, 3};
// dùng lamda làm hàm COMPARATOR trong hàm sort để tiết kiệm bộ nhớ RAM : [](int i, int j){ return i < j;}
//sort là hàm trong thư viện chuẩn std::algorithm
sort(vec.begin(),vec.end(),[](int i, int j){ return i < j;});
for (auto& v:vec){
    cout << v << " ";
}

return 0;

}
```
VD2:
```cpp

/**
 * @brief: chương trình tìm số chẵn lẻ và lưu vào 2 vector chẵn lẻ riêng
 */
#include <iostream>
#include <vector>
#include<algorithm>
using namespace std;

/**
 * @brief: chương trình tìm số chẵn lẻ và lưu vào 2 vector chẵn lẻ riêng
 */
#include <iostream>
#include <vector>
#include<algorithm>
using namespace std;

int main(){

vector<int> vec {1, 8, 10, 35, 4, 6, 123, 3};
//in ra vec
for (auto& items:vec){
    cout << items << " ";
}
cout<< endl;

int count_even = 0;
int count_odd = 0;
vector<int> evens, odds; //vector chứa biến chẵn lẻ

auto n= count_if(vec.begin(), vec.end(), [&](int x){
if (x % 2 == 0){
count_even++;
evens.push_back(x);
}
else{
count_odd++; 
odds.push_back(x);
}
return false; // do mượn cout_if thực hiện ý tưởng riêng, nên lamda phải trả về true hoặc false để không bị lỗi biên dịch count_if
});

//in ra số phần tử chẵn lẻ trong vector
cout << "Number of evens: " << count_even << endl;
cout << "Number of odds: " << count_odd << endl;
cout << "n_count=  " << n << endl;
// in ra 2 vector chẵn lẻ
for (const auto& item: evens)
cout << item << " ";
cout << endl;

for (const auto& item : odds)
cout << item << " ";
cout << endl;
return 0;

}
```
Result:
 

VD3:
```cpp
#include <bits/stdc++.h>
using namespace std;

struct SinhVien {
    string mssv, name; int age;
    void print() const { cout << mssv << " - " << name << " - " << age << '\n'; }
};

int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    // 1) sort với lambda
    vector<SinhVien> ds {
        {"sv003", "Tung", 23},
        {"sv001", "Hoang", 23},
        {"sv002", "Khoi", 22},
        {"sv006", "Tien", 23},
        {"sv009", "Long", 23},
    };

    // sort theo name tăng dần
    sort(ds.begin(), ds.end(),
         [](const SinhVien& a, const SinhVien& b){ return a.name < b.name; });

    cout << "Sau sort theo name:\n";
    for (const auto& sv : ds) sv.print();

    // 2) filter: xóa SV tuổi < 23
    ds.erase(remove_if(ds.begin(), ds.end(),
             [](const SinhVien& sv){ return sv.age < 23; }),
             ds.end());

    cout << "\nSau filter age >= 23:\n";
    for (const auto& sv : ds) sv.print();

    // 3) bắt theo THAM CHIẾU để đếm số SV tên bắt đầu bằng 'T'
    int countT = 0;
    for_each(ds.begin(), ds.end(), [&countT](const SinhVien& sv){
        if (!sv.name.empty() && sv.name[0] == 'T') ++countT;
    });
    cout << "\nSo SV bat dau bang 'T': " << countT << "\n";

    // 4) init-capture để move string lớn vào lambda
    string big = string(1'000, 'x');
    auto use_big = [buf = std::move(big)]() {
        // buf có dữ liệu, big đã rỗng ngoài này
        cout << "buf size in lambda: " << buf.size() << "\n";
    };
    use_big();
    cout << "big size outside: " << big.size() << "\n";

    // 5) generic lambda
    auto add = [](auto a, auto b){ return a + b; };
    cout << "add(2, 3) = " << add(2,3) << "\n";
    cout << "add(string, string) = " << add(string("Hello "), string("Lambda")) << "\n";

    // 6) lambda không capture -> con trỏ hàm
    using FP = void(*)(int);
    FP printInt = [](int x){ cout << "printInt: " << x << "\n"; };
    printInt(42);

    // 7) đệ quy dạng “self”
    auto fib = [&](auto self, int n)->long long {
        return (n<=1) ? n : self(self, n-1) + self(self, n-2);
    };
    cout << "fib(10) = " << fib(fib, 10) << "\n";

    // 8) stateful lambda với mutable (đếm số lần gọi)
    auto counter = [c = 0]() mutable {
        return ++c; // sửa bản sao c bên trong closure object
    };
    cout << "counter(): " << counter() << ", " << counter() << ", " << counter() << "\n";

    return 0;
}
``` 
7.5. Functor
Functor là gì ??
Trong C++, Functor (function object) là một đối tượng (Object) hoạt động như một hàm. Tức là class/struct dùng operator để định nghĩa lại, thêm chức năng cho toán tử ().

VD1:
```cpp
/**
 * Đặt vấn đề:
 * Bài toán : tăng đều tất cả giá trị của vector lên. nếu dùng lamda thì phải tạo nhiều hàm đơn. 
 */
#include <iostream>
#include <vector>
#include <algorithm> 
using namespace std;

auto pred1 = [](const int &x) {return x+1;};
auto pred2 = [](const int &x) {return x+2;};

int main(){

vector<int> v ={1, 2, 3, 4, 5};

//algrorthim::transform duyệt từng phần tử của vector từ begin()->end(). 
// sau đó transform thực hiện các hàm lamda pred1, và ghi đè vào bộ nhớ từ vị trí begin().
transform(v.begin(), v.end(), v.begin(), pred1);
transform(v.begin(), v.end(), v.begin(), pred2);

for (auto& item:v){
    cout << item << " ";
}
return 0;
}
```
VD2:
```cpp
#include <iostream> 
using namespace std;

template <typename T1, typename T2> 
class Test{
public:
//Nạp chồng hàm cho toán ()., tạo 3 case: tham số truyền vào : empty, 1 Ts, 2 ts.
void operator ()(){ cout << "functor 1\n";}
void operator ()(T1 x){ cout << "x = " << x << endl;}
void operator ()(T1 x, T2 y){cout << "x = " << x <<" - y= " << y << endl;}
};
int main() {

Test<int, double> test; // tao obj test

test();
test(99); // obj hoạt động như hàm = test.operator()
test(66,10.66);

return 0;
}
```
VD3:

