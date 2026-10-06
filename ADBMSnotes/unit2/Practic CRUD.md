# MongoDB CRUD Practice – 15 Scenario-Based Problems

**Assume each problem has its own collection unless specified otherwise.**

---

### 1. College Student Management System

A college maintains student records in a `students` collection with fields such as:

`studentId, name, branch, year, cgpa, city, skills`

Perform the following:

1. Insert  **5 student documents** .
2. Display all students.
3. Display students belonging to the  **CSE branch** .
4. Find students having a  **CGPA greater than 8.0** .
5. Update the city of a particular student.
6. Delete a student who has withdrawn from the college.

---

### 2. Online Book Store

An online bookstore stores books in a `books` collection containing:

`bookId, title, author, category, price, stock`

Perform the following:

1. Add **8 books** to the collection.
2. Display all books belonging to the **Programming** category.
3. Find books costing less than ₹500.
4. Increase the price of a particular book.
5. Update the stock of a book after receiving new inventory.
6. Remove a book that is no longer available for sale.

---

### 3. Employee Management System

A company stores employee information in an `employees` collection:

`empId, name, department, designation, salary, experience, city`

Perform the following:

1. Insert  **10 employee records** .
2. Display all employees working in the  **IT department** .
3. Find employees whose salary is greater than ₹60,000.
4. Update the designation of an employee after promotion.
5. Increase the salary of an employee by ₹5,000.
6. Delete an employee who has left the organization.

---

### 4. E-Commerce Product Catalog

An e-commerce website maintains products in a `products` collection:

`productId, name, category, brand, price, quantity, rating`

Perform the following:

1. Insert at least  **10 products** .
2. Display all products belonging to the **Electronics** category.
3. Find products with a rating of  **4 or above** .
4. Update the price of a particular product.
5. Increase the quantity of a product after restocking.
6. Delete a discontinued product.

---

### 5. Hospital Patient Records

A hospital maintains patient information in a `patients` collection:

`patientId, name, age, gender, disease, doctor, roomNo`

Perform the following:

1. Insert  **8 patient records** .
2. Display all patients suffering from  **Diabetes** .
3. Find patients whose age is above 60.
4. Change the room number of a patient.
5. Update the doctor assigned to a patient.
6. Delete the record of a patient who has been discharged.

---

### 6. Movie Streaming Platform

A streaming platform stores movies in a `movies` collection:

`movieId, title, genre, language, year, rating, duration`

Perform the following:

1. Insert  **10 movie documents** .
2. Display all movies in the **Action** genre.
3. Find movies released after 2020.
4. Find movies having a rating greater than 8.
5. Update the rating of a movie.
6. Delete a movie that has been removed from the platform.

---

### 7. Food Delivery Application

A food delivery application maintains restaurant information in a `restaurants` collection:

`restaurantId, name, cuisine, city, rating, deliveryTime, isOpen`

Perform the following:

1. Add  **8 restaurants** .
2. Display restaurants serving  **Indian cuisine** .
3. Find restaurants with a rating above 4.0.
4. Update the delivery time of a restaurant.
5. Change `isOpen` when a restaurant closes.
6. Delete a restaurant that permanently shuts down.

---

### 8. College Course Registration

A university maintains courses in a `courses` collection:

`courseId, courseName, department, credits, faculty, seats`

Perform the following:

1. Insert  **10 courses** .
2. Display all courses offered by the  **CSE department** .
3. Find courses having  **4 credits** .
4. Update the faculty assigned to a course.
5. Reduce the number of available seats after student registration.
6. Delete a course that is no longer offered.

---

### 9. Banking Customer Management

A bank stores customer information in a `customers` collection:

`customerId, name, accountType, balance, city, status`

Perform the following:

1. Insert  **10 customer records** .
2. Display customers having a **Savings** account.
3. Find customers whose balance is greater than ₹1,00,000.
4. Update the balance of a customer after a deposit.
5. Change the account status from `Active` to `Inactive`.
6. Delete a customer account that has been permanently closed.

---

### 10. Vehicle Rental System

A vehicle rental company maintains vehicles in a `vehicles` collection:

`vehicleId, type, brand, model, year, rentPerDay, availability`

Perform the following:

1. Insert  **10 vehicles** .
2. Display all available cars.
3. Find vehicles having a rental price below ₹2,000 per day.
4. Change a vehicle's availability after it is rented.
5. Update the rental price of a vehicle.
6. Delete a vehicle that has been permanently removed from the fleet.

---

### 11. Employee Attendance System

A company stores attendance information in an `attendance` collection:

`attendanceId, empId, employeeName, date, status, workingHours`

Perform the following:

1. Insert attendance records for multiple employees.
2. Display all employees marked  **Absent** .
3. Find employees who worked more than 8 hours.
4. Correct the attendance status of an employee mistakenly marked absent.
5. Update working hours for a particular attendance record.
6. Delete an attendance record entered incorrectly.

---

### 12. Online Learning Platform

An online learning platform maintains courses in a `courses` collection:

`courseId, title, instructor, category, duration, fee, studentsEnrolled`

Perform the following:

1. Insert  **10 online courses** .
2. Display all courses in the **Data Science** category.
3. Find courses costing less than ₹5,000.
4. Update the course fee.
5. Increase `studentsEnrolled` after new registrations.
6. Delete a course that is no longer available.

---

### 13. Hotel Reservation System

A hotel stores room information in a `rooms` collection:

`roomNo, type, price, floor, capacity, status`

Perform the following:

1. Insert records for  **10 rooms** .
2. Display all available rooms.
3. Find rooms with a capacity of at least 3 people.
4. Update the status of a room after booking.
5. Change the price of a room after a pricing revision.
6. Delete a room that is permanently unavailable.

---

### 14. Internship Management System

A college maintains internship opportunities in an `internships` collection:

`internshipId, company, domain, location, stipend, duration, openings`

Perform the following:

1. Insert  **10 internship opportunities** .
2. Display internships related to  **Artificial Intelligence** .
3. Find internships offering a stipend greater than ₹20,000.
4. Update the number of openings after students are selected.
5. Update the location of an internship.
6. Delete an internship after its application deadline has passed.

---

### 15. Student Project Management System

A department maintains student project information in a `projects` collection:

`projectId, title, domain, students, guide, status, year`

Perform the following:

1. Insert at least  **10 project records** .
2. Display all projects belonging to the  **AI/ML domain** .
3. Find all projects whose status is `Completed`.
4. Update the project status from `In Progress` to `Completed`.
5. Change the faculty guide assigned to a project.
6. Delete a project that has been cancelled.

---
