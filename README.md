# day9
day9
Creating a mini-search engine in C++ is a great project to practice file handling, data structures, and algorithm design. Below is a step-by-step guide to implementing a mini-search engine with full code and explanations.

### Project Overview:
The mini-search engine will:
1. **Index Text Files**: Read and store the contents of multiple text files.
2. **Search for Keywords**: Allow the user to search for specific keywords across the indexed files.
3. **Rank Search Results**: Rank the search results based on keyword relevance (e.g., frequency of occurrence).
4. **Display Results**: Show the files where the keyword appears and their relevance scores.

### Step-by-Step Implementation:

#### Step 1: Include Necessary Headers
```cpp
#include <iostream>
#include <fstream>
#include <string>
#include <map>
#include <vector>
#include <sstream>
#include <algorithm>
```

#### Step 2: Define a Structure to Store File Data
We'll use a structure to store the filename and the frequency of the keyword in that file.
```cpp
struct FileData {
    std::string filename;
    int frequency;
};
```

#### Step 3: Function to Index Files
This function reads all the files and stores the keywords and their occurrences in a map.
```cpp
void indexFiles(const std::vector<std::string>& files, std::map<std::string, std::vector<FileData>>& index) {
    for (const auto& file : files) {
        std::ifstream infile(file);
        if (!infile) {
            std::cerr << "Could not open file: " << file << std::endl;
            continue;
        }

        std::string word;
        std::map<std::string, int> wordCount;

        while (infile >> word) {
            // Convert to lowercase to make the search case-insensitive
            std::transform(word.begin(), word.end(), word.begin(), ::tolower);
            wordCount[word]++;
        }

        for (const auto& entry : wordCount) {
            index[entry.first].push_back({file, entry.second});
        }
    }
}
```

#### Step 4: Function to Search for Keywords
This function takes a keyword, searches it in the index, and returns the matching files and their relevance.
```cpp
std::vector<FileData> searchKeyword(const std::string& keyword, const std::map<std::string, std::vector<FileData>>& index) {
    std::string lowerKeyword = keyword;
    std::transform(lowerKeyword.begin(), lowerKeyword.end(), lowerKeyword.begin(), ::tolower);

    if (index.find(lowerKeyword) != index.end()) {
        return index.at(lowerKeyword);
    } else {
        return {}; // Return an empty vector if the keyword is not found
    }
}
```

#### Step 5: Function to Display Search Results
This function sorts the search results based on the frequency and displays them.
```cpp
void displayResults(const std::vector<FileData>& results) {
    if (results.empty()) {
        std::cout << "No results found." << std::endl;
        return;
    }

    std::vector<FileData> sortedResults = results;
    std::sort(sortedResults.begin(), sortedResults.end(), [](const FileData& a, const FileData& b) {
        return b.frequency < a.frequency;
    });

    std::cout << "Search Results: " << std::endl;
    for (const auto& result : sortedResults) {
        std::cout << "File: " << result.filename << " | Frequency: " << result.frequency << std::endl;
    }
}
```

#### Step 6: Main Function
This is where the program runs, taking user input, indexing files, and performing searches.
```cpp
int main() {
    std::vector<std::string> files = {"file1.txt", "file2.txt", "file3.txt"};
    std::map<std::string, std::vector<FileData>> index;

    // Index the files
    indexFiles(files, index);

    std::string keyword;
    while (true) {
        std::cout << "Enter keyword to search (or type 'exit' to quit): ";
        std::cin >> keyword;
        if (keyword == "exit") {
            break;
        }

        // Search for the keyword
        std::vector<FileData> results = searchKeyword(keyword, index);

        // Display the results
        displayResults(results);
    }

    return 0;
}
```

### How the Code Works:

1. **Indexing Files**:
   - `indexFiles` reads each file, counts the occurrences of each word, and stores this data in a `std::map` where the key is the keyword, and the value is a vector of `FileData` structs containing the filename and frequency.

2. **Searching Keywords**:
   - `searchKeyword` searches for the given keyword in the index and returns the files where the keyword was found along with the frequency of occurrence.

3. **Displaying Results**:
   - `displayResults` sorts the search results by frequency and prints them out in descending order.

### How to Run the Program:
1. Create some text files named `file1.txt`, `file2.txt`, and `file3.txt` in the same directory as the program.
2. Populate these files with text content.
3. Compile and run the C++ program.
4. Enter keywords to search for within the indexed files.

This simple search engine demonstrates how you can build more complex search functionalities and data indexing systems using C++.

To post this project on GitHub, you'll want to structure it in a way that's clear and easy for others to understand. Here's how you can redesign the project with GitHub in mind, including a README file, directory structure, and well-commented code.

### Project Structure
```
Mini-Search-Engine/
├── src/
│   └── main.cpp
├── data/
│   ├── file1.txt
│   ├── file2.txt
│   └── file3.txt
├── README.md
└── .gitignore
```

### `main.cpp`
Place the main C++ code in the `src/` directory.

```cpp
// src/main.cpp

#include <iostream>
#include <fstream>
#include <string>
#include <map>
#include <vector>
#include <sstream>
#include <algorithm>

// Struct to store file name and frequency of keyword
struct FileData {
    std::string filename;
    int frequency;
};

// Function to index the text files
void indexFiles(const std::vector<std::string>& files, std::map<std::string, std::vector<FileData>>& index) {
    for (const auto& file : files) {
        std::ifstream infile("../data/" + file);
        if (!infile) {
            std::cerr << "Could not open file: " << file << std::endl;
            continue;
        }

        std::string word;
        std::map<std::string, int> wordCount;

        while (infile >> word) {
            // Convert to lowercase to make the search case-insensitive
            std::transform(word.begin(), word.end(), word.begin(), ::tolower);
            wordCount[word]++;
        }

        for (const auto& entry : wordCount) {
            index[entry.first].push_back({file, entry.second});
        }
    }
}

// Function to search for a keyword in the index
std::vector<FileData> searchKeyword(const std::string& keyword, const std::map<std::string, std::vector<FileData>>& index) {
    std::string lowerKeyword = keyword;
    std::transform(lowerKeyword.begin(), lowerKeyword.end(), lowerKeyword.begin(), ::tolower);

    if (index.find(lowerKeyword) != index.end()) {
        return index.at(lowerKeyword);
    } else {
        return {}; // Return an empty vector if the keyword is not found
    }
}

// Function to display search results
void displayResults(const std::vector<FileData>& results) {
    if (results.empty()) {
        std::cout << "No results found." << std::endl;
        return;
    }

    // Sort the results by frequency in descending order
    std::vector<FileData> sortedResults = results;
    std::sort(sortedResults.begin(), sortedResults.end(), [](const FileData& a, const FileData& b) {
        return b.frequency < a.frequency;
    });

    std::cout << "Search Results: " << std::endl;
    for (const auto& result : sortedResults) {
        std::cout << "File: " << result.filename << " | Frequency: " << result.frequency << std::endl;
    }
}

int main() {
    std::vector<std::string> files = {"file1.txt", "file2.txt", "file3.txt"};
    std::map<std::string, std::vector<FileData>> index;

    // Index the files
    indexFiles(files, index);

    std::string keyword;
    while (true) {
        std::cout << "Enter keyword to search (or type 'exit' to quit): ";
        std::cin >> keyword;
        if (keyword == "exit") {
            break;
        }

        // Search for the keyword
        std::vector<FileData> results = searchKeyword(keyword, index);

        // Display the results
        displayResults(results);
    }

    return 0;
}
