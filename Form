#include <iostream>
#include <fstream>
#include <cstdlib>
#include <regex>
#include <cctype>
#include <algorithm>
#include <ctime>  // For timestamp in logging

using namespace std;

class temp {
    string userName, Email, password;
    string searchName, searchPass, searchEmail;
    fstream file;
    string logFileName = "log.txt";  // Log file name
    string activeUser = "";  // Current active user

public:
    void login();
    void signUP();
    void forgot();
    void userMenu();
    void changePassword();
    void updateProfile();
    void deactivateAccount();
    void logout();
    void log(const string& message);
    void setActiveUser(const string& user);
    const string& getActiveUser() const;
};

temp obj;

bool isPunctuation(char c) {
    return ispunct(c);
}

int main() {
    char choice;

    do {
        cout << "\n1- Login";
        cout << "\n2- Sign-Up";
        cout << "\n3- Forgot Password";
        cout << "\n4- Exit" << endl;
        cout << "\nEnter Your Choice: ";
        cin >> choice;

        switch (choice) {
            case '1':
                cin.ignore();
                obj.login();
                break;
            case '2':
                cin.ignore();
                obj.signUP();
                break;
            case '3':
                cin.ignore();
                obj.forgot();
                break;
            case '4':
                return 0;
            default:
                cout << "Invalid Selection...!";
        }

        // Check if the user is logged in and skip asking for another operation
        if (!obj.getActiveUser().empty()) {
            obj.userMenu();
        } else {
            cout << "\nDo you want to perform another operation? (y/n): ";
            cin >> choice;
        }

    } while (choice == 'y' || choice == 'Y');

    return 0;
}

void temp::log(const string& message) {
    // Log messages with a timestamp to a log file
    ofstream logFile(logFileName, ios::app);
    if (logFile.is_open()) {
        time_t now = time(0);
        tm* localTime = localtime(&now);
        logFile << "[" << localTime->tm_year + 1900 << '-'
                << localTime->tm_mon + 1 << '-' << localTime->tm_mday << ' '
                << localTime->tm_hour << ':' << localTime->tm_min << ':'
                << localTime->tm_sec << "] " << message << endl;
        logFile.close();
    }
}

void temp::setActiveUser(const string& user) {
    activeUser = user;
}

const string& temp::getActiveUser() const {
    return activeUser;
}
void temp::logout() {
    log("Logout for user: " + activeUser);
    cout << "Logout successful!\n";
    activeUser = "";  // Clear the active user
}



void temp::signUP() {
    cout << "\n*******SIGN UP*******\n\n";

    // Get User Name
    do {
        cout << "\nEnter Your User Name: ";
        getline(cin, userName);

        // Check if username already exists (case-insensitive)
        file.open("loginData.txt", ios::in);
        if (!file.is_open()) {
            cout << "Error opening file for reading\n";
            return;
        }
        while (getline(file, searchName, '*')) {
            getline(file, searchEmail, '*');
            getline(file, searchPass, '\n');

            bool usernameExists = equal(userName.begin(), userName.end(), searchName.begin(),
                                         [](char a, char b) { return tolower(a) == tolower(b); });

            if (usernameExists) {
                cout << "Username already exists. Please choose a different username.\n";
                file.close();
                continue;
            }
        }
        file.close();

        // Validate User Name Format (only alphabets and digits)
        if (any_of(userName.begin(), userName.end(), [](char c) { return !isalnum(c); })) {
            cout << "Invalid username format. Please use only alphabets and digits.\n";
        } else {
            break;
        }
    } while (true);

    // Get Email
    do {
        cout << "\nEnter A Valid mail Address: ";
        getline(cin, Email);

        // Check if email already exists (case-insensitive)
        file.open("loginData.txt", ios::in);
        if (!file.is_open()) {
            cout << "Error opening file for reading\n";
            return;
        }
        while (getline(file, searchName, '*')) {
            getline(file, searchEmail, '*');
            getline(file, searchPass, '\n');

            bool emailExists = equal(Email.begin(), Email.end(), searchEmail.begin(),
                                     [](char a, char b) { return tolower(a) == tolower(b); });

            if (emailExists) {
                cout << "Email address already exists. Please choose a different email.\n";
                file.close();
                continue;
            }
        }
        file.close();

        // Validate Email Format using Regular Expression
        regex emailRegex(R"([a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,})");
        if (!regex_match(Email, emailRegex)) {
            cout << "Invalid email format. Please enter a valid email address.\n";
        } else {
            break;
        }
    } while (true);

    // Get Password
    string password;
    do {
        cout << "\nEnter Your Password (minimum 8 characters): ";
        getline(cin, password);

        if (password.length() < 8 ||
            !any_of(password.begin(), password.end(), ::isdigit) ||
            !any_of(password.begin(), password.end(), ::isupper) ||
            !any_of(password.begin(), password.end(), ::islower) ||
            !any_of(password.begin(), password.end(), isPunctuation)) {
            cout << "\nEnter a stronger password with at least 8 characters, including uppercase, lowercase, digit, and special character: ";
        } else {
            break;
        }
    } while (true);

    // Open the file in append mode and write the new user information
    file.open("loginData.txt", ios::out | ios::app);
    if (!file.is_open()) {
        cout << "Error opening file for writing\n";
        cout << "File stream state: " << file.rdstate() << endl;
        return;
    }

    file << userName << "*" << Email << "*" << password << endl;
    file.close();

    cout << "\nSign up successful!" << endl;
}

void temp::login() {
    char choice;
    do {
        cout << "\n*******LOGIN*******\n\n";
        cout << "Enter Your User Name: ";
        getline(cin, searchName);
        cout << "\nEnter Your Password: ";
        getline(cin, searchPass);

        file.open("loginData.txt", ios::in);
        while (getline(file, userName, '*')) {
            getline(file, Email, '*');
            getline(file, password, '\n');

            if (userName == searchName && password == searchPass) {
                obj.setActiveUser(userName);  // Set the active user
                cout << "\nAccount Login Successful...!"<<endl;
                cout << "\nUsername: " << userName << endl;
                cout << "\nEmail: " << Email << endl;
                file.close();
                return;   // Exit the function if login is successful
            }
        }

        // If the loop reaches here, it means the account is not found
        cout << "\nIncorrect Username or Password...!";
        // Move the prompt inside the login function
        cout << "\nDo you wish to try again? (y/n)?: ";
        cin >> choice;
        cin.ignore(); // Clear the newline character
        if (choice != 'y' && choice != 'Y') {
            file.close();
            return;
        }
        // Remove the break statement to allow the loop to continue
    } while (true);
}


void temp::userMenu() {
    char userChoice;
    do {
        cout << "\n*******USER MENU*******\n\n";
        cout << "1- Change Password\n";
        cout << "2- Update Profile\n";
        cout << "3- Deactivate Account\n";
        cout << "4- Logout\n";
        cout << "Enter Your Choice: ";
        cin >> userChoice;

        switch (userChoice) {
            case '1':
                changePassword();
                break;
            case '2':
                updateProfile();
                break;
            case '3':
                deactivateAccount();
                return;  // Return after deactivating the account
            case '4':
                log("Logout for user: " + activeUser);
                cout << "Logout successful!\n";
                return;
            default:
                cout << "Invalid Selection...!";
        }

        cout << "\nDo you want to perform another operation? (y/n): ";
        cin >> userChoice;
    } while (userChoice == 'y' || userChoice == 'Y');
}

void temp::changePassword() {
    string newPassword;
    cout << "\nEnter Your New Password (minimum 8 characters): ";
    cin.ignore();
    getline(cin, newPassword);

    while (newPassword.length() < 8 ||
           !any_of(newPassword.begin(), newPassword.end(), ::isdigit) ||
           !any_of(newPassword.begin(), newPassword.end(), ::isupper) ||
           !any_of(newPassword.begin(), newPassword.end(), ::islower) ||
           !any_of(newPassword.begin(), newPassword.end(), isPunctuation)) {
        cout << "\nEnter a stronger password with at least 8 characters, including uppercase, lowercase, digit, and special character: ";
        getline(cin, newPassword);
    }

    // Update the password in the file
    file.open("loginData.txt", ios::in);
    fstream tempFile("tempData.txt", ios::out | ios::app);

    while (getline(file, searchName, '*')) {
        getline(file, searchEmail, '*');
        getline(file, searchPass, '\n');

        if (searchName == activeUser) {
            log("Password changed for user: " + activeUser);
            tempFile << searchName << "*" << searchEmail << "*" << newPassword << endl;
        } else {
            tempFile << searchName << "*" << searchEmail << "*" << searchPass << endl;
        }
    }

    file.close();
    tempFile.close();

    remove("loginData.txt");
    rename("tempData.txt", "loginData.txt");

    cout << "\nPassword updated successfully!\n";
}

void temp::updateProfile() {
    cout << "\n*******UPDATE PROFILE*******\n\n";
    cout << "Enter Your New User Name: ";
    cin.ignore();
    getline(cin, userName);

    // Update the username in the file
    file.open("loginData.txt", ios::in);
    fstream tempFile("tempData.txt", ios::out | ios::app);

    while (getline(file, searchName, '*')) {
        getline(file, searchEmail, '*');
        getline(file, searchPass, '\n');

        if (searchName == userName) {
            cout << "Username already exists. Please choose a different username.\n";
            file.close();
            tempFile.close();
            remove("tempData.txt");
            return;
        }

        tempFile << searchName << "*" << searchEmail << "*" << searchPass << endl;
    }

    file.close();
    tempFile.close();

    remove("loginData.txt");
    rename("tempData.txt", "loginData.txt");

    log("Profile updated for user: " + activeUser);
    cout << "\nProfile updated successfully!\n";
}

void temp::deactivateAccount() {
    log("Account deactivated for user: " + activeUser);
    cout << "\nAccount deactivated successfully!\n";
    activeUser = "";  // Clear the active user
}

void temp::forgot() {
    cout << "\n*******FORGOT PASSWORD*******\n\n";
    cout << "\nEnter Your UserName: ";
    cin.ignore();
    getline(cin, searchName);
    cout << "\nEnter A valid mail Address: ";
    getline(cin, searchEmail);

    file.open("loginData.txt", ios::in);
    getline(file, userName, '*');
    getline(file, Email, '*');
    getline(file, password, '\n');

    while (!file.eof()) {
        if (userName == searchName) {
            if (Email == searchEmail) {
                cout << "\nAccount Found...!" << endl;
                cout << "Your Password: " << password << endl;
                file.close();
                return;
            } else {
                cout << "Account not found...!\n";
            }
        } else {
            cout << "\nAccount not found...!\n";
        }
        getline(file, userName, '*');
        getline(file, Email, '*');
        getline(file, password, '\n');
    }
    file.close();
    cout << "Have a nice day!" << endl;
}
