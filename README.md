# Contact Manager System

---

##  Group Members

| Name | Student ID |
|------|------------|
| Omar Wael Mahmoud Mahmoud | 2300109 |
| Hassan Ebrahim Mohamed | 2300166 |
| Mohamed Osama Fathy Farag | 2300533 |
| Mohammed Zaki Abdelazim Elsayed | 2300465 |
| Ahmed Sameh Mohamed Hassouna | 2300082 |

---

##  Project Description

The Contact Manager System is a C++ application that simulates a phonebook. Each contact stores a name, phone number, email, and an auto-assigned unique ID. All contacts are held in a custom-built singly linked list — no STL containers are used for storage.

The system enforces real-world Egyptian phone number formats (010 / 011 / 012 / 015), restricts email domains to Gmail, iCloud, and Yahoo, and rejects names containing digits. Contacts can be added, deleted, updated, searched, sorted, and filtered entirely through the `ContactManager` interface.

---

## Data Structures Used & Why

### Singly Linked List (`LinkedList`)

The core data structure. Each `Node` holds a `Contact` object and a pointer to the next node.

**Why a linked list?**
- **Dynamic size** — no fixed capacity; contacts are added and removed at runtime without resizing.
- **Efficient insertion / deletion** — appending is O(1); removing a node only requires redirecting one pointer, with no element shifting.
- **Merge sort compatibility** — merge sort works naturally on linked lists using the slow/fast pointer technique for midpoint finding, achieving O(n log n) without needing random access.

An array or `std::vector` was deliberately avoided to satisfy the assignment requirement of implementing and using a custom linked structure.

---

##  Project Structure

```
ContactManager/
├── Exe/                        # Ready-to-run Windows executable + all required Qt DLLs
│   └── ContactManager.exe
├── ContactClass.h              # Contact class declaration
├── Contact.cpp                 # Contact implementation (getters, setters, validation)
├── LinkedList.h                # LinkedList + Node class declaration
├── LinkedList.cpp              # LinkedList implementation (insert, erase, sort, search, filter)
├── ContactManagerClass.h       # ContactManager class declaration
├── ContactManager.cpp          # ContactManager implementation (CRUD + display)
└── main.cpp                    # Entry point / Qt application bootstrap
```

---

##  How to Run the Project

There are two ways to run this project: using the pre-built executable, or compiling from source.

---

### Option 1 — Run the Pre-Built Executable (Easiest)

A ready-to-use Windows executable is provided inside the `Exe` folder.  
It includes the `.exe` file and all required Qt DLLs — no installation needed.

**Steps:**
1. Open the `Exe` folder.
2. Double-click `ContactManager.exe`.
3. The application launches immediately — no setup required.

>  Keep all files inside the `Exe` folder together. Moving only the `.exe` out of the folder will cause it to fail because it won't find the Qt libraries.

---

###  Option 2 — Compile from Source


#### Steps using Qt Creator
 
1. **Open Qt Creator**.
2. Click **File → Open File or Project**.
3. Navigate to the project folder and open the `.pro` file (e.g. `ContactManager.pro`).
4. Qt Creator will configure the project automatically — click **Configure Project** if prompted.
5. Click the Run button to build and run.

#### Steps using the command line (Qt + MinGW)

Open a terminal with Qt in your PATH (use the **Qt MinGW terminal** from the Start menu on Windows):

```bash
# 1. Navigate to the project folder
cd path/to/ContactManager

# 2. Generate the Makefile from the .pro file
qmake ContactManager.pro

# 3. Build the project
mingw32-make        # Windows
# or
make                # Linux / macOS

# 4. Run the executable
ContactManager.exe     
```

#### Compile without Qt (console-only version)

If you only want to test the core C++ logic without the GUI, use `g++` directly:

```bash
g++ -std=c++11 -Wall -o ContactManager \
    Contact.cpp LinkedList.cpp ContactManager.cpp main.cpp

# Run
ContactManager.exe     
```

---

##  Implemented Features

| # | Feature | Description |
|---|---------|-------------|
| 1 | **Add Contact** | Stores a new contact with auto-incremented unique ID and full validation |
| 2 | **Delete Contact** | Removes a contact by ID; reports error if not found |
| 3 | **Update Contact** | Finds a contact by ID and overwrites its fields in-place |
| 4 | **Search by ID** | Returns a pointer to the matching contact |
| 5 | **Search by Name** | Exact-match name lookup |
| 6 | **Search by Phone** | Exact-match phone number lookup |
| 7 | **Display All** | Prints every contact with all fields |
| 8 | **Display by ID / Name / Phone** | Formatted single-contact display |
| 9 | **Sort by ID** | Merge sort — ascending numeric order |
| 10 | **Sort by Name** | Merge sort — ascending lexicographic order |
| 11 | **Substring Filter** | Returns all contacts whose name contains a given substring |
| 12 | **Input Validation** | Phone: 11-digit Egyptian codes; Email: gmail/icloud/yahoo; Name: no digits |
| 13 | **Auto-Increment IDs** | Static counter assigns an immutable unique ID at construction |
| 14 | **Deep Copy Safety** | Copy constructor and assignment operator prevent shallow-copy pointer bugs |
| 15 | **Clear All** | Deletes every contact and resets the list |

---

##  AI Usage Declaration

> Transparency note: Using AI tools did not reduce any marks — this section is included as required.

| Field | Details |
|-------|---------|
| **Tools used** | ChatGPT |
| **Used for** | Generating test cases; adding code comments; helping structure the README |
| **What was modified** | Sorting comparator logic was rewritten to handle Egyptian phone number validation rules, which the AI was unaware of |
| **What was rejected** | AI suggested an array-based storage approach — rejected and replaced with a singly linked list to meet the assignment specification |

### Example where AI output was incorrect

When asked to implement merge sort on the linked list, the AI generated a version that calculated the midpoint using `size / 2` and integer indices — a pattern that works for arrays but **not for linked lists**, since they don't support random access. We rewrote the midpoint-finding logic using the **slow/fast pointer technique** (tortoise and hare), where the fast pointer moves two steps and the slow pointer moves one, so when fast reaches the end, slow is at the middle.

### What the group understood and implemented themselves

- The full pointer-based `LinkedList` class: `Node` struct, `insert`, `append`, `erase`, copy constructor, assignment operator, and destructor
- The slow/fast pointer split for merge sort
- The `sortedMerge` recursive helper and the `mergeSort` driver — both written and debugged by the group
- All input validation logic (phone prefix rules, email domain rules, digit-in-name check)
- The layered class design: `Contact` → `LinkedList` → `ContactManager`

---

##  Notes

- Contact IDs are assigned automatically and **cannot be changed** after creation (`const int contact_id`).
- Phone numbers must be exactly 11 digits and start with `010`, `011`, `012`, or `015`.
- Email addresses must use `@gmail.com`, `@icloud.com`, or `@yahoo.com`.
- A contact is only stored if all three fields pass validation (`is_valid()` returns `true`).
- The list is sorted in-place; the sort modifies the `head` pointer directly.
