[Event Store](https://eventstore.org/) in a Windows Container (Windows Server Core base-image).

# Getting Started

Pull the docker image

`docker pull codeblack/eventstore:5.0.2`

## Single node mode

Run the container using

`docker run --name eventstore -d -p 2113:2113 -p 1113:1113 codeblack/eventstore:5.0.2`

> Note : The admin UI and atom feeds will only work if you publish the node's http port to a matching port on the host. (i.e. you need to run the container with -p 2113:2113)

To be able to keep the data between container instances, map the data & log volumes to the host; e.g.

`docker run --name eventstore -d -p 2113:2113 -p 1113:1113 -v C:\volumes\eventstore\data:C:\data -v C:\volumes\eventstore\logs:C:\logs codeblack/eventstore`

# Web UI

If you publish the node's http port to a matching port on the host (i.e. you run the container with -p 2113:2113 as in the example above), you can use the browser to view the Event Store admin UI; e.g.

`http://localhost:2113/`

Where 2113 is the host-port you specified when running the container.
Default username and password is `admin` and `changeit` respectively.

If you didn't publish the port to the host, get the docker IP address:

`docker inspect -f '{{.NetworkSettings.Networks.nat.IPAddress }}' eventstore`

Now use the browser to view the Event Store admin UI using the container IP address, the port is always 2113;  e.g.

`http://172.28.16.172:2113/`

# Using your own configuration

When running the docker image, you have the ability to configure Event Store by providing environment variables. e.g.

`docker run -d -p 2113:2113 -e EVENTSTORE_RUN_PROJECTIONS=None codeblack/eventstore:5.0.2`

The environment variables override the values supplied via the configuration file.

More documentation on Event Store's configuration can be found [here](https://eventstore.org/docs/server/command-line-arguments/).
