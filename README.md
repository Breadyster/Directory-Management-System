#include <iostream>
#include <string>
#include <direct.h>

void listAllFiles() {
    std::cout << "\nFiles in current directory:\n";
    system("dir /b");
}

void listFilesByExtension(const std::string& ext) {
    std::string command = "dir /b *" + ext;
    std::cout << "\nFiles with extension '" << ext << "':\n";
    system(command.c_str());
}

void listFilesByPattern(const std::string& pattern) {
    std::string command = "dir /b " + pattern;
    std::cout << "\nFiles matching pattern '" << pattern << "':\n";
    system(command.c_str());
}

void listFiles() {
    int subchoice;
    std::string input;
    do {
        std::cout << "\n--- List Files Submenu ---\n";
        std::cout << "[1] List All Files\n";
        std::cout << "[2] List Files by Extension\n";
        std::cout << "[3] List Files by Pattern\n";
        std::cout << "[4] Back to Main Menu\n";
        std::cout << "Enter your choice: ";
        std::cin >> subchoice;
        std::cin.ignore();
        switch (subchoice) {
            case 1:
                listAllFiles();
                break;
            case 2:
                std::cout << "Enter file extension (e.g., .txt): ";
                std::getline(std::cin, input);
                listFilesByExtension(input);
                break;
            case 3:
                std::cout << "Enter file pattern (e.g., moha*.*): ";
                std::getline(std::cin, input);
                listFilesByPattern(input);
                break;
            case 4:
                break;
            default:
                std::cout << "Invalid choice. Try again.\n";
        }
    } while (subchoice != 4);
}

void createDirectory(const std::string& path) {
    if (_mkdir(path.c_str()) == 0) {
        std::cout << "Directory created successfully: " << path << "\n";
    } else {
        std::cout << "Failed to create directory (it may already exist).\n";
    }
}

void changeDirectory() {
    std::string path;
    std::cout << "Enter directory path to change to: ";
    std::getline(std::cin, path);
    if (_chdir(path.c_str()) == 0) {
        char cwd[260];
        _getcwd(cwd, sizeof(cwd));
        std::cout << "Current directory changed to: " << cwd << "\n";
    } else {
        std::cout << "Error: Directory does not exist or cannot be accessed.\n";
    }
}

void mainMenu() {
    int choice;
    do {
        char cwd[260];
        _getcwd(cwd, sizeof(cwd));
        std::cout << "\n=== Directory Management System ===\n";
        std::cout << "[1] List Files\n";
        std::cout << "[2] Create Directory\n";
        std::cout << "[3] Change Directory\n";
        std::cout << "[4] Exit\n";
        std::cout << "Current Directory: " << cwd << "\n";
        std::cout << "Enter your choice: ";
        std::cin >> choice;
        std::cin.ignore();
        switch (choice) {
            case 1:
                listFiles();
                break;
            case 2: {
                std::string path;
                std::cout << "Enter directory name to create: ";
                std::getline(std::cin, path);
                createDirectory(path);
                break;
            }
            case 3:
                changeDirectory();
                break;
            case 4:
                std::cout << "Exiting program.\n";
                break;
            default:
                std::cout << "Invalid choice. Try again.\n";
        }
    } while (choice != 4);
}

int main() {
    mainMenu();
    return 0;
}
