# SOP 
1. Using JS to access DOM objects of the web site

   ![image](https://github.com/user-attachments/assets/5f48d35c-88ce-4ae1-8ef4-0ada9fc8e58d)

User Authentication: Since the victim is already logged in to mysite.com, their browser will include the valid session cookies or authentication tokens automatically when the request is sent to http://mysite.com/transfer.php.
Trust Misuse: The server assumes the request comes from a legitimate user and processes it, not realizing it was crafted by an attacker.

   docker exec -it naughty_ramanujan sh -l

 echo "10.9.0.5 mysite.com" >> /etc/hosts

 echo "10.9.0.5 darksite.com" >> /etc/hosts


 ![image](https://github.com/user-attachments/assets/c1bc280a-9aeb-4216-aeb3-4566f35fed3e)


 ![image](https://github.com/user-attachments/assets/cc3b1826-63fc-4266-9f4a-ff88d6614837)


4c. Fail because The User Must Be Logged In: The victim must already be authenticated on http://mysite.com with a valid session.

4d. Enter the cookie value 

![image](https://github.com/user-attachments/assets/59a8a108-dcf8-4732-887c-8c2b59dc4846)


![image](https://github.com/user-attachments/assets/4132d313-715b-44f8-99e0-c16a080c8b91)


```php
<?php
session_start();

// Generate CSRF token if not already set
if (!isset($_SESSION['csrf_token'])) {
    $_SESSION['csrf_token'] = bin2hex(random_bytes(32)); // Generate a new token
}

// Authenticate the user
if (!isset($_SESSION['authenticated']) || !$_SESSION['authenticated']) {
    header('Location: login.php');
    exit();
} else {
    echo "Welcome, " . $_SESSION['username'] . "!";
}

if ($_SERVER['REQUEST_METHOD'] === 'POST') {
    // Validate CSRF token
    if (!isset($_POST['csrf_token']) || $_POST['csrf_token'] !== $_SESSION['csrf_token']) {
        die('CSRF token validation failed!');
    }

    $amount = $_POST['amount'];
    $to = $_POST['to'];

    // Transfer the amount
    $balance = $_SESSION['balance'];
    if ($balance >= $amount) {
        $balance -= $amount;
        $_SESSION['balance'] = $balance;
        // Perform transfer
        $transfer = "Transfer {$amount} to {$to}<br>";
        echo $transfer;
        echo "Amount left {$balance}<hr>";
    } else {
        die('Insufficient balance!');
    }
}
?>

<form action="" method="POST">  
    <input type="hidden" name="csrf_token" value="<?php echo $_SESSION['csrf_token']; ?>">
    <label for="amount">Amount:</label>
    <input type="number" id="amount" name="amount" required>
    <br>
    <label for="to">To:</label>
    <input type="text" id="to" name="to" required>
    <br>
    <input type="submit" name="transfer" value="Transfer">
</form>
```

# XSS 

   console.log(document.cookie)

4.4.1. Modify your own salary.

![image](https://github.com/user-attachments/assets/dfc5832f-4204-4ed6-a1e8-f36776b8cd6d)


![image](https://github.com/user-attachments/assets/4636f1c2-4c86-45bf-91b1-d3b995e00d42)


    

4.4.2. Modify other people’ salary


![image](https://github.com/user-attachments/assets/9d3fec17-d006-4ecd-838a-d2559d03c986)


![image](https://github.com/user-attachments/assets/b98a910c-8c37-4243-9e16-9cd45f33fcfb)




![image](https://github.com/user-attachments/assets/b6a0c010-b8b5-4e9a-98b2-fc76dae9dc4b)


son ' , password = 'c22b5f9178342609428d6f51b2c5af4c0bde6a42' where name='Boby' --  

![image](https://github.com/user-attachments/assets/13d117b7-2d02-4744-b40b-2fdbf48cc81c)



![image](https://github.com/user-attachments/assets/1ccfbcf9-f3b3-4964-8355-c1fe30ef673b)



4.5. Countermeasure – Prepared Statement


![image](https://github.com/user-attachments/assets/e44d37c4-daa6-40a9-95b2-9dae628f460d)


![image](https://github.com/user-attachments/assets/10283733-2f68-4018-8e2b-c11f24f8aa21)

```php
<?php
// Function to create a sql connection.
function getDB() {
  $dbhost="10.9.0.6";
  $dbuser="seed";
  $dbpass="dees";
  $dbname="sqllab_users";

  // Create a DB connection
  $conn = new mysqli($dbhost, $dbuser, $dbpass, $dbname);
  if ($conn->connect_error) {
    die("Connection failed: " . $conn->connect_error . "\n");
  }
  return $conn;
}

$input_uname = $_GET['username'];
$input_pwd = $_GET['Password'];
$hashed_pwd = sha1($input_pwd);

// create a connection
$conn = getDB();

// Prepare the SQL statement
$stmt = $conn->prepare("SELECT id, name, eid, salary, ssn FROM credential WHERE name = ? AND Password = ?");
if ($stmt === false) {
    die("Prepare failed: " . htmlspecialchars($conn->error));
}

// Bind parameters
$stmt->bind_param("ss", $input_uname, $hashed_pwd); // "ss" means two strings

// Execute the statement
$stmt->execute();

// Get the result
$result = $stmt->get_result();
// do the query
//$result = $conn->query("SELECT id, name, eid, salary, ssn
//                        FROM credential
//                        WHERE name= '$input_uname' and Password= '$hashed_pwd'");
if ($result->num_rows > 0) {
  // only take the first row
  $firstrow = $result->fetch_assoc();
  $id     = $firstrow["id"];
  $name   = $firstrow["name"];
  $eid    = $firstrow["eid"];
  $salary = $firstrow["salary"];
  $ssn    = $firstrow["ssn"];
}


// close the sql connection
$stmt->close();
$conn->close();
?>
```

change this code  



```php
// Prepare the SQL statement
$stmt = $conn->prepare("SELECT id, name, eid, salary, ssn FROM credential WHERE name = ? AND Password = ?");
if ($stmt === false) {
    die("Prepare failed: " . htmlspecialchars($conn->error));
}

// Bind parameters
$stmt->bind_param("ss", $input_uname, $hashed_pwd); // "ss" means two strings

// Execute the statement
$stmt->execute();

// Get the result
$result = $stmt->get_result();
// do the query
//$result = $conn->query("SELECT id, name, eid, salary, ssn
//                        FROM credential
//                        WHERE name= '$input_uname' and Password= '$hashed_pwd'");






$stmt->close();


```
![image](https://github.com/user-attachments/assets/db627429-6bfe-4c30-8f21-2798e37b5e1e)










Web security – XSS – File Upload Vulnerabilitie 


<script>
  console.log(document.cookie);
</script>











