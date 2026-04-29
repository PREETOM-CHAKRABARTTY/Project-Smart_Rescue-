## *Code :*
```C++
#include<iostream>
#include<string>

using namespace std;


// ========== Base Class ==========
class Person {
protected:
    int age;
    float height;
    string skinColor;
    string specialMark;

public:
    Person() {
        age = 0;
        height = 0;
        skinColor = "Unknown";
        specialMark = "None";
    }

    // Pure virtual function (Abstract)
    virtual void display() = 0;
    virtual ~Person(){};
};


// =Derived Class 1 = which will take input according to family point of view
class FamilyPov : virtual public Person {
protected:
    string name;
    string contact;

public:
    void display() override
    {
        cout << "\n--- Missing Person ---\n";
        cout << "Name: " << name << endl;
        cout << "Contact: " << contact << endl;
        cout << "Age: " << age << endl;
        cout << "Height: " << height << endl;
        cout << "Skin: " << skinColor << endl;
        cout << "Mark: " << specialMark << endl;
    }
};
```
