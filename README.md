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

using namespace std;

class book{
    int bookid;
    string title;
    string author;
    int quantity;
    public:
    void createbook();
    void showbook();
    int getbookid() const{
        return bookid;
    }
    string gettitle() const{
        return title;
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
    geline(cin,title);
    cout<<"Modify Author Name : ";
    getline(cin,author);
    cout<<"Modify Quantity : ";
    cin>>quantity;
}
int main(){
    

    return 0;
}
