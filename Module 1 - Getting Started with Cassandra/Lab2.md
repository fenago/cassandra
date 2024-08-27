### **Lab Guide: Exploring Your Astra Cassandra Account**

**Objective:**  
In this lab, you will explore your Astra Cassandra account by creating a new database, exploring the CQL console, and reviewing the settings for your database. The database created will be serverless and non-vector.

---

### **Prerequisites:**
- You must have an active Astra account. If you do not have one, please sign up at [Astra DataStax](https://astra.datastax.com/).

---

### **Step 1: Log in to Astra**

1. **Open your web browser** and go to [Astra DataStax](https://astra.datastax.com/).
2. **Log in** using your Astra credentials.

---

### **Step 2: Create a New Database**

1. Once logged in, you will be on the **Astra Dashboard**.
2. Click on the **"Create Database"** button.
3. **Fill in the required details:**
   - **Database Name:** Choose a unique name for your database.
   - **Keyspace Name:** Enter a keyspace name. This will be the namespace for your tables.
   - **Cloud Provider and Region:** Select your preferred cloud provider (e.g., AWS, Google Cloud, etc.) and a region close to your location.
   - **Database Type:** Ensure that the option is set to **Serverless**. Ensure **Vector** is NOT selected (this lab is focused on a non-vector database).
4. **Click on "Create Database".**

---

### **Step 3: Wait for Database Provisioning**

1. Your database will now begin the provisioning process. This can take a few minutes.
2. While you wait, you can explore the Astra interface, including **Documentation** and **Support** sections available in the sidebar.

---

### **Step 4: Access the CQL Console**

1. Once your database is provisioned, you will see it listed under **Databases** in the dashboard.
2. Click on your newly created database to view its details.
3. In the database view, click on **"CQL Console"**.
4. The CQL Console is an interactive command-line interface that allows you to run CQL (Cassandra Query Language) commands against your database.

---

### **Step 5: Explore the CQL Console**

1. **Run Basic CQL Commands**:
   - To see all keyspaces:  
     ```sql
     DESCRIBE KEYSPACES;
     ```
   - To use your keyspace (replace `your_keyspace_name` with the name you chose):  
     ```sql
     USE your_keyspace_name;
     ```
   - To create a new table (example):  
     ```sql
     CREATE TABLE users (
         user_id UUID PRIMARY KEY,
         name TEXT,
         email TEXT
     );
     ```
   - To insert data into your table:  
     ```sql
     INSERT INTO users (user_id, name, email) VALUES (uuid(), 'John Doe', 'john.doe@example.com');
     ```
   - To query data from your table:  
     ```sql
     SELECT * FROM users;
     ```

2. **Experiment** with additional CQL commands:
   - Explore various CQL commands like `ALTER TABLE`, `DROP TABLE`, `TRUNCATE`, etc.
   - Feel free to create more tables, insert additional data, and experiment with different queries.

---

### **Step 6: Review Database Settings**

1. Return to the **Database View** by navigating back from the CQL Console.
2. Click on the **"Settings"** tab to review:
   - **General Information:** Database name, keyspace, cloud provider, region.
   - **Serverless Details:** Review the cost model and understand how serverless pricing works.
   - **Security Settings:** Explore options like **Token Management** and **Private Links**.
   - **Data Storage:** Check the storage usage and learn how to monitor your database capacity.

---

### **Step 7: Clean Up (Optional)**

1. If you want to delete the database after completing the lab, you can do so from the **Settings** tab:
   - Scroll down to the **"Danger Zone"** section.
   - Click on **"Delete Database"** and confirm your action.

---

### **Conclusion**

By completing this lab, you have:
- Created a new serverless Cassandra database on Astra.
- Explored the CQL Console and executed basic CQL commands.
- Reviewed the settings and configurations of your database.

This hands-on experience gives you a foundational understanding of managing Cassandra databases in Astra, preparing you for more advanced tasks and real-world applications.

---

Feel free to revisit this guide as you continue to work with Astra and Cassandra!
