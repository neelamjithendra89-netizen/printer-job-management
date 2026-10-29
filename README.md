//printer-job-management
#include <stdio.h>
#define MAX 100

struct PrintJob {
    int id;
    char name[50];
    int pages;
};

struct PrintJob queue[MAX];
int front = 0, rear = -1, count = 0;

void addJob() {
    struct PrintJob job;

    if (count == MAX) {
        printf("Print queue is full!\n");
        return;
    }

    printf("Enter Job ID: ");
    scanf("%d", &job.id);

    for (int i = 0; i < count; i++) {
        int index = (front + i) % MAX;
        if (queue[index].id == job.id) {
            printf("Job ID already exists!\n");
            return;
        }
    }

    printf("Enter Document Name: ");
    scanf(" %49[^\n]", job.name);

    printf("Enter Number of Pages: ");
    scanf("%d", &job.pages);

    if (job.pages <= 0) {
        printf("Invalid number of pages!\n");
        return;
    }

    rear = (rear + 1) % MAX;
    queue[rear] = job;
    count++;

    printf("Print job added successfully!\n");
}

void processJob() {
    if (count == 0) {
        printf("No print jobs waiting.\n");
        return;
    }

    printf("\nProcessing Print Job:\n");
    printf("Job ID: %d\n", queue[front].id);
    printf("Document: %s\n", queue[front].name);
    printf("Pages: %d\n", queue[front].pages);

    front = (front + 1) % MAX;
    count--;

    if (count == 0)
        rear = front - 1;

    printf("Print job completed!\n");
}

void displayJobs() {
    if (count == 0) {
        printf("No pending print jobs.\n");
        return;
    }

    printf("\n--- Pending Print Jobs ---\n");

    for (int i = 0; i < count; i++) {
        int index = (front + i) % MAX;

        printf("\nJob ID: %d\n", queue[index].id);
        printf("Document: %s\n", queue[index].name);
        printf("Pages: %d\n", queue[index].pages);
    }
}

void searchJob() {
    int id, found = 0;

    printf("Enter Job ID to search: ");
    scanf("%d", &id);

    for (int i = 0; i < count; i++) {
        int index = (front + i) % MAX;

        if (queue[index].id == id) {
            printf("Print job found!\n");
            printf("Job ID: %d\n", queue[index].id);
            printf("Document: %s\n", queue[index].name);
            printf("Pages: %d\n", queue[index].pages);
            found = 1;
            break;
        }
    }

    if (!found)
        printf("Print job not found.\n");
}

void countJobs() {
    printf("Total pending jobs: %d\n", count);
}

int main() {
    int choice;

    do {
        printf("\n=== PRINTER JOB MANAGEMENT SYSTEM ===\n");
        printf("1. Add Print Job\n");
        printf("2. Process Next Job\n");
        printf("3. Display Pending Jobs\n");
        printf("4. Search Print Job\n");
        printf("5. Count Pending Jobs\n");
        printf("6. Exit\n");
        printf("Enter your choice: ");

        if (scanf("%d", &choice) != 1) {
            printf("Invalid input!\n");
            return 1;
        }

        switch (choice) {
            case 1: addJob(); break;
            case 2: processJob(); break;
            case 3: displayJobs(); break;
            case 4: searchJob(); break;
            case 5: countJobs(); break;
            case 6: printf("Exiting program.\n"); break;
            default: printf("Invalid choice!\n");
        }
    } while (choice != 6);

    return 0;
}