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
Worked on my project **Box Brain**
1. Finalised a colour palette
    * This took waaaay longer than I expected, with me browsing different websites and resources to find the right palette
    * Learned about the required contrast ratio of background and text colours for accessibility. Found this useful website (https://monsido.com/tools/contrast-checker) for contrast checker
    * TO DO next:
      - Move the colour palette (_colors.scss) and other styles-related files to frontend server
      - Create an API Endpoint in frontend server that returns the colour palette data (to re-assess if needed)
      - Update all the relevant models to take in colour name instead of hex code (to utilise the set colours in the palette)
2. Created a route to quick set up Stack & Boxes based on specific templates

### R1D2
Worked on my project **Box Brain**
1. Learned how to update an existing field in database (updating Model, updating Validation, and updating the current data via script)
2. Learned a few things in Joi validation schema
    - how to include a set of acceptable value via `valid()`
    - how to set a default value via `valid()`
    - how to set different validation rules that depends on another field's value via `when("other_field_name", { is: valueToCheckFor, then: validationRulesIfTrue, otherwise: validationRulesIfFalse})`
3. Updated my resource creation route to include validations
