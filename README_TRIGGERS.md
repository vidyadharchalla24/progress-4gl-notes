# DATABASE ACCESS TRIGGERS
- Data Dictionary - most important tool for database development
- Used to create Databases and Database objects (Tables,Fields,Sequences, and Indexes) which is collectively called as a Database Schema.
- Once the schema is established, the user can create procedures to create records and populate the Database.

## Creation of Database
- This can be done in 2 ways:
    - Using Data Dictionary 
        - In GUI Procedure Editor -> Tools -> Data Dictionary -> Create a New Database -> Click on files and select the location where we would like to store the database.

    - Using prodb command in PROENV
        - Open Proenv command line
        - **prodb path_location/database_name.db**
        - connecting to database: **pro location/db_name.db** -> This will make the connection in Single User mode 
        - If we want to execute something in the background then we can go for the batch mode command: **bpro db-name -p procedure-name**
        - Single User Mode means we cannot make another connection to same database.
        - Inorder to acheive that we need to use one more command called :  **proserve db-name -S 9999** (9999 - port)
        - Here to connect to the datebase we need to use : **mpro db-name**.
        - for Multi-user connection batch mode command: **mbpro db-name -p procedure-name**
    
    - Connecting to a DB with the "CONNECT" statement (GUI Procedure Editor)
        - Syntax:
            - CONNECT {physical-name [parameters] | -db physical-name [parameters]} [NO-ERROR]
            - physical-name : The actual name of the database on the disk. It can be a simple filename, relative pathname, or a fully qualified pathname, represented as a unquoted string, or a quoted string
            - parameters : One or more database connection parameters (-1 for one connection and nothing for more connections).
            - -db : Physical database name startup parameter, which allows to specify multiple databases.
            - NO-ERROR : Suppresses ABL errors or error messages that would otherwise occur.
        - Example :
        ```
            CONNECT mydb1 -1 -db mydb2 NO-ERROR.

            HERE, 
            mydb1 -> physical-name of database.
            -1 is a single user mode connection
            -db -> allows to specify the second database name.
        ```
- One more way is using Developer Studio: 
    - open developer studio -> goto File -> New -> OpenEdge Project -> create the project by prefereed name and click next ... and finish at last.
    - create a .p file in this project.
    - Right click on the Project and goto Properties -> Progress OpenEdge -> Database Connections -> Configure database Configurations -> new -> Physical name (browse and select the database which you have created previously), Host name : localhost, Service/Port : 9999 -> Click on Next ... Auto-start database server to be checked and click on Finish.-> then select the database and finish.
    - Now you can write any query and test. for running -> Right Click on procedure -> Run As -> Progress OpenEdge Application.
    - we can view the **Data Dictionary** by navigating to -> OpenEdge -> Admin -> Data Dictionary.

### Database Objects
- Tables 
- Fields
- Sequences
- Indexes

### Indexes
- An Index is a field or combination of fields from one table that Progress uses as the basis for searching, sorting, or otherwise processing the records of the table.
- Primary : The Primary index is the one that Progress uses by default. Each table has one and only one primary index. Progress uses the primary index when retrieving records or ordering records for a list (like a report) if we don't specify another index.
- Unique : A unique index is one where each index value must be different. For example, each customer needs a unique number to identify them. Thus, the cust-num index is unique index.
- Active : A Active index is one that Progress updates when we create,delete or modify a record. If the index were inactive, we could still find the definition of the index in the Data Dictionary, but Progress would not update it until we made it active again.
- Word : A word index is one based on a character field where each distinct word in the data becomes a seperate index entry. Word indexes allow us to search character fields quickly for key words.
- Abbreviated : An Abbreviated index is an index for a character data type that can be searched through using just the first few characters of a field.
- Example : 
    ```
    PROMPT-FOR Customer.name.
    FIND Customer USING Customer.CustName.
    ```
- If the name field is an abbreviated index of the Customer table, ABL(Advanced Business Language) converts the FIND statement with the USING option into this following statement. 
- FIND Customer WHERE Customer.name BEGINS INPUT Customer.name.

- In Data Dictionary -> Create-> index -> Create the indexes -> After creating the indexes -> Edit -> Commit Transactions to save the cahnages.
 
### Sequences
- A Sequence is an algorithm for creating an incremental number series. The database holds the sequence definition and keeps track of the next available value.
- Sequence Properties:
    - Initial Value : The Starting value of the sequence. we can enter any positive or negative integer.
    - Increment by : The value used to determine the next sequence number. we can use a positive or negative integer.
    - Upper Limit : The highest acceptable number for the sequence. we can use any positive or negative integer.
    - Cycle at Limit : Determine whether the initial value is re-used when the sequence reaches the upper limit. If the Upper limit is reached and Cycle at Limit is set, then the cycle will be started again.
- Example: 
    ```
    DEFINE VARIABLE i AS INTEGER NO-UNDO.
    MESSAGE "Current value: " CURRENT-VALUE(sequence_name) VIEW-AS ALERT-BOX INFO BUTTON OK.
    REPEAT:
        MESSAGE NEXT-VALUE(sequence_name) VIEW-AS ALERT-BOX INFO BUTTON OK.
        i = i + 1.
        if i > 6 THEN
        LEAVE.
    END.

    /*
        CURRENT-VALUE(sequence_name) -> Which will accept the current value
        NEXT-VALUE(sequence_name) -> Which will moves to next value. // need to look into it later.
    */
    ```