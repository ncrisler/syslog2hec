# syslog2hec

Send syslog to Splunk HTTP Event Collector via preconfigured syslog-ng container.

I just wrapped the config up into an image to learn how to build a Docker container, full credit goes to the following:

Ryan Faircloth - [Building a More Perfect Syslog Collection Infrastructure](https://www.rfaircloth.com/2017/02/10/building-perfect-syslog-collection-infrastructure/)

Mark Bonsack - [Syslog-ng and HEC: Scalable Aggregated Data Collection in Splunk](https://www.splunk.com/blog/2017/03/30/syslog-ng-and-hec-scalable-aggregated-data-collection-in-splunk.html)

Today this still needs quite a bit of polish.  The command below will run the conainer, using host networking, to listen on TCP/UDP 514 and send data to the raw HEC endpoint.  As discussed in Mark's blog, this is more desireable so that Splunk TAs can be used.  The provided config also includes a template for sending JSON formatted data, however the necessary TAs would need to be updated to work with this data formatting.

### How Do I Run This?
```
docker run \
-d \
--name syslog2hec \
--network=host \
-e HEC_TOKEN=12345678-1234-1234-123456789012 \
-e SPLUNK_SERVER=1.2.3.4 \
noisufnoc/syslog2hec:latest
```

### Parameters
- `HEC_TOKEN` The token generated by the Splunk server.  The configuration uses plaintext HTTP, but could be updated to use HTTPS if desired.
- `SPLUNK_SERVER` The destination to send the HEC events.  

### Notes
I had best luck with this using Docker host networking.  You can omit this setting and expose TCP/UDP 514, however I experienced some NATing in the syslog data which caused the sending IP to be that of my container on the Docker network. YMMV, if you have a better way of accomplishing this, feel free to file a PR/Issue.
