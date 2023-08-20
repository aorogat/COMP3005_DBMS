## Set Up PostgreSQL Database:

### Step 1: 
Install PostgreSQL if you haven't already. You can download it from [here](https://www.postgresql.org/download/).

### Step 2: 
Open `pgAdmin`, the management tool for PostgreSQL.

### Step 3: 
Create a new database called `DemoDB`.
- Right-click on `Databases` → `Create` → `Database…`
- Enter `DemoDB` in the Database field.

### Step 4: 
Create a table called `users`.
- Open a SQL Query tool and run:
\```
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    name VARCHAR(50),
    email VARCHAR(100)
);
\```

## Java CRUD Application using JDBC:

### Step 1: 
Set up a new Java project in your preferred IDE.

### Step 2: 
Add the JDBC driver to your project.
- Download the PostgreSQL JDBC driver from [here](https://jdbc.postgresql.org/).
- Add the `.jar` file to your project as an external library.

### Step 3: 
Java Code:
\```
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.SQLException;
import java.util.Scanner;


public class DatabaseOperations {

    private final String url = "jdbc:postgresql://localhost:5432/DemoDB";
    private final String user = "YOUR_USERNAME";
    private final String password = "YOUR_PASSWORD";

    // Add a user
    public void addUser(String name, String email) {
        String SQL = "INSERT INTO users(name,email) VALUES(?,?)";

        try (Connection conn = DriverManager.getConnection(url, user, password);
             PreparedStatement pstmt = conn.prepareStatement(SQL)) {

            pstmt.setString(1, name);
            pstmt.setString(2, email);
            pstmt.executeUpdate();
            System.out.println("User added successfully!");

        } catch (SQLException ex) {
            System.out.println(ex.getMessage());
        }
    }

    // Update user's email based on name
    public void modifyUserEmail(String name, String newEmail) {
        String SQL = "UPDATE users SET email=? WHERE name=?";

        try (Connection conn = DriverManager.getConnection(url, user, password);
             PreparedStatement pstmt = conn.prepareStatement(SQL)) {

            pstmt.setString(1, newEmail);
            pstmt.setString(2, name);
            pstmt.executeUpdate();
            System.out.println("User email updated!");

        } catch (SQLException ex) {
            System.out.println(ex.getMessage());
        }
    }

    // Delete user based on name
    public void deleteUser(String name) {
        String SQL = "DELETE FROM users WHERE name=?";

        try (Connection conn = DriverManager.getConnection(url, user, password);
             PreparedStatement pstmt = conn.prepareStatement(SQL)) {

            pstmt.setString(1, name);
            pstmt.executeUpdate();
            System.out.println("User deleted!");

        } catch (SQLException ex) {
            System.out.println(ex.getMessage());
        }
    }

    public static void main(String[] args) {
      DatabaseOperations dbOps = new DatabaseOperations();
      Scanner scanner = new Scanner(System.in);
      
      // Add a user
      System.out.println("Would you like to add a user? (yes/no)");
      if (scanner.nextLine().equalsIgnoreCase("yes")) {
          dbOps.addUser("John Doe", "john@example.com");
      }
  
      // Modify user's email
      System.out.println("Would you like to modify a user's email? (yes/no)");
      if (scanner.nextLine().equalsIgnoreCase("yes")) {
          dbOps.modifyUserEmail("John Doe", "john.doe@example.com");
      }
  
      // Delete user
      System.out.println("Would you like to delete a user? (yes/no)");
      if (scanner.nextLine().equalsIgnoreCase("yes")) {
          dbOps.deleteUser("John Doe");
      }
      scanner.close();
  }
}
\```

Replace `YOUR_USERNAME` and `YOUR_PASSWORD` with your PostgreSQL username and password.

### Step 4: 
Run the Java program. If all goes well, it will add a user, update the user's email, and then delete the user.
