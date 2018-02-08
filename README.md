# Quagga BGPD
*(BGP Daemon of the [Quagga] routing suite)*

## What's is in this image ?

 - Based on debian:jessie image
 - Expose port 179 (BGP peering) and 2605 (vtysh / telnet administration).

## Usage

#### Minimal configuration (full runtime config)

```sh
$ cat bgpd.conf
Current configuration:
!
hostname docker-bgpd
password amazing_password
log stdout
!
debug bgp updates
!
end
#
```

#### Deploy

We need to push capabilities as quagga will modprobe capabilities and try to manage some stuff in the network stack.

```sh
$ docker run --cap-add NET_ADMIN --cap-add NET_BROADCAST -d -ti -p 2605:2605 -p 179:179 pierrecdn/quagga-bgpd
```

You may want to :
* expose TCP/2605 port over an administrative network only as it allows remote control on the softrouter.
* import an existing configuration : add a volume by passing

```sh
-v /my_dir/bgpd.conf:/etc/quagga/bgpd.conf
```

#### Configuring at runtime

```sh
$ telnet <host> 2605
Trying <host>...
Connected to <host>.
Escape character is '^]'.

Hello, this is Quagga (version 0.99.23.1).
Copyright 1996-2005 Kunihiro Ishiguro, et al.

User Access Verification
Password:
docker-bgpd> en
docker-bgpd# sh run
(...)
```

[Quagga]:http://www.nongnu.org/quagga/

