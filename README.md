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


//file handling functions
void addbook();
void displayallbooks();
void displaybook(int);
void modifybookrecord(int);
void deletebookrecord(int);


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
            case '6':
                cout<<"\n Exiting......";
                cout<<"Thank You for using Liberary Management System!\n";
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
    ofstream outFile("library.dat",ios::binary|ios::app);
    bk.createbook();
    outFile.write(reinterpret_cast<char*>(&bk),sizeof(book));
    outFile.close();
}
void displayallbooks(){
    book bk;
    ifstream inFile("library.dat",ios::binary);
    if(!inFile){
        cout<<"\n File could not be opened! No data available.\n";
        return;
    }
    cout<<"\n\n\t=====ALL BOOKS LIST=====\n";
    cout<<"------------------------------\n";
    cout<<setw(10)<<"Book ID"<<setw(25)<<"Title"<<setw(20)<<"Author"<<setw(10)<<"Qty\n";
    cout<<"-------------------------------\n";
    while(inFile.read(reinterpret_cast<char*>(&bk),sizeof(book))){
        cout<<setw(10)<<bk.getbookid()<<setw(25)<<bk.gettitle()<<setw(20)<<bk.getquantity()<<endl;
    }
    inFile.close();
}
void displaybook(int n){
    book bk;
    bool found =false;
    ifstrema inFile("liberary.dat",ios::binary);
    if(!inFile){
        cout<<"\n File could not be opened!\n";
        return;
    }
    while(intFile.read(reinterpret_cast<char*>(&bk),sizeof(book))){
        if(bk.getbookid()==n){
            bk.showbook();
            found=true;
        }
    }
    inFile.close();
    if(!found){
        cout<<"\n Book with ID "<<n<<"not found \n".
    }
}
void modifybookrecord(int n){
    book bk;
    bool found=false;
    fstream file("library.dat",ios::binary|ios::in|ios::out);
    if(!file){
        cout<<"\n file could not be opened!\n";
        return;
    }
    while(!file.eof()&&!found){
        streampos pos=file.tellg();
        file.read(reinterpret_cast<char*>(&bk),sizeof(book));
        if(bk.getbookid()==n){
            cout<<"\n Existing Book Details : ";
            bk.showbook();
            cout<<"\n Enter New Details : ";
            bk.modifybook();
            file.seekp(pos);
            file.write(reinterpret_cast<char*>(&bk),sizeof(book));
            cout<<"\n Record Updated Successfully!\n";
            found=true;
        }
    }
    file.close();
    if(!found){
        cout<<"\n book not found \n";
    }
}

