# Library-Managment-System

#include <iostream>
#include <fstream>
#include <string>
using namespace std;

struct Book
{
    int id;
    string title;
    string author;
    int isIssued;
};

// Add a new book
void addBook()
{
    Book newBook;

    ofstream file("library.txt", ios::app);

    if (!file)
    {
        cout << "Error opening file!" << endl;
        return;
    }

    cout << "Enter Book ID: ";
    cin >> newBook.id;
    cin.ignore();

    cout << "Enter Book Title: ";
    getline(cin, newBook.title);

    cout << "Enter Book Author: ";
    getline(cin, newBook.author);

    newBook.isIssued = 0;

    file << newBook.id << "|"
         << newBook.title << "|"
         << newBook.author << "|"
         << newBook.isIssued << endl;

    file.close();

    cout << "Book added successfully!" << endl;
}

// Display all books
void displayBooks()
{
    Book book;
    string line;

    ifstream file("library.txt");

    if (!file)
    {
        cout << "No books found!" << endl;
        return;
    }

    while (getline(file, line))
    {
        size_t pos1 = line.find('|');
        size_t pos2 = line.find('|', pos1 + 1);
        size_t pos3 = line.find('|', pos2 + 1);

        if (pos1 == string::npos ||
            pos2 == string::npos ||
            pos3 == string::npos)
        {
            continue;
        }

        book.id = stoi(line.substr(0, pos1));
        book.title = line.substr(pos1 + 1, pos2 - pos1 - 1);
        book.author = line.substr(pos2 + 1, pos3 - pos2 - 1);
        book.isIssued = stoi(line.substr(pos3 + 1));

        cout << "\nID: " << book.id << endl;
        cout << "Title: " << book.title << endl;
        cout << "Author: " << book.author << endl;
        cout << "Status: "
             << (book.isIssued ? "Issued" : "Available")
             << endl;
    }

    file.close();
}

// Issue a book
void issueBook()
{
    Book book;
    int bookID;
    bool found = false;

    ifstream file("library.txt");
    ofstream temp("temp.txt");

    if (!file || !temp)
    {
        cout << "Error opening file!" << endl;
        return;
    }

    cout << "Enter Book ID to Issue: ";
    cin >> bookID;

    string line;

    while (getline(file, line))
    {
        size_t pos1 = line.find('|');
        size_t pos2 = line.find('|', pos1 + 1);
        size_t pos3 = line.find('|', pos2 + 1);

        if (pos1 == string::npos ||
            pos2 == string::npos ||
            pos3 == string::npos)
        {
            continue;
        }

        book.id = stoi(line.substr(0, pos1));
        book.title = line.substr(pos1 + 1, pos2 - pos1 - 1);
        book.author = line.substr(pos2 + 1, pos3 - pos2 - 1);
        book.isIssued = stoi(line.substr(pos3 + 1));

        if (book.id == bookID && book.isIssued == 0)
        {
            book.isIssued = 1;
            found = true;

            cout << "Book issued successfully!" << endl;
        }

        temp << book.id << "|"
             << book.title << "|"
             << book.author << "|"
             << book.isIssued << endl;
    }

    file.close();
    temp.close();

    if (!found)
    {
        cout << "Book not found or already issued." << endl;
    }

    remove("library.txt");
    rename("temp.txt", "library.txt");
}

// Return a book
void returnBook()
{
    Book book;
    int bookID;
    bool found = false;

    ifstream file("library.txt");
    ofstream temp("temp.txt");

    if (!file || !temp)
    {
        cout << "Error opening file!" << endl;
        return;
    }

    cout << "Enter Book ID to Return: ";
    cin >> bookID;

    string line;

    while (getline(file, line))
    {
        size_t pos1 = line.find('|');
        size_t pos2 = line.find('|', pos1 + 1);
        size_t pos3 = line.find('|', pos2 + 1);

        if (pos1 == string::npos ||
            pos2 == string::npos ||
            pos3 == string::npos)
        {
            continue;
        }

        book.id = stoi(line.substr(0, pos1));
        book.title = line.substr(pos1 + 1, pos2 - pos1 - 1);
        book.author = line.substr(pos2 + 1, pos3 - pos2 - 1);
        book.isIssued = stoi(line.substr(pos3 + 1));

        if (book.id == bookID && book.isIssued == 1)
        {
            book.isIssued = 0;
            found = true;

            cout << "Book returned successfully!" << endl;
        }

        temp << book.id << "|"
             << book.title << "|"
             << book.author << "|"
             << book.isIssued << endl;
    }

    file.close();
    temp.close();

    if (!found)
    {
        cout << "Invalid book ID or book is not issued." << endl;
    }

    remove("library.txt");
    rename("temp.txt", "library.txt");
}


void deleteBook()
{
    Book book;
    int bookID;
    bool found = false;

    ifstream file("library.txt");
    ofstream temp("temp.txt");

    if (!file || !temp)
    {
        cout << "Error opening file!" << endl;
        return;
    }

    cout << "Enter Book ID to Delete: ";
    cin >> bookID;

    string line;

    while (getline(file, line))
    {
        size_t pos1 = line.find('|');
        size_t pos2 = line.find('|', pos1 + 1);
        size_t pos3 = line.find('|', pos2 + 1);

        if (pos1 == string::npos ||
            pos2 == string::npos ||
            pos3 == string::npos)
        {
            continue;
        }

        book.id = stoi(line.substr(0, pos1));
        book.title = line.substr(pos1 + 1, pos2 - pos1 - 1);
        book.author = line.substr(pos2 + 1, pos3 - pos2 - 1);
        book.isIssued = stoi(line.substr(pos3 + 1));

        if (book.id == bookID)
        {
            found = true;
            cout << "Book deleted successfully!" << endl;
        }
        else
        {
            temp << book.id << "|"
                 << book.title << "|"
                 << book.author << "|"
                 << book.isIssued << endl;
        }
    }

    file.close();
    temp.close();

    if (!found)
    {
        cout << "Book ID not found." << endl;
        remove("temp.txt");
    }
    else
    {
        remove("library.txt");
        rename("temp.txt", "library.txt");
    }
}

int main()
{
    int choice;

    while (true)
    {
        cout << "\n==============================" << endl;
        cout << "   Library Book Search System" << endl;
        cout << "==============================" << endl;

        cout << "1. Add Book" << endl;
        cout << "2. Display All Books" << endl;
        cout << "3. Issue Book" << endl;
        cout << "4. Return Book" << endl;
        cout << "5. Delete Book" << endl;
        cout << "6. Exit" << endl;

        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice)
        {
        case 1:
            addBook();
            break;

        case 2:
            displayBooks();
            break;

        case 3:
            issueBook();
            break;

        case 4:
            returnBook();
            break;

        case 5:
            deleteBook();
            break;

        case 6:
            cout << "Thank you for using the Library Management System!"
                 << endl;
            return 0;

        default:
            cout << "Invalid Choice!" << endl;
        }
    }

    return 0;
}

