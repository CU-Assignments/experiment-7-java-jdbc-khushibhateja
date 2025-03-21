[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/D9ntAWdC)



# 1. Easy Level: Fetch Employee Data

```
import java.sql.*;

public class EmployeeData {
    public static void main(String[] args) {
        String url = "jdbc:mysql://localhost:3306/dbname"; // Replace 'dbname' with your database
        String user = "root"; // Replace with MySQL username
        String password = "password"; // Replace with MySQL password

        try (Connection con = DriverManager.getConnection(url, user, password);
             Statement stmt = con.createStatement();
             ResultSet rs = stmt.executeQuery("SELECT * FROM Employee")) {

            System.out.println("EmpID | Name  | Salary");
            System.out.println("----------------------");
            while (rs.next()) {
                System.out.println(rs.getInt("EmpID") + " | " +
                                   rs.getString("Name") + " | " +
                                   rs.getDouble("Salary"));
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}

```

OUTPUT:

![image](https://github.com/user-attachments/assets/4e04d546-c0e3-4793-b714-10191520b8cc)


# 2. Medium Level: CRUD Operations on Product Table

```

import java.sql.*;
import java.util.Scanner;

public class ProductCRUD {
    static Connection con;

    static {
        try {
            con = DriverManager.getConnection("jdbc:mysql://localhost:3306/dbname", "root", "password");
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    // Create Product
    static void createProduct(int id, String name, double price, int qty) throws SQLException {
        String query = "INSERT INTO Product VALUES (?, ?, ?, ?)";
        try (PreparedStatement ps = con.prepareStatement(query)) {
            ps.setInt(1, id);
            ps.setString(2, name);
            ps.setDouble(3, price);
            ps.setInt(4, qty);
            ps.executeUpdate();
            System.out.println("Product added successfully!");
        }
    }

    // Read Products
    static void readProducts() throws SQLException {
        try (Statement st = con.createStatement();
             ResultSet rs = st.executeQuery("SELECT * FROM Product")) {
            System.out.println("ProductID | Name   | Price  | Quantity");
            System.out.println("-------------------------------------");
            while (rs.next()) {
                System.out.println(rs.getInt(1) + " | " +
                                   rs.getString(2) + " | " +
                                   rs.getDouble(3) + " | " +
                                   rs.getInt(4));
            }
        }
    }

    // Delete Product
    static void deleteProduct(int id) throws SQLException {
        String query = "DELETE FROM Product WHERE ProductID = ?";
        try (PreparedStatement ps = con.prepareStatement(query)) {
            ps.setInt(1, id);
            ps.executeUpdate();
            System.out.println("Product deleted successfully!");
        }
    }

    public static void main(String[] args) throws SQLException {
        Scanner sc = new Scanner(System.in);
        while (true) {
            System.out.println("\n1. Add Product\n2. View Products\n3. Delete Product\n4. Exit");
            System.out.print("Choose an option: ");
            int choice = sc.nextInt();

            if (choice == 1) {
                System.out.print("Enter ID, Name, Price, Quantity: ");
                createProduct(sc.nextInt(), sc.next(), sc.nextDouble(), sc.nextInt());
            } else if (choice == 2) {
                readProducts();
            } else if (choice == 3) {
                System.out.print("Enter Product ID to delete: ");
                deleteProduct(sc.nextInt());
            } else {
                break;
            }
        }
        sc.close();
    }
}

```

OUTPUT:

![image](https://github.com/user-attachments/assets/15a0d597-186b-4d2f-8ba5-89069f994c11)


# 3. Hard Level: Student Management System (MVC)

```
import java.sql.*;
import java.util.Scanner;

// Model: Student Class
class Student {
    int studentID;
    String name, department;
    double marks;

    public Student(int id, String name, String dept, double marks) {
        this.studentID = id;
        this.name = name;
        this.department = dept;
        this.marks = marks;
    }
}

// Controller: Handles Database Operations
class StudentDAO {
    Connection con;

    // Constructor: Establish Database Connection
    public StudentDAO() {
        try {
            con = DriverManager.getConnection("jdbc:mysql://localhost:3306/dbname", "root", "password");
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    // Insert Student into Database
    public void insertStudent(Student s) {
        try {
            String query = "INSERT INTO Student VALUES (?, ?, ?, ?)";
            PreparedStatement ps = con.prepareStatement(query);
            ps.setInt(1, s.studentID);
            ps.setString(2, s.name);
            ps.setString(3, s.department);
            ps.setDouble(4, s.marks);
            ps.executeUpdate();
            System.out.println("Student added successfully!");
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }

    // Fetch and Display All Students
    public void fetchStudents() {
        try {
            Statement st = con.createStatement();
            ResultSet rs = st.executeQuery("SELECT * FROM Student");
            System.out.println("ID | Name  | Department | Marks");
            System.out.println("-------------------------------");
            while (rs.next()) {
                System.out.println(rs.getInt(1) + " | " +
                                   rs.getString(2) + " | " +
                                   rs.getString(3) + " | " +
                                   rs.getDouble(4));
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}

// View: Main Class with Menu-Driven System
public class StudentManagement {
    public static void main(String[] args) {
        StudentDAO dao = new StudentDAO();
        Scanner sc = new Scanner(System.in);

        while (true) {
            System.out.println("\n1. Add Student\n2. View Students\n3. Exit");
            System.out.print("Choose an option: ");
            int choice = sc.nextInt();

            if (choice == 1) {
                System.out.print("Enter ID, Name, Department, Marks: ");
                Student s = new Student(sc.nextInt(), sc.next(), sc.next(), sc.nextDouble());
                dao.insertStudent(s);
            } else if (choice == 2) {
                dao.fetchStudents();
            } else {
                System.out.println("Exiting program...");
                break;
            }
        }
        sc.close();
    }
}

Before running the program, create the Student table in MySQL:

CREATE DATABASE dbname;
USE dbname;

CREATE TABLE Student (
    studentID INT PRIMARY KEY,
    name VARCHAR(50),
    department VARCHAR(50),
    marks DOUBLE
);

```

OUTPUT:

![image](https://github.com/user-attachments/assets/561516eb-8123-4934-bb2b-dca9c2a6e9e1)


