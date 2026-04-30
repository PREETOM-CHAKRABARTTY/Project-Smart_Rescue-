## *Code :*
```C++
#include<iostream>
#include<string>
#include<cmath>
#include<fstream>
#include<sstream>

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

// =Derived Class 2 = which will take input according to rescuer point of view
class RescuerPov : virtual public Person {
protected:
    string location;

public:
    void display() override {
        cout << "\n--- Found Person ---\n";
        cout << "Location: " << location << endl;
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



//Feature no 02

bool ExceptionHandling();

//Main base class holding all necessary information
class Medical_Info{
protected:
    bool unconscious;
    bool breathing;
    bool bleeding;
    bool bluelips;
    bool coughing;
    bool coldPale;
    bool pain;
public:
    Medical_Info()
    {
        unconscious = bleeding = bluelips =
        coughing = coldPale = pain = false;

        breathing = true; 
    }

};

//Deriving a class which will show probable risks victim might have
class RiskProvider : virtual public Medical_Info{
public:
    virtual void displayRisks() = 0;
};

//This class is designed to show actions that rescuser should take
class ActionProvider : virtual public Medical_Info{
public: 
    virtual void displayActions() = 0;
};

//That is our brain class that calculates everything
class Victim_condition : public RiskProvider, public ActionProvider{
public:
    friend void TakingInput(Victim_condition &vc);

    void displayRisks() override{
        int num = 1;
        if(unconscious)
        {
            cout << num << ". Head injury." << endl;
            num += 1;
        }
        if(breathing == false)
        {
            cout << num << ". Respiratory failure." << endl;
            num += 1;
        }
        if(bleeding)
        {
            cout << num << ". Severe blood loss." << endl;
            num += 1;
        }
        if(pain)
        {
            cout << num << ". Bone injury." << endl;
            num += 1;
        }
        if(coldPale)
        {
            cout << num << ". Hipothermia." << endl;
            num += 1;
        }
        if(coughing)
        {
            cout << num << ". Smoke inhalation." << endl;
            num += 1;
        }
        if(bluelips)
        {
            cout << num << ". Oxygen deprivation" << endl;
            num += 1;
        }
    }

    void displayActions() override{
        int num = 1;
        if(unconscious)
        {
            cout << num << ". Call medical team immediately." << endl;
            num += 1;
            cout << num << ". Do not give food or water." << endl;
            num += 1;
        }
        if(breathing == false)
        {
            cout << num << ". Begin CPR if trained." << endl;
            num += 1;
            cout << num << ". Loosen tight clothing." << endl;
            num += 1;
        }
        if(bleeding)
        {
            cout << num << ". Apply firm pressure on wound." << endl;
            num += 1;
        }
        if(pain)
        {
            cout << num << ". Don't Move injured limb." << endl;
            num += 1;
        }
        if(coldPale)
        {
            cout << num << ". Keep victim warm using blankets or clothing" << endl;
            num += 1;
        }
        if(coughing)
        {
            cout << num << ". Avoid giving water if coughing heavily" << endl;
            num += 1;
            cout << num << ". Move victim to fresh air." << endl;
            num += 1;
        }
        if(bluelips)
        {
           cout << num << ". Give immediate oxygen support." << endl;
           num += 1;
        }
    }
};


void TakingInput(Victim_condition &vc)
{
    cout << "1. Is victim unconscious?(yes or no)" << endl;
    vc.unconscious = ExceptionHandling();

    cout << "2. Is the victim breathing?(yes or no)" << endl;
    vc.breathing = ExceptionHandling();

    cout << "3. Is there heavy bleeding?(yes or no)" << endl;
    vc.bleeding = ExceptionHandling();

    cout << "4. Has extreme pain while moving?(yes or no)" << endl;
    vc.pain = ExceptionHandling();

    cout << "5. Is victim cold, pale, confused or shaking?(yes or no)" << endl;
    vc.coldPale = ExceptionHandling();

    cout << "6. Badly coughing or has black mark around mouth?(yes or no)" << endl;
    vc.coughing = ExceptionHandling();

    cout << "7. Have blue or purple lips/ figertips?(yes or no)" << endl;
    vc.bluelips = ExceptionHandling();

}

//here we calculate the severity level
class MedicalReport : public Victim_condition{
private:
    int HighRisks;
public:
/*we calculated non critical symptoms
for accurately determine the severity level */

    void calculatingNonCriticalSymptoms()
    {
        HighRisks = bleeding + pain + coldPale + coughing;
    }

    void calculateSeverity ()
    {
        cout << endl;
        if(unconscious || breathing == false || bluelips)
        {
            cout << "===Victim status : Critical!(Has life threat! Need emergency help)" << endl;
        }

        else if(HighRisks >= 2)
        {
           cout << "===Victim status : Severe!(Not free from risk)\n" << endl; 
        }

        else if(HighRisks == 1)
        {
            cout << "===Victim status : Moderate!(Free from risk.)\n" << endl;
        }

        else
        {
            cout <<"===Victim status : Safe\n" << endl;
        }

    }
};


//Exceptions are handled here
bool ExceptionHandling()
{
    string input;

    while(1)
    {
        try {
            cin >> input ;

            if(input != "yes" && input != "no")
            {
                throw "Invalid input. Please enter yes or no";
            }
            
            if(input == "yes")
            {
                return 1;
            }
            if(input == "no")
            {
                return 0;
            }
        }

        catch(const char* msg)
        {
            cout << msg << endl;
            cout << "Try again : " << endl;
        }
    }

}


// Main
int main() {
    cout << "=====Smart Rescue System=====" << endl;
    cout << "1. Add missing person." << endl;
    cout << "2. Match found person." << endl;
    cout << "3. Exit feature 01." << endl;
    cout << "enter your choice : " ;
    

    int choice;
    cin >> choice;

    MatchSystem missingObj, foundObj;
    switch(choice)
    {
        case 1: 
        {
            inputFamily(missingObj);
            return 0;
        }
        case 2:
        {
            inputRescuer(foundObj);
            MatchFromFile(foundObj);
            break;
        }
        case 3:
        {
            break;
        }
        default :
        {
            cout << "Invalid choice." << endl;
        }
    }

    //For feature 02
    cout << endl;
    cout << "===You are at Medical gudiance section :===" << endl;
    MedicalReport OMR;

    TakingInput(OMR);
    OMR.calculatingNonCriticalSymptoms();
    OMR.calculateSeverity();

    cout << "===Probable risks : ===" << endl;
    OMR.displayRisks();
    cout << endl;

    cout << "===Imediate actions : ===" << endl;
    OMR.displayActions();

    
    return 0;
}

```


## *Output screenshots :* 

<p align="center">
<img alt="2310012_lab2_prob_1" src="https://github.com/user-attachments/assets/eb6f3424-0bcf-451e-98db-0758dc4943dc">
</p>


<p align="center">
<img alt="2310012_lab2_prob_1" src="https://github.com/user-attachments/assets/978cd9d9-2e74-4c4a-b4fd-1287345219b9">
</p>


<p align="center">
<img alt="2310012_lab2_prob_1" src="https://github.com/user-attachments/assets/088a25d6-2964-4931-8311-c9cbca0df100">
</p>


<p align="center">
<img alt="2310012_lab2_prob_1" src="https://github.com/user-attachments/assets/b5ae923c-4aac-44d6-b267-b2b7bf4a74df">
</p>


