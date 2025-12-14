# Assignment 12

## Title  
**Implementation of Ticket Reservation System for Galaxy Multiplex Using Doubly Circular Linked List**  

The Galaxy Multiplex has 8 rows, with 8 seats in each row. To manage seat availability dynamically, a **doubly circular linked list** is used for each row. An **array stores head pointers** for each row’s linked list. Initially, some seats are assumed to be **randomly booked**. The system provides a flexible way to **display available seats**, **book seats**, and **cancel bookings** as per customer requests.  

---

## Theory  

A **Doubly Circular Linked List (DCLL)** is a variation of a linked list where:  
1. Each node contains a **data field** (here, seat number or status) and two pointers: **prev** and **next**.  
2. The **last node points back to the first node**, forming a circle, which allows **continuous traversal** from any seat.  

**Application in Ticket Reservation:**  
- Each **row of seats** is represented by a separate DCLL.  
- An **array of head pointers** maintains access to all rows efficiently.  
- **Booking a seat** marks the seat as reserved in the corresponding list.  
- **Cancelling a booking** updates the seat status to available.  
- **Displaying seats** traverses the DCLL for each row and shows availability.  

**Advantages of DCLL for this System:**  
- **Efficient Traversal:** Both forward and backward traversal is possible.  
- **Circular Structure:** Easy to loop through all seats repeatedly.  
- **Dynamic Updates:** Seats can be booked or cancelled without shifting elements, unlike arrays.  
- **Scalable:** Adding or modifying rows is straightforward using the head pointer array.  

---
## Code
```
#include <iostream>
#include <cstdlib>
#include <ctime>
using namespace std;

const int ROWS = 8;
const int SEATS_PER_ROW = 8;

struct SeatNode {
    int seatNo;
    bool isBooked;
    SeatNode* next;
    SeatNode* prev;
};


SeatNode* rowHeads[ROWS];

SeatNode* createRow(int rowNumber) {
    SeatNode* head = nullptr;
    SeatNode* tail = nullptr;

    for (int i = 1; i <= SEATS_PER_ROW; ++i) {
        SeatNode* newNode = new SeatNode;
        newNode->seatNo = i;
        newNode->isBooked = rand() % 2;  
        newNode->next = nullptr;
        newNode->prev = nullptr;

        if (!head) {
            head = newNode;
            tail = newNode;
        } else {
            tail->next = newNode;
            newNode->prev = tail;
            tail = newNode;
        }
    }

    
    tail->next = head;
    head->prev = tail;

    return head;
}


void initializeMultiplex() {
    for (int i = 0; i < ROWS; ++i) {
        rowHeads[i] = createRow(i);
    }
}


void displaySeats() {
    cout << "\nCurrent Seat Availability:\n";
    for (int i = 0; i < ROWS; ++i) {
        cout << "Row " << i + 1 << ": ";
        SeatNode* temp = rowHeads[i];
        if (!temp) continue;
        do {
            if (temp->isBooked)
                cout << "[X] ";
            else
                cout << "[" << temp->seatNo << "] ";
            temp = temp->next;
        } while (temp != rowHeads[i]);
        cout << "\n";
    }
}


void bookSeat(int row, int seatNo) {
    if (row < 1 || row > ROWS || seatNo < 1 || seatNo > SEATS_PER_ROW) {
        cout << "Invalid row or seat number.\n";
        return;
    }

    SeatNode* temp = rowHeads[row - 1];
    for (int i = 1; i < seatNo; ++i) {
        temp = temp->next;
    }

    if (temp->isBooked) {
        cout << "Seat " << seatNo << " in Row " << row << " is already booked.\n";
    } else {
        temp->isBooked = true;
        cout << "Seat " << seatNo << " in Row " << row << " successfully booked.\n";
    }
}

void cancelSeat(int row, int seatNo) {
    if (row < 1 || row > ROWS || seatNo < 1 || seatNo > SEATS_PER_ROW) {
        cout << "Invalid row or seat number.\n";
        return;
    }

    SeatNode* temp = rowHeads[row - 1];
    for (int i = 1; i < seatNo; ++i) {
        temp = temp->next;
    }

    if (!temp->isBooked) {
        cout << "Seat " << seatNo << " in Row " << row << " is not currently booked.\n";
    } else {
        temp->isBooked = false;
        cout << "Booking for Seat " << seatNo << " in Row " << row << " has been cancelled.\n";
    }
}

void menu() {
    int choice;
    do {
        cout << "\n====== Galaxy Multiplex Ticket Reservation ======\n";
        cout << "1. Display Available Seats\n";
        cout << "2. Book Seat(s)\n";
        cout << "3. Cancel Seat Booking\n";
        cout << "4. Exit\n";
        cout << "Enter your choice: ";
        cin >> choice;

        if (choice == 1) {
            displaySeats();
        } else if (choice == 2) {
            int row, seatCount;
            cout << "Enter Row Number (1-8): ";
            cin >> row;
            cout << "Enter number of seats to book: ";
            cin >> seatCount;
            for (int i = 0; i < seatCount; ++i) {
                int seatNo;
                cout << "Enter Seat Number to book: ";
                cin >> seatNo;
                bookSeat(row, seatNo);
            }
        } else if (choice == 3) {
            int row, seatNo;
            cout << "Enter Row Number (1-8): ";
            cin >> row;
            cout << "Enter Seat Number to cancel: ";
            cin >> seatNo;
            cancelSeat(row, seatNo);
        } else if (choice == 4) {
            cout << "Exiting the system. Goodbye!\n";
        } else {
            cout << "Invalid choice. Try again.\n";
        }

    } while (choice != 4);
}

int main() {
    srand(time(0)); 
    initializeMultiplex();
    menu();
    return 0;
}

```

## Output
====== Galaxy Multiplex Ticket Reservation ======
1. Display Available Seats
2. Book Seat(s)
3. Cancel Seat Booking
4. Exit
Enter your choice: **1**
Current Seat Availability:
Row 1: [X] [2] [3] [X] [5] [6] [7] [8]
Row 2: [1] [X] [3] [4] [5] [X] [7] [8]
Row 3: [1] [2] [X] [4] [5] [6] [7] [X]
Row 4: [X] [2] [3] [4] [X] [6] [7] [8]
Row 5: [1] [2] [3] [X] [5] [6] [X] [8]
Row 6: [1] [X] [3] [4] [5] [6] [7] [X]
Row 7: [X] [2] [3] [4] [5] [X] [7] [8]
Row 8: [1] [2] [X] [4] [5] [6] [7] [8]

====== Galaxy Multiplex Ticket Reservation ======
1. Display Available Seats
2. Book Seat(s)
3. Cancel Seat Booking
4. Exit
Enter your choice: **2**
Enter Row Number (1-8): **1**
Enter number of seats to book: **2**
Enter Seat Number to book: **2**
Seat 2 in Row 1 successfully booked.
Enter Seat Number to book: **3**
Seat 3 in Row 1 successfully booked.

====== Galaxy Multiplex Ticket Reservation ======
1. Display Available Seats
2. Book Seat(s)
3. Cancel Seat Booking
4. Exit
Enter your choice: **2**
Enter Row Number (1-8): **1**
Enter number of seats to book: **1**
Enter Seat Number to book: **1**
Seat 1 in Row 1 is already booked.

====== Galaxy Multiplex Ticket Reservation ======
1. Display Available Seats
2. Book Seat(s)
3. Cancel Seat Booking
4. Exit
Enter your choice: **2**
Enter Row Number (1-8): **9**
Enter number of seats to book: **1**
Enter Seat Number to book: **1**
Invalid row or seat number.

====== Galaxy Multiplex Ticket Reservation ======
1. Display Available Seats
2. Book Seat(s)
3. Cancel Seat Booking
4. Exit
Enter your choice: **2**
Enter Row Number (1-8): **1**
Enter number of seats to book: **1**
Enter Seat Number to book: **9**
Invalid row or seat number.

====== Galaxy Multiplex Ticket Reservation ======
1. Display Available Seats
2. Book Seat(s)
3. Cancel Seat Booking
4. Exit
Enter your choice: **3**
Enter Row Number (1-8): **1**
Enter Seat Number to cancel: **1**
Booking for Seat 1 in Row 1 has been cancelled.

====== Galaxy Multiplex Ticket Reservation ======
1. Display Available Seats
2. Book Seat(s)
3. Cancel Seat Booking
4. Exit
Enter your choice: **3**
Enter Row Number (1-8): **1**
Enter Seat Number to cancel: **5**
Seat 5 in Row 1 is not currently booked.

====== Galaxy Multiplex Ticket Reservation ======
1. Display Available Seats
2. Book Seat(s)
3. Cancel Seat Booking
4. Exit
Enter your choice: **3**
Enter Row Number (1-8): **10**
Enter Seat Number to cancel: **1**
Invalid row or seat number.

====== Galaxy Multiplex Ticket Reservation ======
1. Display Available Seats
2. Book Seat(s)
3. Cancel Seat Booking
4. Exit
Enter your choice: **3**
Enter Row Number (1-8): **1**
Enter Seat Number to cancel: **0**
Invalid row or seat number.

====== Galaxy Multiplex Ticket Reservation ======
1. Display Available Seats
2. Book Seat(s)
3. Cancel Seat Booking
4. Exit
Enter your choice: **1**
Current Seat Availability:
Row 1: [1] [X] [X] [X] [5] [6] [7] [8]
Row 2: [1] [X] [3] [4] [5] [X] [7] [8]
Row 3: [1] [2] [X] [4] [5] [6] [7] [X]

====== Galaxy Multiplex Ticket Reservation ======
1. Display Available Seats
2. Book Seat(s)
3. Cancel Seat Booking
4. Exit
Enter your choice: **5**
Invalid choice. Try again.

====== Galaxy Multiplex Ticket Reservation ======
1. Display Available Seats
2. Book Seat(s)
3. Cancel Seat Booking
4. Exit
Enter your choice: **4**
Exiting the system. Goodbye!

## Conclusion  

The program implements a **dynamic ticket reservation system** for Galaxy Multiplex using **Doubly Circular Linked Lists**. It allows **real-time booking, cancellation, and display of seats**, demonstrating efficient management of resources in a multiplex setting. Using DCLLs ensures **easy traversal, modification, and maintenance** of seat availability, reflecting a real-world reservation system effectively.  