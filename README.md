# Yourreal PDF

Welcome to Yourreal PDF, a powerful platform designed to extract specific pages from PDF files and create new PDFs with ease. With Yourreal PDF, you can effortlessly upload your PDF files and customize your new PDFs by specifying the pages you need. This project leverages a robust technology stack that includes Node.js, Express.js, MongoDB, Mongoose, and TypeScript, following clean architecture principles to ensure maintainability and scalability.

## Live Link of the application

[ Yourreal PDF](https://pdf-extractor-ecru.vercel.app)

## Table of Contents

1. [Installation](#installation)
2. [Documentation and Testing](#documentation-and-testing)
3. [Usage](#usage)
   - [Development](#usage-for-development)
   - [Production](#usage-for-production)
4. [How to Use Yourreal PDF](#How-to-Use-Yourreal-PDF)
4. [Key Features](#key-features)
5. [Error Handling](#error-handling)
6. [APIs](#apis)
7. [Detailed API Documentation](#detailed-api-documentation)
8. [Database Schema](#database-schema)
9. [Architecture and Technologies](#architecture-and-technologies)
10. [Tools and Stack](#tools-and-stack)
11. [Contact](#contact)


## Installation

1. Clone the repository:

   ```shell
   git clone https://github.com/sairaghunandanpulibandla/pdf-extractor1.git   
   ```

2. Navigate to the project directory:

   ```shell
   cd pdf-extractor1
   ```

3. Install the dependencies using your preferred package manager in both the server and client directories:

   ```shell
   cd server && yarn install  # or npm install
   cd ../client && yarn install  # or npm install
   ```

4. Set up the required environment variables by renaming the `.env.example` file to `.env` and providing the necessary values for your environment.

## Documentation and Testing

Explore the Swagger documentation at [http://localhost:4000/docs](http://localhost:4000/docs) after starting the server. You can also access a JSON version of the documentation at [http://localhost:4000/docs.json](http://localhost:4000/docs.json).

## Usage for Development

1. Start the development server:

   ```shell
   cd server && yarn run dev  # or npm run dev
   ```

2. Access the server at [http://localhost:4000](http://localhost:4000).

3. Explore the Swagger documentation at [http://localhost:4000/docs](http://localhost:4000/docs).

4. Register and log in as a user to extract PDFs. Authenticated users can manage their PDF files within their accounts.

(Repeat the same process to set up and access the React application located in the client directory.)

## Usage for Production

1. Build the code for production:

   ```shell
   cd server && yarn run build  # or npm run build
   ```

2. Start the production server:

   ```shell
   cd server && yarn run start  # or npm run start
   ```

3. Access the server at [http://localhost:4000](http://localhost:4000).

4. Explore the Swagger documentation at [http://localhost:4000/docs](http://localhost:4000/docs).

5. Register and log in as a user to extract PDFs. Authenticated users can manage their PDF files within their accounts.

(Repeat the same process to set up and access the React application located in the client directory.)

## How to Use Yourreal PDF

Here's a step-by-step guide on how to use Yourreal PDF to extract specific pages from PDF files:

1. Select a PDF File

   - Click the "Select a File" button to choose the PDF file you want to extract pages from.

   ![Select a File](https://github.com/sairaghunandanpulibandla/pdf-extractor1/blob/master/assets/home-page.png)

2. Choose Extraction Mode
   ![Extraction Page](https://github.com/sairaghunandanpulibandla/pdf-extractor1/blob/master/assets/pdf-view.png)
   - After selecting the PDF file, you can choose between two extraction modes: "Select by Page Range" or "Select by Random."
   - Select by Range
     
   ![Select by range](https://github.com/sairaghunandanpulibandla/pdf-extractor1/blob/master/assets/select-range.png)
   
   - Select by Random
     
   ![Select random](https://github.com/sairaghunandanpulibandla/pdf-extractor1/blob/master/assets/select-random.png)


3. Select Pages to Extract

   - If you choose "Select by Page Range," enter the page range you want to extract (e.g., 1-5).

   - If you choose "Select by Random," tick the check boxes to specify page number.

4. Extract and Download

   - Click the "Extract" button to process the extraction.

   - Once the extraction is complete, you'll be able to download the newly created PDF with the selected pages.

   ![Download PDF](https://github.com/sairaghunandanpulibandla/pdf-extractor1/blob/master/assets/download.png)

## Key Features

- User registration and login.
- Select specific pages from PDF files to create new customized PDFs.
- Two modes available: select by page range and select by random.
- Download option for newly created PDFs.

## Error Handling

### AppError Class

A custom `AppError` class is used for error handling:

```javascript
class AppError extends Error {
    statusCode: number;
    status: string;
    isOperational: boolean;
    keyValue: any;
    constructor(message: string, statusCode: HttpStatusCodes) {
      super(message);
      this.statusCode = statusCode;
      this.status = `${statusCode}`.startsWith('4') ? 'failed' : 'error';
      this.isOperational = true;
      Error.captureStackTrace(this, this.constructor);
    }
}
```

### Error Handling Middleware

The error handling middleware catches errors and sends appropriate error messages with their status code, reducing the need for extensive try-catch blocks:

```javascript
const errorHandlingMiddleware = (err: AppError, req: Request, res: Response, next: NextFunction) => {
  err.statusCode = err.statusCode || 500;
  err.status = err.status || 'error';
  if (err.statusCode === 404) {
    res.status(err.statusCode).json({ errors: err.status, errorMessage: err.message });
  } else {
    res.status(err.statusCode).json({
      status: err.status,
      message: err.message,
    });
  }
};
```

## APIs

### Auth
APIs for user authentication:

- **POST /api/v1/auth/sign-up**
  - Description: Register a user.

- **POST /api/v1/auth/sign-in**
  - Description: Login a user.

### PDF
APIs related to PDF operations:

- **POST /api/v1/pdf/upload**
  - Description: Upload a PDF file to the server.

- **GET /api/v1/pdf/:pdfId**
  - Description: Fetch an uploaded PDF file.

- **POST /api/v1/extract-pages/:pdfId**
  - Description: Extract new PDFs from the original PDF by selecting specific pages.

## Detailed API Documentation 

For detailed documentation on available API endpoints and their usage, please refer to the [API Documentation](https://documenter.getpostman.com/view/23308654/2s9YXcd56u).

## Database Schema

### User Schema

```javascript
const userSchema = new Schema({
    name:{
        type:String,
        required:true
    },
    email:{
        type:String,
        required:true
    },
    password:{
        type:String,
        required:true
    },
    createdAt:{
        type:Date,
        required:true
    }
})
```
Explanation:

- `name`: Represents the user's name.
- `email`: Represents the user's email address.
- `password`: Contains the user's password, with a minimum length requirement of 8 characters.
- `createdAt`: Timestamp of the user's creation.


### Saved Pdf schema 

```javascript
   const pdfSchema = new Schema({
    userId:{
        type:Types.ObjectId,
        required:true,
        ref:"user"
    },
    saved:{
        type:Array<String>,
        default:[],
    },
    createdAt:{
        type:Date,
        required:true
    },
    updatedAt:{
        type:Date,
        required:true
    }
})
```

Explanation:

- `userId`: Unique identifier of the user
- `createdAt`: Timestamp of the user's creation.
- `saved`: An array containing id's of the newly extracted pdf's
- `updatedAt`: Last updated time

## Architecture and Technologies

The project follows a clean architecture and utilizes the following technologies:

- Node.js
- Express.js
- React.js
- Redux.js
- MongoDB with Mongoose
- TypeScript
- Redis for caching

## Tools and Stack

### Framework (Express.js & Node.js)

Express.js is a fast, minimalist, and flexible Node.js web application framework that simplifies the development of server-side applications, making it ideal for building RESTful APIs, web applications, and microservices.

### Database (MongoDB)

MongoDB is a popular NoSQL database known for its flexibility and scalability, making it suitable for a wide range of data types and structures.

### Database Framework (Mongoose)

Mongoose is an Object Data Modeling (ODM) library for MongoDB and Node.js, simplifying interactions with MongoDB databases.

### State management (Redux)

Redux is used for efficient state management in the React application.

### User Interface (Tailwind and React.js)

The user interface is developed using Tailwind CSS and React.js, providing a clean and responsive design.

### Documentation (Swagger)

Swagger is employed for designing, building, and documenting RESTful APIs, simplifying API development and integration.

### Authentication (JWT)

JSON Web Token (JWT) is used for secure authentication and authorization.

### Other Third-Party Libraries

- express-rate-limit: A middleware for rate limiting requests to an Express.js application, which helps protect your server from abuse or malicious attacks by limiting the number of requests a client can make in a specified time frame.

- uuid: A library for generating universally unique identifiers (UUIDs). These are unique 128-bit values often used to identify resources or entities in distributed systems or databases.

- winston: A popular logging library for Node.js that allows you to log information and errors in various formats and transports, making it easier to monitor and troubleshoot your application.

- cors: A middleware for enabling Cross-Origin Resource Sharing (CORS) in your Express.js application. It allows you to control which domains or origins can access your server resources, which is essential for web security and integrating with other domains.

- bcrypt: A library for securely hashing and salting passwords in your application. It's commonly used to store and verify user passwords in a way that protects them from common security threats like brute force attacks.

- multer: Middleware for handling file uploads in Express.js. It simplifies the process of receiving and processing files uploaded by clients, such as images or documents.

- pdf-lib: A library for creating and manipulating PDF documents in JavaScript. It provides tools for generating, modifying, and extracting data from PDF files, making it useful for tasks like PDF generation and manipulation in web applications.

- axios: Axios is a popular JavaScript library for making HTTP requests from both the browser and Node.js. It provides a simple and convenient API for sending and receiving data from web servers, making it a common choice for handling network requests in web applications.

- react-pdf-viewer: A React component library for rendering and interacting with PDF documents in a web application. It allows you to display PDFs and provides various features for navigation and customization, making it useful for applications that need to display PDF content.

- react-icons: React Icons is a library that provides a collection of popular icon sets as React components. It simplifies the process of adding icons to your React application, offering a wide range of icon options for various use cases.

- react-toastify: A library for displaying toast notifications in React applications. Toast notifications are non-intrusive pop-up messages often used to provide feedback or alerts to users in a user-friendly way. React Toastify makes it easy to create and customize these notifications in your application.

## Contact

For any questions, feedback, or inquiries, please reach out to:

- Sairaghunandan Pulibandla
  - Email: sairaghunandan_p@srmap.edu.in
  - LinkedIn: [LinkedIn Profile](https://www.linkedin.com/in/sai-raghunandan-pulibandla-2668332a9/)


