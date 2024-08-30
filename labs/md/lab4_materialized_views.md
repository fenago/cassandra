### Lab 4: Working with Apache Cassandra in Zeppelin

**Objective**:  
In this lab, you will learn how to start Apache Cassandra within an Apache Zeppelin notebook, create a new keyspace, table, and materialized view using CQL (Cassandra Query Language). This hands-on lab will deepen your understanding of Cassandra's partitioning and materialized views.

#### Step 1: Open the Notebook

1. **Launch Apache Zeppelin**.
   - Ensure that your Apache Zeppelin instance is running.

2. **Open the Notebook**:
   - Locate and open the notebook named `02_Partitions_Solution`.

#### Step 2: Start Cassandra

1. **Run the Paragraph**:
   - In the notebook, find the paragraph that contains the following code:

   ```sh
   %sh
   STATUS="$(service cassandra status)"

   if [[ $STATUS == *"is running"* ]]; then
       echo "Cassandra is running"
   else 
       echo "Cassandra not running .... Starting"  
       service cassandra restart > /dev/null 2>&1 &
       echo " Started"  
   fi
   ```

   - Run this paragraph to check if Cassandra is running. If it is not running, this script will start the Cassandra service.

#### Step 3: Create a New Keyspace

1. **Create a New Paragraph**:
   - Below the existing paragraphs, create a new paragraph in Zeppelin.

2. **Define a New Keyspace**:
   - In the new paragraph, write the CQL command to create a new keyspace. For example:

   ```sql
   %cassandra
   CREATE KEYSPACE lab4_keyspace 
   WITH replication = {'class': 'SimpleStrategy', 'replication_factor': '1'};
   ```

   - Run the paragraph to create the keyspace.

#### Step 4: Create a Table

1. **Create a New Paragraph**:
   - Add another new paragraph below the keyspace creation.

2. **Create a Table**:
   - In the new paragraph, write the CQL command to create a new table within the keyspace. For example:

   ```sql
   %cassandra
   USE lab4_keyspace;

   CREATE TABLE users (
       user_id UUID PRIMARY KEY,
       name TEXT,
       age INT,
       email TEXT
   );
   ```

   - Run the paragraph to create the table.

#### Step 5: Create a Materialized View

1. **Create a New Paragraph**:
   - Add another paragraph below the table creation.

2. **Create a Materialized View**:
   - In the new paragraph, write the CQL command to create a materialized view. For example:

   ```sql
   %cassandra
   CREATE MATERIALIZED VIEW users_by_email AS
   SELECT * FROM users
   WHERE email IS NOT NULL 
   PRIMARY KEY (email, user_id);
   ```

   - Run the paragraph to create the materialized view.

### Conclusion:

Upon completing this lab, you should be able to start Cassandra within Zeppelin, create a new keyspace, define a table, and establish a materialized view. Ensure you verify each step to confirm the successful creation of each object in Cassandra.

### Lab 4 Addendum: Inserting Data and Querying with CQL

**Objective**:  
In this addendum, you will insert data into the table you created in Lab 4 and run several CQL queries to understand how the table and the materialized view work in Cassandra. Additionally, you'll learn how to handle queries on non-primary key columns using the `ALLOW FILTERING` clause and creating secondary indexes.

#### Step 6: Insert Data into the Table

1. **Create a New Paragraph**:
   - Below the previous paragraphs, create a new paragraph in Zeppelin.

2. **Insert Data**:
   - In the new paragraph, write the CQL command to insert data into the `users` table. For example:

   ```sql
   %cassandra
   USE lab4_keyspace;

   INSERT INTO users (user_id, name, age, email) 
   VALUES (uuid(), 'Alice Smith', 30, 'alice@example.com');

   INSERT INTO users (user_id, name, age, email) 
   VALUES (uuid(), 'Bob Johnson', 25, 'bob@example.com');

   INSERT INTO users (user_id, name, age, email) 
   VALUES (uuid(), 'Charlie Brown', 35, 'charlie@example.com');
   ```

   - Run the paragraph to insert the data.

#### Step 7: Query the Table and Materialized View

1. **Create a New Paragraph**:
   - Add a new paragraph below the data insertion.

2. **Query the Users Table**:
   - In the new paragraph, write a CQL command to query all records from the `users` table. For example:

   ```sql
   %cassandra
   SELECT * FROM users;
   ```

   - Run the paragraph to see the data you inserted.

3. **Query the Materialized View**:
   - Create another new paragraph and write a CQL command to query the materialized view `users_by_email`. For example:

   ```sql
   %cassandra
   SELECT * FROM users_by_email;
   ```

   - Run the paragraph to see how the data is organized in the materialized view.

#### Step 8: Handling Queries on Non-Primary Key Columns

1. **Filtering by Age with ALLOW FILTERING**:
   - To query the table by a non-primary key column, such as `age`, create a new paragraph and write:

   ```sql
   %cassandra
   SELECT * FROM users WHERE age = 30 ALLOW FILTERING;
   ```

   - Run the paragraph. This will retrieve the data filtered by age, but remember that using `ALLOW FILTERING` can lead to performance issues.

2. **Creating a Secondary Index**:
   - To optimize queries on the `age` column, you can create a secondary index. Create a new paragraph and write:

   ```sql
   %cassandra
   CREATE INDEX ON users (age);
   ```

   - Run this command to create the index.

3. **Query Using the Index**:
   - After creating the index, you can query the `age` column without needing `ALLOW FILTERING`. Create a new paragraph and write:

   ```sql
   %cassandra
   SELECT * FROM users WHERE age = 30;
   ```

   - Run the paragraph to see how the query performs with the index in place.

### Explanation

- **ALLOW FILTERING**: This clause enables Cassandra to process queries that filter on non-primary key columns. However, this can impact performance, especially with large datasets.

- **Secondary Index**: Creating a secondary index on a non-primary key column, like `age`, allows more efficient querying on that column, but should be used judiciously due to potential performance trade-offs.

### Conclusion

This addendum demonstrates how to insert data into your Cassandra tables, handle queries on non-primary key columns using `ALLOW FILTERING`, and optimize such queries with secondary indexes. Understanding these concepts will help you design more efficient and effective queries in Cassandra.

### Materialized Views in Cassandra

**Overview**:  
Materialized views in Cassandra are a powerful feature that allows you to automatically replicate data from a base table into a new table with a different primary key. This can be incredibly useful when you need to query the same data in multiple ways without maintaining several copies of the data manually.

#### How Materialized Views Work

When you create a materialized view, Cassandra essentially creates a new table that is automatically kept in sync with the base table. Any changes (inserts, updates, deletes) made to the base table are automatically propagated to the materialized view, ensuring data consistency across both.

For example, if you have a `users` table where the primary key is `user_id`, but you often need to query users by their `email`, you might create a materialized view with `email` as the primary key. This allows you to efficiently retrieve users by email without scanning the entire `users` table.

**Example**:

```sql
CREATE MATERIALIZED VIEW users_by_email AS
SELECT * FROM users
WHERE email IS NOT NULL
PRIMARY KEY (email, user_id);
```

In this example:
- The base table is `users`.
- The materialized view `users_by_email` allows queries to be executed efficiently using the `email` column as the primary key.

#### When to Use Materialized Views

1. **Query Optimization**:  
   Use materialized views when you need to optimize queries that filter by columns that are not part of the primary key in the base table. Instead of using `ALLOW FILTERING`, which can be slow and resource-intensive, a materialized view provides a more efficient way to retrieve data.

2. **Reducing Data Duplication**:  
   Materialized views automatically stay in sync with the base table, which reduces the need for manually maintaining multiple tables with similar data. This helps prevent inconsistencies and simplifies your data model.

3. **Simplifying Application Logic**:  
   By using materialized views, you can simplify your application logic. Instead of implementing custom indexing or data duplication in your application code, you can rely on Cassandra to manage these complexities for you.

#### When Not to Use Materialized Views

1. **High Write Loads**:  
   Materialized views can add overhead to write operations because every write to the base table must also be propagated to all associated materialized views. If your application experiences high write loads, this could lead to performance degradation.

2. **Complex Base Table Schemas**:  
   If your base table schema is complex, maintaining multiple materialized views can increase the risk of errors and complicate schema management. Changes to the base table schema may require corresponding changes to the materialized views, adding to maintenance efforts.

3. **Read Performance is Not a Priority**:  
   If read performance is not a critical factor for your application, using materialized views might be unnecessary. In such cases, simpler query techniques or using `ALLOW FILTERING` for occasional queries might be sufficient without the overhead of materialized views.

4. **Limited Use Cases**:  
   If the query pattern that benefits from the materialized view is rare or infrequent, it might not justify the additional complexity and overhead. Instead, consider using alternative methods like creating secondary indexes or adjusting your application's query logic.

### Conclusion

Materialized views in Cassandra are a powerful tool for optimizing query performance and simplifying data management, especially in scenarios where you need to query the same data using different keys. However, they should be used judiciously, considering the trade-offs in write performance and maintenance complexity. Understanding when to use and when to avoid materialized views is crucial for designing an efficient and scalable Cassandra database.
