# Reverse Engineering
## In the /Develop file

### Server.js:
The main purpose of this file is to set up an express server. This file sets up the port,  configures middleware for authentication, and uses the passport package to keep track of our user’s login status through sessions. The database needs to be connected first, then the port can run and if those are both successful the console will log that it is listening.

### Package.json:
This file includes the required package and information. Line 1 shows us the name of the package, and line 2 shows the version. Lastly,  lines 14 -21 include all the dependencies the package relies on to be used successfully.

### routes/api-routes.js:
This file handles all of our “post” and “get” routes and needs our database and passport to make this possible. The middleware is referenced just to send the user to the correct page according to if they have an account or not and if they don't have an account they got to the /api/signup page, if the user logs out /logout, and if the user wants to access data client side the get out /api/user_data is put into place.

### routes/html-routes.js:
This file is mainly to handle most situations for the login page including if the user needs to sign up, log in or trying to access the member page without actually being signed in. This route file requires both “path” and our middleware. This page makes sure that if the user tried to log in and already has an account it sends them to the members page or if they login to their account they get to the members page. Finally, If the user is not logged in and tries to access the members page they will be redirected to the signup page.

### Public/signup.html:
This file shows the UI for the signup page. It includes the sign up form that requires the users email address and password. It includes the sign up button and a log in here option to redirect you to the login page. This file uses bootstrap, stylesheets/style.css, jquery and the js/signup.js file. 

### Public/members.html:
This file shows the UI for the members page after you have logged in. It includes a “welcome” and inserts the member name for the user and a logout button as well. This file uses bootstrap, stylesheets/style.css, jquery and the js/members.js file. 

### Public/login.html:
This file shows the UI for the login  page after the user has logged in. It includes a “Login Form” which includes an Email address and Password input and a login button as well. The page also includes a “sign up here” link that leads you to the sign up page. This file uses bootstrap, stylesheets/style.css, jquery and the js/login.js file. 

### public/stylesheets/style.css:
The css stylesheet it used mainly to add style to the web application. In this case it references the form signup and the form logins to style and adds 50px margins to those classes.

### public/js/login.js:
This file references our form and inputs on our login.html page. We validate that the input information was an email and password and if that is the case we run the login user function and clear the form. The login user function does a post to our api/login route and goes to the members page if successful.

### public/js/members.js:
This file references the members.html page and does a get request to figure out which user is logged in and updates the html on the page. It uses the api/user_data to get the user information.

### public/js/signup.js:
This file references the form and input in our signup.html page. It has an onclick function the validates the email and password upon click of the signup button.if successful in posting to the signup route it takes us to the members page. Otherwise, it will indicate there was an error.

### Models/index.js:
Sequelize CLI automatically makes this database file and exports the database object. It used fs, path, sequelize, and references basename, env, config in order to auto create an object with the data information.

### Models/user.js:
This file requires the bcrypt package and works with sequelize for the datatables. This file creates the table information with the user email and password information. It also checks that it is a proper email and requires a password as well as comparing the unhashed password entered by the user with the hase password stored in the database.

### config/config.json:
This file has our database and requires our local password in order to access this information. This indicated development, test and production databases used form this app. Within all these databases we have the specific information needed to have access to these databases.

### config/passport.js:
This file uses “passport” to login with email and password. When the user tries to sign in, it runs the find one function in order to find the email that matches the db and then checks if the password is correct. If either of those are correct it will send a message indicating this entry is incorrect.

### config/middleware/isAuthenticated:
The middleware is used to run a function before you get to the routes. This file restricts the routes a user is not allowed to visit if not logged on. If it is authenticated it will let you run the members page and if not it will not run.

## Improvements:
Since we are working with objects, it may be easier to work with handlebars for this application because it is cleaner to deal with the front and back end as opposed to using regular html. 

I would add a confirm password function to this application as well. I don't trust users can type and/or remember the passwords they choose so id add a function that asks them to confirm their password when creating it and then make sure they match. If they do it will message “success” if not it will ask the user to start over and message “passwords did not match”.

The /develop folder should include a ReadMe.md file that goes with the app as well even if it’s a coding assignment.

[object, object] displays if you try to sign up again with the same email. This should be displaying the error message that is stored in the object. In the signup.js file, I did a console.log for the err parameter and saw that the only thing that had any kind of alert message was the statusText which has "error". So i made the following change:

        function handleLoginErr(err) {
            console.log(err)
            $("#alert .msg").text(err.statusText);
            $("#alert").fadeIn(500);
        }