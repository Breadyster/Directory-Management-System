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
