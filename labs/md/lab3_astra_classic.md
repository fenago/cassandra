### Lab 3: Create and Manage a Classic Cassandra/Astra DataStax Database

In Astra DB, the main differences between a classic database and a serverless database lie in their provisioning, scaling, and cost models:

### **Classic Database:**
1. **Provisioning:**
   - A classic database requires predefined capacity settings (e.g., number of nodes, vCPU, memory, and storage) when you create it. You need to estimate your workload in advance and configure the database accordingly.
   
2. **Scaling:**
   - Scaling in a classic database typically requires manual intervention. If your workload increases, you must manually add more nodes or adjust the capacity settings. This process can involve downtime and requires careful planning.

3. **Cost Model:**
   - You pay for the resources you provision, regardless of whether they are fully utilized. The cost is based on the allocated capacity (e.g., nodes, storage), so you might pay for more than what you use during off-peak times.

### **Serverless Database:**
1. **Provisioning:**
   - A serverless database does not require predefined capacity settings. It is automatically provisioned based on your actual workload, without needing you to specify the number of nodes or other capacity-related parameters.
   
2. **Scaling:**
   - Scaling is automatic and dynamic in a serverless database. The system scales up or down based on real-time demand, allowing you to handle variable workloads without manual intervention or downtime.

3. **Cost Model:**
   - You pay only for the actual resources consumed, typically measured in terms of read/write operations, storage, and compute usage. This "pay-as-you-go" model can be more cost-effective, especially for workloads that are unpredictable or have significant fluctuations.

### **Summary:**
- **Classic databases** are better suited for predictable, steady workloads where you can anticipate resource needs in advance. They involve manual scaling and may have a higher cost if you over-provision resources.
- **Serverless databases** offer more flexibility and efficiency for variable workloads, as they automatically scale and charge based on actual usage, which can result in cost savings for workloads with fluctuating demands.

For many modern applications, particularly those with unpredictable or highly variable workloads, serverless databases are often the preferred choice due to their simplicity and cost-effectiveness.

#### **Objective:**
This lab guides you through creating and managing a Classic Astra DB database in DataStax. You will also learn how to load data into your database using DataStax Bulk Loader.

---

### **Part 1: Create Your Classic Astra DB Database**

1. **Access the Astra Portal:**
   - Go to the Astra Portal by navigating to [Astra Portal](https://astra.datastax.com/).

2. **Create a Classic Database:**
   - On the portal's main page, click on the **Databases** section.
   - Select **Create Classic Database** from the available options.

3. **Select Database Tier:**
   - Choose a tier under **Production Workloads** or **High Density Production Workloads** based on your anticipated usage and data needs.

4. **Select Cloud Provider and Region:**
   - Choose your preferred **cloud provider** (e.g., AWS, GCP, Azure).
   - Select a **region** that is geographically close to your users to optimize performance.

5. **Review Cost Summary:**
   - Before proceeding, review the **Cost Summary**. You can switch between hourly or monthly estimates depending on your needs.

6. **Configure Database:**
   - Click on **Configure Database** to proceed with setup.

7. **Enter Database Information:**
   - **Database Name:** Enter a meaningful and descriptive name for your database. Ensure that the name consists only of alphanumeric characters.
   - **Keyspace Name:** Provide a name that reflects your data model. Remember, keyspaces in Astra DB Classic databases group tables similarly to schemas in relational databases. Avoid using reserved names like `dse` or `system`, and ensure your keyspace name is no longer than 48 characters.

8. **Create the Database:**
   - Click **Create Database** to start the creation process.
   - You will receive a confirmation banner on the Astra Portal when the database creation begins. An email will also notify you once the database is ready.

9. **Manage Your Database:**
   - On the **Databases** page, find your newly created database in the list. Click on the database’s name to manage or connect with it.

---

### **Part 2: Load and Retrieve Data**

#### **Prerequisites:**
   - You should have an Astra DB account and a newly created Classic Astra DB database.

1. **Generate an Application Token:**
   - Navigate to **Tokens** in the Astra Portal.
   - Select the appropriate role for the token (e.g., Database Administrator) and click **Generate Token**.
   - Store the generated **Client ID**, **Client Secret**, and **Token** securely, as you won’t be able to retrieve them again after closing the page.

2. **Load Data into Your Database:**
   - You can use several methods to load data into your database. For this lab, we'll use the **DataStax Bulk Loader**.

   **Steps:**
   - Prepare your data in CSV or JSON format. If you have data from a previous lab, you can use that.
   - Follow the steps below to load your data:

     - Install the DataStax Bulk Loader if you haven’t already.
     - Run the following command in your terminal to load data from your CSV file:
       ```bash
       dsbulk load -k <keyspace_name> -t <table_name> -url <path_to_csv> -header true -u <client_id> -p <client_secret> -h <database_contact_point>
       ```
     - Replace `<keyspace_name>`, `<table_name>`, `<path_to_csv>`, `<client_id>`, `<client_secret>`, and `<database_contact_point>` with your specific values.

---

### **Part 3: Invite Users and Generate Application Tokens**

1. **Create an Organization (if not already created):**
   - In the Astra Portal, click on your current organization and select **Manage Organizations**.
   - Click **Add Organization**, enter a name and notification email, and click **Add**.

2. **Optional: Create a Custom Role:**
   - Go to **Settings** and click on **Roles**.
   - Click **Add Custom Role**, name the role, assign the necessary permissions, and click **Create Role**.

3. **Add a User:**
   - Go to **Settings** and click on **Users**.
   - Click **Invite User**, enter the user’s email, and select a role.
   - The user will receive an invitation email to join your organization.

4. **Generate an Application Token:**
   - In the Astra Portal, navigate to **Tokens**.
   - Select the role to attach to your token, click **Generate Token**, and securely store the generated credentials.

---

### **Conclusion:**
By completing this lab, you have successfully created a Classic Astra DB database, loaded data into it, and managed users and roles. You now have a foundational understanding of working with Astra DB Classic databases.
