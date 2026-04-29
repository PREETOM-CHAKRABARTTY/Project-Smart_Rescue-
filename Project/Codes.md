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

// Friend input for family
void inputFamily(MatchSystem &f)
{
    cout << "\n[Family]\n";
    cout << "Missing Person's Name : "  ;  cin >> f.name ;
    cout << "Family Contact        : "  ;  cin >> f.contact ;

    f.age = getAge();
    f.height = getHeight();
    f.skinColor = chooseSkin();
    f.specialMark = chooseMark();

    //saving information to file 
    ofstream file("missing.csv", ios::app);

    file << f.name << ","
         << f.contact << ","
         << f.age << ","
         << f.height << ","
         << f.skinColor << ","
         << f.specialMark << "\n";

    file.close();
}

// Friend input for rescuer team
void inputRescuer(MatchSystem &r)
{
    cout << "\n[Rescuer]\n";
    cout << "Location : " ;  

    cin >> r.location ;
    r.age = getAge();
    r.height = getHeight();
    r.skinColor = chooseSkin();
    r.specialMark = chooseMark();
}

void MatchFromFile(MatchSystem &Foundobj)
{
    ifstream file("missing.csv"); //opening the file to read
    
    if(!file)
    {
        cout << "File not found!" << endl;
        return;
    }

    string line;
    float bestScore = 0;
    MatchSystem bestMatch;

    while(getline(file, line))
    {
        stringstream ss(line);

        MatchSystem temp;
        string temstr;

        //now extracting data from each line of csv
        getline(ss, temp.name, ',');
        getline(ss, temp.contact, ',');
        getline(ss, temstr, ',');
        temp.age = stoi(temstr);
        getline(ss, temstr, ',');
        temp.height = stof(temstr);
        getline(ss, temp.skinColor, ',');
        getline(ss, temp.specialMark, ',');

        //now, we will calculate the matching score
        float score = temp.weightedScore(Foundobj);

        //Tracking the best match
        if(score > bestScore)
        {
            bestScore = score;
            bestMatch = temp;
        }

    }

    file.close();

    cout << "\n===Final result : ===" << endl;

    if(bestScore >= 50)
    {
        cout << "Match Found-" << bestScore << "%" << endl;
        bestMatch.FamilyPov::display();
        Foundobj.RescuerPov :: display();
    } 
    else
    {
        cout << "No suitable match is found." << endl;
    }
}

```
