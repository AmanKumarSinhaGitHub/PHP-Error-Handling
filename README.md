# PHP Error Handling


Effective error handling is critical for diagnosing issues, debugging, and ensuring the reliability of PHP applications. This guide provides comprehensive instructions on handling various types of errors and exceptions in PHP, including configuration, debugging, and best practices.

## Table of Contents
1. [PHP Errors](#php-errors)
2. [Database Errors](#database-errors)
3. [Form Validation Errors](#form-validation-errors)
4. [File Upload Errors](#file-upload-errors)
5. [Custom Errors](#custom-errors)
6. [Exception Handling](#exception-handling)
7. [Logging Errors](#logging-errors)
8. [Debugging SQL Queries](#debugging-sql-queries)

---

## 1. PHP Errors

### Types of PHP Errors
- **Syntax Errors**: Incorrect syntax in PHP code. Identified by the parser during script execution.
- **Runtime Errors**: Occur during script execution due to undefined variables, functions, or other runtime issues.
- **Deprecated Warnings**: Notifications about functions or features that are obsolete and might be removed in future PHP versions.

### Enabling Error Reporting
Ensure error reporting is enabled in your development environment to catch all issues.

```php
// Enable full error reporting
error_reporting(E_ALL);
ini_set('display_errors', 1);
ini_set('display_startup_errors', 1);
```

### Disabling Error Reporting in Production
In a production environment, disable error display to prevent exposing sensitive information.

```php
// Disable error display in production
ini_set('display_errors', 0);
ini_set('log_errors', 1);
ini_set('error_log', '/path/to/error.log');
```

## 2. Database Errors

### Types of Database Errors
- **Connection Errors**: Failures in connecting to the database due to incorrect credentials or server issues.
- **Query Errors**: Problems executing SQL queries, often due to syntax errors or constraint violations.

### Handling Database Errors
```php
// Database connection example
$conn = mysqli_connect($servername, $username, $password, $dbname);
if (!$conn) {
    die("Connection failed: " . mysqli_connect_error());
}

// Query execution example
$result = mysqli_query($conn, $sql);
if (!$result) {
    die("Query failed: " . mysqli_error($conn));
}
```

### Prepared Statements
Use prepared statements to avoid SQL injection and handle errors more gracefully.

```php
$stmt = $conn->prepare("SELECT * FROM table WHERE id = ?");
$stmt->bind_param("i", $id);
if (!$stmt->execute()) {
    die("Execution failed: " . $stmt->error);
}
```

## 3. Form Validation Errors

### Types of Form Validation Errors
- **Missing Required Fields**: Required fields left empty.
- **Invalid Input**: Data that does not conform to expected formats or constraints.

### Handling Form Validation Errors
```php
// Example of handling form validation errors
if (empty($_POST['field_name'])) {
    $errors[] = "Field is required.";
}
if (!filter_var($_POST['email'], FILTER_VALIDATE_EMAIL)) {
    $errors[] = "Invalid email format.";
}
```

## 4. File Upload Errors

### Types of File Upload Errors
- **Upload Failures**: Issues like exceeding file size limits or invalid file types.

### Handling File Upload Errors
```php
// Example of handling file upload errors
if ($_FILES['file']['error'] !== UPLOAD_ERR_OK) {
    switch ($_FILES['file']['error']) {
        case UPLOAD_ERR_INI_SIZE:
        case UPLOAD_ERR_FORM_SIZE:
            echo "File size exceeds limit.";
            break;
        case UPLOAD_ERR_PARTIAL:
            echo "File upload incomplete.";
            break;
        case UPLOAD_ERR_NO_FILE:
            echo "No file uploaded.";
            break;
        default:
            echo "Unknown upload error.";
            break;
    }
}
```

## 5. Custom Errors

### Types of Custom Errors
- **Application-Specific Errors**: Errors specific to the application logic or business rules.

### Handling Custom Errors
```php
// Custom error handler function
function customErrorHandler($errno, $errstr, $errfile, $errline) {
    error_log("Error [$errno]: $errstr - $errfile:$errline");
    echo "An error occurred. Please try again later.";
}

set_error_handler("customErrorHandler");
```

## 6. Exception Handling

### Handling Uncaught Exceptions
- **Uncaught Exceptions**: Errors thrown by exceptions that are not caught by try-catch blocks.

### Exception Handling Example
```php
try {
    // Code that may throw an exception
} catch (Exception $e) {
    error_log("Caught exception: " . $e->getMessage());
    echo "An error occurred: " . $e->getMessage();
}
```

### Custom Exception Handling
```php
class CustomException extends Exception {}

try {
    throw new CustomException("Custom exception thrown.");
} catch (CustomException $e) {
    error_log("Caught custom exception: " . $e->getMessage());
    echo "Custom error: " . $e->getMessage();
}
```

## 7. Logging Errors

### Types of Error Logging
- **Error Logging**: Record errors in a log file for later review and debugging.

### Configuring Error Logging
```php
// Configure PHP to log errors to a file
ini_set('log_errors', 1);
ini_set('error_log', '/path/to/error.log');
```

### Using Monolog for Advanced Logging
For more advanced logging, consider using libraries like Monolog.

```php
use Monolog\Logger;
use Monolog\Handler\StreamHandler;

$log = new Logger('name');
$log->pushHandler(new StreamHandler('/path/to/your.log', Logger::WARNING));

$log->warning('Warning message');
$log->error('Error message');
```

## 8. Debugging SQL Queries

### Printing SQL Queries
When debugging, print SQL queries to ensure they are correct.

```php
// Debug: Print the SQL query
echo "SQL Query: " . $updateSql . "<br>";
```

### Using SQL Error Reporting
Enable SQL error reporting to troubleshoot query issues.

```php
// Example: Enable SQL error reporting
mysqli_report(MYSQLI_REPORT_ERROR | MYSQLI_REPORT_STRICT);
```

---

## Summary

- **Enable Error Reporting**: Use `error_reporting(E_ALL)` and `ini_set('display_errors', 1)` for development.
- **Database Error Handling**: Use error checking and prepared statements to handle database errors.
- **Form Validation**: Validate and sanitize user inputs to avoid common form errors.
- **File Uploads**: Manage file upload errors and validate file types and sizes.
- **Custom Errors**: Implement custom error handlers for application-specific issues.
- **Exception Handling**: Use try-catch blocks for robust exception handling.
- **Logging**: Configure error logging and use advanced tools like Monolog.
- **Debugging**: Print SQL queries and enable SQL error reporting for debugging.

By following these practices, you can effectively manage errors and maintain the reliability of your PHP applications.
