Run the script for OPENVPN-AS

bash <(curl -fsS https://raw.githubusercontent.com/Augustine423/vpn/refs/heads/main/install.sh)


To connect a Node.js application to a MySQL database with two nodes, you can use the mysql or mysql2 package. Here’s a step-by-step guide to set up the connection:

Step 1: Install MySQL Package
First, install the mysql or mysql2 package using npm:

npm install mysql
# or
npm install mysql2

Step 2: Create a Connection Pool
Using a connection pool allows you to manage multiple connections efficiently. Here’s an example using the mysql package:

JavaScript

const mysql = require('mysql');

const pool = mysql.createPool({
  connectionLimit: 10, // Adjust the limit as needed
  host: 'localhost',
  user: 'yourusername',
  password: 'yourpassword',
  database: 'yourdatabase'
});

pool.getConnection((err, connection) => {
  if (err) throw err; // Not connected!
  console.log('Connected to MySQL database');

  // Use the connection
  connection.query('SELECT * FROM your_table', (error, results, fields) => {
    // When done with the connection, release it.
    connection.release();

    if (error) throw error;
    console.log(results);
  });
});
AI-generated code. Review and use carefully. More info on FAQ.
Step 3: Handling Multiple Nodes
If you have two MySQL nodes (e.g., for load balancing or failover), you can configure the pool to connect to both nodes. Here’s an example:

JavaScript

const poolCluster = mysql.createPoolCluster();

poolCluster.add('MASTER', {
  host: 'master_host',
  user: 'yourusername',
  password: 'yourpassword',
  database: 'yourdatabase'
});

poolCluster.add('SLAVE', {
  host: 'slave_host',
  user: 'yourusername',
  password: 'yourpassword',
  database: 'yourdatabase'
});

poolCluster.getConnection('MASTER', (err, connection) => {
  if (err) throw err;
  console.log('Connected to MASTER');

  connection.query('SELECT * FROM your_table', (error, results, fields) => {
    connection.release();
    if (error) throw error;
    console.log(results);
  });
});

poolCluster.getConnection('SLAVE', (err, connection) => {
  if (err) throw err;
  console.log('Connected to SLAVE');

  connection.query('SELECT * FROM your_table', (error, results, fields) => {
    connection.release();
    if (error) throw error;
    console.log(results);
  });
});
AI-generated code. Review and use carefully. More info on FAQ.
This setup allows you to connect to both the master and slave nodes, ensuring high availability and load balancing12.

If you need further customization or have specific requirements, feel free to ask!
