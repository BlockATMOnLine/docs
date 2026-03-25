# Request Limit

BlockATM aims to provide a stable, secure platform for all users at all times. As such, traffic is continuously monitored to identify patterns that may negatively impact the network. If traffic from a single account is identified as excessive and a rate limit is reached, BlockATM may temporarily throttle or restrict that account.

You have a rate limit on a per API endpoint and per minute basis. If your rate limit is reached, you will see an HTTP 429 error response letting you know that your rate limit has been temporarily exceeded.

Partners should gracefully handle the HTTP 429 status code and drop and/or retry requests accordingly.

The following headers are returned with each request

If a less restrictive rate limit is required, please contact your implementation manager to determine your use case. Please note that increasing your rate limit may incur additional charges.
