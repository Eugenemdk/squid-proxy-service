## Squid Proxy Server Setup Guide

This guide provides step-by-step instructions on setting up a Squid proxy server within a Docker container and configuring your host machine to access the internet through the proxy server.

**Prerequisites**

Before you begin, make sure you have the following installed:

- Docker: [Install Docker](https://docs.docker.com/get-docker/)
- Command-line interface (Terminal or Command Prompt)

**Step 1: Pull Squid Proxy Docker Image**

Open your command-line interface and run the following command to pull the Squid proxy server Docker image:

```bash
docker pull sameersbn/squid
```

**Step 2: Run Squid Proxy Docker Container**

Run the Squid proxy Docker container with the following command:

```bash
docker run -d --name <container_name> -p <host_port>:3128 sameersbn/squid
```
**Step 3: Configure Host Machine**

**Determine Docker Container IP Address**

Find the IP address of the Squid proxy Docker container

```bash
docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' <container_name>
```

or

```bash
docker inspect <container_name>
```

**Windows Setup : Port Forwarding(Mapping)**

If you're on Windows, create a port forwarding rule to forward traffic from port e.g. 8082 to the Squid proxy container's port 3128

```bash
netsh interface portproxy add v4tov4 listenport=<host_machine_port> listenaddress=<ip_address_host_machine> connectport=<squid_port> connectaddress=<container_ip_address>

```

**Step 4: Configure Proxy Settings on Host Machine**

1.Open the Network and Internet settings on your host machine.
2.Go to Proxy settings and enable the "Manual proxy setup" option.
3.Set the proxy server to IP address of host machine and the port to host machine port that listens to (or 3128 if you didn't set up port forwarding).

```bash
docker run -d --name squid-proxy -p <host_port>:3128 sameersbn/squid
```

**Step 5: Test Connection**

**Using cURL :**

Open your command-line interface and test the connection using cURL:

```bash
curl http://192.168.0.151:3128 -I https://edition.cnn.com/
```

