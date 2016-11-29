
# Docker image for Varnish HTTP Accelerator

Varnish is an HTTP accelerator. It is an open source reverse proxy server for HTTP protocol which stores cached pages in virtual memory.

## Usage

Main configurations are available as variables (you can see it with `options` command).
You can add your VCL file by `VARNISH_VCL_CONF` variable:

```sh
docker run --name varnish -v /path/to/default.vcl:/etc/varnish/default.vcl:ro -e VARNISH_VCL_CONF=/etc/varnish/custom.vcl saidsef/docker-varnish
```
