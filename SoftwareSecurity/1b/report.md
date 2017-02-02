[back to Manifest](/a2/README.md)

#  Table of Vulnerabilities to Harden
1. [SQL Injection](#sql-injection)
2. [XSS](#xss)
3. [XSRF](#xsrf)
4. [Broken Authentication and Session Management](#broken-authentication-and-session-management)
5. [Server Configuration Ideas](#server-configuration)

SQL Injection
---------------------
[back to table](#table-of-vulnerabilities-to-harden)

###  Summary of SQL Fixes:

1. [Proper Prepared Statements Accross Application](#proper-prepared-statements)
2. [Whitelisting Expressions Input](#whitelisting-expressions-input)
3. [Industry Standard Authentication Methods](#industry-standard-authentication-methods)
4. [Use Completely Stored Proceedures](#use-completely-stored-proceedures)

### Fixes

1. __Fix__:
    #### Proper Prepared Statements

    [back to exploit summary](#summary-of-sql-fixes)

   1. __Problem:__
    
      Prepared statements used to prevent SQL injection are not written properly causing some malicious SQL 
      inputs to still exploit the applicaton
	
      __Referenced Code (From input.php, Lines 20-22 etc. ):__

		>	$query= "SELECT id, username, firstName, lastName, passwd FROM account WHERE username='$user' AND passwd='$password'";
		>
		>	$result = pg_prepare($dbconn, "", $query);
		>
		>	$result = pg_execute($dbconn, "", array());
	
      __Referenced Code (From input.php, Lines 44-46 etc. ):__

		> $dbconn = pg_connect_db();
		>
		> $result = pg_prepare($dbconn, "", "SELECT * FROM solution WHERE expression='$expression'");
		>
		> $result = pg_execute($dbconn, "", array());
		
	  __Referenced Code (From input.php, Lines 48-49 etc. ):__
		> $result = pg_prepare($dbconn, "", "insert into solution (value, expression, accountId) values ($value, '$expression', $accountId)");
		>
		> $result = pg_execute($dbconn, "", array()); 
			
	  __Referenced Code (From input.php, Lines 95-96 etc. ):__
	  > $result = pg_prepare($dbconn, "", "SELECT firstName, lastName, value, expression, s.accountId, s.id FROM account a, solution s WHERE a.id=s.accountId AND value=$i ORDER BY firstName, lastName, expression");
	  >
	  > $result = pg_execute($dbconn, "", array());

   3. __Solution:__
    
      Use prepared statements properly via inserting inputs into the array and replacing '$input' with the a variable placeholder
      
      __Example Code:__
       >	$query= "SELECT id, username, firstName, lastName, passwd FROM account WHERE username=$1 AND passwd=$2";
	   >
	   >	$result = pg_prepare($dbconn, "", $query);
	   >
	   >	$result = pg_execute($dbconn, "", array($user, $password));

2. __Fix__:

    #### Whitelisting Expressions Input

    [back to exploit summary](#summary-of-sql-fixes)

   1. __Problem:__
    
      User inputs are directly trusted to be valid and non-malicious by the web applicaton.
	
      __Referenced Code (From input.php, Line 40 etc. ):__

	  > $expression = $_REQUEST['expression'];

   3. __Possible Solution:__
    
      Filter out malicious characters code from php inputs before processing via whitelisting.
      
        __Example Code:__

	    >  $expression = preg_replace('[^0-9!*/-+^sqrt]', '',$_REQUEST['expression']);
	
	
	If we restrict users from entering characters that can not be used maliciously then we significantly limit exploits via inputs inside the web application. 
	Note that with more effort (and time to complete the assignment), we could also check to ensure the result of the expression is complient with the rules of the fourFours game.
	Note also that it is recommend ed we take a similar approach to sanitize the inputs of username and password, however due to time contrainst and the redundancy of functionality we leave the above as the only example
	
3. __Fix__:

    #### Industry Standard Authentication Methods

    [back to exploit summary](#summary-of-sql-fixes)

   1. __Problem:__
    
      Injection flaws are heavily present in the code from developers building custom authentication schemes. 
	
      __Referenced Code (From input.php, Line 20 etc. ):__

	  > $query= "SELECT id, username, firstName, lastName, passwd FROM account WHERE username='$user' AND passwd='$password'";

   3. __Possible Solution:__
    
      Consider using other authentication tools such as OWASP's ESAPI Authenticator or Google, Facebook, etc. login  APIs  
      [__Example Code__](http://www.krizna.com/general/login-with-facebook-using-php/)

2. __Fix__:

    #### Use Completely Stored Proceedures

    [back to exploit summary](#summary-of-sql-fixes)

   1. __Problem:__
      The arbitrary input paradigm of the assignment leads to too many security breaches. The developer should consider switching to an input type that is more easy to secure

   3. __Possible Solution:__
      Alter the application so that it does not depend on arbitrary inputs (ie use radio buttons). This we we can rely on static SQL statements etc.
 
XSS
---------------------
[back to table](#table-of-vulnerabilities-to-harden)

###  Summary of XSS Fixes:

1. [Sanitizing Dynamically Generated Outputs](#sanitizing-dynamically-generated-outputs)

### Fixes

1. __Fix__:
    #### Sanitizing Dynamically Generated Outputs

    [back to exploit summary](#summary-of-sql-fixes)

   1. __Problem:__
    
      Application does not account for malicious html inputs that get interpeted by the browser
      when dynamically outputted.
	
      __Referenced Code (From input.php, Line 74 etc. ):__
	> Welcome <?= $g_userFirstName ?>.

      __Referenced Code (From input.php, Line 106 etc. ):__
        > echo("\<tr\> \<td\>$expression\</td\>\<td\>$deleteLink\</td\>\<td\>$firstName $lastName\</td\>\</tr\>");
      
   2. __Solution:__
	Convert special html characters into entities that will get displayed by the browser ratehr than interpretted by it via the `PHP` function `htmlspecialchars(htmlString)`.
    	
      __Example Code:__
   	 > echo("\<tr\> \<td\>htmlspecialchars($_REQUEST['expression'], ENT_QUOTES, 'UTF-8')\</td\>\<td\>$deleteLink\</td\>\<td\>$firstName $lastName\</td\>\</tr\>");

	Note: Alternative fuctions include `htmlentities(htmlString)` and `htmlescape(htmlString)`. 

XSRF
---------------------
[back to table](#table-of-vulnerabilities-to-harden)

###  Summary of XSRF Fixes:

1. [Implement Page Tokens](#implement-page-tokens)

### Fixes

1. __Fix__:
    #### Implement Page Tokens

    [back to exploit summary](#summary-of-xsrf-fixes)

   1. __Problem:__
  
     Links and forms that lack an unpredictable token can be accessed by malicious links outside the site.
	
      __Referenced Code (From input.php, Line 16 ):__
      ```PHP
	   if($operation == "login"){
	   	...
	   	}
      ```
      __Referenced Code (From input.php, Line 33 etc. ):__
	  ```PHP
	  } elseif($operation == "addExpression"){
	  	...
	  }
	  ```
      
   2. __Solution:__
	  Associate intentional user actions a unique identifier that can verified
    	
      __Example Code:__
       On login set a hashed token:
       ```PHP
		$_SESSION["token"] = md5(uniqid(mt_rand(), true));
	   ?>
       ```
       In forms add a hidden token:
       ```PHP
	   <input type="hidden" name="action_token" value="<?php echo $_SESSION["token"]; ?>">
       ```
	   When recieveing an action require a matching token:
      ```PHP
	   } elseif($operation == "addExpression"){
		if (isset($_GET["action_token"]) && $_GET["action_token"] == $_SESSION["token"]) {
	    	...
	     }
	   }
      ```
	  This approach effectively handles XSRF attempts since it is virtually impossible to guess the generated number and have access to the same functionality that was once unprotected.
	  Note that altenative solutions could have been to prompt the user for their credentials before completing each action (instead of asking for the action token), which is more inconvienent to the user, OR verifying that a real user is requesting an action by using a CAPTCHA etc.


	
Broken Authentication and Session Management
---------------------
[back to table](#table-of-vulnerabilities-to-harden)

###  Summary of BASM Fixes:

1. [Encrypting Stored Passwords](#encrypting-stored-passwords)
2. [Handout Session id after login]
3. [Sessions are not properly invalidated on logout]
4. [Session does not timeout]
### Fixes

1. __Fix__:
    #### Encrypting Stored Passwords

    [back to exploit summary](#summary-of-sql-fixes)

   1. __Problem (lines 17-22 and similar...):__
      User authentication credtitials are not stored securely and are very accessible on system breach.
      ```PHP
      $user=$_REQUEST['user'];
      
	  $password=$_REQUEST['password'];
	  
	  $dbconn = pg_connect_db();
	  
	  $query= "SELECT id, username, firstName, lastName, passwd FROM account WHERE username='$user' AND passwd='$password'";
	  
	  $result = pg_prepare($dbconn, "", $query);
	  
	  $result = pg_execute($dbconn, "", array());
	  ```
   2. __Solution:__
      Note: This could not actually be implemented since the site did not have a regestration form and the timeline of the assignment did not permit building one. However, when users are added into the database the PostgreSQL function [`md5(string)`](http://www.postgresql.org/docs/9.1/static/auth-methods.html) should be used. If we assume these pass when asking for user passwords so the passwords are stored encrypted and thus less accessable to potiential hackers.
	  We assume the following registration form would have existed for the web application:
	  ```PHP
		.
		.
		.
		 //verify the inputs are not malicious and set them to variables
		 
		$pass = md5($password_clean);
		
		$query = "INSERT INTO account (..., username,...,..., password) VALUES (..., $1,...,...,$2)";
		$result = pg_prepare($dbconn, "", $query);
		$result = pg_execute($dbconn, "", array($user, $pass));
	  ```
	  In that case, we can handle user authentication more securly in the following manner
	  ```PHP
	  	.
		.
		.
		//verify the inputs are not malicious and set them to variables
		$pass = md5($password_clean);
		
	  	$query= "SELECT id, username, firstName, lastName, passwd FROM account WHERE username=$1 AND passwd=$2";
		$result = pg_prepare($dbconn, "", $query);
		$result = pg_execute($dbconn, "", array($user, $pass));
		
	  ```
	With this implementation our passwords can exist more securly on the database so that even if it were hacked into it would be difficult for the hacker to obtain the real passwords quickly. In that time we may have discovered the breach and had our users change thier passwords, this mitigating the issue.
	Note that we could also use hash functions to produce passwords that are even harder to crack such as `$password = hash("sha256", $password);` 
	Alternatively we could have chosen to use the `PHP` function `crypt()` to store and verify our passwords.
	with `crypt($password, $salt)` we follow the same process except instaed of only supplying the password to be hashed we supply a second parameter that act as a `$salt`. This `$salt` is appended to the password and should be random ain order to further encrypt the password for additional security:
	
	same as above except
	```PHP
	...
	$salt = 'Try1ngN0tT0F41Lc5c347';
	$pass =  crypt($passwrod_clean, $salt);
	...
	```

	Note that `crypt()` is capable of offering even stronger encryption stragies such as `CRYPT_BLOWFISH` etc.
	Finally since we are incrypting before sending over the network, we get the added benefit of protected passwords here as well.

1. __Fix__:
    #### Handout Session id after login

    [back to exploit summary](#summary-of-sql-fixes)

   1. __Problem:__
    
	  The website hands out session ids to anyone that visits it, which may create a problem if a malicious user obtains one of said cookies and then force-loads their cookie onto an unsuspecting user. If this happened and the user logged in than the attacker could use the original session id to get access to this user's account.
		
      __Referenced Code (From input.php, Line 2-5 etc. ):__

	  ```PHP
	  session_start();

		if(!isset($_SESSION['isLoggedIn']))
	       
	     ...
 	  ```
      
   2. __Solution:__
	  Only give out a a session id once the user has logged in.
    	
      __Example Code:__
   ```PHP
		if(isset($_SESSION['isLoggedIn']) && $_SESSION['isLoggedIn'] )
			session_start();
	     ...
	```      

1. __Fix__:
    #### Sessions are not properly invalidated on logout

    [back to exploit summary](#summary-of-sql-fixes)

   1. __Problem:__
  
      Session not fully destroyed on logout
	
      __Referenced Code (From input.php, Line 74 etc. ):__
	  ```PHP
	  	} elseif($operation == "logout"){
			unset($_SESSION);
		$_SESSION['isLoggedIn']=False;
	  ```
      
   2. __Solution:__

    	Destroy session using the statard process from `PHP` documentation
    	
      __Example Code:__
      
      ```PHP
      	} elseif($operation == "logout"){
		    $_SESSION = array();
        	session_destroy();
		$_SESSION['isLoggedIn']=False;
      ```
      
1. __Fix__:
    #### Session does not timeout

    [back to exploit summary](#summary-of-sql-fixes)

   1. __Problem:__
      
		Session dies not time out for a very long time

      __Referenced Code (From input.php, Line 74 etc. ):__
      
   2. __Solution:__

		Destroy the session after a given period of time
		
      __Example Code:__

	  ```PHP
	  	if (isset($_SESSION['TOUCH']) && ($_SESSION['TOUCH'] + 1000 < time() )) {
		   	$_SESSION = array();
		    session_destroy();
		    $_SESSION['TOUCH'] = time(); 
		}else{
		
			$_SESSION['TOUCH'] = time(); 
		}
	  ```
	Note that this code can also work to periodically change the sessions for avoid a session fixation attack:
	  ```PHP
  		if (isset($_SESSION['TOUCH']) && ($_SESSION['TOUCH'] + 1000 < time() )) {
	    	session_regenerate_id(true);  
	    	$_SESSION['TOUCH'] = time(); 
		}else{
			$_SESSION['TOUCH'] = time(); 
		}
  	  ```


Server Configuration Ideas:
-------------------
[back to table](#table-of-vulnerabilities-to-harden)
###  Server Configuration Fixes:

1. Configure Error Reports
   When the debugger information is outputted to all users of the application, it reveals sensitive information . Instead the debugging information should be restricted to developers via functions like [`client_min_messages (enum)` etc.] (http://www.postgresql.org/docs/9.4/static/runtime-config-logging.html). The configuration files of apache `/etc/apache2/apache2.conf` etc. should be changed to reveal less information about the server in the header 

2. Protect Against Unwanted Requests
   The server may still responds to get requests from the application. In this case the post requests to the application need to be locked down via a `.htaccess` file. This htaccess file may be modified to only include the users that are logged in currently (where once logged out they are removed from the file) or they may be included on registration.

3. Do Not make Requests Through URL
   Line 102 of the application has the follwong code:
   ```HTML
    <$deleteLink="<a href=\"?operation=deleteExpression&expressionId=$expressionId&accountId=$g_accountId\"><img src=\"delete.png\" width=\"20\" border=\"0\" /></a>";>
   ```	
   which should not

5. Limit Database Permissions
   
   It is safer to use a database connection with the most limited rights possible, thus the program permissions should be changed such that there is Query-only access relevant tables


6. Connect to database with hidden password
	Line 12 of the application has the passwords shown in  plain site: 
	```HTML
	$dbconn = pg_connect("dbname=fourfours user=ff host=localhost password=adg135sfh246"); <=> use environment variable>
	```
	The password should be stored in a variable outside the function and hashed so thet the password does not get 'sniffed' over the network.


7. Obviscate Server Logic
   The programmers of the web application should make a better attempt at masking the logic of the server in case of breach by changing form vairables and PSQL feild names to be different.


