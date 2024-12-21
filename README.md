# queue-with-linklist

queue.h
#include "Node.h"

class Queue {
private:
    Node* front;
    Node* rear;

public:
    Queue();
    bool isEmpty();
    void enQueue(int data, int key);
    void deQueue();
    void print();
    void sort();
    void swap(Node* current, Node* next);
};

Node.h
#include <iostream>
using namespace std;

class Node {
private:
    int data;
    int key;
    Node* next;

public:
    Node() {
        data = 0;
        key = 0;
        next = NULL;
    }
    Node(int data, int key) {
        this->data = data;
        this->key = key;
        this->next = NULL;
    }
    void setData(int data) {
        this->data = data;
    }
    void setKey(int key) {
        this->key = key;
    }
    void setNext(Node* next) {
        this->next = next;
    }
    int getData() {
        return data;
    }
    int getKey() {
        return key;
    }
    Node* getNext() {
        return next;
    }
};

imp

#include "Queue.h"
#include <iostream>
using namespace std;

Queue::Queue() {
    front = NULL;
    rear = NULL;
}

bool Queue::isEmpty() {
    return front == NULL;
}

void Queue::deQueue() {
    if (isEmpty()) {
        cout << "Queue is empty. Cannot dequeue." << endl;
        return;
    }
    Node* temp = front;
    front = front->getNext();
    if (front == NULL) {
        rear = NULL;
    }
    delete temp;
}

void Queue::enQueue(int data, int key) {
    Node* new_Node = new Node(data, key);
    if (isEmpty()) {
        front = new_Node;
        rear = new_Node;
    }
    else {
        rear->setNext(new_Node);
        rear = new_Node;
        sort();
    }
}

void Queue::print() {
    if (isEmpty()) {
        cout << "Queue is empty." << endl;
        return;
    }
    Node* temp = front;
    cout << "\tData\t\tPriority" << endl;
    while (temp != NULL) {
        cout << "\t" << temp->getData() << "\t\t" << temp->getKey() << endl;
        temp = temp->getNext();
    }
}

void Queue::swap(Node* current, Node* next) {
    int tempData = current->getData();
    int tempKey = current->getKey();
    current->setData(next->getData());
    current->setKey(next->getKey());
    next->setData(tempData);
    next->setKey(tempKey);
}

void Queue::sort() {
    if (isEmpty()) {
        return;
    }

    for (Node* temp = front; temp->getNext() != NULL; temp = temp->getNext()) {
        for (Node* temp2 = temp->getNext(); temp2 != NULL; temp2 = temp2->getNext()) {
            if (temp2->getKey() < temp->getKey()) {
                swap(temp, temp2);
            }
        }
    }
}

main

#include "Queue.h"
#include <iostream>
using namespace std;

void displayMenu() {
    cout << "\n----- Priority Queue Menu -----\n";
    cout << "1. Enqueue (Insert)\n";
    cout << "2. Dequeue (Remove)\n";
    cout << "3. Display Queue\n";
    cout << "4. Exit\n";
    cout << "Enter your choice: ";
}

int main() {
    Queue queue;
    int choice, data, priority;

    do {
        displayMenu();
        cin >> choice;

        switch (choice) {
        case 1:
            cout << "Enter data: ";
            cin >> data;
            cout << "Enter priority: ";
            cin >> priority;
            queue.enQueue(data, priority);
            cout << "Element " << data << " with priority " << priority << " added to the queue.\n";
            break;

        case 2:
            if (queue.isEmpty()) {
                cout << "Queue is empty. Cannot dequeue.\n";
            }
            else {
                queue.deQueue();
                cout << "Element removed from the queue.\n";
            }
            break;

        case 3:
            cout << "Current Queue:\n";
            queue.print();
            break;

        case 4:
            cout << "Exiting program...\n";
            break;

        default:
            cout << "Invalid choice! Please try again.\n";
        }
    } while (choice != 4);

    return 0;
}
