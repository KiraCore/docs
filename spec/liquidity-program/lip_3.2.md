# [⏎](README.md#Roadmap) LIP_3.2
>  Token Balances Caching Service

It must be possible to cache token balance information in the function of time and block height. Data should be stored and statically hosted on S3 using AWS Lambda.

The auction frontend must have ability to easily query auction contract balance by querying s3 by account address and network id (e.g. mainnet, kovan etc.).

Frequency of updates should not be lower than 10 seconds and the service should be configurable by using github raw file where multiple team members have ability to access and modify it live for testing purposes.