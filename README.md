//FULL CODING FOR DATA STRUCTURE
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

// Define a structure for a contact
typedef struct Contact {
    char name[50];
    char phone[15];
    char email[50];
    struct Contact *next;
} Contact;

void addContact(Contact **head) {
    Contact *newContact = (Contact *)malloc(sizeof(Contact));
    if (!newContact) {
        printf("Memory allocation failed\n");
        return;
    }
    printf("Enter name: ");
    scanf("%s", newContact->name);
    printf("Enter phone: ");
    scanf("%s", newContact->phone);
    printf("Enter email: ");
    scanf("%s", newContact->email);
    newContact->next = *head;
    *head = newContact;
}
void displayContacts(Contact *head) {
    Contact *current = head;
    if (!current) {
        printf("No contacts found.\n");
        return;
    }
    while (current) {
        printf("Name: %s\n", current->name);
        printf("Phone: %s\n", current->phone);
        printf("Email: %s\n", current->email);
        printf("-----------\n");
        current = current->next;
    }
}

Contact* searchContact(Contact *head, const char *name) {
    Contact *current = head;
    while (current) {
        if (strcmp(current->name, name) == 0) {
            return current;
        }
        current = current->next;
    }
    return NULL;
}
void deleteContact(Contact **head, const char *name) {
    Contact *current = *head, *prev = NULL;
    while (current && strcmp(current->name, name) != 0) {
        prev = current;
        current = current->next;
    }
    if (!current) {
        printf("Contact not found.\n");
        return;
    }
    if (prev) {
        prev->next = current->next;
    } else {
        *head = current->next;
    }
    free(current);
    printf("Contact deleted successfully.\n");
}

void freeContacts(Contact *head) {
    Contact *tmp;
    while (head) {
        tmp = head;
        head = head->next;
        free(tmp);
    }
}
int main() {
    Contact *head = NULL;
    int choice;
    char name[50];

    while (1) {
        printf("1. Add Contact\n");
        printf("2. Display Contacts\n");
        printf("3. Search Contact\n");
        printf("4. Delete Contact\n");
        printf("5. Exit\n");
        printf("Enter your choice: ");
        scanf("%d", &choice);

        switch (choice) {
            case 1:
                addContact(&head);
                break;
            case 2:
                displayContacts(head);
                break;
            case 3:
                printf("Enter name to search: ");
                scanf("%s", name);
                Contact *contact = searchContact(head, name);
                                if (contact) {
                    printf("Contact found:\n");
                    printf("Name: %s\n", contact->name);
                    printf("Phone: %s\n", contact->phone);
                    printf("Email: %s\n", contact->email);
                } else {
                    printf("Contact not found.\n");
                }
                break;
            case 4:
                printf("Enter name to delete: ");
                scanf("%s", name);
                deleteContact(&head, name);
                break;
            case 5:
                freeContacts(head);
                exit(0);
            default:
                printf("Invalid choice, try again.\n");
        }
    }

    return 0;
}
