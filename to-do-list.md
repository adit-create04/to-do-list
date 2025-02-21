# to-do-list
#include <iostream>
#include <vector>
#include <fstream>

using namespace std;

struct Task {
    string description;
    bool completed;
};

vector<Task> tasks;

void saveTasks() {
    ofstream file("tasks.txt");
    for (const auto &task : tasks) {
        file << task.description << "|" << task.completed << endl;
    }
    file.close();
}

void loadTasks() {
    ifstream file("tasks.txt");
    string description;
    bool completed;
    tasks.clear();
    while (getline(file, description, '|') && file >> completed) {
        file.ignore();  // Ignore newline
        tasks.push_back({description, completed});
    }
    file.close();
}

void addTask() {
    cout << "Enter task description: ";
    string desc;
    cin.ignore();
    getline(cin, desc);
    tasks.push_back({desc, false});
    saveTasks();
}

void viewTasks() {
    if (tasks.empty()) {
        cout << "No tasks available!\n";
        return;
    }
    for (size_t i = 0; i < tasks.size(); i++) {
        cout << i + 1 << ". [" << (tasks[i].completed ? "✔" : " ") << "] " << tasks[i].description << endl;
    }
}

void completeTask() {
    viewTasks();
    cout << "Enter task number to mark as complete: ";
    int num;
    cin >> num;
    if (num > 0 && num <= tasks.size()) {
        tasks[num - 1].completed = true;
        saveTasks();
    } else {
        cout << "Invalid task number!\n";
    }
}

void deleteTask() {
    viewTasks();
    cout << "Enter task number to delete: ";
    int num;
    cin >> num;
    if (num > 0 && num <= tasks.size()) {
        tasks.erase(tasks.begin() + num - 1);
        saveTasks();
    } else {
        cout << "Invalid task number!\n";
    }
}

int main() {
    loadTasks();
    int choice;
    do {
        cout << "\nTo-Do List Menu:\n";
        cout << "1. Add Task\n2. View Tasks\n3. Complete Task\n4. Delete Task\n5. Exit\n";
        cout << "Enter choice: ";
        cin >> choice;
        switch (choice) {
            case 1: addTask(); break;
            case 2: viewTasks(); break;
            case 3: completeTask(); break;
            case 4: deleteTask(); break;
            case 5: cout << "Exiting...\n"; break;
            default: cout << "Invalid choice!\n";
        }
    } while (choice != 5);

    return 0;
}
