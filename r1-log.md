# #100DaysOfCode Log - Round 1 - Zoe Nguyen

The log of my ~~reattempt at~~ #100DaysOfCode challenge. Started on 12 Jun 2024.

## Log

### R1D0

(This was before I officially restarted the challenge, but I still want to note it down just for my own learning record)
Learned about and implemented **centralised error handling** middleware for my MERN project (Box Brain)
- Benefits:
  1. Consistency:
      * Uniform Error Responses: consistent formats, messages, etc
      * Standadised Logging: centralized logging of errors (not yet implemented for my project)
  2. Maintainability
      * Single point of change
      * Separation of Concerns
  3. Simplified Development
      * Reduced Redundancy: can `throw error` or `next(error)` instead of repeating the code to return error to client
      * Easier Testing
  4. Scalability
      * Easier Adaptation: adapt more easily to new types of error
      * Consistent User Experience
  5. Enhanced Debugging (not yet implemented for my project)
      * Centralized Logging
      * Comprehensive Error Data
- Implementation
```
\\ in errorHandler.js middleware
const defaultErrorMessages = {
  400: "Bad Request / Invalid Inputs",
  401: "Unathorized / No valid authentication credentials",
  403: "Forbidden / Users do not have the required access",
  404: "Not Found",
  500: "Internal Server Error",
};

const errorHandler = (error, req, res, next) => {
  // set default error response
  const statusCode = error.statusCode || 500;
  // update error.message to follow the default message
  const errMessage = defaultErrorMessages[statusCode];
  // if there exists an error.message, to move the message to the details property
  const errDetails = error.details || error.message;

  const updatedError = {
    ... error, 
    message: errMessage, 
    details: errDetails,
  };

  // send error response
  res.status(statusCode).json(updatedError);
};

module.exports = errorHandler;
```

```
// in notFoundHandler.js
const notFoundHandler = (req, res, next) => {
  const error = new Error("Resource not found");
  error.statusCode = 404;
  next(error);
};

module.exports = notFoundHandler;
```

```
// in server.js
// after all the packages and routes
const notFoundHandler = require("./middlewares/notFoundHandler");
const errorHandler = require("./middlewares/errorHandler");

// Handle 404 errors for routes that cannot be found
app.use(notFoundHandler);

// Centralized error handling middleware
app.use(errorHandler);
```

### R1D1 


