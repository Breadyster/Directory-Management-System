#include <iostream>
#include <direct.h>       
#include <filesystem>     
#include <string>

namespace fs = std::filesystem;

void createDirectory(const std::string& path) {
    if (_mkdir(path.c_str()) == 0) {
        std::cout << "Directory created: " << path << std::endl;
    } else {
        std::cout << "Failed to create directory or it already exists.\n";
    }
}

void deleteDirectory(const std::string& path) {
    try {
        if (fs::remove_all(path) > 0) {
            std::cout << "Directory deleted: " << path << std::endl;
        } else {
            std::cout << "Directory does not exist.\n";
        }
    } catch (fs::filesystem_error& e) {
        std::cerr << "Error deleting directory: " << e.what() << std::endl;
    }
}

void listFiles(const std::string& path) {
    if (!fs::exists(path)) {
        std::cout << "Directory does not exist.\n";
        return;
    }

    std::cout << "Contents of directory: " << path << std::endl;
    for (const auto& entry : fs::directory_iterator(path)) {
        std::cout << (entry.is_directory() ? "[DIR] " : "[FILE] ") 
                  << entry.path().filename().string() << std::endl;
    }
}

int main() {
    std::string path;
    int choice;

    do {
        std::cout << "\n--- Directory Management System ---\n";
        std::cout << "1. Create Directory\n";
        std::cout << "2. Delete Directory\n";
        std::cout << "3. List Directory Contents\n";
        std::cout << "4. Exit\n";
        std::cout << "Enter choice: ";
        std::cin >> choice;

        std::cin.ignore();  // Clear newline from input buffer

        switch (choice) {
            case 1:
                std::cout << "Enter directory path to create: ";
                std::getline(std::cin, path);
                createDirectory(path);
                break;
            case 2:
                std::cout << "Enter directory path to delete: ";
                std::getline(std::cin, path);
                deleteDirectory(path);
                break;
            case 3:
                std::cout << "Enter directory path to list: ";
                std::getline(std::cin, path);
                listFiles(path);
                break;
            case 4:
                std::cout << "Exiting...\n";
                break;
            default:
                std::cout << "Invalid choice.\n";
        }

    } while (choice != 4);

    return 0;
}
