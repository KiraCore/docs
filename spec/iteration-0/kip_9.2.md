# [⏎](README.md#Roadmap) KIP_9
> Rate Limiting

We should rate limit client requests based on whether account is authenticated or not as per `KIP_9.1`. 

INTERX config should be modified so that rate limit can be defined globally for authenticated and unauthenticated users. This information should be further visible in the node_info responses from the  `rpc_methods` endpoint as per example in the `KIP_9`.

The current state of rate limits for unauthenticated clients should be based per individual IP addresses. We should enable defining "hit points" for each individual endpoint (default 1) in the config this way we can adjust individual rate limits but only caching a single counter whether for individual ip addresses or individual authenticated users. The `rpc_methods` endpoint should further provide information regarding `hit_points` for each endpoint.

Implementation should be based on the counter and the timemstamp. If timestamp is older than `X minutes` we can zero the counter to current UTC time. The counter should be incremented each time an endpoint is hit by the number equal to the `hit_points`.

Beside ability co configure `X` we also have to create a boolean field in the config responsible for enabling and disabling rate limiting. 