# project-1
Voting system
#include <iostream>
using namespace std;
class Candidate {
public:
    int candidateID;
    char name[20];
    void setCandidate(int id, const char* n) {
        candidateID = id;
        int i = 0;
        while (n[i] != '\0') {
            name[i] = n[i];
            i++;
        }
        name[i] = '\0';
    }
    void display() {
        cout << "ID: " << candidateID << ", Name: " << name << endl;
    }
};
class Voter {
public:
    char voterID[20];
    char password[20];
    bool hasVoted;
    void registerVoter(const char* id, const char* pass) {
        int i = 0;
        while (id[i] != '\0') {
            voterID[i] = id[i];
            i++;
        }
        voterID[i] = '\0';
        i = 0;
        while (pass[i] != '\0') {
            password[i] = pass[i];
            i++;
        }
        password[i] = '\0';
        hasVoted = false;
    }
    bool login(const char* id, const char* pass) {
        int i = 0;
        while (voterID[i] != '\0' && id[i] != '\0' && voterID[i] == id[i]) i++;
        if (voterID[i] != '\0' || id[i] != '\0') return false;
        i = 0;
        while (password[i] != '\0' && pass[i] != '\0' && password[i] == pass[i]) i++;
        if (password[i] != '\0' || pass[i] != '\0') return false;
        return true;
    }
};
Voter allVoters[100];
int voterCount = 0;
bool alreadyVoted(const char* id) {
    for (int i = 0; i < voterCount; i++) {
        int j = 0;
        while (allVoters[i].voterID[j] != '\0' && id[j] != '\0' && allVoters[i].voterID[j] == id[j]) j++;
        if (allVoters[i].voterID[j] == '\0' && id[j] == '\0' && allVoters[i].hasVoted) {
            return true;
        }
    }
    return false;
}
int main() {
    Candidate c1, c2, c3;
    c1.setCandidate(1, "Anas");
    c2.setCandidate(2, "Ahmad");
    c3.setCandidate(3, "Amjad");
    int votes1 = 0, votes2 = 0, votes3 = 0;
    while (true) {
        cout << "\n--- Voter Registration ---\n";
        Voter voter;
        char id[20], pass[20];
        cout << "Enter Voter ID: ";
        cin.getline(id, 20);
        if (alreadyVoted(id)) {
            cout << "You have already voted!\n";
            continue;
        }
        cout << "Set Password: ";
        cin.getline(pass, 20);
        voter.registerVoter(id, pass);
        allVoters[voterCount++] = voter;
        cout << "\n--- Login ---\n";
        char loginID[20], loginPass[20];
        cout << "Enter Voter ID: ";
        cin.getline(loginID, 20);
        cout << "Enter Password: ";
        cin.getline(loginPass, 20);
        if (!voter.login(loginID, loginPass)) {
            cout << "Login failed!\n";
            continue;
        }
        cout << "\n--- Candidates ---\n";
        c1.display();
        c2.display();
        c3.display();
        int choice;
        cout << "Enter Candidate ID to vote for: ";
        cin >> choice;
        cin.ignore();
        if (choice == 1) votes1++;
        else if (choice == 2) votes2++;
        else if (choice == 3) votes3++;
        else cout << "Invalid Candidate ID!\n";
        allVoters[voterCount - 1].hasVoted = true;
        char cont[5];
        cout << "\nDo you want to allow another voter? (yes/no): ";
        cin.getline(cont, 5);
        if (cont[0] == 'n' || cont[0] == 'N') {
            break;
        }
    }
    cout << "\n--- Election Results ---\n";
    cout << "Anas got " << votes1 << " votes\n";
    cout << "Ahmad got " << votes2 << " votes\n";
    cout << "Amjad got " << votes3 << " votes\n";
    return 0;
}

