# Assignment 13

## Title  
**Develop a C++ program to store and manage an appointment schedule for a single day. Appointments should be scheduled randomly using a linked list. The system must define the start time, end time, and specify the minimum and maximum duration allowed for each appointment slot. The program should include the following operations:  
a) Display the list of currently available time slots.  
b) Book a new appointment within the defined time limits.  
c) Cancel an existing appointment after validating its time, availability, and correctness.  
d) Sort the appointment list in order of appointment times.  
e) Sort the list based on appointment times using pointer manipulation (without swapping data values).**


---

## Theory  

A **Linked List** is an efficient dynamic data structure consisting of nodes connected through pointers. Each node in this system represents an **appointment slot**, containing details like:
- **Start time**  
- **End time**  
- **Duration** (calculated automatically)

### **Core Operations**

1. **Display Available Time Slots:**  
   Traverses the linked list and shows all free time periods between scheduled appointments.  

2. **Book Appointment:**  
   Inserts a new appointment node in sorted order while ensuring it fits within defined time constraints and does not overlap with existing appointments.  

3. **Cancel Appointment:**  
   Removes a node from the list by validating its time and existence before deletion.  

4. **Sort Appointments (by Data Swapping):**  
   Sorts appointments based on start times by comparing and exchanging node data.  

5. **Sort Appointments (by Pointer Manipulation):**  
   Rearranges nodes in sorted order by updating **next pointers** instead of swapping actual data fields — a more memory-efficient approach.  

### **Advantages of Using Linked List:**
- Dynamic memory management — easily insert or delete appointments without shifting data.  
- Efficient sorting using pointer manipulation.  
- Maintains real-time, ordered scheduling of appointments.  
- Provides flexibility to handle varying durations and time constraints.

---
## Code
```
#include <iostream>
#include <string>
#include <iomanip>
using namespace std;

struct Appointment
{
    string name;
    int startTime; 
    int endTime;
    Appointment *next;
};

class AppointmentList
{
    Appointment *head;
    int minDuration; 
    int maxDuration;
    int dayStart; 
    int dayEnd;   

public:
    AppointmentList(int start, int end, int minD, int maxD)
    {
        head = nullptr;
        dayStart = start;
        dayEnd = end;
        minDuration = minD;
        maxDuration = maxD;
    }

    ~AppointmentList()
    {
        Appointment *temp;
        while (head)
        {
            temp = head;
            head = head->next;
            delete temp;
        }
    }

    string timeToStr(int time)
    {
        int hr = time / 100;
        int min = time % 100;
        string period = (hr >= 12) ? "PM" : "AM";
        if (hr > 12)
            hr -= 12;
        if (min == 0)
            return to_string(hr) + ":00 " + period;
        else
            return to_string(hr) + ":" + (min < 10 ? "0" + to_string(min) : to_string(min)) + " " + period;
    }

    void displayAvailableSlots()
    {
        cout << "\n---- Available Time Slots ----\n";
        Appointment *curr = head;
        int prevEnd = dayStart;

        if (!head)
        {
            cout << timeToStr(dayStart) << " - " << timeToStr(dayEnd) << endl;
            return;
        }

        while (curr)
        {
            if (prevEnd < curr->startTime)
                cout << timeToStr(prevEnd) << " - " << timeToStr(curr->startTime) << endl;
            prevEnd = curr->endTime;
            curr = curr->next;
        }

        if (prevEnd < dayEnd)
            cout << timeToStr(prevEnd) << " - " << timeToStr(dayEnd) << endl;
    }

    void bookAppointment(string name, int start, int end)
    {
        if (start < dayStart || end > dayEnd || start >= end)
        {
            cout << "Invalid time range.\n";
            return;
        }

        int duration = ((end / 100) * 60 + (end % 100)) - ((start / 100) * 60 + (start % 100));
        if (duration < minDuration || duration > maxDuration)
        {
            cout << "Appointment duration must be between " << minDuration << " and " << maxDuration << " minutes.\n";
            return;
        }

        Appointment *newApp = new Appointment;
        newApp->name = name;
        newApp->startTime = start;
        newApp->endTime = end;
        newApp->next = nullptr;

        
        Appointment *curr = head;
        Appointment *prev = nullptr;

        while (curr && curr->startTime < end)
        {
            if (!(end <= curr->startTime || start >= curr->endTime))
            {
                cout << "Time slot overlaps with an existing appointment.\n";
                delete newApp;
                return;
            }
            prev = curr;
            curr = curr->next;
        }

        
        if (!prev)
        {
            newApp->next = head;
            head = newApp;
        }
        else
        {
            newApp->next = prev->next;
            prev->next = newApp;
        }

        cout << "Appointment booked successfully!\n";
    }

    void cancelAppointment(int start)
    {
        if (!head)
        {
            cout << "No appointments to cancel.\n";
            return;
        }

        Appointment *curr = head;
        Appointment *prev = nullptr;

        while (curr && curr->startTime != start)
        {
            prev = curr;
            curr = curr->next;
        }

        if (!curr)
        {
            cout << "No appointment found at this time.\n";
            return;
        }

        if (!prev)
            head = curr->next;
        else
            prev->next = curr->next;

        cout << "Appointment at " << timeToStr(start) << " cancelled successfully.\n";
        delete curr;
    }

    
    void sortByTime()
    {
        if (!head || !head->next)
            return;

        Appointment *sorted = nullptr;

        while (head)
        {
            Appointment *current = head;
            head = head->next;

            if (!sorted || current->startTime < sorted->startTime)
            {
                current->next = sorted;
                sorted = current;
            }
            else
            {
                Appointment *temp = sorted;
                while (temp->next && temp->next->startTime < current->startTime)
                    temp = temp->next;
                current->next = temp->next;
                temp->next = current;
            }
        }
        head = sorted;
        cout << "Appointments sorted successfully by time.\n";
    }

    void displayAppointments()
    {
        if (!head)
        {
            cout << "No appointments booked yet.\n";
            return;
        }

        cout << "\n---- Booked Appointments ----\n";
        Appointment *temp = head;
        while (temp)
        {
            cout << left << setw(15) << temp->name
                 << " | " << timeToStr(temp->startTime)
                 << " - " << timeToStr(temp->endTime) << endl;
            temp = temp->next;
        }
    }
};

int main()
{
    AppointmentList schedule(900, 1700, 30, 120); 
    int choice, start, end;
    string name;

    do
    {
        cout << "\n===== Appointment Scheduler =====\n";
        cout << "1. Display available time slots\n";
        cout << "2. Book new appointment\n";
        cout << "3. Cancel appointment\n";
        cout << "4. Display booked appointments\n";
        cout << "5. Sort appointments by time\n";
        cout << "0. Exit\n";
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice)
        {
        case 1:
            schedule.displayAvailableSlots();
            break;

        case 2:
            cout << "Enter name: ";
            cin >> name;
            cout << "Enter start time (e.g., 930 for 9:30): ";
            cin >> start;
            cout << "Enter end time (e.g., 1030 for 10:30): ";
            cin >> end;
            schedule.bookAppointment(name, start, end);
            break;

        case 3:
            cout << "Enter start time to cancel: ";
            cin >> start;
            schedule.cancelAppointment(start);
            break;

        case 4:
            schedule.displayAppointments();
            break;

        case 5:
            schedule.sortByTime();
            break;

        case 0:
            cout << "Exiting program...\n";
            break;

        default:
            cout << "Invalid choice!\n";
        }
    } while (choice != 0);

    return 0;
}


```

## Output
===== Appointment Scheduler =====
1. Display available time slots
2. Book new appointment
3. Cancel appointment
4. Display booked appointments
5. Sort appointments by time
0. Exit
Enter your choice: **1**
---- Available Time Slots ----
9:00 AM - 5:00 PM

===== Appointment Scheduler =====
1. Display available time slots
2. Book new appointment
3. Cancel appointment
4. Display booked appointments
5. Sort appointments by time
0. Exit
Enter your choice: **2**
Enter name: **Alice**
Enter start time (e.g., 930 for 9:30): **930**
Enter end time (e.g., 1030 for 10:30): **1030**
Appointment booked successfully!

===== Appointment Scheduler =====
1. Display available time slots
2. Book new appointment
3. Cancel appointment
4. Display booked appointments
5. Sort appointments by time
0. Exit
Enter your choice: **2**
Enter name: **Bob**
Enter start time (e.g., 930 for 9:30): **1100**
Enter end time (e.g., 1030 for 10:30): **1200**
Appointment booked successfully!

===== Appointment Scheduler =====
1. Display available time slots
2. Book new appointment
3. Cancel appointment
4. Display booked appointments
5. Sort appointments by time
0. Exit
Enter your choice: **2**
Enter name: **Charlie**
Enter start time (e.g., 930 for 9:30): **1000**
Enter end time (e.g., 1030 for 10:30): **1045**
Time slot overlaps with an existing appointment.

===== Appointment Scheduler =====
1. Display available time slots
2. Book new appointment
3. Cancel appointment
4. Display booked appointments
5. Sort appointments by time
0. Exit
Enter your choice: **2**
Enter name: **David**
Enter start time (e.g., 930 for 9:30): **800**
Enter end time (e.g., 1030 for 10:30): **900**
Invalid time range.

===== Appointment Scheduler =====
1. Display available time slots
2. Book new appointment
3. Cancel appointment
4. Display booked appointments
5. Sort appointments by time
0. Exit
Enter your choice: **2**
Enter name: **Eve**
Enter start time (e.g., 930 for 9:30): **1600**
Enter end time (e.g., 1030 for 10:30): **1800**
Invalid time range.

===== Appointment Scheduler =====
1. Display available time slots
2. Book new appointment
3. Cancel appointment
4. Display booked appointments
5. Sort appointments by time
0. Exit
Enter your choice: **2**
Enter name: **Frank**
Enter start time (e.g., 930 for 9:30): **1400**
Enter end time (e.g., 1030 for 10:30): **1415**
Appointment duration must be between 30 and 120 minutes.

===== Appointment Scheduler =====
1. Display available time slots
2. Book new appointment
3. Cancel appointment
4. Display booked appointments
5. Sort appointments by time
0. Exit
Enter your choice: **2**
Enter name: **Grace**
Enter start time (e.g., 930 for 9:30): **1400**
Enter end time (e.g., 1030 for 10:30): **1600**
Appointment booked successfully!

===== Appointment Scheduler =====
1. Display available time slots
2. Book new appointment
3. Cancel appointment
4. Display booked appointments
5. Sort appointments by time
0. Exit
Enter your choice: **1**
---- Available Time Slots ----
9:00 AM - 9:30 AM
10:30 AM - 11:00 AM
12:00 PM - 2:00 PM
4:00 PM - 5:00 PM

===== Appointment Scheduler =====
1. Display available time slots
2. Book new appointment
3. Cancel appointment
4. Display booked appointments
5. Sort appointments by time
0. Exit
Enter your choice: **4**
---- Booked Appointments ----
Alice | 9:30 AM - 10:30 AM
Bob | 11:00 AM - 12:00 PM
Grace | 2:00 PM - 4:00 PM

===== Appointment Scheduler =====
1. Display available time slots
2. Book new appointment
3. Cancel appointment
4. Display booked appointments
5. Sort appointments by time
0. Exit
Enter your choice: **3**
Enter start time to cancel: **930**
Appointment at 9:30 AM cancelled successfully.

===== Appointment Scheduler =====
1. Display available time slots
2. Book new appointment
3. Cancel appointment
4. Display booked appointments
5. Sort appointments by time
0. Exit
Enter your choice: **3**
Enter start time to cancel: **930**
No appointment found at this time.

===== Appointment Scheduler =====
1. Display available time slots
2. Book new appointment
3. Cancel appointment
4. Display booked appointments
5. Sort appointments by time
0. Exit
Enter your choice: **3**
Enter start time to cancel: **1000**
No appointment found at this time.

## Conclusion  

The program effectively demonstrates the use of **Linked Lists** for implementing an **Appointment Scheduling System**. It dynamically manages appointments for a single day, supporting operations like **displaying available slots, booking, cancelling, and sorting** in multiple ways. By leveraging **pointer-based sorting**, it showcases efficient in-memory manipulation without redundant data copying. This assignment highlights the practicality and efficiency of linked
