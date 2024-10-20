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



















