# Campus Event Management System

A web-based Java application built as a college mini-project for managing campus events, student registrations, and administrative event operations using core Java Web technologies (JSP, Servlet, JDBC, MySQL).

---

## 📖 Project Description

The **Campus Event Management System** is designed for educational institutions to streamline event announcements, student registrations, and administrative event tracking. Students can register accounts, log in, browse upcoming technical and cultural events, view detailed schedules and seat availability, register for events, and manage or cancel their registrations. Administrators can create, edit, and delete events, search catalog listings, monitor seat capacities, and inspect detailed student attendee lists.

This project strictly adheres to standard Java Enterprise Web architecture (**JSP → Servlet → JDBC → MySQL**) without relying on heavy frameworks like Spring Boot or Hibernate, making it clean, easy to understand, and ideal for college viva examinations.

---

## ✨ Features

### Phase 1 Features (Completed)
- **Student Registration:** Full validation (email format, password matching, minimum length, duplicate email prevention).
- **Secure Password Hashing:** Standard SHA-256 algorithm via `PasswordUtil`.
- **Unified Authentication:** Common login page supporting both Student and Admin roles.
- **Session Management:** Role-based HTTP Session validation (`STUDENT` vs `ADMIN`).
- **Access Control & Security:** Protection of student and admin dashboard pages against unauthenticated direct URL access and back-button history caching.
- **Logout System:** Complete session invalidation and browser cache clearing.
- **Clean UI & Responsive Design:** Custom vanilla CSS design system with cards, forms, badges, alert boxes, and responsive navbar.

### Phase 2 Features (Student Event Module - Completed)
- **Dynamic Event Browsing (`student/events`):** Displays campus events dynamically from MySQL. Shows date, time, venue, description, and live available seats.
- **Event Search & Venue Filtering:** Search events by title keyword and filter catalog by venue location.
- **Single Event Details View (`student/event-details`):** Detailed page displaying event details, total capacity, remaining seats, and registration controls.
- **Database-Driven Capacity Logic:** Calculates `available_seats = capacity - registered_count` in real-time. Automatically blocks registration when `available_seats <= 0` ("Registration Closed").
- **Duplicate Registration Protection:** Checks `unique_user_event` constraint and prevents a student from registering twice for the same event.
- **My Registrations Management (`student/my-registrations`):** Shows all events registered by the currently logged-in student using SQL `JOIN` queries.
- **Registration Cancellation:** Students can cancel their own registrations; database seat capacity updates automatically.
- **Upgraded Student Dashboard (`student/dashboard`):** Real-time database metrics showing total active events, personal registration counts, and upcoming enrolled event schedules.

### Phase 3 Features (Admin Module - Completed)
- **Live Admin Dashboard (`admin/dashboard`):** Real-time database statistics displaying Total Events, Upcoming Events, Total Registrations, and Total Registered Students.
- **Manage Events Catalog (`admin/manage-events`):** Tabular overview of all campus events with search, venue filter, enrolled student count badges, and action controls.
- **Publish New Event (`admin/add-event`):** Form with comprehensive validation (date, time, positive capacity, title length) submitting via `AddEventServlet`.
- **Edit Event & Capacity Guard (`admin/edit-event`):** Pre-populated edit form. Prevents reducing capacity below the number of students already enrolled in the event.
- **Delete Event (`admin/delete-event`):** POST-enforced destructive action with UI confirmation step. Database foreign key (`ON DELETE CASCADE`) ensures clean deletion of associated registrations without orphaned records.
- **View Student Registrations (`admin/event-registrations`):** Displays student name, email, event title, event date, and registration date using 3-table SQL `JOIN` queries. Supports filtering by specific event.
- **Mandatory Access Control:** Every Admin Servlet and JSP enforces strict server-side `userRole == "ADMIN"` session checks. Non-admin students are blocked and redirected to `login.jsp`.

---

## 🛠️ Technology Stack

- **Backend:** Java JDK 11+, Java Servlets (Servlet 3.1+ API), JavaServer Pages (JSP)
- **Database Connectivity:** JDBC (Java Database Connectivity) with `PreparedStatement`
- **Database:** MySQL Server (via XAMPP)
- **Frontend:** HTML5, Vanilla CSS3 (Custom Design System, No Bootstrap required)
- **Web Server / Servlet Container:** Apache Tomcat (v8.5 / v9.0 / v10.0+)
- **IDE:** Visual Studio Code / Eclipse / IntelliJ IDEA

---

## 📁 Folder Structure

```text
CampusEventManagement/
│
├── database.sql                             # SQL setup script (DB, tables & sample events)
├── README.md                                # Project documentation
│
└── src/
    └── main/
        ├── java/
        │   └── com/
        │       └── campusevent/
        │           ├── controller/          # Servlets handling requests
        │           │   ├── LoginServlet.java
        │           │   ├── LogoutServlet.java
        │           │   ├── RegisterServlet.java
        │           │   ├── StudentDashboardServlet.java
        │           │   ├── StudentEventsServlet.java
        │           │   ├── EventDetailsServlet.java
        │           │   ├── RegisterEventServlet.java
        │           │   ├── CancelRegistrationServlet.java
        │           │   ├── MyRegistrationsServlet.java
        │           │   ├── AdminDashboardServlet.java
        │           │   ├── AdminManageEventsServlet.java
        │           │   ├── AddEventServlet.java
        │           │   ├── EditEventServlet.java
        │           │   ├── DeleteEventServlet.java
        │           │   └── AdminViewRegistrationsServlet.java
        │           │
        │           ├── dao/                 # Database Access Objects
        │           │   ├── UserDAO.java
        │           │   ├── EventDAO.java
        │           │   └── RegistrationDAO.java
        │           │
        │           ├── model/               # Entity Model POJOs
        │           │   ├── User.java
        │           │   ├── Event.java
        │           │   └── Registration.java
        │           │
        │           └── util/                # DB Connection & Password Hashing
        │               ├── DBConnection.java
        │               └── PasswordUtil.java
        │
        └── webapp/                          # Web Content Root
            ├── css/
            │   └── style.css                # Custom CSS Design System
            ├── images/                      # Image assets
            ├── student/
            │   ├── dashboard.jsp            # Protected Live Student Dashboard
            │   ├── events.jsp               # Dynamic Event Catalog & Search
            │   ├── event-details.jsp        # Event Specification & Registration
            │   └── my-registrations.jsp     # Registered Events & Cancellation
            ├── admin/
            │   ├── dashboard.jsp            # Protected Live Admin Dashboard
            │   ├── manage-events.jsp        # Events Management & Search Table
            │   ├── add-event.jsp            # Add Event Form
            │   ├── edit-event.jsp           # Edit Event Form
            │   └── event-registrations.jsp  # Student Registrations Inspection
            ├── WEB-INF/
            │   ├── classes/                 # Compiled .class files
            │   ├── lib/                     # JDBC Driver JAR (mysql-connector-j.jar)
            │   └── web.xml                  # Deployment Descriptor
            ├── index.jsp                    # Landing Home Page
            ├── login.jsp                    # Login Page
            ├── register.jsp                 # Student Registration Page
            └── logout.jsp                   # Logout Helper Page
```

---

## 🛢️ XAMPP & MySQL Database Setup

1. Open **XAMPP Control Panel** and click **Start** next to **MySQL** (and Apache if needed).
2. Open your web browser and navigate to `http://localhost/phpmyadmin`.
3. Click on the **SQL** tab.
4. Copy the entire contents of the [`database.sql`](file:///c:/Users/GI/Desktop/Java%20Mini%20Project/database.sql) file.
5. Paste it into the SQL query box and click **Go**.

---

## 🛠️ Setup & Installation

### 1. Prerequisites
* **Java Development Kit (JDK)** (Recommended: JDK 8 or 11/17 depending on your setup)
* **Apache Tomcat Server** (Recommended: Tomcat 9 or later)
* **MySQL Database**

### 2. External Dependencies (.jar files)
Because build artifacts are ignored in this repository, you will need to download and place the following JAR libraries into the `src/main/webapp/WEB-INF/lib/` directory before running the application:

#### Database Driver:
* **MySQL Connector/J** (e.g., `mysql-connector-j-26.7.0.jar` or equivalent)

#### Hibernate & Persistence Framework:
* `hibernate-core-5.6.15.Final.jar`
* `hibernate-commons-annotations-5.1.2.Final.jar`
* `javax.persistence-api-2.2.jar`
* `jboss-logging-3.4.3.Final.jar`
* `byte-buddy-1.12.18.jar`
* `antlr-2.7.7.jar`
* `classmate-1.5.1.jar`
* `dom4j-2.1.3.jar`
* `jboss-transaction-api_1.2_spec-1.1.1.Final.jar`

#### XML Binding Utilities (Required for Java 9+):
* `jaxb-api-2.3.1.jar`
* `jaxb-runtime-2.3.1.jar`
* `istack-commons-runtime-3.0.7.jar`
* `stax-ex-1.8.jar`
* `txw2-2.3.1.jar`
* `FastInfoset-1.2.15.jar`
* `javax.activation-api-1.2.0.jar`

### 3. Step-by-Step Local Deployment
1. **Clone the Repository:**
   ```bash
   git clone https://github.com
   cd college-event-management-system-Java-
   ```
2. **Add the JARs:** Download the JARs listed above and drop them into `src/main/webapp/WEB-INF/lib/`.
3. **Database Setup:** Import the `database.sql` script into your MySQL server. Update your database credentials in `src/main/resources/hibernate.cfg.xml`.
4. **Build & Run:** Open the project in your IDE (Eclipse/IntelliJ), configure your Apache Tomcat server, add the project to the server deployment assembly, and hit **Run**.
---

## 🔐 Default Credentials

### Admin Credentials:
- **Email:** `admin@campus.edu`
- **Password:** `admin123`
- **Role:** `ADMIN`

### Sample Student Credentials:
- **Email:** `student@campus.edu`
- **Password:** `student123`
- **Role:** `STUDENT`

---

## 📊 Current Implementation Status (All Phases Completed)

| Component | Status | Details |
|---|---|---|
| Project Architecture | ✅ Completed | JSP → Servlet → JDBC → MySQL architecture |
| Authentication & Hashing | ✅ Completed | Unified login with SHA-256 password hashing |
| Session & Access Security | ✅ Completed | Server-side `STUDENT` and `ADMIN` role protection |
| Student Event Browsing | ✅ Completed | Live event catalog with keyword search and venue filter |
| Event Registration | ✅ Completed | Real-time capacity check and duplicate registration guard |
| Registration Cancellation | ✅ Completed | Students can cancel enrollments; frees up seat capacity |
| Admin Dashboard | ✅ Completed | Real database statistics for events, registrations, and users |
| Admin Event CRUD | ✅ Completed | Full Create, Read, Update, Delete for events |
| Capacity Reduction Guard | ✅ Completed | Prevents reducing capacity below existing registrations count |
| Foreign Key Integrity | ✅ Completed | Controlled `CASCADE` deletion prevents orphaned records |
| Student Registrations View | ✅ Completed | 3-table SQL `JOIN` query listing student attendees per event |
| Team Event Module | ✅ Completed | Support for Team Events, member email search, and atomic JDBC transactions |
| Hibernate Analytics Module | ✅ Completed | Hibernate ORM, SessionFactory, JPA Annotations, HQL aggregate queries & Team Analytics |

---

## 🍃 HIBERNATE MODULE (Admin Event Analytics & ORM)

The application features a dedicated **Hibernate-based Event Analytics & ORM Module** on the Admin side, demonstrating Object-Relational Mapping (ORM) and Hibernate Query Language (HQL) on the same MySQL database (`campus_event_db`).

### Architecture Highlights:
- **Dual Persistence Architecture:** The Student Event Registration Engine uses **JDBC** for synchronized concurrent seat allocation, while the Admin Analytics Module uses **Hibernate ORM & HQL** for analytical reporting.
- **Annotated Domain Entities:** `@Entity`, `@Table`, `@Id`, `@GeneratedValue`, `@Column`, `@ManyToOne`, `@OneToMany`, `@JoinColumn`.
- **Hibernate SessionFactory:** Configured via `hibernate.cfg.xml` and managed through `HibernateUtil.java`.
- **HQL Queries:**
  1. *Most Popular Events:* Aggregated JOIN query `SELECT e, COUNT(r.id) FROM Event e LEFT JOIN e.registrationsList r GROUP BY e.id ORDER BY COUNT(r.id) DESC`.
  2. *Upcoming Events:* Named date parameter query `FROM Event e WHERE e.eventDate >= :today`.
  3. *Total Statistics:* Aggregate queries `SELECT COUNT(u.id) FROM User u`.
  4. *Team Analytics:* Calculates total teams, team participants, average team size, smallest, and largest team sizes dynamically via HQL.
- **Documentation & Viva Resources:** See [`HIBERNATE_DOCUMENTATION.md`](file:///c:/Users/GI/Desktop/Java%20Mini%20Project/HIBERNATE_DOCUMENTATION.md) and [`HIBERNATE_VIVA.md`](file:///c:/Users/GI/Desktop/Java%20Mini%20Project/HIBERNATE_VIVA.md).

