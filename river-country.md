River.h
#pragma once
#include <iostream>
#include <string>
    using namespace std;
    class River {
    private:
        string name;
        int length;

    public:
        River();
        River(const string& n, int l);

        friend istream& operator>>(istream& is, River& r);
        friend ostream& operator<<(ostream& os, const River& r);
        bool operator>(const River& other);

        int getLength();
        const string& getName();
    };


River.cpp
#include "River.h"

River::River() {
    name = "";
    length = 0;
}

River::River(const string& n, int l) {
    name = n;
    length = l;
}

istream& operator>>(istream& is, River& r) {
    is >> r.name >> r.length;
    return is;
}

ostream& operator<<(ostream& os, const River& r) {
    os << r.name << " " << r.length;
    return os;
}

bool River::operator>(const River& o) {
    return length > o.length;
}

int River::getLength() {
    return length;
}
const string& River::getName() {
    return name;
}

Country.h
#pragma once
#include <iostream>
#include <string>

#include "River.h"
    int const MAX_RIVERS = 10;
    class Country {
    private:
        string name;
        River rivers[MAX_RIVERS];
        int riverCount;

    public:
        Country();
        Country(const string& c, int co);

        friend istream& operator>>(istream& is, Country& c);
        friend ostream& operator<<(ostream& os, const Country& c);
        bool hasRiver(const string& name);
        void printLongestRiver();

        const string& getName();
        int getTotalLength();
    };


Country.cpp

#include "Country.h"

Country::Country() {
    name = "";
    int riverCount = 0;
}

Country::Country(const string& c, int co) {
    name = c;
    riverCount = co;
    for (int i = 0; i < co; ++i) {
        cin >> rivers[i];
    }
}

istream& operator>>(istream& is, Country& c) {
    is >> c.name >> c.riverCount;

    for (int i = 0; i < c.riverCount; ++i) {
        is >> c.rivers[i];
    }

    return is;
}

ostream& operator<<(ostream& os, const Country& c) {
    os << "Country: " << c.name << ", Number of rivers: " << c.riverCount << endl;
    for (int i = 0; i < c.riverCount; ++i) {
        os << "  " << c.rivers[i] << endl;
    }
    return os;
}

const string& Country::getName() {
    return name;
}
bool Country::hasRiver(const string& r) {
    for (int i = 0; i < riverCount; ++i) {
        if (rivers[i].getName() == r) {
            return true;
        }
    }
    return false;
}

void Country::printLongestRiver() {
    River* longestRiver = &rivers[0];

    for (int i = 1; i < riverCount; ++i) {
        if (rivers[i] > *longestRiver) {
            longestRiver = &rivers[i];
        }
    }

    cout << "Longest river in " << name << ": " << *longestRiver << endl;
}


int Country::getTotalLength() {
    int totalLength = 0;
    for (int i = 0; i < riverCount; ++i) {
        totalLength += rivers[i].getLength();
    }
    return totalLength;
}
Source.cpp

#include <iostream>
#include <fstream>
#include "Country.h"

int main() {
    ifstream file("countries.txt");

    if (!file.is_open()) {
        cerr << "Error opening file." << endl;
        return 1;
    }

    int k;
    file >> k;

    Country* countries = new Country[k];

    for (int i = 0; i < k; ++i) {
        file >> countries[i];
    }

    file.close();

    for (int i = 0; i < k; ++i) {
        cout << countries[i];
    }

    Country* maxLengthCountry = nullptr;
    int maxLength = 0;

    for (int i = 0; i < k; ++i) {
        int currentTotalLength = countries[i].getTotalLength();
        if (currentTotalLength > maxLength) {
            maxLength = currentTotalLength;
            maxLengthCountry = &countries[i];
        }
    }

    if (maxLengthCountry) {
        std::cout << "Country with maximum total river length: " << maxLengthCountry->getName() << std::endl;
    }

    string name;
    cout << "Enter the name of the river: ";
    cin >> name;

    cout << "Countries where the river '" << name << "' flows:" << endl;
    for (int i = 0; i < k; ++i) {
        if (countries[i].hasRiver(name)) {
            cout << countries[i].getName() << endl;
        }
    }



    string name1;
    cout << "Enter the name of the country to find the longest river: ";
    cin >> name1;

    for (int i = 0; i < k; ++i) {
        if (countries[i].getName() == name1) {
            countries[i].printLongestRiver();
            break;
        }
    }

    delete[] countries;

    return 0;
}
txt
4
Ukraine
3
Dnipro 3079
Dnister 1362
Danube 2850
Poland
4
Dnipro 840
Severn 354
Vistula 1049 
Warta 808
France 
3
Loire 1006
Garonne 529 
Dnipro 925 
UK
0
