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
---

### Data Handling Statements
- Statements that move data from one location to another are called data-handling statements.
- Progress stores data in various locations - database, record buffer and screen buffer.
- **Database** : Stores the data on disk.
- **Record buffer** : stores the data temporarily and allows the procedure to access the data in it and stores the value of the variables used in procedure.
- **Screen buffer** : Stores the data that is being displayed on the screen or being sent to another output destination. Also stores the data that is being entered from the terminal other Input source.
- The Procedure cannot interact with the data until the procedure copies the data from the **database** to a **record buffer**. Once the data is in a record buffer, the procedure can access that data.
- End users cannot interact with the data until the procedure copies the data from **record buffer** to the **Screen buffer**. Once the data is in the screen buffer, the data is visible on screen.

#### Creating Records
- Records are created in Database using CREATE statement.
- Syntax: ``` CREATE <table-name> ```
- CREATE statement creates a new empty record in Database with all the fields set to the initial values as specified in Data Dictionary.
- Once an empty record is created, ASSSIGN or UPDATE statement can be used to assign the values to the empty record.
- Examples: 
```
/* Using UPDATE statement (Creates Single record) */
    CREATE Student.
    UPDATE Student.
/* Using UPDATE Statement (Creates Multiple records) */
    REPEAT: 
        CREATE Student.
        UPDATE Student.
    END.
/* Using ASSIGN Statement */
    CREATE Student.
    ASSIGN Student.number=1.
            Student.name="test".

```

#### Retrieving data from a Database.
- Following Statements can be used to retrieve data from a Database.
    - FIND
    - FOR EACH
    - OPEN QUERY
    - PRESELECT

- FIND Statement: 
    - Syntax: 
    ```
    FIND {FIRST | NEXT | PREV | LAST | CURRENT } record-phrase NO-ERROR
    - FIND FIRST : Retrieves the First Record.
    - FIND NEXT : Retrieves the record next after the one that is currently in Record Buffer.
    - FIND PREV : Retrieves the record previous to the one that is currently in Record Buffer.
    - FIND LAST : Retrieves the Last Record.
    - FIND CURRENT : Retrieves the Record that is currently in Record Buffer.
    - NO-ERROR : Suppresses the normal error messages that the Progress throws if the FIND attempt fails. 
    ```
- FIND Statement uses the primary index of the table to retrieve the record.
- FIND always fetches a single record from the database table.
- Example: 
```
FIND FIRST customer WHERE custno=1.
MESSAGE custno VIEW-AS ALERT-BOX INFO BUTTON OK.

FIND NEXT customer.
MESSAGE custno VIEW-AS ALERT-BOX INFO BUTTON OK.

FIND PREV customer.
MESSAGE custno VIEW-AS ALERT-BOX INFO BUTTON OK.

FIND LAST customer.
MESSAGE custno VIEW-AS ALERT-BOX INFO BUTTON OK.

FIND CURRENT customer.
MESSAGE custno VIEW-AS ALERT-BOX INFO BUTTON OK.

/* NO-ERROR (Let's take a scenario where custno is not present and we would not want to display the error message which is shown by default rather we want to display our own error message by suppressing default error message.)*/

FIND FIRST customer WHERE custno=13 NO-ERROR.
IF AVAILABLE customer THEN 
    MESSAGE custno VIEW-AS ALERT-BOX INFO BUTTON OK.
ELSE
    DO:
        MESSAGE "Customer not found!" VIEW-AS ALERT-BOX INFO BUTTON OK.
    END.
```
#### FOR STATEMENT
- **Syntax:**
```
FOR {EACH | FIRST | LAST} record-phrase
{, {EACH | FIRST | LAST} record-phrase}
{BREAK}
{BY expression {DESCENDING} ...}:

- EACH : Starts an iterating block, finding a single record on each iteration.
- FIRST : Uses the criteria in the record-phrase to find the first record in the table that meets the criteria. AVM finds the first record before any sorting.
- LAST : Uses the criteria in the record-phrase to find the Last record in the table that meets the criteria. AVM finds the Last record before any sorting.
- BREAK - Groups the records based on the field specified in BY Statement.
- BY expression : Sorts the records by the value of expression in Ascending order, DESCENDING option sorts the record in descending order.
```

- **Example**
```
FOR EACH customer NO-LOCK BREAK BY country:
    IF FIRST-OF(country) THEN 
        DISPLAY cust-num country.
END.

- This query will first displays all the cust-number and countries in ascending order based on country in groups.
- Then later line says that in every group first record is displayed. FIRST-OF - will take the first record from each group. 
- If we replace FIRST-OF with LAST-OF - then it will get the last record from each group. 
```

#### OPEN QUERY STATEMENT
- **Syntax:**
```
OPEN QUERY query {FOR | PRESELECT} EACH record-phrase
[, {EACH | FIRST | LAST} record-phrase]...
[BREAK]
[BY expression [DESCENDING]]...

- query : The query to open. The query name may have been defined previously in a DEFINE QUERY statement. Otherwise, the OPEN QUERY implicitly defines the query.
- {FOR | PRESELECT} EACH record-phrase : First buffer of the query.
- {EACH | FIRST | LAST} record-phrase : Subsequent buffers in the query.
- BREAK : Groups the records based on the field specified in BY Statement.
- BY expression : Sorts the records by the value of expression in Ascending order, DESCENDING option sorts the record in descending order. 
```
- **Example:**
```
DEFINE QUERY qCust FOR customer,order.
OPEN QUERY qCust FOR EACH customer,
    EACH order OF customer NO-LOCK.
GET FIRST qCust.
DO WHILE AVAILABLE(customer):
    DISPLAY customer.cust-num customer.name order.order-num.
PAUSE.
GET NEXT qCust.
END.

CLOSE QUERY qCust.

-> first we need to declare the query -> DEFINE QUERY query_name FOR table_names.
-> OPEN QUERY query_name(same as defined query_name) actual_query.
-> At last we need to close the query that we have defined.
```

#### PRESELECT 
- Specifies a set of records to preselect for a DO or REPEAT block
- PRESELECT EACH, does not actually fetch the record, it creates a temporary index to all the db records which match the record criteria, which can then be accessed using the FIND command.
- **Syntax:**
```
PRESELECT 
[EACH | FIRST | LAST] record-phrase
[,[EACH | FIRST | LAST] record-phrase]...
[BREAK]
{BY expression [DESCENDING]}

-> [EACH | FIRST | LAST] record-phrase - Selects the records that meets the criteria in record-phrase.
-> BREAK - Groups the records based on the field specified in BY statement.
-> BY expression - Sorts the records by the value of expression in Ascending order. DESCENDING option sorts the record in descending order.
```

- If we modify an indexed field using FOR EACH, they would appear again later in the retrieval process.
- The PRESELECT keyword tells the AVM to build up a list of pointers to all the records that satisfy the selection criteria before it starts iterating through the block. This assures us that each record is accessed only once.
- **Examples:**
```
FOR EACH customer.
cust-num = cust-num + 100.
END.

REPEAT PRESELECT EACH customer:
    FIND NEXT customer.
    cust-num = cust-num + 100.
END.

/* Using DO Statement example*/

DEFINE VARIABLE ord-recid AS RECID NO-UNDO.
FIND FIRST order WHERE order-num = 4 NO-ERROR.
    ord-recid = RECID(order).

DO PRESELECT EACH order WHERE order-num > 5:
    FIND FIRST order WHERE RECID(order) = ord-recid.
        DISPLAY order.
END.

```
#### ASSIGN Statement
- Moves the data placed in Screen buffer by a data input statement to the corresponding fields and variables in Record buffer.
- **Example:**
```
FIND FIRST customer EXCLUSIVE-LOCK WHERE custnum=1.
DISPLAY customer.custnum customer.name.
PROMPT-FOR customer.name.
ASSIGN customer.name.
```
- Moves the data specified within the ASSIGN statement by an expression to the corresponding fields and variables in Record buffer.
- **Example:**
```
FIND FIRST customer EXCLUSIVE-LOCK WHERE custnum=1.
ASSIGN customer.country = "USA".
```

#### UPDATE Statement
- Combination of DISPLAY, PROMPT-FOR and ASSIGN statements.
- Moves the value of the fields/variables to Screen Buffer (DISPLAY)
- Prompts the User for Data and puts that data into Screen buffer (PROMPT-FOR)
- Moves data from the Screen Buffer to Record Buffer (ASSIGN).
- **Example:**
```
FIND FIRST customer EXCLUSIVE-LOCK WHERE custnum=1.
UPDATE customer.name.
```

#### DELETE Records (DELETE Statement)
- Removes a record from a Record buffer and from the database.
- **Example:**
```
FIND FIRST customer EXCLUSIVE-LOCK WHERE custnum=1,
DELETE customer.

FOR EACH customer EXCLUSIVE-LOCK WHERE customer.name begins "a":
    DELETE customer.
END.
```

#### PROMPT-FOR 
- Accepts data from the User (keyboard) to the screen buffer.
- **Syntax:**
```
PROMPT-FOR 
{
    field | 
    SPACE[(n)] |
    SKIP[(n)]
}...{[frame-phrase]}
```
- **Example:**
```
FIND FIRST customer EXCLUSIVE-LOCK WHERE custnum=1.
DISPLAY customer.custnum customer.name.
PROMPT-FOR customer.name.
ASSIGN customer.name.
```

#### SET 
- Combination of PROMPT-FOR and ASSIGN Statements.
- Prompts the user for data and puts the data in Screen buffer (PROMPT_FOR).
- Moves the data from screen buffer to record buffer (ASSIGN).
- **Syntax:**
```
SET
{
    field
    | SPACE[(n)]
    | SKIP[n]
}....{[frame-phrase]}
```
- **Example:**
```
FIND FIRST customer EXCLUSIVE-LOCK WHERE customer.custnum=1.
DISPLAY customer.custnum customer.name.
SET customer.name.
```

#### INSERT
- Combination of CREATE, DISPLAY, PROMPT-FOR and ASSIGN statements.
- **Syntax:**
```
INSERT record
[EXCEPT field...] {[frame-phrase]}
```
- **Example:**
```
REPEAT:
    INSERT employee WITH 1 COL.
END.
```
---

### Dumping and Loading
- Dumping and Loading of database is used while creating a new Database or using constant tables/schema in multiple databases.
    - Dump a Database / Table definitions and then dump the Table contents.
    - Load another database with the database/ Table definitions and then the table contents.
- Both Dumping and Loading is done using Data Administration tool (in GUI) or Data Dictionary (in CHUI).
- lets say we have a 4 environments Developer, QA, Pre-Production, Production. We have a requirement where we would like design a table which should be created in all 4 environments.
- In this case we won't be creating the table in every environment instead we will 
1. create a dump file (.df)
2. load df file to different environment.

#### How to create dump of tables?
- Goto Data Dictionary -> Tools -> Data Administration -> Admin -> Dump Data and Definitions -> Data Definitions (.df file) -> Select the tables -> give the location where output dump files should be stored (session1.df).
- create a new database : data dictionary -> Database ->create -> select the new database -> tools -> Data Administration -> Admin -> Load Data and Definitions -> Data Definitions (.df file) -> Search for session1.df and load it.
- If we would like to move the database tables content also then select the database -> tools -> Data Administration -> Admin -> Dump Data and Definitions -> Table Contents (.d file) -> Select required tables -> select output directory.
- now select new database -> Tools -> Data Administration -> Admin -> Load Data and Definitions -> Table Contents (.d file) -> select the .d files that we have stored.
- Lets say we have added few tables and columns in the old database. 
- Incremental df means it will comparing two databases and finding differences alone.
- select the first database (session1) -> Tools -> Data Administration -> Admin -> Dump Data and Definitions -> Create Incremental .df file -> select the second database with which we want the differences. -> give the output location and file name.

---

### Database Triggers 
- A Database Trigger is a block of 4GL code that executes whenever a specific database event occurs.
- Database Events : 
    - CREATE
    - DELETE
    - FIND
    - WRITE
    - ASSIGN
- Types of Triggers:
    - **Schema**
    - **Session**
#### Schema Triggers
- A Schema Trigger is a .p Procedure, that is added through the Data Dictionary, to the Schema of a Database.
- Progress allows to define these triggers while creating/modifying a table or a field.
- These are defined at the Database level and independent of procedures.
- When application procedures are compiled against a Database, Schema triggers are not compiled.
- Schema triggers are : 
    - CREATE (Table level trigger)
    - DELETE (Table level trigger)
    - FIND (Table level trigger)
    - WRITE (Table level trigger)
    - ASSIGN (Field level trigger) -> We don't have this in Progress

#### CREATE TRIGGER
- CREATE triggers are fired whenever the Progress executes a CREATE or INSERT statement for a Database table.
- **Syntax:**
```
TRIGGER PROCEDURE FOR 
{CREATE} OF table
```
- **Example**
```
TRIGGER PROCEDURE FOR CREATE OF customer.
ASSIGN customer.custno=NEXT-VALUE(sequence_name).
```
- The above example automatically increments the Customer number in Customer table(customer.custno) using the sequence sequence_name, Whenever Progress creates a new record to Customer table.
- In Procedure Editor -> Tools -> Data Dictionary -> in Tables double click on any table -> we can now see the Triggers, click on it -> Events (CREATE,FIND,DELETE,WRITE) select one and we need to create an .p procedure and select that file location -> replace with above example -> Save and close.

#### DELETE Trigger
- DELETE Trigger are fired whenever the progress executes a DELETE statement for a database table.
- **Syntax:**
```
TRIGGER PROCEDURE FOR {DELETE} OF table
```
- **Example**
```
TRIGGER PROCEDURE FOR DELETE OF customer.
FOR EACH order OF customer.
    DELETE order.
END.
```
- The above example deletes all order records of the customer, when progress delete the customer.

#### FIND Trigger
- Find triggers are fired whenever the progress read a record from database table using FIND or a GET statement or a FOR EACH loop.
- **Syntax**
```
TRIGGER PROCEDURE FOR {FIND} OF table
```

#### WRITE TRIGGER
- WRITE triggers are fired whenever the progress changes the contents of a record for a Database table.
- **Syntax**
```
TRIGGER PROCEDURE FOR {WRITE} OF table
[NEW [BUFFER] buffer-name]
[OLD [BUFFER] buffer-name1]
```
- **Example**
```
TRIGGER PROCEDURE FOR WRITE OF OrderLine NEW BUFFER bufNewRecord OLD BUFFER bufOldRecord.
ASSIGN bufNewRecord.ExtendedPrice = bufNewRecord.Price * bufNewRecord.Qty * (1-(Discount/100)).
```
- The above example automatically calculates the Extended price in Orderline table, based on new price, Qty and Discount, whenever a new OderLine record is created.

#### ASSIGN Trigger
- ASSIGN triggers are fired whenever the progress updates a field in the database.
- **Syntax:**
```
TRIGGER PROCEDURE FOR ASSIGN
{
    OF table.field
    | NEW [VALUE] parameter1 {AS data-type | LIKE field}
    [OLD [VALUE] parameter2 {AS data-type | LIKE field}]
}
```
- **Example:**
```
TRIGGER PROCEDURE FOR ASSIGN OF  customer.cust-num
    NEW VALUE newCust
    OLD VALUE oldCust.
FOR EACH order WHERE order.cust-num = oldCust:
    order.cust-num = customer.cust-num.
END.
```
- The above example update the value of cust-num in order table whenever the progress updates the cust-num in customer table.
---
### Session Triggers
- A session trigger is defined within a procedure.
- Session Triggers are defined as part of a particular application and are in effect only for that particular application.
- These triggers remains active, from the time when progress runs the procedure, until the time when progress terminates the procedure.
- **Syntax:**
```
ON <event> OF <object>
DO:
    <Trigger Block>
END.
```
- **Example**
```
DEFINE VARIABLE custId AS INTEGER NO-UNDO.
DEFINE FRAME fcust custId.

ON 'F1':U OF FRAME fcust
DO:
    ASSIGN custId.
    FIND FIRST customer NO-LOCK WHERE customer.cust-num = INPUT custId NO-ERROR.
    IF AVAILABLE customer THEN
        MESSAGE customer.cust-num customer.name VIEW-AS ALERT-BOX INFO BUTTONS OK.
    ELSE
        MESSAGE "Customer Not Found" VIEW-AS ALERT-BOX INFO BUTTONS OK.
    APPLY 'CLOSE' TO THIS-PROCEDURE.
END.

ENABLE custId WITH FRAME fcust.
WAIT-FOR 'CLOSE' OF THIS-PROCEDURE.
```
---
### Questions
1. How to trace out the table "Employee" belongs to "DB1" Database programmatically?
```
MESSAGE LDBNAME(BUFFER Employee) VIEW-AS ALERT-BOX.
```
- **LDBNAME()** : it returns the logical name of the database.
- **BUFFER Employee** : Buffer is used to refer to a buffer associated with a database table. Here, it's refering to the buffer for the "Employee" table. The "Employee" table must be a table that is defined in our Progress database schema. when we work with database records in progress 4GL, we often work with themm through buffers.
