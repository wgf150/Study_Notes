16.0.2 宏定义应当定义在所有范围（namespace）之外

9.3.3 函数成员如果可以，需要定义成const/static
注：常量函数 
不可访问成员对象的非常量成员
可以访问成员对象指针 指向对象的非常量成员
```C++
class Member {
public:
    void nonConstFunction() {
        // 非常量成员函数，可能修改成员变量
    }

    void constFunction() const {
        // 常量成员函数，不能修改成员变量
    }
};

class MyClass {
private:
    Member* memberPtr; // 成员变量是指针
    Member member; 

public:
    MyClass(Member* m) : memberPtr(m) {}

    void constFunction() const {
        // 在常量成员函数中，不能修改指针本身
        // 但是可以通过指针修改指向的对象
        memberPtr->constFunction();   // 可以调用成员对象的常量成员函数
        memberPtr->nonConstFunction(); // 可以调用成员对象的非常量成员函数
        member.nonConstFunction() // 错误：不能调用成员对象的非常量成员函数
        // memberPtr = new Member();  // 错误：不能修改指针本身
    }
};

```
