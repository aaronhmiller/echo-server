# Echo Server

A very simple HTTP echo server with support for websockets and server-sent
events (SSE).

The server is designed for testing HTTP proxies and clients. It echoes
information about HTTP request headers and bodies back to the client.

## Behavior

- Any messages sent from a websocket client are echoed as a websocket message.
- Visit `/.ws` in a browser for a basic UI to connect and send websocket messages.
- Request `/.sse` to receive the echo response via server-sent events.
- Request any other URL to receive the echo response in plain text.

## Configuration

- The `PORT` environment variable sets the server port, which defaults to `8080`.
- Set the `LOG_HTTP_BODY` environment variable to dump request bodies to `STDOUT`.
- Set the `LOG_HTTP_HEADERS` environment variable to dump request headers to `STDOUT`.
- Set the `SEND_SERVER_HOSTNAME` environment variable to `false` to prevent the
  server from responding with its hostname before echoing the request. The
  client may send the `X-Send-Server-Hostname` request header to `true` or
  `false` to override this server-wide setting on a per-request basis.

## Running the server

The examples below show a few different ways of running the server with the HTTP
server bound to a custom TCP port of `10000`.

### Running locally

```
go get -u github.com/jmalloc/echo-server/...
PORT=10000 echo-server
```

### Running under Docker

To run the latest version as a container:

```
docker run --detach -p 10000:8080 jmalloc/echo-server
```

Or, as a swarm service:

```
docker service create --publish 10000:8080 jmalloc/echo-server
```

The docker container can be built locally with:

```
make docker
```

### Running in Heroku
Login to Heroku

`heroku login`

You’ll be prompted to hit any key and a browser will open so you can login to Heroku.

Create a Heroku app with the command

`heroku create`

Now deploy the echo-server 

`git push heroku main`

Once the deployment is done - open the app with

`heroku open`

Modify the URL in the address bar of your browser by adding /.ws at the end of your URL.

Alternately, in Postman you can use its websocket connection with the Heroku server you just spun up (do NOT specify the `http://` from within Postman because it's a direct websocket connection)
