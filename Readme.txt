Introduction
Oracle DataBeast is a user-friendly web application designed to assist Oracle database users and administrators in managing and optimizing their databases. The application simplifies tasks such as creating, viewing, and deleting tables, indexing, managing tablespaces and partitions, optimizing SQL queries, and user management. By providing an intuitive interface and automating complex processes, Oracle DataBeast aims to improve database performance and reduce the time and effort required for database management tasks.
Technology Stack
To develop Oracle DataBeast, we used the following tools and technologies:
Python
We chose Python for its readability, ease of use, and extensive library support. Python is a popular language for web development and data manipulation, making it an ideal choice for this project.
Streamlit
Streamlit is a powerful open-source library for building custom web applications with Python. We used Streamlit for its simplicity, ease of use, and rapid prototyping capabilities. It also allowed for hot reloads, which, for an application which requires incremental building and credential management was an invaluable time-saver. It allowed us to create an interactive and user-friendly interface for our application.
Oracle Instant Client
Oracle Instant Client is a set of libraries used to connect Python to an Oracle database. We used it to establish a connection with the database and execute SQL queries.
Oracle Database Express Edition 12c
We used this to create an Oracle Server on our local system so we could test the application.
SQL Developer
We used this application to connect to our Oracle DB and create test data which we used to test the functionality of our application.
cx_Oracle
cx_Oracle is a Python extension module that enables access to Oracle Database. We used this library to interact with the Oracle database and execute various operations.
SQL Parse
SQL Parse is a non-validating SQL parser module for Python. We used it to break down SQL queries into components, making it easier to analyze and optimize them.
Regular Expressions (regex)
We utilized Python's built-in re module to perform pattern matching and manipulate strings when working with SQL queries.
Application Overview
The Oracle DataBeast application consists of several pages, each designed to perform specific database management tasks:
Login Page
Allows users to enter their database credentials and connect to an Oracle database.
Diagnosis and Report Generation Page
The Diagnosis and Report Generation page provides a detailed performance analysis of the database. It presents key metrics and statistics related to the database's health and efficiency, such as the top SQL queries by elapsed time and CPU time, the top sessions by wait time, and the top segments by physical reads. Additionally, it shows the database buffer cache hit ratio and the library cache hit ratio, which are important indicators of the database's performance. This information helps database administrators identify areas for improvement and optimize the overall performance of the database.
Table Management Page
Enables users to view existing tables, create new tables, and delete tables. It provides information on table schemas and allows users to manage their tables efficiently.
Indexing Page
Offers indexing suggestions based on table analysis and query analysis. Users can create indexes to optimize their database performance.
Tablespace and Partition Management Page
Allows users to view and manage tablespaces, create and delete tablespaces, and view partitioned tables. It also provides partitioning suggestions based on query analysis to improve performance.
Query Optimization Page
Enables users to input SQL queries, analyzes them, and provides optimization suggestions to improve query performance.
User Management Page
Provides users with DBA privileges the ability to manage other users, including creating, modifying, and deleting users. It also allows users to "Run As" another user, enabling them to perform actions on behalf of that user.

 
Methodology and Approach

Error Handling
One of the core principles of our approach was to ensure robust error handling throughout the application. We implemented error handling using Python's try and except blocks, which allowed us to catch any exceptions that might occur during the execution of various database operations. By integrating these error handling mechanisms with Streamlit's native error display capabilities, we ensured that users would receive informative and actionable error messages whenever an issue arises. This approach not only helps users understand what went wrong but also guides them towards resolving the issue.

Privilege Handling
We designed Oracle DataBeast with a focus on user privilege management to ensure that users can only access and modify information that they have the necessary permissions for. The application takes into account the logged-in user's privileges and adjusts the available options and displayed data accordingly. For instance, users can only view their own tables, while system administrators can see all tables, but with limited functionality to prevent accidental damage to the system.

Ease of Use and UI Principles
Oracle DataBeast was built with user experience in mind. We aimed to create an intuitive and easy-to-use interface that streamlines database management tasks for users. To achieve this, we followed several UI principles, such as:

1.	Providing clear messages and cues to inform users about the current state of the application, such as displaying the logged-in user's name and offering instructions on the home page.
2.	Adopting a clean, minimalistic design that reduces visual clutter and focuses on the most essential information.
3.	Organizing the application into distinct pages and functionalities, making it easy for users to navigate and manage different aspects of their databases.
4.	Ensuring that visual representations of data are easy to interpret and interact with, such as displaying tables, tablespaces, and suggested indexes in a clear and structured manner.
Security
The security of the application and the user's data is of utmost importance. To address this concern, we implemented several security measures:
1.	Ensuring that user credentials are securely transmitted to cx_oracle and not natively stored anywhere on the app so that the credentials are not compromised.
2.	Implementing proper input validation and sanitization to prevent SQL injection and other forms of malicious input.
3.	Following the principle of least privilege, where users are granted the minimum level of access necessary to complete their tasks, and their actions are restricted based on their role in the system.
4.	Using the most updated dependencies and libraries to ensure that the application is protected against known vulnerabilities and exploits.
Modularity
In our approach to building the application, we emphasized modularity by breaking down complex problems into smaller, more manageable functions and sections. This design choice has several benefits:

1.	Simplified debugging: By isolating different parts of the application into separate functions and components, we make it easier to identify and fix issues. When a bug occurs, we can quickly locate the responsible module and address the problem without affecting the rest of the system.
2.	Reusability: By designing modular functions, we promote code reusability. Functions can be easily reused in different parts of the application or even in other projects, reducing development time and effort.
3.	Maintainability: Modular code is easier to maintain because changes to one module typically do not impact other parts of the system. This separation of concerns allows us to update, optimize, or extend individual modules without introducing unexpected side effects.
4.	Improved readability: Modular code is often more readable because it breaks down complex tasks into smaller, more focused pieces. This structure makes it easier for developers to understand the purpose and functionality of each module, facilitating collaboration and knowledge transfer among team members.
Overall, our modular approach to building the application has resulted in a more robust, maintainable, and user-friendly tool that can effectively meet the needs of Oracle DB users and administrators.
By adopting these approaches, we aimed to create a secure, user-friendly, and efficient tool for managing and optimizing Oracle databases. This combination of robust error handling, strict privilege management, a focus on user experience, and a commitment to security ensures that Oracle DataBeast is an invaluable tool for database administrators and users alike.

 
In-depth Functionality
In this section, we'll discuss the key functions implemented in the Oracle DataBeast application and the database logic behind them:
Table Management Page
get_user_tables(connection)
This function retrieves the list of tables owned by the current user or all tables if the user is a DBA. It takes the database connection as an argument, executes the appropriate SQL query, and returns the fetched table list.

manage_tables(connection)
This function provides the interface to manage tables in the database. It has three main sections:
1.	Display existing tables: This section retrieves the list of tables owned by the current user using the get_user_tables() function and displays them on the page.
2.	Create a new table: This section allows users to create a new table by entering the table name and schema. The table is created using the specified schema when the "Create Table" button is clicked. It checks if the user is a DBA and warns against creating tables as SYSTEM or DBA.
3.	Delete a table: This section enables users to delete a table by selecting a table from a dropdown list and clicking the "Delete Table" button. It executes the "DROP TABLE" SQL query to delete the selected table from the database.

Indexing Page
get_tables(connection)
This function retrieves the table names from the all_tables view for the current user, excluding tables owned by SYS and SYSTEM. It returns a list of table names.

analyze_table(connection, table_name)
This function analyzes a given table by executing the "ANALYZE TABLE" command with the "COMPUTE STATISTICS" option. This computes the statistics for the table, which are used to make indexing suggestions.

get_column_statistics(connection, table_name)
This function retrieves the column statistics for a given table from the all_tab_col_statistics view. It returns a list of tuples containing the column name, number of distinct values, number of null values, and density.

should_create_index(column_stats)
This function takes the column statistics as input and determines if an index should be created on the column. It calculates the selectivity of the column and compares it with a selectivity_threshold (customizable). If the selectivity is greater than or equal to the threshold, it returns True, indicating an index should be created.

create_index(connection, table_name, column_name)
This function creates an index on the specified column of the given table. It generates an index name using the table and column names and executes the "CREATE INDEX" command.

get_used_columns(query)
This function takes the user's input query and extracts the columns used in the SELECT clause. It returns a list of used column names.

indexing(connection)
This function serves as the main entry point for the indexing page. It performs the following tasks:
•	Retrieves the list of tables using the get_tables() function.
•	Provides a textarea for users to input their query and a "Run Query" button to execute it.
•	When the button is clicked, it runs the input query, extracts the columns used, analyzes the relevant tables, gets column statistics, and determines if indexes should be created based on the query.
•	Displays the suggested indexes based on the query.
•	Analyzes all tables for the current user and gets column statistics.
•	Determines if indexes should be created based on table analysis.
•	Displays the suggested indexes based on table analysis.
•	When the "Create suggested indexes" button is clicked, it creates the suggested indexes using the create_index() function.
These functions work together to provide query optimization and indexing suggestions for the user, helping them improve the performance of their SQL queries and database.

Tablespace and Partition Management Page

is_user_dba(connection)
This function checks if the currently logged-in user has the 'DBA' role. It takes a connection object as an argument and queries the USER_ROLE_PRIVS table to check if the 'DBA' role is granted to the user. If the user has the 'DBA' role, it returns True, otherwise, it returns False.

get_tablespaces(connection)
This function retrieves information about tablespaces available to the currently logged-in user. It takes a connection object as an argument and checks if the user is a DBA using the is_user_dba function. If the user is a DBA, it queries the dba_data_files table to fetch tablespaces; otherwise, it queries the user_tablespaces table. The function then returns the fetched tablespaces as a list of tuples.

create_tablespace(connection)
This function creates a new tablespace based on user input. It takes a connection object as an argument and displays input fields for the user to enter a tablespace name, datafile path, and initial size in MB. When the "Create Tablespace" button is clicked, the function constructs a CREATE TABLESPACE SQL query using the provided inputs and executes it. If successful, it displays a success message, and if an exception occurs, it displays an error message.

drop_tablespace(connection, tablespaces)
This function deletes an existing tablespace selected by the user. It takes a connection object and a list of tablespaces as arguments. The function displays a dropdown menu containing the available tablespaces and a "Drop Tablespace" button. When the button is clicked, the function constructs a DROP TABLESPACE SQL query using the selected tablespace and executes it. If successful, it displays a success message, and if an exception occurs, it displays an error message.

manage_partitions(connection)
This function allows the user to enter a query, which is then executed and analyzed for potential partitioning suggestions. It takes a connection object as an argument and displays a text area for the user to input a query and an "Execute Query" button. When the button is clicked, the function calls analyze_query_for_partitions(query) to get partitioning suggestions based on the query, displays any suggestions, and then executes the query using the execute_query(connection, query) function.

get_partitioned_tables(connection, current_user)
This function fetches partitioned tables for the current user. It takes a connection object and the current user's username as arguments. It then constructs a SQL query to fetch table names and partition names from the all_tab_partitions table for the current user, and executes the query. The function returns the fetched partitioned tables as a list of tuples.

tablespace_partition(connection)
This function is the main entry point for the Tablespace and Partition Management page. It takes a connection object as an argument and calls the following functions to display and manage tablespaces and partitions:

1.	get_tablespaces(connection)
2.	create_tablespace(connection)
3.	drop_tablespace(connection, tablespaces)
4.	get_partitioned_tables(connection, st.session_state.username)
5.	manage_partitions(connection)


Query Optimization Page
query_optimization()
This function serves as the main entry point for the query optimization page. It provides an interface for the user to input a SQL query and press the "Optimize Query" button. When the button is clicked, the function calls optimize_query() to optimize the input query and then displays the original and optimized query along with the rationale for the changes made.

optimize_query(query)
This function accepts a SQL query as input and performs basic optimizations on it. It first parses the query using the sqlparse library and performs the following optimizations:
•	Removes unnecessary DISTINCT clauses.
•	Simplifies INNER JOIN to JOIN.
•	Replaces SELECT * with specific column names (if needed).
The function then returns the optimized query and the rationale for the changes made.

User Management Page
get_users_and_roles(connection)
This function fetches the list of users from either the DBA_USERS or ALL_USERS view, depending on the user's access privileges. It executes a SELECT query to get the list of usernames that start with 'C##' and returns this list.

check_dba_users_access(connection)
This function checks if the user has SELECT privilege on the DBA_USERS view. It queries the ALL_TAB_PRIVS view to find if the user has the SELECT privilege on DBA_USERS. It returns a boolean value (True or False) based on the user's access.

create_user(connection, username, password)
This function creates a new user in the Oracle DB with the given username and password. It checks if the username starts with 'C##', then executes a CREATE USER query with the specified DEFAULT and TEMPORARY tablespaces.

modify_user_password(connection, username, new_password)
This function changes the password of the specified user. It checks if the username starts with 'C##', then executes an ALTER USER query to update the user's password.

delete_user(connection, username)
This function deletes the specified user from the Oracle DB. It checks if the username starts with 'C##', then executes a DROP USER query with the CASCADE option to delete the user and their associated objects.

change_user_role(connection, username, role, action, object_name=None)
This function either grants or revokes a specified role to/from a user. It checks if the action is 'GRANT' or 'REVOKE', then executes the appropriate query to perform the desired action on the specified role. If the role is 'SELECT', it also requires the object_name (table name) for the operation.

has_user_management_privileges(connection)
This function checks if the user has the required privileges (CREATE USER, ALTER USER, DROP USER) to manage users. It queries the USER_SYS_PRIVS view to fetch the user's privileges and returns a boolean value (True or False) based on whether the user has all the required privileges.

user_management(connection)
This function serves as the main entry point for the User Management page. It checks if the user has the required privileges for user management and displays the appropriate forms and options for the user to create, modify, delete, and manage user roles. It also handles form submissions and calls the relevant functions to perform the requested actions.

These functions work together to provide a comprehensive User Management page that allows users with the necessary privileges to create, modify, and delete users and manage their roles in the Oracle DB.

Diagnosis and Report Generation Page
Here is a detailed explanation of each function in the diagnosis report:

diagnosis_report(connection)
This function serves as the main entry point for the diagnosis report. It first checks if the user is logged in as SYSTEM or SYS, as these users have the required privileges to access the relevant views. If the user is authorized, it calls various functions to fetch different performance-related metrics and displays these metrics as subheaders on the Streamlit page.

get_library_cache_hit_ratio(connection)
This function calculates the library cache hit ratio. It executes a query on the V$LIBRARYCACHE view, which contains information about the library cache's performance. The hit ratio is calculated as the sum of (pins - reloads) divided by the sum of pins, and then multiplied by 100 to get the percentage.

get_buffer_cache_hit_ratio(connection)
This function calculates the buffer cache hit ratio. It executes a query on the V$SYSSTAT view, which contains various system performance metrics. The hit ratio is calculated as (1 - (physical reads / (db block gets + consistent gets))) multiplied by 100 to get the percentage.

get_top_segments_physical_reads(connection)
This function retrieves the top 10 segments with the highest physical reads. It queries the V$SEGMENT_STATISTICS view, which contains performance metrics for various segments. The query filters the results based on the 'physical reads' statistic and orders the results in descending order.

get_top_sessions_wait_time(connection)
This function fetches the top 10 sessions with the highest wait time. It queries the V$SESSION view, which contains information about active sessions. The query orders the results based on the wait time in descending order and limits the results to the top 10.

get_top_sql_cpu_time(connection)
This function retrieves the top 10 SQL queries with the highest CPU time. It queries the V$SQL view, which contains information about SQL statements currently in the shared SQL area. The query calculates the CPU time per execution by dividing the total CPU time by the number of executions (if not zero). The results are ordered by CPU time in descending order.

get_top_sql_elapsed_time(connection)
This function fetches the top 10 SQL queries with the longest elapsed time. It queries the V$SQL view, which contains information about SQL statements currently in the shared SQL area. The query calculates the elapsed time per execution by dividing the total elapsed time by the number of executions (if not zero). The results are ordered by elapsed time in descending order.

These functions work together to provide a comprehensive diagnosis report that helps users with the necessary privileges to monitor the performance of various aspects of the Oracle database, such as SQL queries, sessions, segments, and cache hit ratios.

Main Function
app()
This function defines the main Streamlit app. It handles the display of the login form when the user is not logged in, and after a successful login, it shows a sidebar with user options for various database management tasks. Based on the user's selection, it calls the corresponding function to handle the selected task.



Conclusion

Oracle DataBeast is a comprehensive and user-friendly web application that provides an efficient way for Oracle database users and administrators to manage and optimize their databases. By offering an intuitive interface and automating complex processes, the application aims to improve database performance and reduce the time and effort required for database management tasks. The application is suitable for both novice and experienced users, making it a valuable tool for managing Oracle databases.
