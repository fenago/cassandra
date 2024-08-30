### Lab: Working with Secondary Indexes in Cassandra

**Objective**:  
This lab will guide you through the process of creating and using secondary indexes in Cassandra. You will learn how to create secondary indexes, understand their limitations, and execute queries using them.

### Prerequisites

- Ensure that you have a running Cassandra instance.
- You should have a basic understanding of Cassandra data modeling and CQL (Cassandra Query Language).
- Make sure you have access to Apache Zeppelin for running CQL queries, or use a CQL shell (`cqlsh`).

### Step 1: Create a Keyspace and Table

1. **Create the Keyspace**:
   - Open a new paragraph in Zeppelin or use the CQL shell.

   ```sql
   %cassandra
   CREATE KEYSPACE IF NOT EXISTS lab_secondary_index 
   WITH REPLICATION = {'class': 'SimpleStrategy', 'replication_factor': 1};
   ```

   - This command creates a keyspace named `lab_secondary_index` with a replication factor of 1.

2. **Create a Table**:
   - Create a table within the `lab_secondary_index` keyspace.

   ```sql
   %cassandra
   USE lab_secondary_index;

   CREATE TABLE IF NOT EXISTS users (
       user_id UUID PRIMARY KEY,
       name TEXT,
       age INT,
       city TEXT
   );
   ```

   - This table `users` has `user_id` as the primary key and additional columns `name`, `age`, and `city`.

### Step 2: Insert Data into the Table

1. **Insert Sample Data**:
   - Insert some sample data into the `users` table.

   ```sql
   %cassandra
   INSERT INTO users (user_id, name, age, city) VALUES (uuid(), 'John Doe', 30, 'New York');
   INSERT INTO users (user_id, name, age, city) VALUES (uuid(), 'Jane Smith', 25, 'Los Angeles');
   INSERT INTO users (user_id, name, age, city) VALUES (uuid(), 'Alice Johnson', 28, 'Chicago');
   INSERT INTO users (user_id, name, age, city) VALUES (uuid(), 'Bob Brown', 35, 'Houston');
   INSERT INTO users (user_id, name, age, city) VALUES (uuid(), 'Eve Davis', 40, 'New York');
   ```

   - This populates the table with five users, each with different `user_id`, `name`, `age`, and `city` values.

### Step 3: Create a Secondary Index

1. **Create a Secondary Index on `city`**:
   - Now, create a secondary index on the `city` column.

   ```sql
   %cassandra
   CREATE INDEX ON users (city);
   ```

   - This creates a secondary index on the `city` column, allowing you to query users by city.

### Step 4: Query Using the Secondary Index

1. **Query by City**:
   - Now that the secondary index is created, you can query the `users` table using the `city` column.

   ```sql
   %cassandra
   SELECT * FROM users WHERE city = 'New York';
   ```

   - This query will return all users who live in `New York`.

2. **Query by City and Age**:
   - You can also combine the secondary index with other columns in your query.

   ```sql
   %cassandra
   SELECT * FROM users WHERE city = 'New York' AND age > 35;
   ```

   - This query returns all users who live in `New York` and are older than 35.

### Step 5: Understanding Secondary Index Performance

1. **Query with High Cardinality Column**:
   - Secondary indexes work best on low cardinality columns (columns with few distinct values). Let's try creating an index on the `name` column, which has high cardinality.

   ```sql
   %cassandra
   CREATE INDEX ON users (name);
   ```

   - Now, query by `name`:

   ```sql
   %cassandra
   SELECT * FROM users WHERE name = 'John Doe';
   ```

   - Notice that while this works, it's less efficient because `name` is a high cardinality column.

2. **Avoiding Inefficient Queries**:
   - **Inefficient Query Example**:
     ```sql
     %cassandra
     SELECT * FROM users WHERE age = 30;
     ```
   - This query will be inefficient without an index on `age`, as Cassandra would need to perform a full table scan.

   - **Add Index to Age**:
     ```sql
     %cassandra
     CREATE INDEX ON users (age);
     ```

   - **Run Query Again**:
     ```sql
     %cassandra
     SELECT * FROM users WHERE age = 30;
     ```
   - This should now be faster, but remember that indexing on high-cardinality columns like `age` can still be inefficient if the distribution of values is even.

### Step 6: Drop the Secondary Index

1. **Drop the Index**:
   - If you no longer need the secondary index, you can drop it.

   ```sql
   %cassandra
   DROP INDEX lab_secondary_index.users_city_idx;
   ```

   - Replace `lab_secondary_index.users_city_idx` with the actual name of your index if different.

### Step 7: Clean Up

1. **Drop the Table**:
   - If you wish to clean up the environment by dropping the `users` table:

   ```sql
   %cassandra
   DROP TABLE users;
   ```

2. **Drop the Keyspace**:
   - Finally, drop the keyspace to clean up everything:

   ```sql
   %cassandra
   DROP KEYSPACE lab_secondary_index;
   ```

### Conclusion

In this lab, you learned how to create and use secondary indexes in Cassandra, query the data using the indexed columns, and understand the performance implications of using secondary indexes. Secondary indexes can be a powerful tool in Cassandra, but they should be used with care, particularly with high cardinality columns, as they can lead to inefficient queries and increased overhead.
