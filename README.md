# Sql-Injection-How-to-Avoid-
how to AVOID Sql Injection by modifying the Mysql query code (just a lil bit)

What is SQL Injection ?
SQL injection usually occurs when you ask a user for input, like their username/userid, and instead of a name/id, the user gives you an SQL statement that you will unknowingly run on your database. (W3schools)

For example, we have a form that gets value of name field from an input box as so-
$name = $_POST['name'];

If $name is not cleansed and fed directly to query, a malicious user can enter something like 'JOHN' OR 1=1.
Now the query becomes-
select * from Users where name = 'JOHN' OR 1=1;

The SQL above is valid and will return ALL rows from the "Users" table, since OR 1=1 is always TRUE.
This is just an example and it can get much more serious than this.

To avoid Mysql Injection attacks, we can bin variables before being fed to the query like so-

Normal Mysql Select Query and fetching recods->
$sql = "select * from Users where name=".$name;
$result=mysqli_query($db,$sql);
while($row = mysqli_fetch_array($result,MYSQLI_ASSOC))
{echo $row['id'].") ".$row['names']."</br>";}


Bind variables to avoid sql injection->
$stmt = $db->prepare('SELECT * FROM Users WHERE name = ?');           //Select query
$stmt->bind_param('s', $name);                                       //this $name is from the form input
$stmt->execute();
$result = $stmt->get_result();
while ($row = $result->fetch_assoc()) 
{
	echo "Result with new method- ".$row['name']."</br>";
}

$stmt = $mysqli->prepare("INSERT INTO table (column) VALUES (?)");        //Insert query
$stmt->bind_param("s", $unsafe_variable);
$stmt->execute();
