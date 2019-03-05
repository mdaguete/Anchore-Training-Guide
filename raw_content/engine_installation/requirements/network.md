## Network

The Anchore Engine requires two categories of network access:

- Registry Access
    Network connectivity, including DNS resolution, to the registries from which the Anchore Engine needs to download images.
- Feed Service
    The Anchore Engine synchronizes feed data such as operating system vulnerabilities (CVEs) from the Anchore Cloud Service. The initial synchronization may take 5 to 10 minutes, based on network speed, after which time the Anchore Engine will download updated feed data at a user configurable interval, by default every 4 hours. Only a single end point is required for this synchronization, host: ancho.re TCP port: 443

Notes:

- The Anchore Engine can optionally be configured to download policies created on the Anchore Cloud service. If configured this will use the same host/port as the feed service.
- No data is uploaded to Anchore, the synchronization is one-way.
- An optional on-premises feed service and policy editor is available to commercial users.
- A Network Proxy can be used to provide external connectivity to the Anchore Engine. See: Network Proxy documentation.
