# JavaSE-JDBC

JDBC, which stands for Java Database Connectivity, is a Java-based API that allows Java applications to interact with databases, both SQL and NoSQL. 

I'll provide you with a brief overview and examples for both SQL and NoSQL databases.

# JDBC with SQL (Relational Database)

### Step 1: Import JDBC Packages

```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.Statement;
import java.sql.ResultSet;
```

### Step 2: Connect to the Database

```java
// JDBC URL, username, and password of MySQL server
String url = "jdbc:mysql://localhost:3306/yourdatabase";
String user = "yourusername";
String password = "yourpassword";

// Establish a connection
Connection connection = DriverManager.getConnection(url, user, password);
```

### Step 3: Execute SQL Queries

```java
// Create a SQL statement
Statement statement = connection.createStatement();

// Execute a query
ResultSet resultSet = statement.executeQuery("SELECT * FROM yourtable");

// Process the result set
while (resultSet.next()) {
    // Retrieve data from the result set
    String column1 = resultSet.getString("column1");
    int column2 = resultSet.getInt("column2");

    // Do something with the data
    System.out.println("Column 1: " + column1 + ", Column 2: " + column2);
}
```

### Step 4: Close the Connection
```java
// Close the result set, statement, and connection
resultSet.close();
statement.close();
connection.close();
```

### Summary

First create the MySQL docker container

Running MySQL Docker Container

```bash
docker run -d --name mysql-container -p 3307:3306 -e MYSQL_ROOT_PASSWORD=1234 -e MYSQL_DATABASE=luisprueba -e MYSQL_USER=luiscoco -e MYSQL_PASSWORD=1234 mysql:latest
```
Explanation:

-d: Runs the container in the background.

--name mysql-container: Names the container as mysql-container.

-p 3306:3306: Maps port 3306 on your machine to port 3306 in the container.

-e MYSQL_ROOT_PASSWORD=yourpassword: Sets the root password for MySQL.

-e MYSQL_DATABASE=yourdatabase: Creates an initial database.

-e MYSQL_USER=yourusername -e MYSQL_PASSWORD=yourpassword: Creates a user with a password.

```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.Statement;

public class MySQLExample {
    public static void main(String[] args) {
        try {
            // Step 1: Import JDBC Packages
            // Import statements are at the beginning of the file

            // Step 2: Connect to the Database
            // JDBC URL, username, and password of MySQL server
            String url = "jdbc:mysql://localhost:3306/";
            String user = "yourusername";
            String password = "yourpassword";

            // Establish a connection
            Connection connection = DriverManager.getConnection(url, user, password);

            // Step 3: Create a Database and Table
            Statement createStatement = connection.createStatement();

            // Create the database if it doesn't exist
            createStatement.executeUpdate("CREATE DATABASE IF NOT EXISTS luisprueba");

            // Switch to the new database
            createStatement.executeUpdate("USE luisprueba");

            // Create a table with columns column1 and column2
            createStatement.executeUpdate("CREATE TABLE IF NOT EXISTS yourtable ("
                    + "column1 VARCHAR(255), "
                    + "column2 INT"
                    + ")");

            createStatement.close();

            // Step 4: Execute SQL Queries
            // Create a SQL statement
            Statement statement = connection.createStatement();

            // Insert some data into the table
            statement.executeUpdate("INSERT INTO yourtable (column1, column2) VALUES ('value1', 123)");

            // Execute a query
            ResultSet resultSet = statement.executeQuery("SELECT * FROM yourtable");

            // Process the result set
            while (resultSet.next()) {
                // Retrieve data from the result set
                String column1 = resultSet.getString("column1");
                int column2 = resultSet.getInt("column2");

                // Do something with the data
                System.out.println("Column 1: " + column1 + ", Column 2: " + column2);
            }

            // Step 5: Close the Connection
            // Close the result set, statement, and connection
            resultSet.close();
            statement.close();
            connection.close();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

# JDBC with NoSQL (MongoDB)

For NoSQL databases like MongoDB, you typically use a driver specific to that database. In this case, let's use the MongoDB Java driver.

### Step 1: Import MongoDB Driver

```java
import com.mongodb.client.MongoClient;
import com.mongodb.client.MongoClients;
import com.mongodb.client.MongoCollection;
import com.mongodb.client.MongoCursor;
import com.mongodb.client.MongoDatabase;
import org.bson.Document;
```

### Step 2: Connect to MongoDB

```java
Copy code
// Connect to MongoDB server
MongoClient mongoClient = MongoClients.create("mongodb://localhost:27017");

// Access a database
MongoDatabase database = mongoClient.getDatabase("yourmongodb");

// Access a collection
MongoCollection<Document> collection = database.getCollection("yourcollection");
```

### Step 3: Perform Operations

```java
Copy code
// Insert a document
Document document = new Document("key1", "value1")
                    .append("key2", 123);
collection.insertOne(document);

// Query documents
MongoCursor<Document> cursor = collection.find().iterator();
while (cursor.hasNext()) {
    Document resultDoc = cursor.next();
    // Process the document
    System.out.println(resultDoc.toJson());
}
```

### Step 4: Close the Connection

```java
// Close the MongoDB connection
mongoClient.close();
```
