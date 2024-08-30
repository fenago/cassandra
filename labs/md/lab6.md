### Lab 6: Exploring Cassandra with `nodetool` and CQL from Zeppelin

**Objective**:  
In this lab, you will set up a basic Cassandra environment, create tables with data, and explore various aspects of Cassandra using the `nodetool` and CQL (Cassandra Query Language) commands through Apache Zeppelin. This lab will ensure that each command returns meaningful and useful information.

#### Step 1: Setting Up the Environment

1. **Create Keyspace**:
   - Create a new paragraph in Zeppelin to create a keyspace for your tables:

   ```sql
   %cassandra
   CREATE KEYSPACE IF NOT EXISTS lab_keyspace WITH replication = {'class': 'SimpleStrategy', 'replication_factor': 1};
   ```

   - Run this paragraph to create the keyspace `lab_keyspace`.

2. **Use the Keyspace**:
   - Set the keyspace for the session:

   ```sql
   %cassandra
   USE lab_keyspace;
   ```

   - Run this paragraph to switch to the `lab_keyspace`.

3. **Create Tables**:
   - Create a `users` table in the keyspace:

   ```sql
   %cassandra
   CREATE TABLE IF NOT EXISTS users (
       user_id UUID PRIMARY KEY,
       name TEXT,
       age INT,
       email TEXT
   );
   ```

   - Create a `orders` table in the keyspace:
```
%cassandra
CREATE TABLE IF NOT EXISTS orders (
    order_id UUID PRIMARY KEY,
    user_id UUID,
    item TEXT,
    price DECIMAL,
    order_date TIMESTAMP
);
```
   ```

   - Run these paragraphs to create the `users` and `orders` tables.

4. **Insert Data into Tables**:
   - Insert sample data into the `users` table:

   ```sql
   %cassandra
   INSERT INTO users (user_id, name, age, email) VALUES (uuid(), 'John Doe', 30, 'john@example.com');
   INSERT INTO users (user_id, name, age, email) VALUES (uuid(), 'Jane Smith', 25, 'jane@example.com');
   INSERT INTO users (user_id, name, age, email) VALUES (uuid(), 'Alice Johnson', 28, 'alice@example.com');
   ```

   - Insert sample data into the `orders` table:

   ```sql
   %cassandra
   INSERT INTO orders (order_id, user_id, item, price, order_date) VALUES (uuid(), (SELECT user_id FROM users WHERE name='John Doe'), 'Laptop', 999.99, toTimestamp(now()));
   INSERT INTO orders (order_id, user_id, item, price, order_date) VALUES (uuid(), (SELECT user_id FROM users WHERE name='Jane Smith'), 'Smartphone', 499.99, toTimestamp(now()));
   INSERT INTO orders (order_id, user_id, item, price, order_date) VALUES (uuid(), (SELECT user_id FROM users WHERE name='Alice Johnson'), 'Tablet', 299.99, toTimestamp(now()));
   ```

   - Run these paragraphs to populate the `users` and `orders` tables with sample data.

#### Step 2: Exploring Cassandra with `nodetool`

1. **Check Cluster Status**:
   - Create a new paragraph to check the overall status of your Cassandra cluster using `nodetool`:

   ```sh
   %sh
   nodetool status
   ```

   - Run this paragraph to display the status of each node in the cluster, including information on the state (Up/Down), load, and the number of tokens.

2. **View Node Information**:
   - Use `nodetool` to view detailed information about the node you're connected to:

   ```sh
   %sh
   nodetool info
   ```

   - Run this paragraph to display information such as the node's ID, uptime, heap memory usage, data center, rack, and more.

3. **Check Ring Information**:
   - To view the token ring, use the following command:

   ```sh
   %sh
   nodetool ring
   ```

   - Run this paragraph to see the token distribution across the cluster, which helps understand how data is distributed.

4. **View Compaction Stats**:
   - To view compaction statistics, use:

   ```sh
   %sh
   nodetool compactionstats
   ```

   - Run this paragraph to check if any compaction operations are running and see their status.

5. **Check CFStats (Column Family Stats)**:
   - To view detailed statistics about keyspaces and tables, use:

   ```sh
   %sh
   nodetool cfstats
   ```

   - Run this paragraph to display statistics for each table, including the number of SSTables, read/write counts, and more.

6. **View Table Histograms**:
   - To get a more detailed view of table performance, use histograms:

   ```sh
   %sh
   nodetool tablehistograms lab_keyspace users
   ```

   - Run this paragraph to see read/write latency, row size, and partition size histograms for the `users` table.

#### Step 3: Exploring Cassandra with CQL

1. **List Keyspaces**:
   - To see all available keyspaces in Cassandra, run the following CQL command:

   ```sql
   %cassandra
   DESCRIBE keyspaces;
   ```

   - Run the paragraph to list all keyspaces.

2. **Describe Tables in a Keyspace**:
   - To view the tables in your keyspace, use:

   ```sql
   %cassandra
   DESCRIBE tables;
   ```

   - Run this paragraph to list all tables in the `lab_keyspace`.

3. **Describe a Table Schema**:
   - To explore the schema of the `users` table, use:

   ```sql
   %cassandra
   DESCRIBE TABLE users;
   ```

   - Run the paragraph to see the table's schema, including column definitions and primary key.

4. **Query Data**:
   - To retrieve data from the `users` table, use a simple `SELECT` statement:

   ```sql
   %cassandra
   SELECT * FROM users LIMIT 10;
   ```

   - Run this paragraph to see the first 10 rows of data from the `users` table.

5. **Check Cluster Health**:
   - To check the health of your cluster from a CQL perspective, run:

   ```sql
   %cassandra
   SELECT peer, rpc_address, release_version FROM system.peers;
   ```

   - Run this paragraph to display information about the peers in your cluster, including their IP addresses and Cassandra version.

6. **Execute an Aggregate Query**:
   - To count the number of users in the `users` table:

   ```sql
   %cassandra
   SELECT COUNT(*) FROM users;
   ```

   - Run this paragraph to get the count of rows in the `users` table.

7. **Monitor Table Size**:
   - To monitor the size of the `users` table, you can run:

   ```sql
   %cassandra
   SELECT table_name, bloom_filter_fp_chance, sstable_compression FROM system_schema.tables WHERE keyspace_name = 'lab_keyspace';
   ```

   - Run this paragraph to get insights into the table's size, SSTable compression, and Bloom filter settings.

#### Step 4: Monitoring and Management

1. **View Cassandra Logs**:
   - To view the recent logs from Cassandra, create a new paragraph:

   ```sh
   %sh
   tail -n 50 /var/log/cassandra/system.log
   ```

   - Run this paragraph to display the last 50 lines of the Cassandra system log. Adjust the number of lines as needed.

2. **Check Keyspace and Table Size**:
   - To get the disk space used by keyspaces and tables:

   ```sh
   %sh
   du -sh /var/lib/cassandra/data/*
   ```

   - Run this paragraph to see the disk usage by each keyspace and its associated tables.

### Conclusion

This lab walks you through setting up a keyspace and tables with sample data, and then exploring your Cassandra environment using `nodetool` and CQL commands in Zeppelin. This hands-on experience will provide you with practical insights into managing and querying a Cassandra database.
