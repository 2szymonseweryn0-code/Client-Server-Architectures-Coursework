# Client-Server-Architectures-Coursework
1. JAX-RX Resource class lifecycle and thread safety.

In Jax-rs the lifecycle of a resource class is created for each HTTP request, but this can depend on it' implementation and configuration. In some cases the same example can be used accross multiple requests for efficiency. This matters because if a single instance is shared, then any shared data like static HashMap or ArrayList can be accessed by multiple requests at the same time. This can lead to problems such as race conditions or inconsistent data. For example, two requests coulf try to update the same map at once, causing unexpected behaviour. Because of this, in-memory data structures need to be managed carefully, and thread-safe alternatives or synchronisation may be required.

2. Importance of HATEOAS in APIs

HATEOAS means that API responses include links that tell the client what it can do next. This is useful because the client does not need to code every endpoint in advance. Instead of relying on external documentation, the API becomes self describing. This makes it easier for developers because they can discover available actions directly fromm responses. It also makes the system more flexible, since the server can change internal structure while still guiding clients using links.

3. Returning IDs vs Simple Objects

Returning only IDs instead of full objects has both advantages and disadvatages. Using IDs keeps responses small and reduces network usage, which can improve performance. However, the client then has to make extra requests to get full details, which can slow things down. On the other hand, returning full objects makes the client simpler because all the data is available immediately. The downside is that responses become larger and may include unnecessary data. So it’s really a trade off between efficiency and convenience.

4. Idempotency of DELETE

DELETE is considered idempotent because calling it multiple times has the same result. In this system, the first DELETE request removes the room from the data store. If the same request is sent again, the room is already gone, so nothing changes. The server might return a sucess response or a 404, but the final state is still the same. That’s what makes DELETE idempotent. Repeating it doesn’t change the outcome.

5. @Consumes and wrong data format

The @Consumes annotation means the endpoint only accepts JSON input. If a client sends data as text/plain or application/xml, JAX-RS will reject the request before it even reaches the method. Usually, the server responds with a 415 Unsupported Media Type errorr. This helps ensure that the API only processes properly formatted requests and avoids unnecessary processing of invalid input.

6. Query param and path params for filtering

Using query parameters like /sensors?type=CO2 is generally better for filtering than putting the filter in the path like /sensors/type/CO2. Query parameters are more flexible because you can combine multiple filters easily e.g. type, status, location. They also keep the URL structure clean, since paths are better suited for identifying specific resources rather than filtering collections. Overall, query parameters make APIs easier to extend and maintain.

7. Sub-Resource locator pattern

The sub-resource locator pattern helps break large APIs into smaller manageable pieces. Instead of putting everything in one large controller, you split related parts into separate classes. For example, sensors and readings can have their own resources instead of being handled in one place. This makes the code easier to read, maintain and extend. It also avoids having one massive controller class that becomes hard to manage as the project grows.

8. HTTP 422 and 404

HTTP 422 is more appropriate than 404 when the request is valid but contains incorrect data. A 404 means the endpoint or resource doesn’t exist. But in this case, the endpoint is fine. The problem is that the JSON references something that doesn’t exist in the system. So 422 is more accurate because it means I understand your request, but I can’t process it because the data doesn’t make sense.

9. Secutity risks of stack traces

Showing stack traces to users is risky because they reveal internal details about the system. For example, they can expose class names, file paths, database queries, and framework versions. Attackers can use this information to understand how the system works and potentially find weaknesses. Because of this, stack traces should be hidden in production, and users should only see generic error messages.

10. Why use jax-rs filters

Filters are useful because they let you handle things like logging in one central place instead of repeating code everywhere. If you manually add logging in every method, it becomes repetitive and easy to miss things. Filters solve this by automatically applying logic to all requests. This keeps the code cleaner and easier to maintain and more consistent across the application.
