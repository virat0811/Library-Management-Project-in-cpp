# Library-Management-Project-in-cpp
A simple C++ Library Management System using OOP and file handling

Features of Library Management Project 
-Book Management: Add, search, issue, and return books with unique IDs.
-User Management: Maintain records of students/ staff who borrow books.
-Availability Tracking: Show wheather a book is available or issued.
-File Handling: Store and retrieve book/ user data persistently in text file.
-Validation: Prevent invalid operations(negative IDs, duplicate entries, issuing unavailable books).


.Here is the code 

#include <iostream>
#include <fstream>
#include <string>
#include <iomanip>
#include <sstream>
#include <vector>

using namespace std;

class book{
    int bookid;
    string title;
    string author;
    int quantity;
    public:
    void createbook();
    void showbook() const;
    void modifybook();
    int getbookid() const{
        return bookid;
    }
    string gettitle() const{
        return title;
    }
    string getauthor() const{
        return author;
    }
    int getquantity() const{
        return quantity;
    }
};
void book::createbook(){
    cout<<"\n Enter Book ID number : ";
    cin>>bookid;
    cin.ignore();
    cout<<"\n Enter Book Title : ";
    getline(cin,title);
    cout<<"\n Enter Author Name : ";
    getline(cin,author);
    cout<<"\n Enter Quantity : ";
    cin>>quantity;
    cout<<"\n Book Record Created Successfully! \n";
}
void book::showbook() const{
    cout<<"\n Book ID : "<<bookid;
    cout<<"\n Title : "<<title;
    cout<<"\n Author : "<<author;
    cout<<"\n Quantity : "<<quantity<<endl;
}
void book::modifybook(){
    cout<<"\n Modify Book Title : ";
    cin.ignore();
    getline(cin,title);
    cout<<"Modify Author Name : ";
    getline(cin,author);
    cout<<"Modify Quantity : ";
    cin>>quantity;
}
//file handling functions
void addbook();
void displayallbooks();
void displaybook(int);
void modifybookrecord(int);
void deletebookrecord(int);
//main menu
int main(){
    char choice;
    int id;
    do{
        cout<<"\n\n\t===== LIBRARY MANAGEMENT SYSTEM =====";
        cout<<"\n 1 -> Add New Book ";
        cout<<"\n 2 -> Display All Books ";
        cout<<"\n 3 -> Search Book by ID ";
        cout<<"\n 4 -> Modify Book Record ";
        cout<<"\n 5 -> Delete Book Record ";
        cout<<"\n 6 -> Exit ";
        cout<<"\n\n Enter your choice (1-6) :";
        cin>>choice;
        switch(choice){
            case '1':
                addbook();
                break;
            case '2':
                displayallbooks();
                break;
            case '3':
                cout<<"\n Enter Book ID to Search: ";
                cin>>id;
                displaybook(id);
                break;
            case '4':
                cout<<"\n Enter Book ID to Modify: ";
                cin>>id;
                modifybookrecord(id);
                break;
            case '5':
                cout<<"\n Enter Book ID to Delete:";
                cin>>id;
                deletebookrecord(id);
                break;
            case '6':
                cout<<"\n Exiting......";
                cout<<"Thank You for using Library Management System!\n";
                break;
            default:
                cout<<"\n Invalid Option! Please try again \n";
        }
        cin.ignore();
        cin.get();
    }while(choice!='6');
    return 0;
}
//file handling implementation
void addbook(){
    book bk;
    ofstream outFile("library.txt",ios::app);
    bk.createbook();
    outFile << bk.getbookid() << "," << bk.gettitle() << "," << bk.getauthor() << "," << bk.getquantity() << endl;
    outFile.close();
}
void displayallbooks(){
    ifstream inFile("library.txt");
    if(!inFile){
        cout<<"\n File could not be opened! No data available.\n";
        return;
    }
    cout<<"\n\n\t=====ALL BOOKS LIST=====\n";
    cout<<"------------------------------\n";
    cout<<setw(10)<<"Book ID"<<setw(25)<<"Title"<<setw(20)<<"Author"<<setw(10)<<"Qty\n";
    cout<<"-------------------------------\n";
    string line;
    while(getline(inFile, line)){
        stringstream ss(line);
        string id, title, author, qty;
        getline(ss, id, ',');
        getline(ss, title, ',');
        getline(ss, author, ',');
        getline(ss, qty, ',');
        cout<<setw(10)<<id<<setw(25)<<title<<setw(20)<<author<<setw(10)<<qty<<endl;
    }
    inFile.close();
}
void displaybook(int n){
    ifstream inFile("library.txt");
    if(!inFile){
        cout<<"\n File could not be opened!\n";
        return;
    }
    string line;
    bool found = false;
    while(getline(inFile, line)){
        stringstream ss(line);
        string id_str, title, author, qty;
        getline(ss, id_str, ',');
        getline(ss, title, ',');
        getline(ss, author, ',');
        getline(ss, qty, ',');
        int id = stoi(id_str);
        if(id == n){
            cout<<"\n Book ID : "<<id<<"\n Title : "<<title<<"\n Author : "<<author<<"\n Quantity : "<<qty<<endl;
            found = true;
        }
    }
    inFile.close();
    if(!found){
        cout<<"\n Book with ID "<<n<<" not found \n";
    }
}
void modifybookrecord(int n){
    vector<string> lines;
    ifstream inFile("library.txt");
    string line;
    while(getline(inFile, line)){
        lines.push_back(line);
    }
    inFile.close();
    bool found = false;
    for(auto& l : lines){
        stringstream ss(l);
        string id_str, title, author, qty;
        getline(ss, id_str, ',');
        getline(ss, title, ',');
        getline(ss, author, ',');
        getline(ss, qty, ',');
        int id = stoi(id_str);
        if(id == n){
            cout<<"\n Existing Book Details : \n Book ID : "<<id<<"\n Title : "<<title<<"\n Author : "<<author<<"\n Quantity : "<<qty<<endl;
            cout<<"\n Enter New Details : ";
            cout<<"\n Enter new title: ";
            cin.ignore();
            getline(cin, title);
            cout<<"Enter new author: ";
            getline(cin, author);
            cout<<"Enter new quantity: ";
            cin>>qty;
            stringstream new_ss;
            new_ss << id << "," << title << "," << author << "," << qty;
            l = new_ss.str();
            found = true;
            break;
        }
    }
    if(found){
        ofstream outFile("library.txt");
        for(auto& l : lines){
            outFile << l << endl;
        }
        outFile.close();
        cout<<"\n Record Updated Successfully!\n";
    } else {
        cout<<"\n book not found \n";
    }
}
void deletebookrecord(int n){
    vector<string> lines;
    ifstream inFile("library.txt");
    string line;
    while(getline(inFile, line)){
        lines.push_back(line);
    }
    inFile.close();
    vector<string> new_lines;
    bool found = false;
    for(auto& l : lines){
        stringstream ss(l);
        string id_str;
        getline(ss, id_str, ',');
        int id = stoi(id_str);
        if(id != n){
            new_lines.push_back(l);
        } else {
            found = true;
        }
    }
    ofstream outFile("library.txt");
    for(auto& l : new_lines){
        outFile << l << endl;
    }
    outFile.close();
    if(found){
        cout<<"\n Book Record Deleted Successfully \n";
    } else {
        cout<<"\n Book not found \n";
    }
}
