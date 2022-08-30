/*
	Author: Kishaan G.
	Description: Library system getting and altering books from a file.
  Made on replit.
*/

// ================= Pre-processor Commands =================

#include <iostream>
#include <bits/stdc++.h>
#include <cmath>
#include <fstream>
using namespace std;

// ======================= Structures =======================

struct Book
{
	string title = "";
	string authorFullName = "";
	int releaseYear = 0;
	string genre = "";
	bool inLibrary = false;
};

// ==================== Global constant ====================

const string FILE_NAME = "libraryBooks.txt";

// ====================== Prototypes ====================== 

void keyToContinue();
int menu(string, string[], int, string);

// ======== Getting and displaying books

void pasteBookInFile(ostream &, Book);
void getIntFromFile(int &);
void getBookFromFile(istream &, Book &);
void getAllBooksFromFile(Book[], int);
void putAllInfoInFile(Book[], int);
void allBooksMenu(Book[], int, string, string, int &);

// ======== Main interfaces

void userOrLibrarianMenu(Book[], int &);
void libraryUserInterface(Book[], int &);
Book* librarianInterface(Book[], int &);

// ================================== User Interface Options

void examineLibraryContents(Book[], int);
void searchForBookInterface(Book[], int);
void borrowOrReturnBookInterface(Book[], int, int);

// ================= Borrow / Return Books

int borrowBookMenu(Book[], int);
int returnBookMenu(Book[], int);

// ================= Searching for books

void searchBasedOnString(string, Book[], int, string, string);
void searchBasedOnInt(int, Book[], int, string);
void searchBasedOnAvailability(Book[], int);

// ============================== Librarian Interface Options

Book* addBookToLibrary(Book[], int &);
void removeBookFromLibrary(Book [], int &);
void modifyBook(Book [], int);

// ===================== Definitions =====================

ostream & operator << (ostream & _stream, Book _book)
{ // This provides a description of the book. 	
	_stream << _book.title << " written by " << _book.authorFullName << " released in " << _book.releaseYear << ".\nGenre: " << _book.genre << endl;
	if(_book.inLibrary)
	{
		_stream << "It is currently available in the library.";
	}
	else
	{
		_stream << "It is currently taken out of the library.";
	}
	return _stream;
}

// ===================================

void keyToContinue()
{
	cout << "Type any key to continue..." << endl;
	cin.ignore();
	getchar();
	return;
}

// ===================================

int menu(string prompt, string options[], int numberOfOptions, string errorMsg)
{
	int response;
	do
	{
		cout << prompt << endl;
		for(int i = 0; i < numberOfOptions; i++)
		{
			cout << i+1 << ") " << options[i] << endl;
		}
		cin >> response;
		if(response < 1 or response > numberOfOptions)
		{
			cout << errorMsg << endl;
			keyToContinue();
			system("clear");
		}
	}while (response < 1 or response > numberOfOptions);
	return response;
}

// ===================================

void pasteBookInFile(ostream & _stream, Book _book)
{ // This will write a book into a file in a format that can be read.
	_stream << _book.title << endl << _book.authorFullName << endl << _book.genre << endl;
	if(_book.inLibrary)
	{
		_stream << "Available" << endl;
	}
	else
	{
		_stream << "Unavailable" << endl;
	}
	_stream << _book.releaseYear << endl;
	return;
}

// =================================== Handling the file

void getIntFromFile(int & intResponse)
{ // This will read an integer from a file. 
	ifstream inputFile;
	inputFile.open (FILE_NAME);
	if(inputFile.is_open())
	{
		inputFile >> intResponse;
		inputFile.close();
	}
	else
	{
		cout << "Could not open the file." << endl;
	}
	return;
}

// ===================================

void getBookFromFile(istream & _stream, Book & _book)
{ // Reads the information of a book from the file. 
	string inLibrary;
	getline(_stream, _book.title);
	getline(_stream, _book.authorFullName);
	getline(_stream, _book.genre);
	_stream >> inLibrary;
	if(inLibrary == "Available")
	{
		_book.inLibrary = true;
	}
	else
	{
		_book.inLibrary = false;
	}
	_stream >> _book.releaseYear;
	return;
}

// ===================================

void getAllBooksFromFile(Book _allBooks[], int _numOfBooks)
{
	ifstream inputFile;
	inputFile.open(FILE_NAME);
	int digitsInNumOfBooks = (log10(_numOfBooks) + 1); // Determines the amount of digits in the variable _numOfBooks.
	inputFile.ignore(digitsInNumOfBooks - 1);
	if(inputFile.is_open())
	{
		inputFile.ignore();
		for(int i = 0; i < _numOfBooks; i++)
		{ // Gets the information of each book in the file. 
			inputFile.ignore();
			getBookFromFile(inputFile, _allBooks[i]);
		}
		inputFile.close();
	}
	else
	{
		cout << "Could not open the file." << endl;
	}
	return;
}

// ===================================

void putAllInfoInFile(Book _allBooks[], int numOfBooks)
{
	ofstream outputFile;
	outputFile.open(FILE_NAME);
	if(outputFile.is_open())
	{
		outputFile << numOfBooks << endl;
		for(int i = 0; i < numOfBooks; i++)
		{ // This will paste all the information of the books into the file. 
			pasteBookInFile(outputFile, _allBooks[i]);
		}
		outputFile.close();
	}
	else
	{
		cout << "Could not open the file." << endl;
	}
	return;
}

// ===================================

void allBooksMenu(Book _allBooks[], int numOfBooks, string prompt, string errorMsg, int & userChoice)
{ // This is a menu for all the books with a return option.
	do
	{
		cout << prompt << endl << endl;
		for(int i = 0; i < numOfBooks; i++)
		{ // Displays all the books
			cout << i+1 << ") " << _allBooks[i] << endl << endl;
		}
		cout << numOfBooks+1 << ") Return." << endl; // Displays a return option
		cin >> userChoice;
		if(userChoice < 1 or userChoice > numOfBooks+1)
		{
			system("clear");
			cout << errorMsg << endl;
			keyToContinue();
			system("clear");
		}
	}while(userChoice < 1 or userChoice > numOfBooks + 1);
	return;
}

// =================================== Main Interface

void userOrLibrarianMenu(Book allBooks[], int & numOfBooks)
{ // Checks if the user is a regular user or librarian. 
	int userMenuChoice = 0;
	string menuOptions[3] = {"User", "Librarian", "Quit"};
	string menuPrompt = "Are you a:";
	string errorMsg = "Please type either 1, 2, or 3.";
	Book* newAllBooks = allBooks;
	do
	{
		userMenuChoice = menu(menuPrompt, menuOptions, 3, errorMsg);
		switch(userMenuChoice)
		{
			case 1:
				system("clear");
				libraryUserInterface(allBooks, numOfBooks);
				break;

			case 2:
				system("clear");
				newAllBooks = librarianInterface(allBooks, numOfBooks);
				allBooks = NULL;
				allBooks = newAllBooks;
				break;

			case 3: // Quit option. 
				system("clear");
				putAllInfoInFile(allBooks, numOfBooks);
				break;
			
		}
		allBooks = newAllBooks;
	}while(userMenuChoice != 3);
	return;
}

// =================================== User Interface

void libraryUserInterface(Book allBooks[], int & numOfBooks)
{ // Provides options for a library user.
	int userMenuChoice = 0;
	string menuOptions[5] = {"Examine the library's books", "Search for a book", "Borrow a book", "Return a book", "Return"};
	string menuPrompt = "What would you like to do?";
	string errorMsg = "Please type either 1, 2, 3 4, or 5.";
	do
	{
		userMenuChoice = menu(menuPrompt, menuOptions, 5, errorMsg);
		switch(userMenuChoice)
		{
			case 1: // Examine the library's books
				system("clear");
				examineLibraryContents(allBooks, numOfBooks);
				break;

			case 2: // Search for a book based on of the properties
				system("clear");
				searchForBookInterface(allBooks, numOfBooks);
				break;

			case 3: // Borrow a book
				system("clear");
				borrowOrReturnBookInterface(allBooks, numOfBooks, 1);
				break;

			case 4: // Return a book
				system("clear");
				borrowOrReturnBookInterface(allBooks, numOfBooks, -1);
				break;

			case 5: // Return option
				system("clear");
				break;			
		}
	}while(userMenuChoice != 5);
	return;
}

// =================================== Librarian Interface

Book* librarianInterface(Book allBooks[], int & numOfBooks)
{ // Provides options for a librarian.
	int userMenuChoice = 0;
	string menuOptions[5] = {"Add a book to the library", "Remove a book from the library", "Modify a book", "Search for a book", "Return"};
	string menuPrompt = "What would you like to do?";
	string errorMsg = "Please type either 1, 2, 3, 4, or 5.";
	Book* newAllBooks = allBooks;
	do
	{
		userMenuChoice = menu(menuPrompt, menuOptions, 5, errorMsg);
		switch(userMenuChoice)
		{
			case 1: // Add books to the library
				system("clear");
				newAllBooks = addBookToLibrary(allBooks, numOfBooks);
				allBooks = NULL;
				allBooks = newAllBooks;
				break;

			case 2: // Remove a book from the library
				system("clear");
				removeBookFromLibrary(allBooks, numOfBooks);
				break;

			case 3: // Modify a book
				system("clear");
				modifyBook(allBooks, numOfBooks);
				break;

			case 4: // Search for a book
				system("clear");
				searchForBookInterface(allBooks, numOfBooks);
				break;

			case 5: // Return option
				system("clear");
				break;			
		}
	}while(userMenuChoice != 5);
	return newAllBooks;
}

// =================================== Interface options

void examineLibraryContents(Book allBooks[], int numOfBooks)
{ // Provides descriptions for all the books in the library.
	for(int i = 0; i < numOfBooks; i++)
	{
		cout << allBooks[i] << endl << endl;
	}
	keyToContinue();
	system("clear");
	return;
}

// ===================================

void searchForBookInterface(Book allBooks[], int numOfBooks)
{ // Provides the user with options to search for a book.
	int userMenuChoice = 0;
	string menuOptions[6] = {"Title", "Author", "Release Year", "Genre", "Library Availability", "Return"};
	string menuPrompt = "What would you like to search for a book based on?";
	string errorMsg = "Please enter a valid number.";
	string stringSearch = "";
	int intSearch = 0;
	do
	{
		userMenuChoice = menu(menuPrompt, menuOptions, 6, errorMsg);
		switch(userMenuChoice)
		{
			case 1: // Title
				system("clear");
				cout << "What is the name of the book?" << endl;
				cin.ignore();
				getline(cin, stringSearch);
				searchBasedOnString(stringSearch, allBooks, numOfBooks, "Title", "Books with the title ");
				break;

			case 2: // Author
				system("clear");
				cout << "What is the name of the author?" << endl;
				cin.ignore();
				getline(cin, stringSearch);
				searchBasedOnString(stringSearch, allBooks, numOfBooks, "Author", "Books with the author ");
				break;

			case 3: // Release year
				system("clear");
				cout << "What is the release year?" << endl;
				cin >> intSearch;
				searchBasedOnInt(intSearch, allBooks, numOfBooks, "Books released near ");
				break;

			case 4: // Genre
				system("clear");
				cout << "What is the genre?" << endl;
				cin.ignore();
				getline(cin, stringSearch);
				searchBasedOnString(stringSearch, allBooks, numOfBooks, "Genre", "Books in the genre ");
				break;

			case 5: // Availability
				system("clear");
				searchBasedOnAvailability(allBooks, numOfBooks);
				break;	

			case 6: // Return option
				system("clear");
				break;
		}
	}while(userMenuChoice != 6);
	return;
}

// =================================== Borrow a Book

void borrowOrReturnBookInterface(Book _allBooks[], int numOfBooks, int borrowOrReturn)
{
	int userChoice = 0;
	if(borrowOrReturn == 1)
	{
		userChoice = borrowBookMenu(_allBooks, numOfBooks);
	}
	else if(borrowOrReturn == -1)
	{
		userChoice = returnBookMenu(_allBooks, numOfBooks);
	}
	
	if(userChoice != numOfBooks+1)
	{
		system("clear");
		if(borrowOrReturn == 1)
		{ // If the user is borrowing a book, the book is no longer available in the library.
			_allBooks[userChoice - 1].inLibrary = false;
			cout << "You succesfully borrowed " << _allBooks[userChoice - 1].title << "." << endl;
		}
		else if(borrowOrReturn == -1)
		{ // If the user is returning a book, the book is now available in the library.
			_allBooks[userChoice - 1].inLibrary = true;
			cout << "You succesfully returned " << _allBooks[userChoice - 1].title << "." << endl;
		}
		keyToContinue();	
	}
	system("clear");
	return;
}

// ===========================

int borrowBookMenu(Book _allBooks[], int numOfBooks)
{
	int userChoice = 0;
	do
	{
		allBooksMenu(_allBooks, numOfBooks, "What book would you like to borrow?", "Please enter a valid input.", userChoice); // Gets what book the user wants to return.
		if(_allBooks[userChoice - 1].inLibrary == false and userChoice != numOfBooks+1)
		{ // If the user selected a book that is not available in the library, they can't borrow the book. 
			system("clear");
			cout << "Please enter a book that is available in the library." << endl;
			keyToContinue();
			system("clear");
		}
	}while(_allBooks[userChoice - 1].inLibrary == false and userChoice != numOfBooks+1);
	return userChoice;
}

// ===========================

int returnBookMenu(Book _allBooks[], int numOfBooks)
{
	int userChoice = 0;
	do
	{
		allBooksMenu(_allBooks, numOfBooks, "What book would you like to return?", "Please enter a valid input.", userChoice);
		if(_allBooks[userChoice - 1].inLibrary == true and userChoice != numOfBooks+1)
		{ // If the user selected a book that is available in the library, they can't return the book. 
			system("clear");
			cout << "Please enter a book that is taken out of the library." << endl;
			keyToContinue();
			system("clear");
		}
	}while(_allBooks[userChoice - 1].inLibrary == true and userChoice != numOfBooks+1);
	return userChoice;
}

// =========================== Search for books

void searchBasedOnString(string search, Book _allBooks[], int numOfBooks, string comparison, string prompt)
{
	string capitalizedSearch = search;
	transform(capitalizedSearch.begin(), capitalizedSearch.end(), capitalizedSearch.begin(), ::toupper); // Transforms the characters that the user searched into capital letters.
	string capitalizedComparison;
	
	cout << endl << prompt << search << " include:" << endl << endl;
	
	for(int i = 0; i < numOfBooks; i++)
	{
		if(comparison == "Title")
		{
			capitalizedComparison = _allBooks[i].title;
		}
		else if(comparison == "Author")
		{
			capitalizedComparison = _allBooks[i].authorFullName;
		}
		else if(comparison == "Genre")
		{
			capitalizedComparison = _allBooks[i].genre;
		}
		transform(capitalizedComparison.begin(), capitalizedComparison.end(), capitalizedComparison.begin(), ::toupper); // Transforms the characters of what the user is searching for.
		if(capitalizedSearch == capitalizedComparison)
		{ // IF the user searched the name of a book, display it.
			cout << _allBooks[i] << endl << endl;
		}
		else if(capitalizedComparison.find(capitalizedSearch) != string::npos) // Checks if the user typed in any similar string of chracters as what they're searching based on. 
		{
			cout << _allBooks[i] << endl << endl;
		}
	}
	keyToContinue();
	system("clear");
	return;
}

// ===========================

void searchBasedOnInt(int search, Book _allBooks[], int numOfBooks, string prompt)
{
	cout << endl << prompt << search << " include:" << endl << endl;
	for(int i = 0; i < numOfBooks; i++)
	{
		if(abs(search - _allBooks[i].releaseYear) < 4)
		{ // Checks if the user's search is within 3 years to the release year of any book.
			cout << _allBooks[i] << endl << endl;
		}
	}
	keyToContinue();
	system("clear");
	return;
}

// ===========================

void searchBasedOnAvailability(Book _allBooks[], int numOfBooks)
{
	string menuOptions[3] = {"Available", "Unavailable", "Return"};
	int userMenuChoice = menu("What would you like to see books that are:", menuOptions, 3, "Please type either 1, 2 or 3.");

	if(userMenuChoice == 1)
	{
		cout << endl << "All books available in the library include:" << endl << endl;
		for(int i = 0; i < numOfBooks; i++)
		{
			if(_allBooks[i].inLibrary)
			{ // Displays all available books.
				cout << _allBooks[i] << endl << endl;
			}
		}
		keyToContinue();
	}
	else if(userMenuChoice == 2)
	{
		cout << endl << "All unavailable books in the library include:" << endl << endl;
		for(int i = 0; i < numOfBooks; i++)
		{
			if(_allBooks[i].inLibrary == false)
			{ // Displays all unavailable books.
				cout << _allBooks[i] << endl << endl;
			}
		}
		keyToContinue();
	}
	system("clear");
	return;
}

// ======================================= Add / Remove Books

Book* addBookToLibrary(Book _allBooks[], int & numOfBooks)
{
	Book *newAllBooks = new Book[numOfBooks+1]; // Creates a pointer to a new array.
	copy(_allBooks, _allBooks + min(numOfBooks, numOfBooks+1), newAllBooks); // Copies over all the books from the original array to the new pointer. 
	numOfBooks++;
	string menuOptions[2] = {"Available", "Unavailable"};
	string menuPrompt = "Is the book available?";
	string errorMsg = "Please enter either 1 or 2.";
	cin.ignore();

	// Gets the information for the new book. 
	cout << "What is the title of the new book?" << endl;
	getline(cin, newAllBooks[numOfBooks-1].title);

	system("clear");
	cout << "What is the author of the new book?" << endl;
	getline(cin, newAllBooks[numOfBooks-1].authorFullName);

	system("clear");
	cout << "What is the release year of the new book?" << endl;
	cin >> newAllBooks[numOfBooks-1].releaseYear;
	cin.ignore();
	
	system("clear");
	cout << "What is the genre of the new book?" << endl;
	getline(cin, newAllBooks[numOfBooks-1].genre);

	system("clear");
	int availabilityChoice = menu(menuPrompt, menuOptions, 2, errorMsg);
	if(availabilityChoice == 1)
	{
		newAllBooks[numOfBooks-1].inLibrary = true;
	}
	else newAllBooks[numOfBooks-1].inLibrary = false;
	system("clear");
	cout << newAllBooks[numOfBooks-1] << endl << "has been added." << endl;
	system("clear");
	return newAllBooks; // Returns the pointer to the new array of books.
}

// ===================================
	
void removeBookFromLibrary(Book _allBooks[], int & numOfBooks)
{
	int removeBook = 0;
	allBooksMenu(_allBooks, numOfBooks, "What book would you like to remove?", "Please enter a valid input.", removeBook); // Gets which book the user wants to remove.

	for(int i = removeBook-1; i < numOfBooks-1; i++)
	{ // Moves all books back one position in the array from the position of the removed book.
		_allBooks[i].title = _allBooks[i+1].title;
		_allBooks[i].authorFullName = _allBooks[i+1].authorFullName;
		_allBooks[i].releaseYear = _allBooks[i+1].releaseYear;
		_allBooks[i].genre = _allBooks[i+1].genre;
		_allBooks[i].inLibrary = _allBooks[i+1].inLibrary;
	}
	numOfBooks--;
	system("clear");
	return;
}

// ===================================

void modifyBook(Book _allBooks[], int numOfBooks)
{
	int modifyBook = 0;
	int modifyAttribute = 0;
	string menuOptions[6] = {"Title", "Author", "Release Year", "Genre", "Library Availability", "Return"};
	allBooksMenu(_allBooks, numOfBooks, "What book would you like to modify?", "Please enter a valid input.", modifyBook);
	system("clear");
	string availabilityOptions[2] = {"Available", "Unavailable"};

	if(modifyBook != 5)
	{ // Determines what book they want to modify.
		modifyAttribute = menu("What would you like to modify?", menuOptions, 6, "Please enter a valid input.");
		system("clear");
	}
	
	cin.ignore();
	if(modifyAttribute == 1)
	{ // Changes the contents of what the user chose to change. 
		cout << "What is the new title?" << endl;
		getline(cin, _allBooks[modifyBook-1].title);
	}
	else if(modifyAttribute == 2)
	{
		cout << "Who is the new author?" << endl;
		getline(cin, _allBooks[modifyBook-1].authorFullName);
	}
	else if(modifyAttribute == 3)
	{
		cout << "What is the new release year?" << endl;
		cin >> _allBooks[modifyBook-1].releaseYear;
	}
	else if(modifyAttribute == 4)
	{
		cout << "What is the new genre?" << endl;
		getline(cin, _allBooks[modifyBook-1].genre);
	}
	else if(modifyAttribute == 5)
	{
		cout << "What is the new availability?" << endl;
		if(menu("Is the book now:", availabilityOptions, 2, "Please type either 1 or 2.") == 1)
		{
			_allBooks[modifyBook-1].inLibrary = true;
		}
		else _allBooks[modifyBook-1].inLibrary = false;
	}
	system("clear");
	return;
}

// ===================================

int main()
{
	int numOfBooks = 0;	
	getIntFromFile(numOfBooks);
	Book allBooks[numOfBooks];
	
	getAllBooksFromFile(allBooks, numOfBooks);
	userOrLibrarianMenu(allBooks, numOfBooks);

	return 0;
}
