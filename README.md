# Student-Record-Management
Student Record Management System using Linked List in C
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

struct student
{
    int rollno;
    char name[50];
    float percentage;
    struct student *next;
};

struct student *head = NULL;

/* Add Student */
void add_student()
{
    struct student *newnode, *temp;
    int roll = 1;

    newnode = (struct student *)malloc(sizeof(struct student));

    if (newnode == NULL)
    {
        printf("Memory allocation failed!\n");
        return;
    }

    /* Find smallest available roll number */
    temp = head;

    while (temp != NULL)
    {
        if (temp->rollno == roll)
        {
            roll++;
            temp = head;
        }
        else
        {
            temp = temp->next;
        }
    }

    newnode->rollno = roll;

    printf("Enter name: ");
    scanf(" %[^\n]", newnode->name);

    printf("Enter percentage: ");
    scanf("%f", &newnode->percentage);

    if (newnode->percentage < 0 || newnode->percentage > 100)
    {
        printf("Invalid percentage!\n");
        free(newnode);
        return;
    }

    newnode->next = NULL;

    if (head == NULL)
    {
        head = newnode;
    }
    else
    {
        temp = head;

        while (temp->next != NULL)
            temp = temp->next;

        temp->next = newnode;
    }

    printf("Student added successfully!\n");
}

/* Show Students */
void show_students()
{
    struct student *temp = head;

    if (head == NULL)
    {
        printf("No student records available.\n");
        return;
    }

    printf("\n----------------------------------------\n");
    printf("Roll No\tName\t\tPercentage\n");
    printf("----------------------------------------\n");

    while (temp != NULL)
    {
        printf("%d\t%-15s %.2f\n",
               temp->rollno,
               temp->name,
               temp->percentage);

        temp = temp->next;
    }

    printf("----------------------------------------\n");
}

/* Delete Student */
void delete_student()
{
    struct student *temp, *prev;
    int roll;

    if (head == NULL)
    {
        printf("List is empty!\n");
        return;
    }

    printf("Enter roll number to delete: ");
    scanf("%d", &roll);

    temp = head;
    prev = NULL;

    while (temp != NULL && temp->rollno != roll)
    {
        prev = temp;
        temp = temp->next;
    }

    if (temp == NULL)
    {
        printf("Student not found!\n");
        return;
    }

    if (prev == NULL)
        head = temp->next;
    else
        prev->next = temp->next;

    free(temp);

    printf("Student deleted successfully!\n");
}

/* Modify Student */
void modify_student()
{
    struct student *temp;
    int roll;

    printf("Enter roll number to modify: ");
    scanf("%d", &roll);

    temp = head;

    while (temp != NULL)
    {
        if (temp->rollno == roll)
        {
            printf("Current Name: %s\n", temp->name);
            printf("Enter new name: ");
            scanf(" %[^\n]", temp->name);

            printf("Enter new percentage: ");
            scanf("%f", &temp->percentage);

            if (temp->percentage < 0 || temp->percentage > 100)
            {
                printf("Invalid percentage!\n");
                return;
            }

            printf("Student modified successfully!\n");
            return;
        }

        temp = temp->next;
    }

    printf("Student not found!\n");
}

/* Reverse List */
void reverse_list()
{
    struct student *prev = NULL;
    struct student *current = head;
    struct student *next;

    while (current != NULL)
    {
        next = current->next;
        current->next = prev;
        prev = current;
        current = next;
    }

    head = prev;

    printf("List reversed successfully!\n");
}

/* Delete All */
void delete_all()
{
    struct student *temp;

    while (head != NULL)
    {
        temp = head;
        head = head->next;
        free(temp);
    }

    printf("All records deleted!\n");
}

/* Sort by Percentage */
void sort_percentage()
{
    struct student *i, *j;
    int temp_roll;
    char temp_name[50];
    float temp_percentage;

    for (i = head; i != NULL; i = i->next)
    {
        for (j = i->next; j != NULL; j = j->next)
        {
            if (i->percentage < j->percentage)
            {
                temp_roll = i->rollno;
                i->rollno = j->rollno;
                j->rollno = temp_roll;

                strcpy(temp_name, i->name);
                strcpy(i->name, j->name);
                strcpy(j->name, temp_name);

                temp_percentage = i->percentage;
                i->percentage = j->percentage;
                j->percentage = temp_percentage;
            }
        }
    }

    printf("Sorted by percentage!\n");
}

/* Main */
int main()
{
    char choice;

    while (1)
    {
        printf("\n******** STUDENT RECORD MENU ********\n");
        printf("a : Add Student\n");
        printf("d : Delete Student\n");
        printf("s : Show Students\n");
        printf("m : Modify Student\n");
        printf("t : Sort by Percentage\n");
        printf("r : Reverse List\n");
        printf("l : Delete All\n");
        printf("e : Exit\n");
        printf("Enter choice: ");

        scanf(" %c", &choice);

        switch (choice)
        {
            case 'a':
            case 'A':
                add_student();
                break;

            case 'd':
            case 'D':
                delete_student();
                break;

            case 's':
            case 'S':
                show_students();
                break;

            case 'm':
            case 'M':
                modify_student();
                break;

            case 't':
            case 'T':
                sort_percentage();
                break;

            case 'r':
            case 'R':
                reverse_list();
                break;

            case 'l':
            case 'L':
                delete_all();
                break;

            case 'e':
            case 'E':
                delete_all();
                printf("Program terminated.\n");
                return 0;

            default:
                printf("Invalid choice!\n");
        }
    }

    return 0;
}
