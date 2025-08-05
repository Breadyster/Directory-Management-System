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
