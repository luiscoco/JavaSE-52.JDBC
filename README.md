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
