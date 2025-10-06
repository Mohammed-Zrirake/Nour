# Nour

## Overview

Nour is an online learning platform backend built with Node.js, Express, and Mongoose. It provides a comprehensive set of features for managing courses, users, enrollments, payments, and more. The frontend, built with React and TypeScript, interacts with this backend to deliver a complete e-learning experience.

- **Repository URL:** https://github.com/Mohammed-Zrirake/Nour
- **Default Branch:** `main`
- **License:** Not specified (Clarification Needed)

## Features

*   **User Authentication:** Secure signup, signin, and signout functionalities with JWT-based authentication.
*   **Role-Based Authorization:** Different user roles (admin, instructor, student) with specific permissions enforced through middleware.
*   **Course Management:** Creation, updating, publishing, and deletion of courses, sections, lectures, and quizzes.
*   **Enrollment Management:** Enrolling students in courses, tracking progress, and managing completion status.
*   **Payment Processing:** Integration with Stripe for handling payments and webhook events.
*   **Cart Management:**  Adding, removing, and clearing courses from user's cart, including coupon application.
*   **Review and Rating System:** Students can submit reviews and ratings for courses.
*   **Content Delivery:** Serving video content and managing access based on enrollment.
*   **Recommendation System:** SVD based system for predicting user ratings and getting the list of recommended courses and similar courses
*   **Cloudinary Integration:**  Uploading and managing images and videos using Cloudinary.
*   **Admin Dashboard:** Analytics and statistics for platform administration.
*   **Certificate Generation:**  Generating PDF certificates for course completion.
*   **Contact Form:**  Submission and handling of contact form messages via email.
*   **API Documentation:** (None Available yet)

## Technology Stack

*   **Backend:**
    *   Node.js
    *   Express
    *   Mongoose (MongoDB ODM)
    *   JWT (JSON Web Tokens)
    *   Stripe API
    *   Cloudinary API
    *   Nodemailer
    *   node-cron
    *   ml-matrix
    *   @tensorflow/tfjs-node
*   **Frontend:**
    *   React
    *   TypeScript
    *   React Router
    *   react-beautiful-dnd
    *   react-dropzone
    *   react-range
    *   react-intersection-observer
    *   express-rate-limit
    *   express-validator
    *   lucide-react
    *   react-apexcharts

## Prerequisites

Before you begin, ensure you have the following installed:

*   Node.js (v18 or higher)
*   MongoDB
*   Nodemon (optional, for development)
*   Cloudinary Account
*   Stripe Account

## Installation

1.  **Clone the repository:**
    ```bash
    git clone https://github.com/Mohammed-Zrirake/Nour.git
    cd Nour
    ```

2.  **Install backend dependencies:**
    ```bash
    cd Nour_Backend
    npm install
    ```

3.  **Configure environment variables:**

    Create a `.env` file in the `Nour_Backend` directory and populate it with the following:

    ```
    MONGO_URL=<your_mongodb_connection_string>
    JWT_KEY=<your_jwt_secret_key>
    PORT=8031 (Optional)
    Frontend_URL=<your_frontend_url>
    CLOUDINARY_CLOUD_NAME=<your_cloudinary_cloud_name>
    CLOUDINARY_API_KEY=<your_cloudinary_api_key>
    CLOUDINARY_API_SECRET=<your_cloudinary_api_secret>
    STRIPE_SECRET_KEY=<your_stripe_secret_key>
    STRIPE_WEBHOOK_SECRET=<your_stripe_webhook_secret>
    EMAIL=<your_email_address>
    EMAIL_KEY=<your_email_password>
    EMAIL_TO=<your_email_receiving_contact_forms>
    ```

4.  **Start the backend server:**
    ```bash
    npm run dev # or npm start
    ```

5.  **Install frontend dependencies:**
    ```bash
    cd Nour_Frontend
    npm install
    ```

6.  **Configure frontend environment variables:**

    Create a `.env` file in the `Nour_Frontend` directory and populate it with the following:

    ```
    VITE_API_URL=<your_backend_url>
    VITE_STRIPE_PUBLIC_KEY=<your_stripe_public_key>
    ```

7.  **Start the frontend development server:**
    ```bash
    npm run dev
    ```

## Usage Guide

### Environment Configuration
Ensure all necessary environment variables are properly set in both the backend (`Nour_Backend/.env`) and frontend (`Nour_Frontend/.env`) as these are critical for the application to function correctly.

### User Authentication
*   **Signup:** New users can register via the `/register` route on the frontend. Ensure to provide valid information and adhere to password requirements (minimum 8 characters with uppercase, lowercase, number, and special character).
*   **Signin:** Existing users can sign in via the `/sign-in` route on the frontend.
*   **Email Verification:**  After signup, users need to verify their email address by entering the OTP code sent to their registered email.
*   **Protected Routes:** Access to profile and other authenticated pages is guarded by the `ProtectedRoutes` component in the frontend.

### Course Management
*   **Creating a Course:** Instructors can create courses by navigating to their profile page, edit and publish it. They will have to provide title, description, thumbnail, and other details.
    *   `src/components/profile/Create Cours/index.tsx`
*   **Adding Sections and Lectures:** Instructors can organize their content by adding sections and lectures to each course.
    *   `src/components/profile/Create Cours/components/SectionBuilder.tsx`
*   **Cloudinary Usage:** Videos and images are uploaded to Cloudinary and their URLs stored in the database.
    *   `Nour_Backend/src/routers/cloudinary/cloud.routers.ts`
*   **Publishing a Course:** Courses must be published by an instructor to be available to students. Publishing is done via PUT request to `/api/courses/:id/publish`. The video files must be uploaded to cloudinary before publishing the course.

### Enrollment
*   **Enrolling in a Course:** Students can enroll in courses through the course details page `/course-details`, if they have credits or a subscription.
*   **Tracking Progress:** Students' progress is automatically tracked as they complete sections and lectures.
    *   `Nour_Backend/src/routers/enrollment/enrollment.routers.ts`

### Payment

*   **Payment Intent:** Students use Stripe to process payments when purchasing a course.
    *   `Nour_Backend/src/routers/Stripe/stripe.routers.ts`
*   **Webhooks:** Stripe webhooks handle payment confirmations and trigger enrollment processes.
    *   `Nour_Backend/src/routers/Stripe/stripe.routers.ts`

### Roles and Permissions

*   **Admin:** Can manage users, courses, and access analytics. Uses `roleIsAdmin` middleware in various routes.
    *   Example: `Nour_Backend/src/routers/admin/admin.routers.ts`
*   **Instructor:** Can create and manage their courses, but cannot access administrative functions. Uses `roleIsInstructor` middleware.
    *   Example: `Nour_Backend/src/routers/course/course.routers.ts`
*   **Student:** Can enroll in courses, track progress, and submit reviews.  Uses `roleIsStudent` middleware.
    *   Example: `Nour_Backend/src/routers/enrollment/enrollment.routers.ts`

### Cloudinary
Cloudinary is used for video and image hosting.
*   **Configuration:** Set environment variables for Cloudinary cloud name, API key, and API secret.
*   **Video Uploads:**
        *   `Nour_Backend/src/service/course/cleanup.service.ts`
        *   `Nour_Frontend/src/components/profile/Create Cours/components/SectionBuilder.tsx`
        *   `Nour_Backend/src/routers/cloudinary/cloud.routers.ts`
*   **Image Uploads:** Images, such as course thumbnails and profile pictures, are also stored on Cloudinary.
        *   `Nour_Backend/src/routers/cloudinary/cloud.routers.ts`

## API Documentation

API documentation can be found locally by doing:

*   **Generate TypeDocs:**
        ```bash
        cd Nour_Backend
        npm i
        npm run typedocs
        ```
    Then, navigate in your browser to `Nour_Backend/docs/index.html`

*   **Example API Endpoints:**
    *   `POST /api/signup`: Registers a new user.
    *   `POST /api/signin`: Logs in an existing user.
    *   `GET /api/courses`: Retrieves a list of courses.
    *   `POST /api/payment-intent`: Creates a Stripe payment intent.
    *   `POST /webhook`: Stripe webhook endpoint for handling payment events.

## Contributing Guidelines

1.  Fork the repository.
2.  Create a new branch for your feature or bug fix.
3.  Implement your changes, adhering to code style and best practices.
4.  Write clear and concise commit messages.
5.  Submit a pull request with a detailed description of your changes.

## License Information

This project does not currently have a specified license. Before using or distributing this project, please contact the repository owner for clarification on licensing terms.

## Contact/Support Information

*   **Repository Owner:** Mohammed-Zrirake
*   **Email:** Please add contact info