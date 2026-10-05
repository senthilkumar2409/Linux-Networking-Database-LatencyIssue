# http status code:

## 2XX:

**200 OK**: The request is successfull, and the response usually contains the requested data (e.g., a GET returning a page or JSON).

**201 Created**: A new resource was created(**Ex: Photo uploaded/created in instagram)**, typically after a POST or PUT. This status code is commonly sent as the result of a POST request.

**204 No Content**: The request succeeded but there's nothing to send back. It's common for DELETE or PUT(Update).

## 3XX:

**300 Multiple Choices:** The request has more than one possible response, and the user or user agent should choose one.

**301 Moved Permanently:** The requested resource has been permanently moved to a new URL.

**302 Found:** The requested resource is temporarily located at a different URL, as specified in the Location header.

**304 Not Modified:** The resource has not been modified since the last request, and the client can use the cached version.

**305 Use Proxy:** The requested resource must be accessed through the specified proxy.

**307 Temporary Redirect:** The requested resource is temporarily located at a different URL, and the client should use the original method for the next request.

**308 Permanent Redirect:** The requested resource has been permanently moved, and future requests should use the new URL provided (similar to 301 but for methods that are not allowed to change).
