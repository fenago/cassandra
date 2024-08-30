Apologies for the confusion! Let's incorporate the `find` command into Step 1 and proceed accordingly.

### Lab 5: Exploring and Modifying `cassandra.yaml` in Apache Cassandra

**Objective**:  
In this lab, you will explore the `cassandra.yaml` configuration file, understand the different parts of it, and make changes to specific settings. You will use Apache Zeppelin to perform these tasks, allowing for a seamless and interactive experience.

#### Step 1: Locate the `cassandra.yaml` File

1. **Create a New Paragraph**:
   - In Zeppelin, create a new paragraph to find where the `cassandra.yaml` file is located. Use the following command:

   ```sh
   %sh
   find / -name "cassandra.yaml" 2>/dev/null
   ```

   - Run this paragraph. The `find` command will search the entire file system for `cassandra.yaml` and display its location.

2. **Verify the Location**:
   - Once you have confirmed that the `cassandra.yaml` file is located at `/etc/cassandra/cassandra.yaml`, make a note of this path as you will use it in the subsequent steps.

#### Step 2: Explore `cassandra.yaml`

1. **Navigate to the Directory**:
   - Using the path you found in Step 1, create a new paragraph to navigate to the directory containing the `cassandra.yaml` file:

   ```sh
   %sh
   cd /etc/cassandra/
   ls -l cassandra.yaml
   ```

   - Run this paragraph to ensure the file is present.

2. **View the Contents of `cassandra.yaml`**:
   - Create another paragraph to view the contents of the file:

   ```sh
   %sh
   less cassandra.yaml
   ```

   - Alternatively, you can output the entire file with `cat`:

   ```sh
   %sh
   cat cassandra.yaml
   ```

   - Run the paragraph to review the file's content. Take some time to scroll through and familiarize yourself with the various sections.

#### Step 3: Understand Key Sections of `cassandra.yaml`

1. **Cluster Name**:
   - Find the section in the `cassandra.yaml` file that defines the `cluster_name`. Create a new paragraph and write a command to display this specific line:

   ```sh
   %sh
   grep "cluster_name:" cassandra.yaml
   ```

   - Run the paragraph to see the current cluster name.

2. **Seeds**:
   - The `seeds` section lists the initial contact points for connecting nodes in your cluster. Create a new paragraph to display the seeds configuration:

   ```sh
   %sh
   grep "seeds:" cassandra.yaml
   ```

   - Run the paragraph to see which IP addresses are listed as seeds.

3. **Data Directory Paths**:
   - Cassandra stores data in directories defined in the `data_file_directories` section. Create a new paragraph to extract this information:

   ```sh
   %sh
   grep "data_file_directories:" -A 2 cassandra.yaml
   ```

   - Run the paragraph to review the data directories.

4. **Commitlog Directory**:
   - The commitlog directory is where Cassandra writes commit logs. Create a paragraph to display this setting:

   ```sh
   %sh
   grep "commitlog_directory:" cassandra.yaml
   ```

   - Run the paragraph to see the path where commit logs are stored.

5. **Endpoint Snitch**:
   - The `endpoint_snitch` defines how Cassandra determines the proximity of nodes. Create a new paragraph to display the snitch setting:

   ```sh
   %sh
   grep "endpoint_snitch:" cassandra.yaml
   ```

   - Run the paragraph to see which snitch is being used.

#### Step 4: Make Changes to `cassandra.yaml`

1. **Change the Cluster Name**:
   - To change the cluster name, create a paragraph that uses `sed` to modify the `cluster_name` directly within the `cassandra.yaml` file:

   ```sh
   %sh
   sed -i 's/^cluster_name:.*/cluster_name: "NewClusterName"/' cassandra.yaml
   ```

   - Run the paragraph to change the cluster name. Replace `"NewClusterName"` with your desired name.

2. **Add a New Seed Node**:
   - To add a new seed node, create a paragraph that appends an IP address to the `seeds` list:

   ```sh
   %sh
   sed -i 's/^    - seeds: ".*"/    - seeds: "127.0.0.1,10.0.0.2"/' cassandra.yaml
   ```

   - Run the paragraph to add `10.0.0.2` as a seed node. Adjust the IP address as needed.

3. **Modify Data Directory Path**:
   - To change the data directory path, create a paragraph that updates the `data_file_directories` section:

   ```sh
   %sh
   sed -i 's|^data_file_directories:.*|data_file_directories:\n    - /new/data/directory/path|' cassandra.yaml
   ```

   - Run the paragraph to update the data directory path. Replace `/new/data/directory/path` with the actual path you want to use.

4. **Update the Commitlog Directory**:
   - Modify the commitlog directory path using the following command:

   ```sh
   %sh
   sed -i 's|^commitlog_directory:.*|commitlog_directory: /new/commitlog/path|' cassandra.yaml
   ```

   - Run the paragraph to change the commitlog directory. Replace `/new/commitlog/path` with your desired path.

5. **Change the Endpoint Snitch**:
   - If you want to use a different snitch, create a paragraph to update the `endpoint_snitch`:

   ```sh
   %sh
   sed -i 's/^endpoint_snitch:.*/endpoint_snitch: GossipingPropertyFileSnitch/' cassandra.yaml
   ```

   - Run the paragraph to set the snitch to `GossipingPropertyFileSnitch`. Adjust the snitch type as needed.

#### Step 5: Restart Cassandra to Apply Changes

1. **Create a New Paragraph**:
   - After making changes to the `cassandra.yaml` file, you need to restart the Cassandra service to apply them. Create a paragraph to restart Cassandra:

   ```sh
   %sh
   service cassandra restart
   ```

   - Run the paragraph to restart Cassandra.

2. **Verify the Changes**:
   - To ensure your changes have taken effect, you can create paragraphs to check the relevant settings again, similar to the commands used in Step 3.

### Conclusion

This lab guides you through locating, exploring, and modifying the `cassandra.yaml` configuration file using Apache Zeppelin. By following these steps, you will gain hands-on experience with configuring and optimizing your Cassandra environment.
