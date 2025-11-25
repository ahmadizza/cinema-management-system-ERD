

# Cinema Management System – ERD Documentation

## 1. What is an ERD?

An **Entity–Relationship Diagram (ERD)** is a visual representation of data structures within a system.
It models:

* **Entities** – objects or concepts such as *Film*, *Customer*, *Studio*.
* **Attributes** – properties of entities such as *title*, *name*, *duration*.
* **Relationships** – connections between entities such as *Customer buys Ticket*.

ERDs help designers understand the logical structure of a database before technical implementation.

---

## 2. System Context

**CineMath Network** is a nationwide cinema chain in Indonesia.
The company aims to build a centralized system to manage:

* Films
* Studios
* Branches
* Showtimes
* Tickets
* Customers
* Reservations
* Sales reports

Each branch has multiple studios with different screen types, and customers may buy or reserve tickets both online and offline.

---

## 3. System Requirements Overview

### **Film**

* `film_id` (PK), `title`, `genre`, `duration`, `release_year`
* Multivalued: `actor_list`
* Attribute: `director`

### **Cinema Branch**

* `branch_id` (PK), `branch_name`
* Composite attribute: `address` (street, city, province, postal_code)
* `phone_number`, `manager`

### **Studio** (Weak Entity of Branch)

* `studio_id` (PK within branch)
* `branch_id` (FK)
* `seat_capacity`, `screen_type` (2D, 3D, IMAX)
* Requires identifying relationship with Branch

### **Showtime**

* `showtime_id` (PK)
* Composite date: `date`, `month`, `year`, `start_time`
* `duration`
* `film_id` (FK), `studio_id` (FK)

### **Customer**

* `customer_id` (PK), `name`, `email`, `phone_number`

### **Ticket**

* `ticket_id` (PK), `price`, `seat_number`
* `showtime_id` (FK)
* `customer_id` (FK)

### **Reservation**

* `reservation_id` (PK)
* Status composite: `confirmed`, `canceled`
* `confirmation_deadline`
* `ticket_id` (FK)

### **Report**

* `report_id` (PK)
* `tickets_sold`, `revenue`
* Composite date: `day`, `month`, `year`
* `studio_id` (FK), `showtime_id` (FK), `branch_id` (FK)
* Composite status: `finished`, `ongoing`, `canceled`

---

## 4. Relationships, Cardinality, and Participation

### **Film → Showtime** (*Shown In*)

* **1 : N**
* Film: partial participation (not all films are scheduled)
* Showtime: total participation

### **Showtime → Studio** (*Displayed In*)

* **1 : N**
* Studio: partial (unused studios possible)
* Showtime: total

### **Branch → Studio** (*Has*)

* **1 : N**
* Branch: partial
* Studio: total (weak entity, must belong to a branch)

### **Showtime → Ticket** (*Generates*)

* **1 : N**
* Showtime: partial (some showtimes sell zero tickets)
* Ticket: total

### **Customer → Ticket** (*Buys*)

* **1 : N**
* Customer: partial (not all customers buy)
* Ticket: total

### **Ticket → Reservation** (*Reserved*)

* **1 : 1**
* Both sides: total participation

### **Showtime → Report** (*Recorded In*)

* **1 : N**
* Both sides: total

### **Studio → Report**

* **1 : N**, total participation

### **Branch → Report**

* **1 : N**, total participation

---

## 5. Weak Entity Explanation: Studio

`Studio` is a **weak entity** because:

* `studio_id` is **not globally unique**
* Unique identification requires `(branch_id, studio_id)`
* It has an **identifying relationship** with Branch
* Existence depends on Branch (existential dependency)

---

## 6. ERD Diagram

*(Insert the exported ERD image into your GitHub repo, e.g. `/assets/erd.png`)*

Example Markdown:

```md
![Cinema ERD](assets/erd.png)
```

---

## 7. Summary

This ERD fully represents the data structure needed for a comprehensive Cinema Management System covering operational flows from film scheduling to ticket sales and reporting.

