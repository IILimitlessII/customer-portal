## PayView Payment Portal Frontend Documentation

This document provides an overview of the frontend code for the PayView Payment Portal. The frontend is built using React, TypeScript, and Vite.

### Inputs

The frontend receives user input through various forms and components, including:

- **Registration Form:** Collects user information for account creation.
- **Login Form:** Takes user credentials for authentication.
- **OTP Form:** Accepts the one-time password sent to the user's email.
- **Edit Profile Form:** Allows users to update their account details.
- **Payment Form:** Gathers payment information and address details.
- **Contact Form:** Takes user messages and inquiries.
- **Chat Component:** Enables user interaction with the virtual assistant.

### Outputs

The frontend presents information to the user through several components and pages, such as:

- **Dashboard:** Displays user account summary, payment options, and contact information.
- **User Payments:** Shows a list of previous payments and their details, both in a table format and visualized through charts (pie chart for payment distribution and bar chart for total amounts by payment method).
- **Payment Confirmation:** Provides feedback on the payment process, including success or error messages.
- **Notifications:** Presents alerts and updates related to user actions, like successful registration, profile edits, or payments.
- **Chat Responses:** Shows replies from the virtual assistant based on user input.

### Usage

Users can interact with the portal to:

- Create a new account.
- Log in to their existing account.
- Securely make payments using various methods (Credit Card, Stripe, PayPal, Apple Pay).
- Review their past payments.
- Update their profile information.
- Send inquiries and messages to the support team.
- Get help from the virtual assistant for navigating the portal and its features.

The frontend code aims to provide a user-friendly and secure interface for managing online payments and interacting with the portal's services.