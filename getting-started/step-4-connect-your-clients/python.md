# Python

## Configuration for Superstream SDK

To leverage the full capabilities of the Superstream SDK, it is essential to set the environment variables provided in the table below before initializing the SDK. Without setting-up the environment variables, the SDK will function as a standard Kafka SDK.

| Environment Variable                | Default          | Required  | Description                                                                                           |
|-------------------------------------|------------------|-----------|-------------------------------------------------------------------------------------------------------|
| `SUPERSTREAM_HOST`                  | -                | Yes       | Specify the host URL of the Superstream service to connect to the appropriate Superstream environment. |
| `SUPERSTREAM_TOKEN`                 | -                | No        | This authentication token is required when the engine is configured to work with local authentication, to securely access the Superstream services. |
| `SUPERSTREAM_TAGS`                  | Empty string     | No        | Set this variable to tag the client. This value of this variable should be a valid JSON string.            |
| `SUPERSTREAM_DEBUG`                 | False            | No        | Set this variable to true to enable Superstream logs. By default, there will not be any Superstream related logs. |
| `SUPERSTREAM_RESPONSE_TIMEOUT`       | 3000            | No        | Set this variable to specify the timeout in milliseconds for the Superstream service response.         |
