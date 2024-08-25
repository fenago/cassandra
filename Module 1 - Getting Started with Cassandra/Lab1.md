<a href="https://youtu.be/GMfTnPCXdec" target="_blank">
    <img src="https://img.youtube.com/vi/GMfTnPCXdec/0.jpg" alt="Get A Free Cassandra Environment Courtesy of DataStax - Dr. Lee">
</a>



### **Lab: Getting Started with DataStax Astra and CQL Console**

---

**Learning Objectives:**

1. Understand the basics of DataStax Astra and the CQL (Cassandra Query Language) Console.
2. Learn how to create keyspaces and tables.
3. Practice inserting, querying, updating, and deleting data.
4. Explore indexing and querying using advanced CQL commands.

---

### **Step 1: Set Up Your Environment**

1. **Sign in to DataStax Astra:**
   - Visit the [DataStax Astra website](https://astra.datastax.com).
   - Log in with your credentials.

2. **Create a Database:**
   - Navigate to the "Databases" section.
   - Click "Create Database".
   - **Region:** Choose `europe-west-1` (as suggested for EMEA).
   - **Database Name:** Enter `killrvideocluster`.
   - Choose the other fields as per your preference or follow the default recommendations.
   - Click "Create Database". This may take a few minutes.

3. **Access the CQL Console:**
   - Once your database is ready, go to the "CQL Console" tab within your database dashboard.

---

### **Step 2: Connecting to Your Database**

1. **Access CQL Console:**
   - In the CQL Console, your database should already be connected. If not, you might need to click "CQL Console" from the Astra dashboard and wait for the connection.

2. **Check Connection:**
   - In the CQL Console, type:
     ```sql
     SHOW HOST;
     ```
   - You should see the current host details where your database is hosted.

---

### **Step 3: Basic CQL Commands**

#### **1. Create a Keyspace:**
   - A keyspace in Cassandra is similar to a schema in a relational database.
   - To create a keyspace, type the following command:
     ```sql
     CREATE KEYSPACE IF NOT EXISTS killrvideo 
     WITH replication = {'class': 'SimpleStrategy', 'replication_factor': 1};
     ```
   - Use the created keyspace:
     ```sql
     USE killrvideo;
     ```

#### **2. Create a Table:**
   - Let's create a table to store user data:
     ```sql
     CREATE TABLE users (
         userid UUID PRIMARY KEY,
         firstname TEXT,
         lastname TEXT,
         email TEXT
     );
     ```

#### **3. Insert Data into the Table:**
   - Insert a few records into the `users` table:
     ```sql
     INSERT INTO users (userid, firstname, lastname, email) 
     VALUES (uuid(), 'John', 'Doe', 'john.doe@example.com');

     INSERT INTO users (userid, firstname, lastname, email) 
     VALUES (uuid(), 'Jane', 'Smith', 'jane.smith@example.com');
     ```

#### **4. Query Data from the Table:**
   - Retrieve all users:
     ```sql
     SELECT * FROM users;
     ```

   - Retrieve a specific user by email:
     ```sql
     SELECT * FROM users WHERE email = 'john.doe@example.com' ALLOW FILTERING;
     ```

#### **5. Update Data:**
   - Update a user's email:
     ```sql
     UPDATE users SET email = 'john.newemail@example.com' WHERE userid = {replace_with_userid};
     ```

#### **6. Delete Data:**
   - Delete a user from the table:
     ```sql
     DELETE FROM users WHERE userid = {replace_with_userid};
     ```

#### **7. Create an Index:**
   - To allow fast lookups on the `lastname` column, create an index:
     ```sql
     CREATE INDEX ON users (lastname);
     ```

   - Query using the index:
     ```sql
     SELECT * FROM users WHERE lastname = 'Smith';
     ```

---

### **Step 4: Advanced CQL Operations**

#### **1. Create a Table with a Composite Primary Key:**
   - Let's create a table to store video data with a composite primary key:
     ```sql
     CREATE TABLE videos (
         videoid UUID,
         title TEXT,
         userid UUID,
         upload_date TIMESTAMP,
         PRIMARY KEY (userid, upload_date)
     );
     ```

#### **2. Insert Data with TTL (Time-To-Live):**
   - Insert data that automatically expires after a certain period:
     ```sql
     INSERT INTO videos (videoid, title, userid, upload_date)
     VALUES (uuid(), 'Video 1', {replace_with_userid}, toTimestamp(now()))
     USING TTL 86400;  -- expires in 1 day
     ```

#### **3. Query with Filtering and Ordering:**
   - Retrieve videos uploaded by a specific user in the last 24 hours:
     ```sql
     SELECT * FROM videos WHERE userid = {replace_with_userid} 
     AND upload_date > dateOf(now() - 86400000)
     ORDER BY upload_date DESC;
     ```

#### **4. Deleting Expired Data:**
   - Manually remove data older than a certain date:
     ```sql
     DELETE FROM videos WHERE userid = {replace_with_userid} 
     AND upload_date < '2023-01-01';
     ```

---

### **Step 5: Practice Exercises**

1. **Exercise 1:**
   - Create a table to store comments on videos, with a composite primary key consisting of `videoid` and `commentid`.
   - Insert at least three comments on different videos.
   - Retrieve all comments for a specific video, sorted by the most recent first.

2. **Exercise 2:**
   - Create a table to store user actions (like, share, etc.) on videos.
   - Make sure the table can efficiently query all actions on a specific video.
   - Insert several user actions and perform a query to retrieve actions on a video.

3. **Exercise 3:**
   - Create an index on a non-primary key column in one of your tables.
   - Test querying the table using this index to retrieve results based on this column.

---

### **Step 6: Wrapping Up**

- **Review:** Go over the key concepts covered, such as keyspaces, tables, primary keys, secondary indexes, and CQL commands.
- **Clean Up:** Optionally, clean up the database by dropping tables or keyspaces you no longer need:
  ```sql
  DROP TABLE users;
  DROP KEYSPACE killrvideo;
  ```
- **Further Learning:** Explore more advanced topics like user-defined types, materialized views, and Cassandra’s eventual consistency model.

---

### **Conclusion**

By completing this lab, you’ve gained hands-on experience with the DataStax Astra environment and the Cassandra Query Language. Continue exploring CQL to fully harness the power of Cassandra databases.

If you need more advanced labs or have any questions, feel free to reach out!

### **Addendum: Additional CQL Commands for Beginners**

This addendum provides a comprehensive list of CQL commands focusing primarily on reading and querying data. These commands will help you further explore and manipulate your data in Cassandra.

---

### **Basic Read Commands**

#### 1. **Selecting All Data from a Table**
   - Retrieve all records from a table.
     ```sql
     SELECT * FROM users;
     ```

#### 2. **Selecting Specific Columns**
   - Retrieve specific columns from a table.
     ```sql
     SELECT firstname, lastname FROM users;
     ```

#### 3. **Filtering Data with WHERE Clause**
   - Filter results based on a specific condition.
     ```sql
     SELECT * FROM users WHERE lastname = 'Doe' ALLOW FILTERING;
     ```
   - **Note:** The `ALLOW FILTERING` clause is required if filtering on a non-primary key column.

#### 4. **Filtering Data with IN Clause**
   - Retrieve rows where the value matches any value in a list.
     ```sql
     SELECT * FROM users WHERE lastname IN ('Doe', 'Smith') ALLOW FILTERING;
     ```

#### 5. **Using LIMIT Clause**
   - Limit the number of rows returned by the query.
     ```sql
     SELECT * FROM users LIMIT 10;
     ```

---

### **Advanced Read Commands**

#### 1. **Ordering Results**
   - Order results by a specific column.
     ```sql
     SELECT * FROM users ORDER BY lastname ASC;
     ```

   - **Note:** Ordering is typically only supported on columns that are part of the primary key or clustering columns.

#### 2. **Using the COUNT Function**
   - Count the number of rows in a table.
     ```sql
     SELECT COUNT(*) FROM users;
     ```

#### 3. **Using Aggregation Functions**
   - Calculate the maximum, minimum, and average values of a column (if applicable).
     ```sql
     SELECT MAX(age), MIN(age), AVG(age) FROM users;
     ```

#### 4. **Using the TTL Function**
   - Retrieve the remaining TTL (Time-to-Live) for a column.
     ```sql
     SELECT TTL(email) FROM users WHERE userid = {replace_with_userid};
     ```

---

### **Working with Indexes**

#### 1. **Creating an Index**
   - Create an index on a non-primary key column to allow efficient querying.
     ```sql
     CREATE INDEX ON users (email);
     ```

#### 2. **Querying Using an Index**
   - After creating an index, you can query the indexed column without `ALLOW FILTERING`.
     ```sql
     SELECT * FROM users WHERE email = 'john.doe@example.com';
     ```

#### 3. **Dropping an Index**
   - Remove an index if it’s no longer needed.
     ```sql
     DROP INDEX users_email_idx;
     ```

---

### **Working with Materialized Views**

Materialized views provide a way to create alternative queries on the same data without duplicating the data.

#### 1. **Creating a Materialized View**
   - Create a materialized view to query data in a different way.
     ```sql
     CREATE MATERIALIZED VIEW users_by_email AS
     SELECT * FROM users
     WHERE email IS NOT NULL
     PRIMARY KEY (email, userid);
     ```

#### 2. **Querying a Materialized View**
   - You can query a materialized view just like a table.
     ```sql
     SELECT * FROM users_by_email WHERE email = 'john.doe@example.com';
     ```

#### 3. **Dropping a Materialized View**
   - Drop a materialized view when it's no longer needed.
     ```sql
     DROP MATERIALIZED VIEW users_by_email;
     ```

---

### **Joins and Complex Queries**

Cassandra does not support traditional joins, but you can achieve similar results using denormalization and materialized views.

#### 1. **Using Nested Queries**
   - Execute a query using the results from another query (not directly supported, but achievable via application logic).
     ```sql
     -- Example: First query user ID, then use it in another query.
     SELECT userid FROM users WHERE email = 'john.doe@example.com';
     ```

#### 2. **Working with Collections**
   - Cassandra supports collections like lists, sets, and maps within a table.

   **Example: Creating a Table with a Collection**
   ```sql
   CREATE TABLE user_activities (
       userid UUID PRIMARY KEY,
       activities LIST<TEXT>
   );
   ```

   **Inserting Data into a Collection**
   ```sql
   UPDATE user_activities SET activities = activities + ['login'] WHERE userid = {replace_with_userid};
   ```

   **Querying a Collection**
   ```sql
   SELECT activities FROM user_activities WHERE userid = {replace_with_userid};
   ```

---

### **Advanced Querying Techniques**

#### 1. **Querying with Composite Keys**
   - When a table has a composite primary key, you can use all or part of the key in the `WHERE` clause.
     ```sql
     SELECT * FROM videos WHERE userid = {replace_with_userid} AND upload_date > '2023-01-01';
     ```

#### 2. **Querying Time Series Data**
   - For tables storing time-series data, you can query based on date ranges.
     ```sql
     SELECT * FROM videos WHERE userid = {replace_with_userid} 
     AND upload_date > '2023-01-01' AND upload_date < '2023-02-01';
     ```

#### 3. **Using Token Functions**
   - Token functions allow querying data across multiple nodes based on the partition key.
     ```sql
     SELECT * FROM users WHERE token(userid) > token({some_value});
     ```

---

### **Miscellaneous Commands**

#### 1. **Describe Commands**
   - Describe the structure of a table.
     ```sql
     DESCRIBE TABLE users;
     ```
   - List all tables in the current keyspace.
     ```sql
     DESCRIBE TABLES;
     ```

   - Describe the keyspace.
     ```sql
     DESCRIBE KEYSPACE killrvideo;
     ```

#### 2. **Tracing Queries**
   - Enable tracing for a query to diagnose performance issues.
     ```sql
     TRACING ON;
     SELECT * FROM users;
     TRACING OFF;
     ```

   - View trace results.
     ```sql
     SELECT * FROM system_traces.events WHERE session_id = {session_id};
     ```

#### 3. **Using Batch Statements**
   - Execute multiple commands in a single batch (use with caution).
     ```sql
     BEGIN BATCH
         INSERT INTO users (userid, firstname, lastname, email) VALUES (uuid(), 'Alice', 'Wonderland', 'alice@example.com');
         UPDATE users SET email = 'alice.new@example.com' WHERE userid = {replace_with_userid};
     APPLY BATCH;
     ```

---

### **Conclusion**

This addendum provides a rich set of CQL commands to help you get more comfortable with querying data in Cassandra. As you become more proficient, you can explore even more advanced concepts like user-defined types, triggers, and custom functions.

Remember, Cassandra is designed to handle massive amounts of data efficiently, so many operations (like joins or aggregations) that are common in SQL might need to be rethought in the context of Cassandra's architecture. Happy querying!

### **Addendum: Cleaning Up Resources in DataStax Astra**

This addendum provides instructions on how to clean up and delete the resources that were created during the course of this lab. Cleaning up resources is important to avoid unnecessary charges and to keep your environment tidy.

---

### **Step 1: Deleting Tables**

To delete the tables that were created, follow these steps:

#### 1. **Ensure You Are Using the Correct Keyspace**
   - Before deleting tables, make sure you’re using the correct keyspace:
     ```sql
     USE killrvideo;
     ```

#### 2. **Drop Individual Tables**
   - Drop each table using the `DROP TABLE` command:
     ```sql
     DROP TABLE users;
     ```

   - Repeat this for any other tables you’ve created, for example:
     ```sql
     DROP TABLE videos;
     DROP TABLE user_activities;
     ```

#### 3. **Drop Materialized Views (If Created)**
   - If you created any materialized views, drop them using:
     ```sql
     DROP MATERIALIZED VIEW users_by_email;
     ```

---

### **Step 2: Deleting Indexes**

If you created any secondary indexes, you can drop them as follows:

#### 1. **Drop Specific Indexes**
   - Use the `DROP INDEX` command to remove an index:
     ```sql
     DROP INDEX users_email_idx;
     ```

   - Replace `users_email_idx` with the actual index name if different.

---

### **Step 3: Dropping Keyspaces**

If you want to completely remove the keyspace along with all the tables and data within it, you can drop the entire keyspace.

#### 1. **Drop the Keyspace**
   - Use the following command to drop the keyspace:
     ```sql
     DROP KEYSPACE killrvideo;
     ```

   - This command will delete the keyspace and all tables, views, and indexes within it.

---

### **Step 4: Deleting the Database from Astra**

Finally, if you want to completely remove the database from DataStax Astra, follow these steps:

#### 1. **Log in to DataStax Astra**
   - Go to the [DataStax Astra dashboard](https://astra.datastax.com) and log in with your credentials.

#### 2. **Navigate to Your Database**
   - In the Astra dashboard, go to the "Databases" section.
   - Find your `killrvideocluster` database.

#### 3. **Delete the Database**
   - Click on the database name to open its details.
   - Click on the "Settings" tab.
   - Scroll down to find the "Delete Database" button.
   - Confirm the deletion when prompted.

   **Warning:** Deleting the database will remove all data, keyspaces, tables, and configurations permanently. Ensure that you no longer need the data before proceeding.

---

### **Step 5: Verifying Cleanup**

After following the steps above:

1. **Check Keyspaces:**
   - Run `DESCRIBE KEYSPACES;` in the CQL Console to ensure the `killrvideo` keyspace has been deleted.

2. **Verify Database Deletion:**
   - Refresh the Astra dashboard to ensure that the `killrvideocluster` database no longer appears in the list of your databases.

---

### **Conclusion**

This addendum ensures that all resources created during your lab exercises are thoroughly cleaned up, helping you avoid unnecessary costs and keeping your environment organized. Remember, when working with cloud resources, it's always a good practice to clean up what you no longer need.
