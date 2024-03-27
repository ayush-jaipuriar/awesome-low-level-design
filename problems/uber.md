# Designing a Ride Sharing App

The problem requires designing a ride-sharing application where drivers can offer rides with details like origin, destination, and number of seats. Riders can request rides by providing similar information. The application should calculate the ride amount based on the number of kilometers traveled and the number of seats occupied. The ride amount calculation follows specific rules, such as applying a discount for rides with two or more seats. Additionally, there is a concept of "preferred riders" who have completed more than 10 rides and are eligible for further discounts. The application should support functionalities like adding drivers, adding riders, creating/updating/withdrawing rides, and closing rides with the calculated amount. The solution should be modular, extensible, handle edge cases gracefully, and be legible and DRY (Don't Repeat Yourself). The problem statement also provides test cases and expectations for the implementation, such as creating sample data, demonstrating the functionality, and following coding guidelines.

## System Requirements
The LMS supports:

Here are the requirements for the ride sharing app written in markdown with bulleted numbered points:

## Ride Sharing App Requirements

### 1. Ride Management
- Drivers can add rides with details like origin, destination, and number of seats.
- Riders can request rides by specifying their origin, destination, and number of seats needed.  
- Multiple rides can happen simultaneously.

### 2. Ride Amount Calculation
- Implement an algorithm to calculate the ride amount based on the following rules:
    - If the number of seats occupied is greater than or equal to 2, the ride amount is calculated as: `Number of kilometers * Number of seats * 0.75 * Amount charged per KM`
    - If the number of seats occupied is 1, the ride amount is calculated as: `Number of kilometers * Amount charged per KM`
- Assume the amount charged per KM is 20.
- The number of kilometers is calculated as the absolute difference between the origin and destination.

### 3. Preferred Rider Status  
- Upgrade a rider to a "preferred rider" if they have completed more than 10 rides.
- For preferred riders, apply a discount on the ride amount:
    - If the number of seats occupied is greater than or equal to 2, the ride amount is calculated as: `Number of kilometers * Number of seats * 0.5 * Amount charged per KM`
    - If the number of seats occupied is 1, the ride amount is calculated as: `Number of kilometers * Amount charged per KM * 0.75`

### 4. Ride Lifecycle Management
- Create a new ride with an ID, origin, destination, and number of seats.
- Update an existing ride with new details.
- Withdraw (cancel) a ride.
- Close a ride and display the calculated ride amount.

### 5. User Management
- Add new drivers with their names.
- Add new riders with their names.

## Key Classes

- `RideSharing`: The main class that manages the overall ride sharing operations, including drivers, riders, and rides.

- `Driver`: Represents a driver in the system with attributes like name and methods to add/offer rides.

- `Rider`: Represents a rider with attributes like name, number of rides completed, and preferred status. Should have methods to request rides.

- `Ride`: Represents a ride with attributes like id, origin, destination, number of seats, driver, riders list, status (open/closed), and calculated amount. Should have methods to add/remove riders, calculate amount, close ride.

- `RideAmountCalculator`: A utility class responsible for calculating the ride amount based on the given rules and algorithms, taking into account the number of seats, distance, preferred rider status, etc.

## Java Implementation

### Person Class
```java
public class Person {
    String name;
    // Getters and setters
}

### Driver Class
```java
public class Driver extends Person{
    String name;
    // getters and setters
}

    // Getters and setters...
}
```
### Member Class
```java
import java.util.ArrayList;
import java.util.List;

public class Member {
    private String name;
    private String memberId;
    private List<Loan> loans;

    public Member(String name) {
        this.name = name;
        this.memberId = generateMemberId();
        this.loans = new ArrayList<>();
    }

    private String generateMemberId() {
        // Generate a unique member ID
        return "MEM" + System.currentTimeMillis();
    }

    // Add loan to member's record
    public void addLoan(Loan loan) {
        loans.add(loan);
    }

    // Getters and setters...
}
```
### Loan Class
```java
import java.time.LocalDate;

public class Loan {
    private Book book;
    private Member member;
    private LocalDate issueDate;
    private LocalDate dueDate;

    public Loan(Book book, Member member) {
        this.book = book;
        this.member = member;
        this.issueDate = LocalDate.now();
        this.dueDate = issueDate.plusDays(14); // 2-week loan period
        book.setIsAvailable(false);
        member.addLoan(this);
    }

    // Return a book
    public void returnBook() {
        book.setIsAvailable(true);
        // Remove this loan from member's record
        member.getLoans().remove(this);
    }

    // Getters and setters...
}
```
### Library Class
```java
import java.util.ArrayList;
import java.util.List;
import java.util.stream.Collectors;

public class Library {
    private List<Book> books;
    private List<Member> members;

    public Library() {
        books = new ArrayList<>();
        members = new ArrayList<>();
    }

    // Add a new book to the library
    public void addBook(Book book) {
        books.add(book);
    }

    // Register a new member
    public void registerMember(Member member) {
        members.add(member);
    }

    // Lend a book to a member
    public void lendBook(String ISBN, Member member) {
        Book book = books.stream()
                         .filter(b -> b.getISBN().equals(ISBN) && b.isAvailable())
                         .findFirst().orElse(null);
        if (book != null) {
            new Loan(book, member);
        }
    }

    // Search for books by title
    public List<Book> searchBooksByTitle(String title) {
        return books.stream()
                    .filter(book -> book.getTitle().contains(title))
                    .collect(Collectors.toList());
    }

    // Other necessary methods...
}
```