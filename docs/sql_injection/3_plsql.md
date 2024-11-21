# SQL

<img src="./img/plsql/plsqlWallpaper.png">

## **Table of Contents**

- [Introduction to PL/SQL](#introduction-to-plsql)

- [Data Types in PL/SQL](#data-types-in-plsql)

  - [Scalar](#scalar-data-types)
  - [Composite](#composite-data-types)

- [Variables and Constants](#variables-and-constants)

- [PL/SQL Blocks](#sql-blocks)

- [Control Structures](#control-structures)

- [PL/SQL Collections](#sql-collections)

- [Error Handling](#error-handling)

- [Best Practices](#best-practices)

- [Procedures](#procedures)

  - [Creating Procedures](#creating-procedures)
  - [Executing Procedures](#executing-procedures)
  - [Modifying Procedures](#modifying-procedures)
  - [Dropping Procedures](#dropping-procedures)

- [Triggers](#triggers)
  - [Creating Triggers](#creating-triggers)
  - [Types of Triggers](#types-of-triggers)
  - [Modifying Triggers](#modifying-triggers)
  - [Dropping Triggers](#dropping-triggers)

## **Introduction to PL/SQL**

PL/SQL (Procedural Language/SQL) extends SQL with procedural constructs, allowing for complex logic in database operations. PL/SQL enables:

- **SQL Integration**: Execute SQL queries within PL/SQL.
- **Procedural Logic**: Use variables, loops, conditionals, and exception handling.
- **Reusability**: Create stored procedures, functions, and packages for modular code.

## **Data Types in PL/SQL**

### **Scalar Data Types**

PL/SQL supports several scalar types for storing single values:

- **Numeric Types**:

  - `NUMBER`: General-purpose numeric type with subtypes (`DECIMAL`, `NUMERIC`, `FLOAT`, `DOUBLE PRECISION`, `INT`, `INTEGER`, `SMALLINT`, `REAL`).
  - `BINARY_FLOAT`, `BINARY_DOUBLE`: High-performance floating-point types.
  - `PLS_INTEGER`, `BINARY_INTEGER`: Subtypes include `NATURAL`, `NATURALN`, `POSITIVE`, `POSITIVEN`, `SIGNTYPE`, `SIMPLE_INTEGER`.

- **Character Types**:

  - `VARCHAR2`, `VARCHAR`: Used for variable-length string storage.
  - `CHAR`, `CHARACTER`: Used for fixed-length character storage.

- **Unicode Types**:

  - `NCHAR`, `NVARCHAR2`: Store Unicode character data.

- **Date and Time Types**:

  - `DATE`, `TIMESTAMP`, `TIMESTAMP WITH TIME ZONE`, `TIMESTAMP WITH LOCAL TIME ZONE`: For storing date and time values.
  - `INTERVAL YEAR TO MONTH`, `INTERVAL DAY TO SECOND`: For storing time intervals.

- **Boolean Type**:
  - `BOOLEAN`: Stores logical values (`TRUE`, `FALSE`, or `NULL`).

### **??Composite Data Types**

Composite types group multiple values:

- **RECORD**: Structure for grouped fields with various types.
- **Collections**:
  - **Index-By Tables**: Key-value pairs.
  - **Nested Tables**: Unordered collections of elements.
  - **VARRAYs**: Ordered collections with a fixed maximum size.

## **Variables and Constants**

### **Declaration Syntax**

Variables and constants are declared with a specific type:

```sql
identifier [CONSTANT] data_type [NOT NULL] [{:= | DEFAULT} value];
```

Example:

```sql
salary NUMBER := 50000;
bonus CONSTANT NUMBER(1,2) := 0.1;
hours NUMBER NOT NULL := 8;
```

### **Using `%TYPE` and `%ROWTYPE`**

**Used instead of a `data_type`**

- **`%TYPE`**: Derives a variable's type from an existing database column or variable.
  ```sql
  identifier table_name.column_name%TYPE;
  ```
- **`%ROWTYPE`**: Creates a variable with the same structure as a database table or cursor.
  ```sql
  identifier table_name%ROWTYPE;
  ```

## **PL/SQL Blocks**

PL/SQL is block-structured, allowing for modular and nested code.

### **Structure**

```sql
[<<block_label>>]
DECLARE
  -- Declarations
BEGIN
  -- Executable statements
[EXCEPTION]
  -- Error-handling statements
END [block_label];
```

### **Types of Blocks**

1. `Anonymous Blocks`:
   - Unnamed, compiled and executed at runtime.
   - Even if they contain a label, they are still anonymus.
2. `Named Blocks`:
   - Procedures, Functions, Triggers, or Packages stored in the database.

---

## **Control Structures**

PL/SQL supports various control statements for managing the flow of execution, enabling developers to implement logic, loops, and conditional branching in their code.

### **Conditional Statements**

- **IF-THEN-ELSE**:
  Used to execute a block of statements based on a condition. If the condition evaluates to `TRUE`, the statements in the `THEN` block are executed; otherwise, the `ELSE` block (if provided) is executed.

  ```sql
  IF condition THEN
    -- Statements executed if condition is TRUE
  ELSE
    -- Statements executed if condition is FALSE
  END IF;
  ```

  **Explanation**:
  The `IF-THEN-ELSE` structure allows you to control the flow of the program based on whether a certain condition is met. If no `ELSE` is provided, the `THEN` block will be skipped when the condition is false.

- **CASE**:
  A more flexible way to handle multiple conditions. It allows you to test one expression against multiple possible values.

  ```sql
  CASE expression
    WHEN value1 THEN
      -- Action for value1
    WHEN value2 THEN
      -- Action for value2
    ELSE
      -- Default action when no value matches
  END CASE;
  ```

  **Explanation**:
  The `CASE` statement checks the value of the given `expression` and compares it with several possible `WHEN` values. If a match is found, the corresponding action is executed. If no match occurs, the `ELSE` block (if present) is executed.

### **Loops**

Loops are used to repeat a block of statements multiple times.

- **Basic Loop**:
  A simple loop structure that keeps executing statements until an explicit exit condition is met.

  ```sql
  LOOP
    -- Statements
    EXIT WHEN condition;  -- Exit loop when condition is TRUE
  END LOOP;
  ```

  **Explanation**:
  The `LOOP` structure will run indefinitely unless the `EXIT` condition is met. The `EXIT WHEN` clause allows you to break out of the loop when a specific condition becomes true.

- **FOR Loop**:
  A loop that iterates a fixed number of times, often used when you know how many times the loop should run.

  ```sql
  FOR i IN 1..10 LOOP
    -- Statements executed for each value of i from 1 to 10
  END LOOP;
  ```

  **Explanation**:
  The `FOR` loop automatically initializes the loop variable (`i` in this case) and runs until the condition (`i <= 10`) is no longer true. The loop variable increments by 1 by default, but you can customize the step size.

- **WHILE Loop**:
  Executes statements as long as the given condition remains `TRUE`.

  ```sql
  WHILE condition LOOP
    -- Statements executed while condition is TRUE
  END LOOP;
  ```

  **Explanation**:
  The `WHILE` loop checks the condition before each iteration. If the condition is `TRUE`, the statements inside the loop are executed. If the condition is `FALSE` from the start, the loop does not execute at all.

### **Flow Control**

- **GOTO**:
  A statement used to jump to a specific label in the code. It's generally discouraged in modern programming because it can lead to unclear code structure, but it's still available in PL/SQL.

  ```sql
  GOTO label;
  -- Some code
  label:
    -- Code executed when GOTO is called
  ```

  **Explanation**:
  `GOTO` transfers control to a specific location in the code, labeled with `label:`. While `GOTO` can be useful in certain cases, it should be used sparingly because it can make the flow of control harder to follow.

- **EXIT**:
  Used to exit from loops, including `LOOP`, `FOR`, or `WHILE`. The `EXIT` statement can be used with a condition to break out of the loop early.

  ```sql
  EXIT WHEN condition;  -- Exit loop when condition is TRUE
  ```

  **Explanation**:
  The `EXIT` statement provides a way to break out of a loop based on a condition. If the condition is `TRUE`, control is transferred to the next statement following the loop.

## **PL/SQL Collections**

Collections allow manipulation of multiple elements of the same type.

### **Index-By Tables**

Dynamic arrays with key-value pairs:

```sql
TYPE index_table IS TABLE OF VARCHAR2(50) INDEX BY PLS_INTEGER;
names index_table;
```

### **Nested Tables**

Unordered collections, like single-column database tables:

```sql
TYPE nested_table IS TABLE OF NUMBER;
numbers nested_table := nested_table();
numbers.EXTEND;
numbers(1) := 10;
```

### **VARRAYs**

Fixed-size ordered arrays:

```sql
TYPE varray_type IS VARRAY(5) OF VARCHAR2(30);
employees varray_type := varray_type('Alice', 'Bob');
```

---

## **Error Handling**

PL/SQL includes robust error management using the `EXCEPTION` block:

```sql
BEGIN
  -- Statements
EXCEPTION
  WHEN NO_DATA_FOUND THEN
    -- Handle exception
  WHEN OTHERS THEN
    -- Handle other exceptions
END;
```

---
