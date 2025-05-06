# OpenEdge Reference Architecture

## 1. Presentation Layers

- Provides user interface.
- Acts as Mediator between User and Business Service Layer.
- Displays the data from business servicing layer and provides data updates to it.

## 2. Integration Layers

- Provides Interface with other Applications.
- Like Web services and Messaging.

## 3. Business Servicing Layers

- Includes the business logic that deals with application data in its logical form.
- Provides interfaces that manage general support services such as:
  - Performing data transformation.
  - Encapsulating reusable business functions, algorithms, and calculations.
  - Providing single-point of definition and interaction for all persistent application entities.

## 4. Data Access Layer

- Part of the application that deals with physical data storage.
- Code dealing with physical data storage is isolated in this layer.
- Higher layers define a logical view of the data, ideal for business logic.
- Changes in physical data storage require changes only in this layer.

## 5. Data Stores Layer

- Physical location of application data.
- Two types:
  - Managed Data Stores (OpenEdge databases or databases accessible through OpenEdge DataServers).
  - Unmanaged Data Stores (XML documents, Flat Files, Spreadsheets - require manual retrieval and update).

---

# Progress 4GL

1. Business Components: Business logic is centralized for future changes.
2. Data Access: Interacts with business logic and data sources (databases).
3. In Progress, we can build UI, Database, and Backend.
4. Progress 4GL: A fourth-generation programming language.
5. Statement terminator: '.' (period) instead of ';' as in Java.
6. Progress DB supports some SQL commands.
7. Progress Compiler generates platform '.r' files.
8. Multiple file types:
   - `.p`: Procedure files.
   - `.i`: Include files.
   - `.f`: Frame definition files (UI).
   - `.xref`: Cross-reference files.
9. Compile command: `compile filename.`
10. Run command: `run Procedure-filename.`

---

# Data Types

| Data Type | Description            | Default Initial Value | Default Display Format   |
| :-------- | :--------------------- | :-------------------- | :----------------------- |
| Integer   | Whole numbers          | 0                     | General numeric format   |
| Int64     | 64-bit integer         | 0                     | General numeric format   |
| Character | Text string            | `?` (Unknown)         | `X(n)` where n is length |
| Long      | Deprecated (use Int64) | 0                     | General numeric format   |
| Logical   | Boolean (True/False)   | FALSE                 | "yes/no" or "true/false" |
| Decimal   | Numbers with decimals  | 0                     | "->>,>>9.99"             |
| Date      | Calendar date          | `?` (Unknown)         | "99/99/9999"             |
| Time      | Time of day            | 0                     | "HH:MM:SS"               |
| Rowid     | Record pointer         | `?` (Unknown)         | Not displayable          |
| Recid     | Record id (4 bytes)    | `?` (Unknown)         | Not displayable          |

> **Note:** `?` represents an Unknown value in Progress 4GL.

---

# Default Initial Values

| Data Type   | Default Initial Value |
| :---------- | :-------------------- |
| Character   | `?`                   |
| Date        | `?`                   |
| Datetime    | `?`                   |
| Datetime-tz | `?`                   |
| Decimal     | 0                     |
| Integer     | 0                     |
| Int64       | 0                     |
| Logical     | FALSE                 |
| Raw         | `?`                   |
| Recid       | `?`                   |
| Rowid       | `?`                   |
| Handle      | `?`                   |
| Memptr      | `?`                   |
| CLOB        | `?`                   |
| BLOB        | `?`                   |

---

# Notes

RECID:
A system-generated unique identifier for a record in a database table. Mostly used internally for record positioning and lookup.

ROWID:
A more advanced record identifier compared to RECID, used for accessing records especially in a multi-user environment.

MEMPTR:
Stands for "memory pointer." Used to manage raw memory buffers, often for handling large objects or interfacing with external APIs.

COM-HANDLE:
Represents a connection or reference to an external COM object (used when integrating with Windows COM automation servers).

Unknown value (?):
In Progress 4GL, "?" is used to indicate an unknown value rather than NULL. It applies to fields like DATE, ROWID, COM-HANDLE, etc.

Default Display Formats:
These are the ways values are formatted when displayed on a screen or report, but they can be customized using the FORMAT option.

```
Example 1: Variable Declarations in Progress 4GL
/* Declaring variables of different data types */
DEFINE VARIABLE iAge         AS INTEGER    NO-UNDO.
DEFINE VARIABLE dSalary      AS DECIMAL    NO-UNDO.
DEFINE VARIABLE cName        AS CHARACTER  NO-UNDO.
DEFINE VARIABLE dtBirthDate  AS DATE       NO-UNDO.
DEFINE VARIABLE lIsActive    AS LOGICAL    NO-UNDO.
DEFINE VARIABLE rRecid       AS RECID      NO-UNDO.
DEFINE VARIABLE rRowid       AS ROWID      NO-UNDO.
DEFINE VARIABLE bImage       AS RAW        NO-UNDO.
DEFINE VARIABLE hExcel       AS COM-HANDLE NO-UNDO.
DEFINE VARIABLE mBuffer      AS MEMPTR     NO-UNDO.

/* Assigning some sample values */
ASSIGN
  iAge        = 30
  dSalary     = 50000.75
  cName       = "John Doe"
  dtBirthDate = TODAY
  lIsActive   = TRUE.

/* Displaying values */
DISPLAY iAge dSalary cName dtBirthDate lIsActive.


Example 2: Behavior of Unknown Values (?)
/* Declaring variables without assigning values */
DEFINE VARIABLE iAge         AS INTEGER    NO-UNDO.
DEFINE VARIABLE dSalary      AS DECIMAL    NO-UNDO.
DEFINE VARIABLE cName        AS CHARACTER  NO-UNDO.
DEFINE VARIABLE dtBirthDate  AS DATE       NO-UNDO.
DEFINE VARIABLE lIsActive    AS LOGICAL    NO-UNDO.

/* Displaying variables without assignment */
DISPLAY iAge dSalary cName dtBirthDate lIsActive WITH FRAME f1.

/* Checking for unknown values */
IF dtBirthDate = ? THEN
  MESSAGE "Birth date is unknown!" VIEW-AS ALERT-BOX INFO.

IF lIsActive = ? THEN
  MESSAGE "Active status is unknown!" VIEW-AS ALERT-BOX INFO.
```

# Display Format Examples

### 1. Integer Display Format

```progress
DEFINE VARIABLE num AS INTEGER NO-UNDO.
num = 123.
DISPLAY num FORMAT "99999".
```

- Output: `  123`

### 2. Decimal Display Format

```progress
DEFINE VARIABLE price AS DECIMAL NO-UNDO.
price = 123.456.
DISPLAY price FORMAT "->>,>>9.99".
```

- Output: `  123.46`

### 3. Date Display Format

```progress
DEFINE VARIABLE today AS DATE NO-UNDO.
today = TODAY.
DISPLAY today FORMAT "99/99/9999".
```

- Output (example): `04/27/2025`

### 4. Character Display Format

```progress
DEFINE VARIABLE name AS CHARACTER NO-UNDO.
name = "OpenEdge".
DISPLAY name FORMAT "X(15)".
```

- Output: `OpenEdge       ` (padded to 15 characters)

---

# Features of Progress 4GL

- Block-structured syntax (procedures, functions, flexible encapsulation).
- Event-driven architecture for creating/managing UIs (Graphical/Character).
- Integrated backend manager (Progress RDBMS, third-party RDBMS, flat files).
- Compiled integrated code (efficient and portable).

# Application Development

- **GUI** using **Application Builder**.
- **CHUI** (Character User Interface) using **Procedure Editor**.
- **Web applications** using **WebSpeed**.
- **Progress RDBMS** is usable with GUI, CHUI, and Web applications.

# Advantages

- Supports complex applications.
- Reduces development effort.
- Less platform/deployment knowledge needed.
- Supports internationalization.
- Reusability.
- Supports Application Server and Web-based applications.

---

# Miscellaneous Notes

### Display Example

```progress
DISPLAY "Hello World".
```

### Check Current Session Date Format

```progress
DISP SESSION:DATE-FORMAT.
```

### Comments

```progress
/* Single and multi-line comments */
```

### Variable Declaration

```progress
DEFINE VARIABLE i AS INTEGER NO-UNDO.
i = 5.
i = 10.
```

> **NO-UNDO** is recommended to optimize memory usage.

### Variable Initialization

```progress
DEFINE VARIABLE i AS INTEGER INITIAL 100 NO-UNDO.
DISPLAY i.
PAUSE.
i = 5.
i = 10.
DISPLAY i.
```

### MESSAGE Statements

Syntax:

```progress
MESSAGE "Your Message Here" VIEW-AS ALERT-BOX INFORMATION BUTTONS OK TITLE "Info".
```

Options:

- COLOR color-phrase
- {expression | SKIP[(n)]} (blank lines)
- VIEW-AS ALERT-BOX (MESSAGE, QUESTION, INFORMATION, ERROR, WARNING)
- BUTTONS (YES-NO, OK, OK-CANCEL, etc.)
- TITLE (Custom title)
- SET/UPDATE (update fields)
- FORMAT (custom format)

Description:

- COLOR color-phrase : Displays a message using the color we specify with the COLOR phrase.
- expression: An expression (a constant, field name, variable name or expression) whose value we want to display in the message area.
- SKIP[(n)] : Indicates a number (n) of blank lines to insert into the message.
- VIEW-AS ALERT-BOX [alert-type] : Specifies that the message is displayed in an alert box rather than in the window message area.
  - MESSAGE
  - QUESTION
  - INFORMATION
  - ERROR
  - WARNING
- BUTTONS button-set: Specifies what sets of buttons are available within the alert box. The possible button sets are as follows:
  - YES NO
  - YES NO CANCEL
  - OK
  - OK-CANCEL
  - RETRY-CANCEL
- TITLE title-string : Specifies a value to display in the title bar of the alert box.
- SET field : Displays the expressions we specified and SETs the field or variable name
- UPDATE field: Displays the expression we specified and update the field or variable we name.
- AS datatype: Defines field as a variable of type data type.
- LIKE field : Defines the field specified in SET or UPDATE as a database field or a previously defined variable.
- FORMAT string : The format that we want to use to display the field used in the SET or UPDATE option.

---

### Local Variable Declarations

Syntax:

- DEFINE VARIABLE <variable-name> AS <Data-type> FORMAT <format-string> INITIAL <initial-value> NO-UNDO.

```
DEFINE VARIABLE iCustomerNumber AS INTEGER FORMAT 9999 INITIAL 10 NO-UNDO.
```

### New Shared Variable Declaration

Syntax:

- DEFINE NEW SHARED VARIABLE <variable-name> AS <dat-type> FORMAT <format-string> INITIAL <initial-value> NO-UNDO.

```
DEFINE NEW SHARED VARIABLE cCustimerName AS CHARACTER INITIAL "New-Shared".
```

### Shared Variable Declaration

Syntax :

- DEFINE SHARED VARIABLE <variable-name> AS <data-type> FORMAT <format-string> INITIAL <Initial-value> NO-UNDO.

```
DEFINE SHARED VARIABLE cCustomerName AS CHARACTER INITIAL "shared".
```

---

- For a single line if we don't use '.' period it's fine but for multiple lines it does matter to end with Period
- Syntax :

```
DEFINE VARIABLE variable_name AS data-type
    [FORMAT format-type]
    [INITIAL initial-value]
    [NO-UNDO].
```

- DEFINE VARIABLE : declares a new variable.
- AS data-type : Specifies the data type of the variable.(e.g., CHARACTER, INTEGER, DECIMAL, DATE, LOGICAL..)
- FORMAT format-type (Optional) : Defines how the variable is displayed
- INITIAL initial-value (Optional) : Sets a default value when the variable is created.
- NO-UNDO (Optional but recommended) : Prevents the variable from being roll back in case of an error (improves performance).

- EXAMPLE :

```
DEFINE VARIABLE orderDate AS DATE FORMAT 99/99/9999 INITIAL TODAY NO-UNDO.
DISPLAY orderDate.

output:
04/28/2025

DEFINE VARIABLE totalPrice AS INTEGER FORMAT "$>>>,>>9.99" INITIAL 1023.00 NO-UNDO.
DISPLAY totalPrice.

Output :
 totalPrice
-----------
  $1,023.00

```

- When to Use FORMAT? 
    - When displaying data in DISPLAY, MESSAGE, or reports.
    ```
    DEFINE VARIABLE phoneNumber AS CHARACTER FORMAT "XXX-XXX-XXXX" INITIAL "1234567890" NO-UNDO.
    DISPLAY phoneNumber.

    OUTPUT: 
    phoneNumber
    ------------
    123-456-7890
   ```
- When to Use INITIAL? 
    - When we want a default value instead of ? (unknown).
    ```
    DEFINE VARIABLE isActive AS LOGICAL INITIAL TRUE NO-UNDO.
    DISPLAY isActive.

    OUTPUT: 
    isActive
    --------
      yes
   ```
- X OR x both are same bcz it is a case in-sensitive
- X in a FORMAT string (like "X(20)") is a format symbol that represents "Any character (alphabetic, numeric, or special ).
- Meaning of X(20) : 
    - X is a Placeholder for any Single Character (letters, numbers, symbols).
    - (20) is length of 20 characters.
    - So, X(20) means: 
        - The variable can display upto 20 characters of any type.
        - If the actual value is shorter, It will be left-aligned and padded with spaces.
```
DEFINE VARIABLE productCode AS CHARACTER FORMAT "XX-999" NO-UNDO.
productCode = "AB123".
DISPLAY productCode.  // Shows: AB-123
```
---

- PAUSE statement: used to temporarily halt program execution until the user takes an action (like pressing a key) or a specified time elapses.
- Syntax: 
    ```
      PAUSE [ n | BEFORE-HIDE | MESSAGE message ].
      n = number of seconds to pause (decimal allowed e.g., PAUSE 2.5. )
      BEFORE-HIDE = Pauses before hiding a frame.
      MESSAGE = displays custom message during pause.
    ```
- Pause for a Fixed Time
    ```
    MESSAGE "Processing...".
    PAUSE 3.  /* Pauses for 3 seconds */
    MESSAGE "Done!".
    ```
- Wait for User Input
    ```
    MESSAGE "Press any key to continue...".
    PAUSE.  /* Waits indefinitely for a keypress */
    ```
- UI Control (BEFORE-HIDE)
    ```
    DISPLAY "Closing soon..." WITH FRAME f.
    PAUSE BEFORE-HIDE.  /* Pauses before hiding the frame */
    HIDE FRAME f.
    ```
- With Custom Message
    ```
    PAUSE MESSAGE "Loading data...".  /* Shows message during pause */
    ```
---

### MESSAGE Statement
- MESSAGE statement is used to display information to the user, typically in a pop-up dialog box or console output. It's commonly used for debugging, user prompts, and simple notifications.

- Syntax:
    ```
    MESSAGE message-text 
    [VIEW-AS ALERT-BOX | ALERT-BOX type] 
    [SET field] 
    [UPDATE field] 
    [SKIP(n)] 
    [COLOR color].

    message-text = Text or variables to display.
    VIEW-AS ALERT-BOX = Shows a pop-up dialog (GUI mode).
    ALERT-BOX type = Specifies alert type (ERROR, WARNING, INFO, QUESTION, MESSAGE)
    SET field = Assigns user input to a variable.
    UPDATE field = Allows editing a variable.
    SKIP(n) = Adds line breaks.
    COLOR color = Sets text color (Console only).
    ```
* COMMON USE CASES
    - Simple Message (console)
      ```
        MESSAGE "THIS IS A SIMPLE MESSAGE".
      ```
    - Alert-box 
      ```
        MESSAGE "File saved successfully!" VIEW-AS ALERT-BOX.
      ```
    - Alert with Type (Warning/Error/Question)
    ```
      MESSAGE "Overwrite file?" ALERT-BOX QUESTION.

      /*
      Supports : ERROR, WARNING, INFO, QUESTION
      */
    ```
    - Multi-Line Messages
    ```
      MESSAGE "Line 1" SKIP "Line 2" SKIP(2) "Line 4".
    /*
      SKIP = single line break
      SKIP(2) = Multiple line breaks
    */
    ```
    - User Input (SET/UPDATE)
    ```
      DEFINE VARIABLE userName AS CHARACTER NO-UNDO.
      MESSAGE "Enter your name:" SET userName.

    ```
    - Debugging Variables: displays variable values for debugging.
    ```
      DEFINE VARIABLE total AS INTEGER INITIAL 100.
      MESSAGE "Total value:" total VIEW-AS ALERT-BOX.
    ```

    - Formatted Messages
    ```
      DEFINE VARIABLE price AS DECIMAL INITIAL 99.99.
      MESSAGE "Price:" price FORMAT "$>>9.99" SKIP "Tax: 10%".
    ```

    - This will give us the error. Because, we cannot directly apply FORMAT within the MESSAGE Statement like we can in a DISPLAY statement.
    ** NOTE ** : The FORMAT clause is meant for defining how a variable should be displayed when used with DISPLAY, not MESSAGE.

    - Correct Approach 
    ```
      DEFINE VARIABLE price AS DECIMAL 
      FORMAT "$>>9.99" 
      INITIAL 99.99 
      NO-UNDO.

      MESSAGE "Price:" price SKIP "Tax: 10%".
    ```

    - Color in Console (Character Mode)
    ```
      MESSAGE "This is red text" COLOR RED.
    ```
---

### DATE and the functions

* Core Date functions
- TODAY : Returns Current System Date.
    ```
      DEFINE VARIABLE currentDate AS DATE NO-UNDO.
      currentDate = TODAY.
      MESSAGE "Today's date : " currentDate VIEW-AS ALERT-BOX.
    ```
- DATE : Creates a date from month/date/year components or validate a date.
    - Syntax: 
        ```
          DATE(month, day, year)  /* Construction */
          DATE(stringDate)        /* Conversion */
        ```
    - EXAMPLE: 
        ```
          DEFINE VARIABLE newDate AS DATE NO-UNDO.
          newDate = DATE(12, 25, 2023).  /* Christmas 2023 */

          /* Validation : ERROR : Day in month is invalid.*/
          IF DATE(2, 30, 2023) = ? THEN 
          MESSAGE "Invalid date" VIEW-AS ALERT-BOX.
        ```

* Date Component Functions 
- DAY : Extracts day of the month (1-31)
    - Syntax : DAY(date)
    ```
      MESSAGE "Today is day" DAY(TODAY) "of the month".
    ```
- MONTH : Extracts month number (1-12)
    - Syntax : MONTH(date)
    ```
      DEFINE VARIABLE monthNames AS CHARACTER EXTENT 12 INITIAL [
      "January", "February", "March", "April", "May", "June",
      "July", "August", "September", "October", "November", "December"
      ].
      MESSAGE "Current month:" monthNames[MONTH(TODAY)].
    ```
- YEAR : Extracts 4-digit year
    - Syntax : YEAR(date)
    ```
      IF YEAR(TODAY) > 2020 THEN
      MESSAGE "Post-2020 date".
    ```
- WEEKDAY : Returns Day name (Monday - Sunday)
    - Syntax : WEEKDAY(date)
    ```
      MESSAGE "Today is" WEEKDAY(TODAY).
    ```
* Date Conversion Functions
- ISO-DATE : Converts to ISO 8601 fromat (YYYY-MM-DD)
    - Syntax: ISO-DATE(date)
    ```
      MESSAGE "ISO format:" ISO-DATE(TODAY).  /* "2023-11-15" */
    ```
- STRING : Custom date formatting.
    - Syntax: STRING(date, "format")
    ```
    /*
      Common Formats: 
      "99/99/9999" -> "11/15/2023"
      "99-99-9999" -> "11-15-2023"
      "Month DD, YYYY" -> "November 15,2023"
    */
      MESSAGE "Formatted:" STRING(TODAY, "Month DD, YYYY").
    ```
* Date/Time Functions 
    - NOW : current datetime (date + time).
      - Syntax: NOW
      ```
        MESSAGE "Current datetime: " NOW.
      ```
    - MTIME : current datetime (date + time).
      - Syntax: MTIME
      ```
        MESSAGE "Milliseconds today: " MTIME.
      ```

---
* Shared Variables
- What are Shared Variables?
    - Shared Variables are global variables that can be accessed across: 
        - Multiple Procedures (*.p files)
        - Internal Procedures
        - Functions
        - Trigger blocks

- When to use Shared Variables ?
  - Use When
    - we need to share data between different program files.
    - Passing many parameters is cumbersome.
    - we need persistent values across procedure calls.
  - Avoid when
    - Local variables can solve the problem.
    - Thread safety is needed (use Parameters instead)
    - we risk naming conflicts.

- Types of Shared Varables
    - SHARED Variables
    - What?
      - variables shared between specific procedures that explicitly declare them.
      - Must be redefined identically in every procedure that uses them.
    - When?
      - When we need to share data between a few related procedures.
      - When we want to control exactly which procedures can access the variable.
    - Example: 
    ```
      /* --- main.p --- */
      DEFINE SHARED VARIABLE svCustomerName AS CHARACTER NO-UNDO.
      svCustomerName = "John Smith".

      RUN helper.p.

      /* --- helper.p --- */
      DEFINE SHARED VARIABLE svCustomerName AS CHARACTER NO-UNDO. /* Must redeclare */
      MESSAGE "Customer:" svCustomerName VIEW-AS ALERT-BOX. /* Shows "John Smith" */
    ```
    - NEW SHARED (GLOBAL) Variables
    - What?
      - Truly global variables available to all procedures after declaration.
      - Only need to be defined once with NEW GLOBAL SHARED.
    - When?
      - For application-wide settings (like user ID, company code).
      - When many procedures need access to the same value.
    - Example: 
    ```
      /* --- config.p --- */
      DEFINE NEW GLOBAL SHARED VARIABLE gvCompanyCode AS CHARACTER INITIAL "ACME" NO-UNDO.

      /* --- report1.p --- */
      /* No need to redeclare - automatically available */
      MESSAGE "Company:" gvCompanyCode VIEW-AS ALERT-BOX. /* Shows "ACME" */

      /* --- report2.p --- */
      /* Also automatically available */
      DISPLAY gvCompanyCode.
    ```
---

### Input and Output Parameters

- Input Parameters (Passing Data into a Procedure)
    - What? 
      - Values passed into a Procedure
      - Cannot be modified by the called Procedure.
    - Syntax: 
    ```
      DEFINE INPUT PARAMETER paramName AS data-type NO-UNDO.
    ```
    - EXAMPLE :
    ```
      /* --- main.p --- */
      DEFINE VARIABLE custID AS INTEGER INITIAL 1001 NO-UNDO.

      RUN GetCustomerName.p (INPUT custID).

      /* --- GetCustomerName.p --- */
      DEFINE INPUT PARAMETER ipiCustID AS INTEGER NO-UNDO.

      MESSAGE "Customer:" ipiCustID VIEW-AS ALERT-BOX.
    ```
- Output Parameters (Returning Data from a Procedure)
    - What? 
      - Values returned from a Procedure
      - must be assigned a value inside the called Procedure.
    - Syntax: 
    ```
      DEFINE OUTPUT PARAMETER paramName AS data-type NO-UNDO.
    ```
    - EXAMPLE :
    ```
      /* --- main.p --- */
      DEFINE VARIABLE custName AS CHARACTER NO-UNDO.

      RUN GetCustomerName.p (INPUT 1001, OUTPUT custName).

      MESSAGE "Customer Name:" custName VIEW-AS ALERT-BOX.

      /* --- GetCustomerName.p --- */
      DEFINE INPUT  PARAMETER ipiCustID  AS INTEGER   NO-UNDO.
      DEFINE OUTPUT PARAMETER opcCustName AS CHARACTER NO-UNDO.

      opcCustName = "vidyadhar challa".
      MESSAGE "Custid: " ipiCustID.
    ```
- INPUT-OUTPUT Parameters (Two-Way Communication)
    - What? 
      - Passes a Value in and  returns a modified value
      - Acts as both input and output.
    - Syntax: 
    ```
      DEFINE INPUT-OUTPUT PARAMETER paramName AS data-type NO-UNDO.
    ```
    - EXAMPLE :
    ```
      /* --- main.p --- */
      DEFINE VARIABLE orderTotal AS DECIMAL INITIAL 100.00 NO-UNDO.

      RUN ApplyDiscount.p (INPUT-OUTPUT orderTotal).

      MESSAGE "Discounted Total:" orderTotal VIEW-AS ALERT-BOX.

      /* --- ApplyDiscount.p --- */
      DEFINE INPUT-OUTPUT PARAMETER iopdTotal AS DECIMAL NO-UNDO.

      iopdTotal = iopdTotal * 0.9. /* Apply 10% discount */
    ```
---
### Arrays
- Arrays in progress (called extents) allow us to store multiple values of the same data type under a single variable name.
- Declaration 
  ```
  DEFINE VARIABLE variableName AS data-type EXTENT array-size NO-UNDO.

  DEFINE VARIABLE cCustomers AS CHARACTER EXTENT 10 NO-UNDO.

  /*
  ASSIGNMENT 
  By index (1-based by default) 
  */
    cCustomers[1] = "John Smith".
    cCustomers[2] = "Maria Garcia".

  /* Initialize all values at once */
  DEFINE VARIABLE iNumbers AS INTEGER EXTENT 3 INITIAL [10, 20, 30] NO-UNDO.

  /* Dynamic Arrays (Unknown Size) */
  DEFINE VARIABLE cDynamicArray AS CHARACTER EXTENT NO-UNDO.
  DEFINE VARIABLE iArraySize AS INTEGER NO-UNDO.

  /* Set size at runtime */
  iArraySize = 5.
  EXTENT(cDynamicArray) = iArraySize. 

  cDynamicArray[1] = "First element".

  /* Multi-Dimensional Arrays: Progress doesn't support true multi-dimensional arrays, but we can simulate them. */
  DEFINE VARIABLE cGrid AS CHARACTER EXTENT 10 EXTENT 10 NO-UNDO. /* 10x10 grid */
  cGrid[1][1] = "Top-left corner".

  /* Get Array Size */
  DEFINE VARIABLE iSize AS INTEGER NO-UNDO.
  iSize = EXTENT(cCustomers).  /* Returns 10 for our first example */
 
  /* Check if Array Element Exists */ 
  IF cCustomers[5] <> ? THEN 
   MESSAGE "Element 5 exists".
  ```
- Limitations: 
    - All elements must be the same data type.
    - Fixed sized arrays can't be resized after declaration.
    - No built-in sort or search functions (code manually).
- When to Use?
    - Lists of similar items (Customer names, daily totals)
    - Fixed size data sets (days of week, months)
    - Temporary storage before database operations.
- When to Avoid?
    - Variable size data (use temp-tables instead).
    - Complex relationships (use temp-tables or Objects).
    - Large datasets (Memory Consumption).
---
### PROPATH
- PROPATH is a environment variable that specifies the search path Progress uses to locate :
    - Procedure files (.p , .w, .cls)
    - Include files (.i)
    - Class files (.r)
    - Other reference files.
- It works similar to PATH environment variable in Operating Sytems.
- Key Uses of PROPATH
    - Locating Program files when using RUN statements.
    - Finding include files during compilation.
    - Loading Class definitions in OOABL (Object-oriented ABL)
    - Managing development vs deployment environments.
- We can get current session PROPATH using : 
```
MESSAGE PROPATH.
```
#### Modifying PROPATH for a Single Session

- Temporarily Add a Directory
```
  PROPATH = PROPATH + ",C:\my_new_directory".
```

- Prepend a Directory (Higher Priority)
```
  PROPATH = "C:\my_new_directory," + PROPATH.
```
---
### Questions
- What is a .p file and .r file?
    - .p file (Source Code Files)
      - Human readable source code file.
      - contains original procedure code we write.
      - must be compiled before execution.
      - we need to use this during development
      - when we need to modify code.
    - .r files (Compiled Runtime Files)
      - Compiled binary version of .p file.
      - created when we compile a .p file.
      - cannot be directly viewed or edited.
      - Executes faster than .p file.
      - Required for execution (after first compilation)
      - Platform-Specific (must be re-compiled if moving between OS).

- What are the files able to be generated using COMPILE statement?
    - When COMPILE statement is run then it can generate following files: 
        - .r : The compiled Runtime file  (main output).
        - .lst : Listing file (If Listing option is used).
        - .xref : Cross-reference file (If XREF option is used).
        - .debug : Debug listing file (If DEBUG-LIST is used).
        - .json : Metadata file (If JSON option is used in newer versions).

- What are the files able to generate using COMPILE statement even .p file has error?
    - If .p (procedure) file has errors, The COMPILE statement does not generate a .r file (compiled runtime file), but it can still generate certain diagnostic files, depending on the options used.
    - Files that can still be generated even if the .p file has errors:
        - .lst – Listing file 
          - Contains the expanded source code and compiler messages.
          - Generated if LISTING option is used.
          - Useful for debugging and viewing preprocessor output.
        - .xref – Cross-reference file
          - Generated if XREF option is used.
          - May be partially filled if compile fails early.
        - .debug – Debug listing file
          - Created if DEBUG-LIST option is used.
          - May show partial debug info before the error occurred.
        - .log – Optional compiler log file 
          - If explicitly redirected or logged.

- What is advantage of .r file?
    - .r files are compiled version of .p files, advantages are :
      - Faster execution (no need for runtime compilation).
      - Protects source code(as they're compiled).
      - Smaller file Size compared to Source
      - Better Performance for frequently used procedures.

- How to add a Path to Propath permanently?
  - Using Command line SET PROPATH "c:\newPathDirectory".

- Which will be executed first both .p and .r file in same directory?
  - .r file will be executed.
  - Lets also see different situations also: 
    | Situation                     |                  What Happens                          |
    |-------------------------------|--------------------------------------------------------|
    | `.r` exists and is up-to-date | `.r` is executed                                       |
    | `.p` is newer than `.r`       | `.p` is recompiled, new `.r` is generated, and executed|
    | `.r` is missing               | `.p` is compiled and executed (if no errors)           |

- Create a Program say a.p which have output message "Hi". Compile and generate .r file. Then modify the a.p to prompt different output message. Execute the a.p in another procedure using RUN command. Verify what the output is and suggest a reason?
    - First lets create an a.p and initialize a message.
    ```
    MESSAGE "HI" VIEW-AS ALERT-BOX.
    ```
    - compile a.p -> this command will generate a.r file in same directory.
    - Let us modify the message of a.p 
    ```
    MESSAGE "HI, THERE WELCOME TO PROGRESS 4GL" VIEW-AS ALERT-BOX.
    ```
    - We don't compile this again. a.r file contains still old version ("HI")
    - Lets create another procedure called "caller.p" 
    ```
      RUN a.p.
    ```
    - When we run this caller.p : RUN caller.p then we get output as "HI", because even though we have changed the text in a.p, the system still executes the compiled .r file, not the modified .p file. As we did not compile again the changed file. When we recompile a.p and try to run Caller.p again then we will be able to see output : HI, THERE WELCOME TO PROGRESS 4GL.

- Explain the datatypes available in progress its default value and maximum limit?
  - Progress 4GL (ABL) Data Types

| Data Type      | Description                            | Default Value | Maximum Limit / Range                                       |
|----------------|----------------------------------------|----------------|-------------------------------------------------------------|
| `CHARACTER`    | String or text                         | `""`           | 32,000 characters                                           |
| `INTEGER`      | 32-bit signed integer                  | `0`            | -2,147,483,648 to 2,147,483,647                             |
| `INT64`        | 64-bit signed integer                  | `0`            | ±9,223,372,036,854,775,807                                  |
| `DECIMAL`      | Precise decimal numbers                | `0`            | Up to 50 digits, default 10 decimal places                  |
| `DATE`         | Date only                              | `?`            | 0001-01-01 to 9999-12-31                                    |
| `DATETIME`     | Date with time                         | `?`            | Same as DATE + time (millisecond precision)                |
| `DATETIME-TZ`  | Date with time and timezone info       | `?`            | Same as DATETIME + timezone offset                         |
| `LOGICAL`      | Boolean value                          | `FALSE`        | `TRUE`, `FALSE`, `?` (unknown)                              |
| `RECID`        | Record identifier                      | `?`            | Unique per record (not portable)                           |
| `RAW`          | Binary data                            | `?`            | 32,000 bytes                                                |
| `BLOB`         | Large Binary Object                    | `?`            | Up to 2 GB (database-dependent)                             |
| `CLOB`         | Large Character Object                 | `?`            | Up to 2 GB (database-dependent)                             |
| `HANDLE`       | Reference to runtime ABL object        | `?`            | Varies (used for dynamic buffers, queries, etc.)            |
| `COM-HANDLE`   | Reference to COM/OLE object (Windows)  | `?`            | Varies (Windows-specific feature)                           |

- What are the differences among local variable, shared variable, Parameters?
  ### Comparison: Local Variable vs Shared Variable vs Parameter


| Feature             | Local Variable                          | Shared Variable                          | Parameter                                    |
|---------------------|------------------------------------------|------------------------------------------|----------------------------------------------|
| **Scope**           | Only visible within the current procedure | Visible across procedures if declared SHARED | Passed between procedures/functions          |
| **Declaration**     | `DEFINE VARIABLE`                         | `DEFINE SHARED VARIABLE`                  | `DEFINE PARAMETER` / `DEFINE INPUT/OUTPUT/INPUT-OUTPUT PARAMETER` |
| **Purpose**         | Temporary storage for internal use        | Share data across multiple procedures     | Pass values between caller and callee         |
| **Lifetime**        | Exists only during the procedure run      | Exists as long as the program is active   | Exists during the procedure call             |
| **Modifiability**   | Only within the procedure                 | Any procedure using it can modify         | Can be read/write based on mode               |
| **Default Behavior** | Not accessible outside the procedure      | Must be declared SHARED in both producer and consumer | Access defined by parameter mode (INPUT, OUTPUT, etc.) |


- Difference between RECID and ROWID?
### Difference Between `RECID` and `ROWID` in Progress 4GL

| Feature          | RECID                                          | ROWID                                                  |
|------------------|------------------------------------------------|--------------------------------------------------------|
| **Definition**   | Unique record identifier assigned by Progress  | Unique record identifier used by the database          |
| **Portability**  | Not portable across databases                  | Portable across databases                              |
| **Uniqueness**   | Unique only within a single table              | Globally unique across all tables                      |
| **Use Case**     | Mainly used in ABL for quick access to records | Used for referencing records across databases          |
| **Data Type**    | Integer                                        | ROWID data type (binary value)                         |
| **Supported In** | Mostly Progress DBs                            | Also compatible with external databases (e.g., Oracle) |
| **Performance**  | Very fast access to records                    | Slightly less efficient than RECID                     |

> ⚠️ Note: `ROWID` is more modern and recommended for database interoperability, while `RECID` is legacy and specific to Progress databases.

- What sets of buttons are available within the alert box?

| Button Set Constant          | Description                                   |
|------------------------------|-----------------------------------------------|
| `BUTTONS OK`                 | Displays only an **OK** button                |
| `BUTTONS OK-CANCEL`          | Displays **OK** and **Cancel**                |
| `BUTTONS YES-NO`             | Displays **Yes** and **No**                   |
| `BUTTONS YES-NO-CANCEL`      | Displays **Yes**, **No**, and **Cancel**      |
| `BUTTONS RETRY-CANCEL`       | Displays **Retry** and **Cancel**             |
| `BUTTONS ABORT-RETRY-IGNORE` | Displays **Abort**, **Retry**, and **Ignore** |


- How do you declare array variable and explain the data types that array can support?
    - We can declare an array variable using the EXTENT keyword, which specifies the number of elements in the array.
    - Syntax: 
    ```
      DEFINE VARIABLE array-name AS data-type EXTENT n.

      -> array-name: Name of the array variable
      -> data-type: any valid ABL (Advanced Business Language) datatype.
      -> n : number of elements in the array.
    ```
    - Example :
    ```
    DEFINE VARIABLE scores AS INTEGER EXTENT 3.
    scores[1] = 85.
    scores[2] = 90.
    scores[3] = 78.
    ```
    - Datatypes that Arrays can support are: 
        - INTEGER
        - DECIMAL
        - CHARACTER
        - DATE
        - LOGICAL
        - INT64
---
- Why can't we use frame with message statement?
  - MESSAGE is a model dialog box which is handled outside of the standard UI frames.It is a window object not tied to or controlled by a frame object.
  - FRAME is used to organize fields, labels, and output in a block of UI.
  - ALERT-BOX is a System-level pop-up.
  ```
  /* ERROR */
  MESSAGE "Processing.."
  MESSAGE "Closing Soon..." VIEW-AS ALERT-BOX MESSAGE TITLE "Message" WITH FRAME f.
  PAUSE 2 BEFORE-HIDE.
  HIDE FRAME f.
  PAUSE 3 MESSAGE "Loading data.."
  DISPLAY "Back again!" WITH FRAME f.

  /*correct */
  MESSAGE "Processing.."
  DISPLAY "Closing Soon..." WITH FRAME f.
  PAUSE 2 BEFORE-HIDE.
  HIDE FRAME f.
  PAUSE 3 MESSAGE "Loading data.."
  DISPLAY "Back again!" WITH FRAME f.
  ```