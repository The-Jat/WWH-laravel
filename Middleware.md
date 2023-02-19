## Middleware

> It provides the method to filter HTTP requests that get entered into your project. 

Middleware acts as a layer between the user and the request. It means that when the user requests the server then the request
will pass through the middleware, and then the middleware verifies whether the request is authenticated or not.
If the user's request is authenticated then the request is sent to the backend. If the user request is not authenticated,
then the middleware will redirect the user to the login screen.



