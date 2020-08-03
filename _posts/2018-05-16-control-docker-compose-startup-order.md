---
layout: post
title: "Control Startup Order In Docker Compose"
date: 2018-06-06 06:00:26 +0200
category: [Docker, Command-Line]
featured-img: dockerlogo
---
A nice feature of Compose is the possibility to define dependencies between Docker containers. Compose starts containers in the defined order and evaluates the dependencies by the *depends_on*, *links*, *volumes_from*, and *network_mode: "service:..."* options. But, Compose only waits until a container is running, not if it's *ready* because it does not know if and when a certain service in a container is in a *ready* state. 

Normaly it is a good idea to implement such a *readiness* check in the service itself. But sometimes, this is not possible. A different solution is to use a small wrapper script in the Docker container itself, to synchronize the start-up of the services's interdependencies.

Here is an example with a [Spring Boot][3] application which has a dependency to a [Postgres][4] database and uses [Liquibase][2] to manage and apply database schema changes.

The wrapper script takes a hostname and a port and checks periodically the availability via netcat. If the service becames available, the script will return success and the Spring Boot service will start. The wrapper script also has a timeout of 90 seconds. If the given hostname und port is not available until the timeout hits the wrapper script exits with an error. In this case the Spring Boot service will not be started and the Docker container stops.
<figure><figcaption>File: wait-for.sh</figcaption>
{% highlight bash %}
#!/usr/bin/env sh

if [ "$#" -ne 1 ]; then
    >&2 echo "Usage: $0 host:port"
    exit -1
fi

ARGUMENT=$1
HOST="$(echo $ARGUMENT | cut -d ':' -f1)"
PORT="$(echo $ARGUMENT | cut -d ':' -f2)"
MAX_RETRY=90

echo "Testing connection to host $HOST and port $PORT."

count=0
while [ $count -lt $MAX_RETRY ]
do
    count=$((count+1))
    nc -z $HOST $PORT
    result=$?
    if [ $result -eq 0 ]; then
        echo "Connection is available after $count second(s)."
        exit 0
    fi
    echo "Retrying..."
    sleep 1
done

>&2 echo "Timeout occurred after waiting $MAX_RETRY seconds for $HOST:$PORT."
exit -1
{% endhighlight %}
</figure>
I created a small Dockerfile to build the Docker image containing the Spring Boot service. The file contains instructions to copy the service's .jar file and the wrapper script to the image and to install netcat, because netcat is not available in the debian-slim openjdk-10 base image:
<figure><figcaption>File: Dockerfile</figcaption>
{% highlight docker %}
FROM openjdk:10-slim

RUN apt-get update && \
 apt-get install -y netcat;

COPY wait-for.sh /opt/wait-for.sh

COPY build/libs/*.jar /opt/app.jar
{% endhighlight %}
</figure>
As mentionend above, a Compose file defines the containers and their dependencies. I have created an example Compose file. The file defines a web container, which is the Spring Boot service, and a database container, which is the Postgres database. The web container defines a dependency to the database container via *depends_on*. The entrypoint of the *web* container is set to *bin/sh* with additional parameters via the *entrypoint* property. What happens here is that first, the wrapper script is called, which checks for the database to be *ready* and if successful starts the Spring Boot service. One can, of course, add multiple hostnames and ports to wait for.
<figure><figcaption>File: docker-compose.yml</figcaption>
{% highlight docker %}
version: '2'

services:
  database:
    image: postgres:10.4
  web:
    depends_on:
      - "database"
    build: .
    command: ["-c", "/opt/wait-for.sh database:5432 && java -jar /opt/app.jar"]
    entrypoint: ["/bin/sh"]
    environment:
      - SPRING_DATASOURCE_URL=jdbc:postgresql://database:5432/postgres
      - SPRING_DATASOURCE_USERNAME=postgres
      - SPRING_DATASOURCE_PASSWORD=
{% endhighlight %}
</figure>
When executing *docker-compose up* the wrapper script checks periodically for the database to become ready (Retrying...) and then starts the Spring Boot Application when the connection becomes available after 4 seconds.

{% highlight raw %}
...
web_1       | Testing connection to host database and port 5432.
web_1       | Retrying...
web_1       | Retrying...
web_1       | Retrying...
web_1       | Retrying...
web_1       | Connection is available after 4 second(s).
web_1       | 
web_1       |   .   ____          _            __ _ _
web_1       |  /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
web_1       | ( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
web_1       |  \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
web_1       |   '  |____| .__|_| |_|_| |_\__, | / / / /
web_1       |  =========|_|==============|___/=/_/_/_/
web_1       |  :: Spring Boot ::        (v2.0.2.RELEASE)
web_1       | 
...
{% endhighlight %}

Find the full, working project including the Spring Boot service, Dockerfile, docker-compose.yml and wait-for.sh on [Github][1].

[1]: https://github.com/sglahn/spring-boot-hello-world
[2]: http://www.liquibase.org
[3]: https://projects.spring.io/spring-boot
[4]: https://www.postgresql.org/
