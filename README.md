# SOCAT vpn





# Server side setup 
- create dummy interface using 

    `sudo ip link add sec0 type dummy`    
- set the ip address and mask for the interface 

    `sudo ip addr add 20.0.0.1/24 dev sec0`
- bring up the interface 

    `sudo ip link set dev sec0 up`
- start socat server 

    `socat TCP-LISTEN:<YOUR_PORT>,reuseaddr,fork TUN:sec0,up`
- setup iptables rules

    `sudo iptables -t nat -A PREROUTING -p tcp --dport <YOUR_PORT> -j DNAT --to-destination <SOCAT_SERVER_IP>:<YOUR_PORT>`

# client side setup 
- create dummy interface using 
    
    `sudo ip link add sec0 type dummy`
- set the ip address and mask for the interface 

    `sudo ip addr add 20.0.0.2/24 dev sec0 `

- bring up the interface 

    `sudo ip link set dev sec0 up`

- start socat client

    `socat TUN:<INTERFACE>,up TCP:<SERVER>:<PORT>`
