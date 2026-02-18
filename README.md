/*
===========================================================
                STUDENT MANAGEMENT SYSTEM
===========================================================

Student Name      : Zaid Manan
Registration No   : SE120252056
Department        : Software Engineering
Professor         : Dr.Amjad Mehmood

Description:
This system allows:
    • Add new students
    • Update student records
    • Delete students
    • Search students
    • Sort records
    • Show statistics
    • Save & Load from file

Language: C++
Concepts Used:
    - OOP (Classes & Objects)
    - File Handling
    - Vectors
    - Sorting
    - Searching
===========================================================
*/

#include <iostream>
#include <fstream>
#include <vector>
#include <iomanip>
#include <algorithm>

using namespace std;

/*=========================================================
                        STUDENT CLASS
=========================================================*/
class Student
{
private:
    int id;
    string name;
    float marks[5];
    float average;
    char grade;

public:

    /* Constructor */
    Student()
    {
        id = 0;
        name = "";
        average = 0;
        grade = 'F';

        for (int i = 0; i < 5; i++)
            marks[i] = 0;
    }

    /* ================= SETTERS ================= */
    void setId(int newId) { id = newId; }

    void setName(string newName) { name = newName; }

    void setMarks(float m1, float m2, float m3, float m4, float m5)
    {
        marks[0] = m1;
        marks[1] = m2;
        marks[2] = m3;
        marks[3] = m4;
        marks[4] = m5;

        calculateAverage();
        calculateGrade();
    }

    /* ================= GETTERS ================= */
    int getId() const { return id; }
    string getName() const { return name; }
    float getAverage() const { return average; }
    char getGrade() const { return grade; }
    float getMark(int i) const { return marks[i]; }

private:

    /* Calculate Average */
    void calculateAverage()
    {
        float sum = 0;

        for (int i = 0; i < 5; i++)
            sum += marks[i];

        average = sum / 5.0;
    }

    /* Calculate Grade */
    void calculateGrade()
    {
        if (average >= 90) grade = 'A';
        else if (average >= 80) grade = 'B';
        else if (average >= 70) grade = 'C';
        else if (average >= 60) grade = 'D';
        else grade = 'F';
    }
};


/*=========================================================
                    STUDENT MANAGER CLASS
=========================================================*/
class StudentManager
{
private:
    vector<Student> students;
    string filename = "students.txt";

    /* Find Student Index by ID */
    int findStudentById(int id)
    {
        for (int i = 0; i < students.size(); i++)
            if (students[i].getId() == id)
                return i;

        return -1;
    }

public:

    /*=====================================================
                        ADD STUDENT
    =====================================================*/
    void addStudent()
    {
        Student s;
        int id;
        string name;
        float m1, m2, m3, m4, m5;

        cout << "\n========= ADD NEW STUDENT =========\n";

        cout << "Enter Student ID: ";
        cin >> id;

        if (findStudentById(id) != -1)
        {
            cout << "⚠ Student ID already exists!\n";
            return;
        }

        cin.ignore();
        cout << "Enter Name: ";
        getline(cin, name);

        cout << "\nEnter marks for 5 subjects:\n";
        cout << "1. Functional English        : "; cin >> m1;
        cout << "2. Islamiyat                 : "; cin >> m2;
        cout << "3. Programming Fundamentals  : "; cin >> m3;
        cout << "4. Discrete Mathematics      : "; cin >> m4;
        cout << "5. Quantitative Reasoning    : "; cin >> m5;

        s.setId(id);
        s.setName(name);
        s.setMarks(m1, m2, m3, m4, m5);

        students.push_back(s);

        cout << "\n✓ Student Added Successfully!\n";
    }

    /*=====================================================
                        DISPLAY ALL
    =====================================================*/
    void displayAll()
    {
        if (students.empty())
        {
            cout << "\nNo students available.\n";
            return;
        }

        cout << "\n============== STUDENT LIST ==============\n";

        for (int i = 0; i < students.size(); i++)
        {
            displayStudent(i);
        }
    }

    /*=====================================================
                        DISPLAY ONE
    =====================================================*/
    void displayStudent(int index)
    {
        const Student &s = students[index];

        cout << "\n-------------------------------------------\n";
        cout << "Student ID   : " << s.getId() << endl;
        cout << "Name         : " << s.getName() << endl;

        cout << "Marks        : ";
        for (int i = 0; i < 5; i++)
            cout << s.getMark(i) << " ";

        cout << "\nAverage      : "
             << fixed << setprecision(2)
             << s.getAverage() << "%";

        cout << "\nGrade        : " << s.getGrade();
        cout << "\n-------------------------------------------\n";
    }

    /*=====================================================
                        SORT BY AVERAGE
    =====================================================*/
    void sortByAverage()
    {
        sort(students.begin(), students.end(),
            [](Student &a, Student &b)
            {
                return a.getAverage() > b.getAverage();
            });

        cout << "\n✓ Sorted by Highest Average\n";
        displayAll();
    }

    /*=====================================================
                        SORT BY NAME
    =====================================================*/
    void sortByName()
    {
        sort(students.begin(), students.end(),
            [](Student &a, Student &b)
            {
                return a.getName() < b.getName();
            });

        cout << "\n✓ Sorted Alphabetically (A-Z)\n";
        displayAll();
    }

    /*=====================================================
                        SHOW STATISTICS
    =====================================================*/
    void showStatistics()
    {
        if (students.empty())
        {
            cout << "\nNo data available.\n";
            return;
        }

        float total = 0;
        float highest = 0;
        int topIndex = 0;

        for (int i = 0; i < students.size(); i++)
        {
            total += students[i].getAverage();

            if (students[i].getAverage() > highest)
            {
                highest = students[i].getAverage();
                topIndex = i;
            }
        }

        cout << "\n============== STATISTICS ==============\n";
        cout << "Total Students : " << students.size() << endl;
        cout << "Class Average  : "
             << fixed << setprecision(2)
             << total / students.size() << "%\n";

        cout << "\nTop Student:\n";
        displayStudent(topIndex);
    }

};


/*=========================================================
                        MAIN MENU
=========================================================*/
void displayMenu()
{
    cout << "\n================================================\n";
    cout << "         STUDENT MANAGEMENT SYSTEM MENU\n";
    cout << "================================================\n";
    cout << "1. Add Student\n";
    cout << "2. Display All Students\n";
    cout << "3. Sort by Average\n";
    cout << "4. Sort by Name\n";
    cout << "5. Show Statistics\n";
    cout << "0. Exit\n";
    cout << "================================================\n";
    cout << "Enter choice: ";
}


/*=========================================================
                            MAIN
=========================================================*/
int main()
{
    StudentManager manager;
    int choice;

    cout << "=========================================\n";
    cout << "     WELCOME TO STUDENT SYSTEM\n";
    cout << "=========================================\n";

    do
    {
        displayMenu();
        cin >> choice;

        switch (choice)
        {
            case 1: manager.addStudent(); break;
            case 2: manager.displayAll(); break;
            case 3: manager.sortByAverage(); break;
            case 4: manager.sortByName(); break;
            case 5: manager.showStatistics(); break;
            case 0: cout << "\nThank You!\n"; break;
            default: cout << "\nInvalid Choice!\n";
        }

    } while (choice != 0);

    return 0;
}
