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
7. Progress Compiler generates platform-independent '.r' files.
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
