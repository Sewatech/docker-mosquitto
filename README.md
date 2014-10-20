# Content

* Debian Jessie
* Mosquitto server
* Mosquitto clients (pub & sub)

# Build the image

	docker build -t sewatech/mosquitto .

# Run the broker

	docker run --name mosquitto -p 1883:1883 -d sewatech/mosquitto

The `--name` argument is optional. It is useful for `docker exec` command, or if you want to stop and start the container.

# Publish messages

If you want to publish a message from the mosquitto container (works with docker 1.3+) :

    docker exec mosquitto mosquitto_pub  -t some/Topic -m "My message"

If you want to publish a message from a different container :

	docker run --rm -ti sewatech/mosquitto mosquitto_pub -h 172.17.42.1 -t some/Topic -m "My message"

Here, 172.17.42.1 is the IP address of the broker. If the broker is the docker host, you may find the address automatically :

	docker run --rm -ti sewatech/mosquitto mosquitto_pub -h `ifconfig docker0 | grep 'inet adr:' | cut -d: -f2 | awk '{ print $1}'` -t some/Topic -m "My message"

See http://mosquitto.org/man/mosquitto_pub-1.html for other arguments

# Read messages

If you want to subscribe to topics from the mosquitto container (works with docker 1.3+) :

    docker exec mosquitto mosquitto_sub -t some/Topic

If you want to subscribe to topics from a different container :

    docker run --rm -ti sewatech/mosquitto mosquitto_sub -h 172.17.42.1 -t some/Topic

Same as above for the IP address.

See http://mosquitto.org/man/mosquitto_sub-1.html for other arguments

