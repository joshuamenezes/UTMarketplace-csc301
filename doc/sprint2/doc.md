# General API
- the base endpoint is where the django server is running. Normally, it will run on localhost:8000
- all endpoint return html file.

# HTTP return codes
- **HTTP 200 OK** is normal return code, including the cases when forms raise validation errors.
- **HTTP 4xx** is the return code when the client sends informal requests. The issue is on the client side usually.
- **HTTP 403 Forbidden** is the return code when the client does not send the csrf token.
- **HTTP 404 Not Found** is the return code when the client send requests to a url not in server.
- **HTTP 5xx** is the return code when the server has errors. The issue is on the server side usually.

# Notes
- every form has a csrf token check. If the client does not send it, server will send HTTP 403 back.
- POST request must get sent to the url with a slash ('/'). Normally, if a user wants to try to get the page without a slash, server will append a slash and return the url. If clients use things like postman to send a POST request to a url without a slash, server will send HTTP 500 back.
- most of validation error and view error will still keep HTTP 200.

# Endpoints
## /users/signup
- GET POST
- Fields
-- *email*
-- *password1*
-- *password2*
-- *username*
- Validation error
-- *This field is required*
-- *Enter a valid email address*
-- *Must be a utoronto address*
-- *The two password fields didn’t match*
-- *This password/The password* password safety
- View error
-- user exists
- Success URL
-- /users/login
- Status code
-- *HTTP 200* when clients access the page, no matter forms raise validation errors or view errors
-- *HTTP 302* when clients signup and go to /users/login
- Note
Server will send an email with an active code to the email address in the form

## /users/login
- GET POST
- Fields
-- *email*
-- *password*
- Validation error
-- *This field is required*
-- *Enter a valid email address*
- View error
-- *wrong user or password*
- Success URL
-- /users/home
- Status code
-- *HTTP 200* when clients access the page, no matter forms raise validation errors or view errors
-- *HTTP 302* when clients send POST request without validation error and view error and go to /users/home

## /users/logout
- GET
- Success URL
-- /users/login
- Status code
-- *HTTP 200* when clients access the page, no matter forms raise validation errors or view errors
-- *HTTP 302* when clients send POST request without validation error and view error and go to /users/login

## /users/active/{active_code}
- GET
- View error
-- *no this user* when the user matching the code is not in databases
-- *no this code* when the code is not is databases
- Status code
-- *HTTP 200* when clients access the page, no matter forms raise validation errors or view errors

## /users/forgetpassword
- GET POST
- Fields
-- *email*
- Validation error
-- *This field is required*
-- *Enter a valid email address*
- View error
-- *no this user* when the user matching the code is not in databases
- Status code
-- *HTTP 200* when clients access the page, no matter forms raise validation errors or view errors



## /users/reset/{reset_code}
- GET POST
- Fields
-- *password1*
-- *password2*
- Validation error
-- *This field is required*
-- *This password/The password* password safety
- View error
-- *no this user* when the user matching the code is not in databases
-- *no this code* when the code is not is databases
- Status code
-- *HTTP 200* when clients access the page, no matter forms raise validation errors or view errors

## /users/change_profile_password
- GET POST
- Fields
-- *old_password*
-- *password1*
-- *password2*
- Validation error
-- *This field is required*
-- *Your old password was entered incorrectly. Please enter it again.*
-- *This password/The password* password safety
- View error
-- *please login* when the client is not logining
- Success URL
-- /users/profile/{id}
- Status code
-- *HTTP 200* when clients access the page, no matter forms raise validation errors or view errors
-- *HTTP 302* when clients send POST request without validation error and view error and go to /users/profile/{id}
- Note
Users do not need to login again after changing their passwords

## /users/search_results
- GET POST
- Fields
-- search
- Status code
-- *HTTP 200* regardless of the user input
- Note
-- This returns a page of listings that matches what a user searched for in the search bar
-- This endpoint should be accessed through a POST request
-- If a user sends a GET request, the page will prompt the user to enter an item in the search bar

## /users/bookmarks
- GET
- Fields
-- None
- Status Code
-- *HTTP 200* regardless of the user input
- Note
-- This endpoint returns a list of all listings that the user has bookmarked.

## /listings/
- GET
- Fields
-- None
- Status Code
-- *HTTP 200* regardless of the user input
- Note
-- This endpoint returns all current listings available on the marketplace.

## /listings/{listing_id}/details
- GET
- Fields
-- None
- Status Code
-- *HTTP 200* If a listing with this id exists.
-- *HTTP 404* If no such listing exists.
- Note
-- This endpoint returns information of a given listing.

## /listings/{listing_id}/edit
- GET POST
- Fields
-- Item Name
-- Listing Name
-- Description
-- Price
-- Category
- Status Code
-- *HTTP 200* If a listing with this id exists.
-- *HTTP 404* If no such listing exists.
- Note
-- This endpoint edits existing information of a given listing.

## /listings/{listing_id}/delete
- GET POST
- Fields
-- None
- Status Code
-- *HTTP 200* Upon successful deletion of the listing.
-- *HTTP 404* If no such listing exists.
- Note
-- This endpoint deletes a given listing. Upon successful deletion