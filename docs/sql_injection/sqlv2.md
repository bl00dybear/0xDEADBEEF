## Table of Contents

- [Introduction](#introduction)

- [Command Types](#command-types)

  - [Data Definition Language (DDL)](#data-definition-language-ddl)
  - [Data Manipulation Language (DML)](#data-manipulation-language-dml)
  - [Transaction Control Language (TCL)](#transaction-control-language-tcl)
  - [Data Control Language (DCL)](#data-control-language-dcl)

- [Data Types](#data-types)

  - [Numeric Data Types](#numeric-data-types)
  - [String Data Types](#string-data-types)
  - [Date & Time Data Types](#date--time-data-types)
  - [Other Data Types](#other--data-types)

- [Operators](#operators)

  - [Arithmetic Operators](#arithmetic-operators)
  - [Comparison Operators](#comparison-operators)
  - [Logical Operators](#logical-operators)
  - [Wildcards](#wildcards)

- [Data Definition](#data-definition)

  - [Creating Tables](#creating-tables)
  - [Modifying Tables](#modifying-tables)
  - [Dropping Tables](#dropping-tables)
  - [Constraints](#constraints)
    - [Primary Key](#primary-key)
    - [Foreign Key](#foreign-key)
    - [Unique](#unique)
    - [Not Null](#not-null)
    - [Check](#check)
    - [Default](#default)
    - [Auto Increment](#auto-increment)

- [Data Manipulation](#data-manipulation)

  - [Inserting Data](#inserting-data)
  - [Updating Data](#updating-data)
  - [Deleting Data](#deleting-data)
  - [Subqueries](#subqueries)
  - [Joins](#joins)
    - [Inner Join](#inner-join)
    - [Left Join](#left-join)
    - [Right Join](#right-join)
    - [Full Outer Join](#full-outer-join)
  - [Clauses](#clauses)
    - [WHERE](#where)
    - [GROUP BY](#group-by)
    - [HAVING](#having)
    - [ORDER BY](#order-by)

- [Functions](#functions)

  - [Scalar Functions](#scalar-functions)
  - [String Functions](#string-functions)
  - [Date & Time Functions](#date--time-functions)
  - [Conversion Functions](#conversion-functions)

- [Aggregate Functions](#aggregate-functions)

  - [COUNT](#count)
  - [SUM](#sum)
  - [AVG](#avg)
  - [MIN](#min)
  - [MAX](#max)

- [Transactions](#transactions)

  - [ACID Properties](#acid-properties)
  - [Starting a Transaction](#starting-a-transaction)
  - [Committing a Transaction](#committing-a-transaction)
  - [Rolling Back a Transaction](#rolling-back-a-transaction)
  - [Savepoints](#savepoints)

- [Users and Permissions](#users-and-permissions)

  - [Creating Users](#creating-users)
  - [Granting Permissions](#granting-permissions)
  - [Revoking Permissions](#revoking-permissions)
  - [Role Management](#role-management)

- [Views](#views)

  - [Creating Views](#creating-views)
  - [Updating Views](#updating-views)
  - [Dropping Views](#dropping-views)

- [Sequences](#sequences)

  - [What are Sequences?](#what-are-sequences)
  - [Creating Sequences](#creating-sequences)
  - [Using Sequences](#using-sequences)
  - [Modifying Sequences](#modifying-sequences)
  - [Dropping Sequences](#dropping-sequences)

- [Indexes](#indexes)

  - [What are Indexes?](#what-are-indexes)
  - [Creating Indexes](#creating-indexes)
  - [Types of Indexes](#types-of-indexes)
  - [Dropping Indexes](#dropping-indexes)

- [Procedures](#procedures)

  - [What are Stored Procedures?](#what-are-stored-procedures)
  - [Creating Procedures](#creating-procedures)
  - [Executing Procedures](#executing-procedures)
  - [Modifying Procedures](#modifying-procedures)
  - [Dropping Procedures](#dropping-procedures)

- [Triggers](#triggers)
  - [What are Triggers?](#what-are-triggers)
  - [Creating Triggers](#creating-triggers)
  - [Types of Triggers](#types-of-triggers)
  - [Modifying Triggers](#modifying-triggers)
  - [Dropping Triggers](#dropping-triggers)
