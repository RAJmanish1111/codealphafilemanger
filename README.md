#include <iostream>
#include <fstream>
#include <sstream>
#include <string>
#include <vector>
#include <filesystem>
using namespace std;
namespace fs = filesystem;

void listDirectory(const string& path) {
    for (const auto& entry : fs::directory_iterator(path)) {
        cout << entry.path().filename() << endl;
    }
}

void createDirectory(const string& path) {
    if (fs::create_directory(path)) {
        cout << "Directory created successfully." << endl;
    } else  {
    cout << "Failed to create directory." << endl;
    }
}

void copyFile(const string& source, const string& destination) {
    fs::copy_file(source, destination, fs::copy_options::overwrite_existing);
    cout << "File copied successfully." << endl;
}

void moveFile(const string& source, const string& destination) {
    fs::rename(source, destination);
    cout << "File moved successfully." << endl;
}

int main() {
    string command;
    string path = ".";
    while (true) {
        cout << "Current directory: " << fs::absolute(path) << endl;
        cout << "Enter command (list, mkdir <name>, cp <source> <destination>, mv <source> <destination>, exit): ";
        getline(std::cin, command);

        istringstream iss(command);
        string cmd;
        iss >> cmd;

        if (cmd == "list") {
            listDirectory(path);
        } else if (cmd == "mkdir") {
            string dirName;
            iss >> dirName;
            createDirectory(path + "/" + dirName);
        } else if (cmd == "cp") {
            string source, destination;
            iss >> source >> destination;
            copyFile(path + "/" + source, path + "/" + destination);
        } else if (cmd == "mv") {
            string source, destination;
            iss >> source >> destination;
            moveFile(path + "/" + source, path + "/" + destination);
        } else if (cmd == "exit") {
            break;
        } else {
            cout << "Invalid command." << endl;
        }
    }

    return 0;
}
