#include <iostream>
#include <string>
using namespace std;

struct TaskNode {
    int id;
    string taskName;
    int priority; // 1-high, 2-medium, 3-low
    TaskNode* next;
};

class TaskManager {
private:
    TaskNode* head;
    int taskCounter;

public:
    TaskManager() {
        head = nullptr;
        taskCounter = 1;
    }

    void addTask() {
        TaskNode* newTask = new TaskNode;
        newTask->id = taskCounter++;
        cout << "Enter task name: ";
        cin.ignore();
        getline(cin, newTask->taskName);
        cout << "Enter priority (1-High, 2-Medium, 3-Low): ";
        cin >> newTask->priority;
        newTask->next = nullptr;

        // Insert based on priority (Higher priority at front)
        if (!head || newTask->priority < head->priority) {
            newTask->next = head;
            head = newTask;
        } else {
            TaskNode* temp = head;
            while (temp->next && temp->next->priority <= newTask->priority)
                temp = temp->next;
            newTask->next = temp->next;
            temp->next = newTask;
        }

        cout << "Task added successfully!\n";
    }

    void viewTasks() {
        if (!head) {
            cout << "No tasks available!\n";
            return;
        }
        TaskNode* temp = head;
        cout << "\nID\tPriority\tTask Name\n";
        cout << "---------------------------------\n";
        while (temp) {
            string prio = (temp->priority==1?"High":(temp->priority==2?"Medium":"Low"));
            cout << temp->id << "\t" << prio << "\t\t" << temp->taskName << endl;
            temp = temp->next;
        }
    }

    void deleteTask(int id) {
        if (!head) { cout << "No tasks to delete!\n"; return; }
        if (head->id == id) {
            TaskNode* temp = head;
            head = head->next;
            delete temp;
            cout << "Task deleted successfully!\n";
            return;
        }
        TaskNode* temp = head;
        while (temp->next && temp->next->id != id)
            temp = temp->next;
        if (temp->next) {
            TaskNode* del = temp->next;
            temp->next = temp->next->next;
            delete del;
            cout << "Task deleted successfully!\n";
        } else
            cout << "Task ID not found!\n";
    }
};

// ------------------- Main Menu -------------------
int main() {
    TaskManager tm;
    int choice, id;

    do {
        cout << "\n=== Daily Task Manager ===\n";
        cout << "1. Add Task\n";
        cout << "2. View Tasks\n";
        cout << "3. Delete Task\n";
        cout << "4. Exit\n";
        cout << "Enter choice: ";
        cin >> choice;

        switch(choice) {
            case 1: tm.addTask(); break;
            case 2: tm.viewTasks(); break;
            case 3: 
                cout << "Enter Task ID to delete: "; 
                cin >> id;
                tm.deleteTask(id);
                break;
            case 4: cout << "Exiting...\n"; break;
            default: cout << "Invalid choice!\n";
        }

    } while(choice != 4);

    return 0;
}
