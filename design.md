# Allocator initial design

### Preparation:
    At least one ping pong server handle HTTP request. 
    The request/response are both in JSON.
    Netty is what I think for now, it is an async framework and we can learn a lot from this. BTW, Java NIO is something we should warm up with.

### Phase 0: 
    We should have a single server that handle request in Json. The request could be something like
    ```json
    	{
    		"budget_setting_group": id,
    		"budget_requested": 100000 (which is $1 in millicents) 
    	}
    ```

    And the response is like
    ```json
    	{
    		"budget_response": 1000000 (which should be the same as request, or 0 if out of budget)
    	}
    ```

    * The data structure behind can be something in memory, for single server we can test its concurrent feature. ConcurrentHashMap is a good start for now, we need to design around it.

    * In case of we have to restart it everytime we want to change the in memory datastructure, we can set up another endpoint to update the budget for each setting_group.

    * Not sure how to perform a performance test to verify the performance and integrity of budget for all groups (overspend or underspend?) My point is we need multiple clients to send query and log the request and response locally, and then
    accumulate all logs together to test the budget integrity, but for the performance
    part we may need to log on both server side and client side for P99 response time and throughput. (Maybe a tool like Graphite or Datadog?)
    
    * Dockerize the server or not? Deploy it locally or on cloud?
