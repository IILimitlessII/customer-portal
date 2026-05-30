## PayView API Documentation

This document outlines the API endpoints for the PayView Customer Portal, a platform designed for secure online payments and detailed visualization of payment history. The API facilitates user registration, login, payment processing, address management, and communication with customer support.

### Authentication Endpoints

These endpoints manage user authentication and related actions.

* **`/register` (POST)**
    * **Description:** Registers a new user. A verification email is sent to the provided email address.
    * **Input:**
        * `fullname`: User's full name (string)
        * `email`: User's email address (string)
        * `password`: User's desired password (string)
        * `capVal`: reCAPTCHA response token (string)
    * **Output:**
        * Success message: "User registered successfully, please check your email to confirm"
        * User object: `{ fullname, email }`

* **`/confirm-email` (GET)**
    * **Description:** Confirms a user's email address using a confirmation link sent during registration.  Upon successful confirmation, an email containing the user's account number is sent.
    * **Input (Query Parameters):**
        * `id`: User ID (string)
        * `confirmation_gen`: Confirmation code from the email link (string)
    * **Output:**
        * Redirects to the login page upon successful confirmation: `https://apdscustomerportal.online/login` 

* **`/login` (POST)**
    * **Description:** Logs in an existing user. Sends an OTP (One-Time Password) email to the user's confirmed email address.
    * **Input:**
        * `accountNumber`: User's account number (string)
        * `password`: User's password (string)
        * `capVal`: reCAPTCHA response token (string)
    * **Output:**
        * Success message: "OTP sent to your email!"
        * `sessionToken`: A unique session token (string)

* **`/verify-otp` (POST)**
    * **Description:** Verifies the OTP provided by the user during login.
    * **Input:**
        * `otp`: 6-digit OTP code (string)
        * `sessionToken`: Session token received during login (string)
    * **Output:**
        * Success message: "Login successful!"

* **`/forgotpassword` (POST)**
    * **Description:** Initiates a password reset. Sends an email containing a new, randomly generated password to the user's registered email address.
    * **Input (Query Parameters):**
        * `accountNumber`: User's account number (string)
        * `token`: User's session token (string) 
    * **Output:**
        * Redirects to the login page after sending the password reset email: `https://apdscustomerportal.online/login`

* **`/changepassword` (POST)**
    * **Description:** Changes the password of an authenticated user.
    * **Input:** 
        * `token`: User's session token (string)
        * `password`: User's current password (string)
        * `newpassword`: User's new password (string)
    * **Output:**
        * Success message: "Password Changed Successfully!"

* **`/logout` (POST)**
    * **Description:** Logs out a user and clears the `sessionToken` and `otp_complete` flag. 
    * **Input:**
        * `sessionToken`: User's session token (string)
    * **Output:**
        * Success message: "Logout successful!"

### User Data Endpoints

These endpoints manage user data and are protected by a session token.

* **`/address` (POST)**
    * **Description:** Adds a new address to a user's profile.
    * **Input:**
        * `line_1`: Street address (string)
        * `line_2`: Optional additional address line (string)
        * `province`: Province (string)
        * `city`: City (string)
        * `postal_code`: Postal code (string)
        * `token`: User's session token (string)
    * **Output:**
        * Success message: "Address saved successfully!"

* **`/myaddresses` (POST)**
    * **Description:** Retrieves all addresses associated with an authenticated user.
    * **Input:**
        * `token`: User's session token (string)
    * **Output:**
        * Success message: "Address saved successfully!" 
        * `addresses`: Array of address objects (array)

* **`/payment` (POST)**
    * **Description:** Processes a payment for an authenticated user. Validates card details for Credit Card and Stripe payments. Sends a confirmation email with payment information.
    * **Input:**
        * `email`: User's email address (string)
        * `fullName`: User's full name (string)
        * `paymentMethod`: Payment method ID (1 for Credit Card, 2 for PayPal, 3 for Stripe, 4 for Apple Pay) (integer)
        * `cardNumber`: Card number (required for Credit Card and Stripe) (string)
        * `expirationDate`: Card expiration date in MM/YY format (required for Credit Card and Stripe) (string)
        * `cvv`: Card CVV (required for Credit Card and Stripe) (string)
        * `amount`: Payment amount (number)
        * `token`: User's session token (string)
    * **Output:**
        * Success message: "Payment processed successfully."
        * Payment data object

* **`/mypayments` (POST)**
    * **Description:** Retrieves all payments associated with an authenticated user.
    * **Input:**
        * `token`: User's session token (string)
    * **Output:**
        * Success message: "Payments retrieved successfully!"
        * `payments`: Array of payment objects (array)

* **`/userdetails` (POST)**
    * **Description:** Fetches the details of an authenticated user.
    * **Input:**
        * `token`: User's session token (string)
    * **Output:**
        * Success message: "User retrieved successfully!"
        * `user`: User object: `{ email, fullname, account_number }` (object)

* **`/edituser` (POST)**
    * **Description:** Updates an authenticated user's full name and email address.
    * **Input:**
        * `token`: User's session token (string)
        * `fullname`: User's new full name (string)
        * `email`: User's new email address (string)
    * **Output:**
        * Success message: "User updated successfully!"
        * Updated user object (object)

### Customer Support Endpoint

* **`/customerform` (POST)**
    * **Description:** Processes a contact form submission from an authenticated user. Uses Google's Gemini AI model to generate a response to the user's message. Sends an email with the AI-generated response to the user's email address. 
    * **Input:**
        * `fullname`: User's full name (string)
        * `email`: User's email address (string)
        * `message`: User's message (string)
    * **Output:**
        * Success message: "Expect an email response shortly..."

### Notes

* All API endpoints are assumed to be prefixed with `/api/`.
* All POST requests require a JSON body.
* The `sessionToken` is used to authenticate all user data endpoints and the customer support endpoint.
* The `capVal` parameter in `/register` and `/login` is used for reCAPTCHA validation to prevent bot submissions.
* Password complexity is enforced during registration and password changes.
* Successful payment processing through `/payment` will redirect the user to the dashboard. 

This documentation provides a high-level overview of the PayView Customer Portal API. For more detailed information, refer to the source code comments. 8