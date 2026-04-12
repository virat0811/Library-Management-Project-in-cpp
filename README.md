# Library-Management-Project-in-cpp
A simple C++ Library Management System using OOP and file handling
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
int main(){
    

    return 0;
}
