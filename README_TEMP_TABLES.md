### Temp table example
- Temp table will inherit all the schema fieds and its indexs.
```
DEFINE TEMP-TABLE t_cust LIKE customer.
FOR EACH customer WHERE state = "TX":
    CREATE t_cust.
    BUFFER-COPY customer TO t_cust.
END.

FOR EACH t_cust:
    DISPLAY t_cust WITH 2 COLUMNS WITH WIDTH 100.
END.
```
- Empty Temp-Table syntax and example
```
/* Syntax */
EMPTY TEMP-TABLE temp-table-name [NO-ERROR].
/* Example */
EMPTY TEMP-TABLE t_cust NO-ERROR.
```

- Lets say if we have to display Texas related customer data and then California related data then we have to empty the temp table and reuse it again in same seesion. It is not an best practice to use same temp-table for both.
```
DEFINE TEMP-TABLE t_cust LIKE customer.
FOR EACH customer WHERE state = "TX":
    CREATE t_cust.
    BUFFER-COPY customer TO t_cust.
END.
/*Texas */
FOR EACH t_cust:
    DISPLAY t_cust WITH 2 COLUMNS WITH WIDTH 100.
END.

EMPTY TEMP-TABLE t_cust.

FOR EACH customer WHERE state = "CL":
    CREATE t_cust.
    BUFFER-COPY customer TO t_cust.
END.
/* California */
FOR EACH t_cust:
    DISPLAY t_cust WITH 2 COLUMNS WITH WIDTH 100.
END.
```

### Work Tables 
- It will not inherit the indexes of the actual table remaining is same as Temp table.so, because it doesn't use the Indexes so it cannot use FIND instead we can use FIND FIRST.
- Syntax : 
```
DEFINE [[NEW] SHARED]
{WORK-TABLE | WORKFILE} work-table-name
[NO-UNDO]
[LIKE tablename [VALIDATE]]
[FIELD field-name
    {
        AS data-type | LIKE field
    }
    [field-options]
]...
```
- Example of Work-File Definition
```
/* r-wrkfil1.p */
DEFINE WORK-TABLE wk_cust
    FIELD wkf_cnum LIKE customer.CS-ID LABEL "Customer"
    FIELD wkf_climit AS DECIMAL FORMAT "999999.99" LABEL "C Limit".

/* r-wrkfil2.p */
DEFINE WORK-TABLE wk_cust NO-UNDO LIKE customer.

```

### Preprocessor Directives
- Preprocessor is a powerful tool that enhances our programming flexibility.
- The Preprocessor is a component of the Progress Compiler which prepares the final version of the progress code just before it is compiled.
- A preprocessor directive is a statement that begins with an ampersand (&) and is meaningful only to the preprocessor.
- Preprocessor can be controlled by using preprocessor directives throughout the progress source code.
- **Compile Time constants**
- The &GLOBAL-DEFINE and &SCOPED-DEFINE directives allow us to define preprocessor names, which are compile-time constants.
- Syntax:
```
/* &GLOBAL-DEFINE */
&GLOBAL-DEFINE preprocessor-name definition

/* Example */
&GLOBAL-DEFINE MAX-EXPENSE 5000
&Message "{&MAX-EXPENSE}".

/* &SCOPED-DEFINE */
&SCOPED-DEFINE preprocessor-name definition

/* Example */
&SCOPED-DEFINE MAX-EXPENSE 5000

```
- **Built-in Pre-Processors**
- BATCH-MODE
- FILE-NAME
- LINE-NUMBER
- SEQUENCE
- WINDOW-SYSTEM

- **Nested Preprocessors**
```
&GLOBAL-DEFINE X Y
&GLOBAL-DEFINE Y Z
&GLOBAL-DEFINE Z "Hello"

DISPLAY {&{&{&{include.i}}}}

/* DECODE */
include.i -> X

    DISPLAY {&{&{&{X}}}}
    X -> Y
    DISPLAY {&{&Y}}
    Y -> Z
    DISPLAY {&Z}
    Z -> "Hello"
    DISPLAY "Hello".
```

### Include Files
- Reusable code should be placed in this so that can be accessed in many sessions. like for example as we cannot use temp-tables in multiple sessions so, we can include that temp-table in this file and use it in multiple sessions.
- Include file contains statements that have to be included during the compilation of a progress main procedure.
- syntax to use include file in procedure is :  {includenewfile.i}
```
{include-file [argument | {&argument-name = "argument-value"}]...}
```

