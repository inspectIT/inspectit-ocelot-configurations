# Transaction recording

Example configurations which show how to record whole transactions 
from frontend to backend with inspectIT Ocelot, the 
[eum-server](https://github.com/inspectIT/inspectit-ocelot-eum-server)
and the [boomerang-opentelemetry-plugin](https://github.com/inspectIT/boomerang-opentelemetry-plugin).

First, the plugin feature [transaction-recording](https://github.com/inspectIT/boomerang-opentelemetry-plugin?tab=readme-ov-file#transaction-recording) 
has to be enabled.

The frontend will send a request to the backend to load the webpage. However, the OpenTelemetry spans 
will be started after the response of the backend. Thus, they would use another trace-ID than the backend
and the transaction would be split into separate traces.

To create one unified trace, inspectIT Ocelot will create a new remote parent context, which will
be used as parent for the root span of the backend.
The remote parent context will be included in the response Server-Timing headers to the frontend,
so the plugin can read those headers and use the context information to start its spans.
