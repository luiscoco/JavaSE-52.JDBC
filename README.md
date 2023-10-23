# JavaSE-JDBC

JDBC, which stands for Java Database Connectivity, is a Java-based API that allows Java applications to interact with databases, both SQL and NoSQL. 

I'll provide you with a brief overview and examples for both SQL and NoSQL databases.

# 1. HOW TO CREATE A JDBC with SQL (Relational Database) WITH A DOCKER MYSQL CONTAINER

First create the MySQL docker container

Running MySQL Docker Container

```bash
docker run -d --name mysql-container -p 3307:3306 -e MYSQL_ROOT_PASSWORD=1234 -e MYSQL_DATABASE=luisprueba -e MYSQL_USER=luiscoco -e MYSQL_PASSWORD=1234 mysql:latest
```
Explanation:

-d: Runs the container in the background.

--name mysql-container: Names the container as mysql-container.

-p 3307:3306: Maps port 3307 on your machine to port 3306 in the container.

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
            String url = "jdbc:mysql://localhost:3307/";
            String user = "luiscoco";
            String password = "1234";

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

### IMPORTANT NOTE: download the MySQL JDBC driver

You need to download the MySQL JDBC driver and include it in your project.

Download the MySQL Connector/J JDBC driver from the official MySQL website: MySQL Connector/J.

https://dev.mysql.com/downloads/connector/j/

Select operating system -> Platform independent

![image](https://github.com/luiscoco/JavaSE-52.JDBC/assets/32194879/4b87766f-dab7-4582-a7ac-2638f5a0eb88)

Once downloaded, extract the contents of the ZIP file. You will find a JAR file named something like mysql-connector-java-x.x.x.jar (the version number may vary).

Move the mysql-connector-java-x.x.x.jar file to a location in your project, for example, a lib folder.

![image](https://github.com/luiscoco/JavaSE-52.JDBC/assets/32194879/c9c00145-fad2-4a0d-88f9-f195ba83f8eb)

![image](https://github.com/luiscoco/JavaSE-52.JDBC/assets/32194879/0c30c642-1f97-42b1-a601-5c54e95ff620)

### How to run the application in VSCode

Open a terminal window in VSCode and run the commands:

```
PS C:\javatest\FirstProject> javac -cp lib/mysql-connector-j-8.1.0.jar src/MySQLExample.java
```

```
PS C:\javatest\FirstProject> java -cp "src;lib/mysql-connector-j-8.1.0.jar" MySQLExample
```

# 2. HOW TO CREATE A JDBC with SQL (Relational Database) WITH A MYSQL WORKBENCH ALREADY INSTALLED IN YOUR LOCAL COMPUTER




# 3. JDBC with NoSQL (MongoDB)

To run MongoDB in a Docker container and create a database along with the modifications you requested, follow these steps:

## Run MongoDB in a Docker Container:

Ensure you have Docker installed on your machine.

Open a terminal and run the following command to start a MongoDB container:

```bash
docker run -d -p 27017:27017 --name my-mongodb mongo
```

This command pulls the official MongoDB image from Docker Hub and runs it in detached mode (-d) with port mapping (-p 27017:27017). 

The container is named my-mongodb.

## Java Code:

Now, let's modify the Java code to create a new database and use the Docker container's IP address.

```java
import com.mongodb.MongoClient;
import com.mongodb.client.MongoCollection;
import com.mongodb.client.MongoDatabase;
import org.bson.Document;

public class MongoDBExample {
    public static void main(String[] args) {
        // Connect to MongoDB server (assuming it's running in a Docker container on localhost)
        try (MongoClient mongoClient = new MongoClient("localhost", 27017)) {

            // Create a new database named "mydatabase"
            MongoDatabase database = mongoClient.getDatabase("mydatabase");
            System.out.println("Database created successfully!");

            // Get a collection (similar to a table in relational databases)
            MongoCollection<Document> collection = database.getCollection("mycollection");

            // Insert a document
            Document document = new Document("name", "John Doe")
                    .append("age", 30)
                    .append("city", "New York");

            collection.insertOne(document);
            System.out.println("Document inserted successfully!");

            // Query the document
            Document query = new Document("name", "John Doe");
            Document result = collection.find(query).first();

            if (result != null) {
                System.out.println("Query result: " + result.toJson());
            } else {
                System.out.println("Document not found.");
            }

        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

Note that we changed the database name to "mydatabase" and assumed the Docker container is running on localhost.

## Run the Java Code:

Compile and run the modified Java code. It should connect to the MongoDB Docker container, create a new database, insert a document, and query it back.

## Clean Up:

After you're done, you can stop and remove the MongoDB Docker container:

```bash
docker stop my-mongodb
docker rm my-mongodb
```
Now you have a complete example that not only interacts with MongoDB using Java but also creates a new database and runs MongoDB in a Docker container.
