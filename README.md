# Online-voting-for-electing-Class-Representative#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define MAX_CANDIDATES 3
#define MAX_VOTERS 130

typedef struct {
    char name[50];
    int votes;
} Candidate;

typedef struct {
    char username[50];
    int voted;
} Voter;

Candidate candidates[MAX_CANDIDATES];
Voter voters[MAX_VOTERS];
int userCount = 0;

void initializeCandidates() {
    strcpy(candidates[0].name, "HEMANTH ");
    candidates[0].votes = 0;

    strcpy(candidates[1].name, "VISHNU");
    candidates[1].votes = 0;
    
   strcpy(candidates[2].name, "NARSHIMHA");
    candidates[2].votes = 0;
   
}

int registerUser() {
    if (userCount < MAX_VOTERS) {
        printf("Enter your username: ");
        scanf("%s", voters[userCount].username);

        voters[userCount].voted = 0;
        userCount++;

        printf("Registration successful!\n");
        return 1;
    } else {
        printf("Maximum number of users reached.\n");
        return 0;
    }
}

int login() {
    char username[50];

    printf("Enter your username: ");
    scanf("%s", username);

    for (int i = 0; i < userCount; i++) {
        if (strcmp(voters[i].username, username) == 0) {
            if (!voters[i].voted) {
                printf("Login successful!\n");
                return i; 
            } else {
                printf("You have already voted.\n");
                return -1;
            }
        }
    }

    printf("Invalid username.\n");
    return -1; 
}

void viewCandidates() {
    printf("\nCandidates:\n");
    for (int i = 0; i < MAX_CANDIDATES; i++) {
        printf("%d. %s\n", i + 1, candidates[i].name);
    }
}

void vote(int userIndex) {
    int choice;

    printf("Enter the number of the candidate you want to vote for: ");
    scanf("%d", &choice);

    if (choice >= 1 && choice <= MAX_CANDIDATES) {
        candidates[choice - 1].votes++;
        printf("Vote for %s recorded successfully!\n", candidates[choice - 1].name);

       
        voters[userIndex].voted = 1;
    } else {
        printf("Invalid choice. Please try again.\n");
    }
}

void viewVotes() {
    printf("\nVotes:\n");
    for (int i = 0; i < MAX_CANDIDATES; i++) {
        printf("%s: %d votes\n", candidates[i].name, candidates[i].votes);
    }
}

int main() {
    int currentUserIndex = -1;

    initializeCandidates();

    while (1) {
        printf("\n1. Register\n");
        printf("2. Login\n");
        printf("3. View Candidates\n");
        printf("4. Vote\n");
        printf("5. View Votes\n");
        printf("6. Exit\n");
        printf("Enter your choice: ");

        int choice;
        scanf("%d", &choice);

        switch (choice) {
            case 1:
                registerUser();
                break;
            case 2:
                currentUserIndex = login();
                break;
            case 3:
                viewCandidates();
                break;
            case 4:
                if (currentUserIndex != -1) {
                    if (!voters[currentUserIndex].voted) {
                        vote(currentUserIndex);
                    } else {
                        printf("You have already voted.\n");
                    }
                } else {
                    printf("You need to login first.\n");
                }
                break;
            case 5:
                viewVotes();
                break;
            case 6:
                printf("Exiting the Voting System. Thank you!\n");
                exit(0);
            default:
                printf("Invalid choice. Please try again.\n");
        }
    }

    return 0;
}
