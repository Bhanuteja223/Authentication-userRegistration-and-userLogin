## Authentication: 
We have to verify whether a user is valid/invalid based on his identity.
This process is called Authentication.

How can we restrict access to book for specific users?

specified user access to books: Maintain the list of user in a table and provide access only to those specified users.

- Goodreads APIs : User needs to be registered and then login to access the books.
Step1:
   - Register user API:
      HTTP Request:         
         -> Method: POST                                                 
         -> Content-Type: application/json                               
         -> Body: User Details
         -> URL: http://localhost:3000/users/

     HTTP Respose:
        -> Status: 200
        -> Body: User successfully Created.

Step2:
   Encrypting Password:
    Storing the password in plain text within a database is not a good idea since they can be misused.
    "**Passwords should be encrypted**".
    To Generate an encrypted password we have a Third-party package called "bcrypt".
    to install it in terminal: "npm install bcrypt --save"

Step3: 
    bcrypt Methods:
      bcrypt package provides methods to perform operations like encryption, comparision, etc.

      1. bcrypt.hash(): encryption
      2. bcrypt.compare(): comparing

   1. Encrypting Password:
                  const hashedPassword = await bcrypt.hash(password, saltRounds);
      bcrypt.hash() uses various processes and encrypts the given password and makes it unpredictable.
Step4:
   Checking for User in the Database:
   SQL Syntax:
            "`SELECT * FROM user WHERE username = '${username}';`"
   condition: If no user present it gives undefined value.
   if condition true:
      Instert the userdetailes:
       SQL Syntax: `INSERT INTO user(username, name, password, gender, location) VALUES ('${username}', '${name}', '${hashedPassword}', '${gender}', '${location}')`
   if condition false:
      // Send invalid username as response
      response.status(400) // bad request
      response.send('User name already exist')

##################################################################

Step1:
   - Login user API:
      HTTP Request:                                                  
         -> Method: POST                                                                    
         -> URL: http://localhost:3000/login/
     HTTP Response
        -> Status: 200
        -> Body: Login Success

Step2:
   Use POST method:
   http://../users/?username=rahul&password=rahul@123
   using query or path parameters for passing credentials is not recommended.

   so we prefer POST method over GET method

Step3:
   Check if User exist in Database:
      SQL query:
         `SELECT * FROM user WHERE username = ${username}`

Step4:
   if condition true:
      check the user entered password and hashedPassword using bcrypt.compare()

      Syntax: bcrypt.compare(password, dbUser.password);

Step5:
   if password matched:
      send response as "Login Success"
   else:
      send response as "invalid Password"
      

