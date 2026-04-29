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

// Final class which will calculate and show result
class MatchSystem : public FamilyPov, public RescuerPov {
public:
    friend void inputFamily(MatchSystem &f);
    friend void inputRescuer(MatchSystem &r);
    friend void MatchFromFile(MatchSystem &Foundobj);

    float weightedScore(MatchSystem &r)
   {
    float score = 0;

    // Weight: Specialmarks=40%, Skincolor=30%, Age=20%, Height=10%
    if (specialMark == r.specialMark) score += 40;
    if (skinColor == r.skinColor)       score += 30;
    if (abs(age - r.age) <= 5)          score += 20;
    if (abs(height - r.height) <= 5)    score += 10;

    return score;
   }

    void display() override {}
};

int getAge()
{
    int age;

    while(true)
    {
        try {
            cout << "Enter Age: ";
            cin >> age;

            if(age < 0)
                throw "Age cannot be negative!";

            return age;
        }
        catch(const char* msg)
        {
            cout << msg << endl;
            cout << "Try again...\n";
        }
    }
}

float getHeight()
{
    float height;

    while(true)
    {
        try {
            cout << "Enter Height in Centi Meter: ";
            cin >> height;

            if(height <= 0)
                throw "Height must be greater than 0!";

            return height;
        }
        catch(const char* msg)
        {
            cout << msg << endl;
            cout << "Try again...\n";
        }
    }
}

string chooseSkin() {
    int c;
    while(true) {
        try {
            cout << "Skin: 1.White 2.Brown 3.Dark : ";
            cin >> c;

            if(c < 1 || c > 3)
                throw "Invalid choice!";

            if(c == 1) return "White";
            if(c == 2) return "Brown";
            if(c == 3) return "Dark";
        }
        catch(const char* msg) {
            cout << msg << endl;
        }
    }
}

string chooseMark()
{
    int m;
    while(true) {
        try {
            cout << "Mark: 1.None 2.Scar 3.Tattoo 4.Missing Limb : ";
            cin >> m;

            if(m < 1 || m > 4)
                throw "Invalid choice!";

            if(m == 1) return "None";
            if(m == 2) return "Scar";
            if(m == 3) return "Tattoo";
            if(m == 4) return "Missing Limb";
        }
        catch(const char* msg) {
            cout << msg << endl;
        }
    }
}


```
