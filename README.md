# LXC_random
Pushing random numbers between containers and displaying them on a web-server

1. Setting up the containers

The containers are set up, by using the command:

lxc-create -n C1 -t download -- -d alpine -r 3.4 -a armhf

2. Set up the container C2 to publish random numbers

The script used to generate random numbers, rng.sh, can be found in this repos.

The publishing part is handled by socat, using the following scipt:

socat -v -v tcp-listen:8080,fork,reuseaddr exec:/bin/rng.sh

3. Set up the C1 to host a webserver

C1 is set up to publish the PHP-script index.php.
  
4. Set up prerouting

Lastly, the Raspberry Pi is set up to reroute traffic to the webserver hosted by C1, using the following command:

iptables -t nat -A PREROUTING -i wlan0 -p tcp --dport 80 -j DNAT --to-destination <ct_ip>:80
