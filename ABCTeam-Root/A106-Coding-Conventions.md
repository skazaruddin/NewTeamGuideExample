
# Coding Conventions for Java

## Introduction

Effective coding conventions improve readability, maintainability, and collaboration within a software project. Following principles from "Clean Code" by Robert C. Martin, these conventions aim to produce code that is easy to understand and modify.

## Naming Conventions

### Variables

- **Use meaningful and descriptive names** for variables that reveal intent.

```java
// Good
int numberOfStudents;
String customerName;
boolean isUserActive;

// Avoid ambiguous names
int n; // What does 'n' represent?
String str; // What does 'str' signify?
boolean flag; // What is the purpose of 'flag'?
```

- **Avoid mental mapping:** Choose names that make the code easier to understand without requiring readers to mentally translate what the name means.

```java
// Good
int numberOfStudents; // Clear and direct
String customerName; // Explicitly states the purpose

// Avoid mental mapping
int count; // Count of what?
String temp; // Temporary what?
```

- **Don't add unneeded context:** Avoid including unnecessary information in variable names that doesn't add to the understanding of the variable's purpose.

```java
// Good
int yearOfBirth; // Clear and focused

// Avoid unnecessary context
int customerYearOfBirth; // Redundant, 'customer' adds no value
```

- **Use explanatory variables:** Introduce variables to explain complex expressions or clarify the intent of a condition.

```java
// Good - Using explanatory variables
int eligibleAge = 18;
boolean isEligible = age >= eligibleAge;

// Avoid complex and unclear conditions without explanatory variables
boolean isEligible = age >= 18; // What does '18' signify?
```

- **Use searchable names:** Use names that can be easily searched within the codebase, making it simpler to find where and how variables are used.

```java
// Good
int studentCount;
String userName;
final int MAX_RETRIES = 3; // Constants are searchable by convention

// Avoid single-letter names or abbreviations that are not easily searchable
int cnt; // Difficult to search for and understand
```
