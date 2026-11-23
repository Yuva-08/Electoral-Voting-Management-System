#include <stdio.h>
#include <stdlib.h>
#include <string.h>

// Candidate structure
struct Candidate {
    int id;
    char name[50];
    int votes;
};

// Voter linked list
struct Voter {
    int voterID;
    int hasVoted;
    struct Voter* next;
};

struct Voter* head = NULL;

// Candidates array
struct Candidate candidates[3] = {
    {1, "Alice", 0},
    {2, "Bob", 0},
    {3, "Charlie", 0}
};

// Add voter
void addVoter(int id) {
    struct Voter* newVoter = (struct Voter*)malloc(sizeof(struct Voter));
    newVoter->voterID = id;
    newVoter->hasVoted = 0;
    newVoter->next = head;
    head = newVoter;
}

// Find voter
struct Voter* findVoter(int id) {
    struct Voter* temp = head;
    while (temp != NULL) {
        if (temp->voterID == id)
            return temp;
        temp = temp->next;
    }
    return NULL;
}

// Display candidates
void displayCandidates() {
    printf("\n--- Candidates ---\n");
    for (int i = 0; i < 3; i++) {
        printf("%d. %s\n", candidates[i].id, candidates[i].name);
    }
}

// Voting function
void vote() {
    int id, choice;
    printf("Enter Voter ID: ");
    scanf("%d", &id);

    struct Voter* voter = findVoter(id);

    if (voter == NULL) {
        printf("❌ Voter not registered!\n");
        return;
    }

    if (voter->hasVoted) {
        printf("❌ You have already voted!\n");
        return;
    }

    displayCandidates();
    printf("Enter your choice: ");
    scanf("%d", &choice);

    if (choice < 1 || choice > 3) {
        printf("❌ Invalid choice!\n");
        return;
    }

    candidates[choice - 1].votes++;
    voter->hasVoted = 1;

    printf("✅ Vote cast successfully!\n");
}

// Show results
void showResults() {
    printf("\n--- Election Results ---\n");

    int maxVotes = -1;
    int winnerIndex = -1;

    for (int i = 0; i < 3; i++) {
        printf("%s: %d votes\n", candidates[i].name, candidates[i].votes);

        if (candidates[i].votes > maxVotes) {
            maxVotes = candidates[i].votes;
            winnerIndex = i;
        }
    }

    if (winnerIndex != -1) {
        printf("\n🏆 Winner: %s\n", candidates[winnerIndex].name);
    } else {
        printf("No votes cast.\n");
    }
}

// Main menu
int main() {
    int choice, id;

    // Pre-register voters
    addVoter(101);
    addVoter(102);
    addVoter(103);

    while (1) {
        printf("\n===== Voting System =====\n");
        printf("1. Register Voter\n");
        printf("2. Vote\n");
        printf("3. Show Results\n");
        printf("4. Exit\n");
        printf("Enter choice: ");
        scanf("%d", &choice);

        switch (choice) {
            case 1:
                printf("Enter new Voter ID: ");
                scanf("%d", &id);
                addVoter(id);
                printf("✅ Voter registered successfully!\n");
                break;

            case 2:
                vote();
                break;

            case 3:
                showResults();
                break;

            case 4:
                printf("Exiting...\n");
                exit(0);

            default:
                printf("❌ Invalid choice!\n");
        }
    }

    return 0;
}