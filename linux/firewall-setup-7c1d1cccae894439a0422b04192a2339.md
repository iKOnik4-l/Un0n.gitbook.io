# Firewall Setup 7c1d1cccae894439a0422b04192a2339

**The iptables utility provides a simple yet powerful command-line interface for configuring firewall rules, which could be used to filter traffic based on various criteria such as IP addresses, ports, protocols, and more.** iptables was designed to be highly customizable and could be used to create complex firewall rulesets that could protect against various security threats such as denial-of-service (DoS) attacks, port scans, and network intrusion attempts.

In Linux, the firewall functionality is typically implemented using the Netfilter framework, which is an integral part of the kernel. Netfilter provides a set of hooks that can be used to intercept and modify network traffic as it passes through the system. The iptables utility is commonly used to configure the firewall rules on Linux systems.

## **Iptables**

There also exist **other solutions** like nftables, ufw, and firewalld. **`Nftables`** provides a more modern syntax and improved performance over iptables. However, the syntax of nftables rules is not compatible with iptables, so migration to nftables requires some effort. **`UFW`** stands for “Uncomplicated Firewall” and provides a simple and user-friendly interface for configuring firewall rules. UFW is built on top of the iptables framework like nftables and provides an easier way to manage firewall rules. Finally, FirewallD provides a dynamic and flexible firewall solution that can be used to manage complex firewall configurations, and it supports a rich set of rules for filtering network traffic and can be used to create custom firewall zones and services. It consists of several components that work together to provide a flexible and powerful firewall solution. The main components of iptables are:

| **Component** | **Description**                                                                                                                                                              |
| ------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `Tables`      | Tables are used to organize and categorize firewall rules.                                                                                                                   |
| `Chains`      | Chains are used to group a set of firewall rules applied to a specific type of network traffic.                                                                              |
| `Rules`       | Rules define the criteria for filtering network traffic and the actions to take for packets that match the criteria.                                                         |
| `Matches`     | Matches are used to match specific criteria for filtering network traffic, such as source or destination IP addresses, ports, protocols, and more.                           |
| `Targets`     | Targets specify the action for packets that match a specific rule. For example, targets can be used to accept, drop, or reject packets or modify the packets in another way. |

## **Tables**

Tables in iptables are used to categorize and organize firewall rules based on the type of traffic that they are designed to handle. These tables are used to organize and categorize firewall rules. Each table is responsible for performing a specific set of tasks.

| **Name** | **Description**                                                             | **Built-in Chains**                             |
| -------- | --------------------------------------------------------------------------- | ----------------------------------------------- |
| `filter` | Used to filter network traffic based on IP addresses, ports, and protocols. | INPUT, OUTPUT, FORWARD                          |
| `nat`    | Used to modify the source or destination IP addresses of network packets.   | PREROUTING, POSTROUTING                         |
| `mangle` | Used to modify the header fields of network packets.                        | PREROUTING, OUTPUT, INPUT, FORWARD, POSTROUTING |

Network traffic is made up of packets. Data is broken up into smaller pieces (called packets), sent over a network, then put back together. Iptables identifies the packets received and then uses a set of rules to decide what to do with them.

Iptables filters packets based on:

* **Tables:** Tables are files that join similar actions. A table consists of several **chains**.
* **Chains:** A chain is a string of **rules**. When a packet is received, iptables finds the appropriate table, then runs it through the chain of **rules** until it finds a match.
* **Rules:** A rule is a statement that tells the system what to do with a packet. Rules can block one type of packet, or forward another type of packet. The outcome, where a packet is sent, is called a **target**.
* **Targets:** A target is a decision of what to do with a packet. Typically, this is to accept it, drop it, or reject it (which sends an error back to the sender).

## **Chains**

There are two types of chains in iptables:

* Built-in chains
* User-defined chains

The built-in chains are pre-defined and automatically created when a table is created. Each table has a different set of built-in chains. For example, the filter table has three built-in chains:

* INPUT
* OUTPUT
* FORWARD

The nat table has two built-in chains:

* PREROUTING
* POSTROUTING

The mangle table has five built-in chains:

* PREROUTING
* OUTPUT
* INPUT
* FORWARD
* POSTROUTING

## **Rules and Targets**

Rules are added to chains using the `-A` option followed by the chain name.

The target specifies the action for packets that match the criteria. They specify the action to take for packets that match a specific rule. For example, targets can accept, drop, reject, or modify the packets. Some of the common targets used in iptables rules include the following:

| **Target Name** | **Description**                                                                                                                                             |
| --------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `ACCEPT`        | Allows the packet to pass through the firewall and continue to its destination                                                                              |
| `DROP`          | Drops the packet, effectively blocking it from passing through the firewall                                                                                 |
| `REJECT`        | Drops the packet and sends an error message back to the source address, notifying them that the packet was blocked                                          |
| `LOG`           | Logs the packet information to the system log                                                                                                               |
| `SNAT`          | Modifies the source IP address of the packet, typically used for Network Address Translation (NAT) to translate private IP addresses to public IP addresses |
| `DNAT`          | Modifies the destination IP address of the packet, typically used for NAT to forward traffic from one IP address to another                                 |
| `MASQUERADE`    | Similar to SNAT but used when the source IP address is not fixed, such as in a dynamic IP address scenario                                                  |
| `REDIRECT`      | Redirects packets to another port or IP address                                                                                                             |
| `MARK`          | Adds or modifies the Netfilter mark value of the packet, which can be used for advanced routing or other purposes                                           |

Let us illustrate a rule and consider that we want to add a new entry to the INPUT chain that allows incoming TCP traffic on port 22 (SSH) to be accepted. The command for that would look like the following:

```bash
JuiceWRLD100@htb[/htb]**$** sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT
```

## **Matches**

`Matches` are used to specify the criteria that determine whether a firewall rule should be applied to a particular packet or connection. Matches are used to match specific characteristics of network traffic, such as the source or destination IP address, protocol, port number, and more.

| **Match Name**          | **Description**                                                    |
| ----------------------- | ------------------------------------------------------------------ |
| `-p` or `--protocol`    | Specifies the protocol to match (e.g. tcp, udp, icmp)              |
| `--dport`               | Specifies the destination port to match                            |
| `--sport`               | Specifies the source port to match                                 |
| `-s` or `--source`      | Specifies the source IP address to match                           |
| `-d` or `--destination` | Specifies the destination IP address to match                      |
| `-m state`              | Matches the state of a connection (e.g. NEW, ESTABLISHED, RELATED) |
| `-m multiport`          | Matches multiple ports or port ranges                              |
| `-m tcp`                | Matches TCP packets and includes additional TCP-specific options   |
| `-m udp`                | Matches UDP packets and includes additional UDP-specific options   |
| `-m string`             | Matches packets that contain a specific string                     |
| `-m limit`              | Matches packets at a specified rate limit                          |
| `-m conntrack`          | Matches packets based on their connection tracking information     |
| `-m mark`               | Matches packets based on their Netfilter mark value                |
| `-m mac`                | Matches packets based on their MAC address                         |
| `-m iprange`            | Matches packets based on a range of IP addresses                   |

In general, matches are specified using the `-m` option in iptables. For example, the following command adds a rule to the `INPUT` chain in the `filter` table that matches incoming TCP traffic on port 80:

```bash
JuiceWRLD100@htb[/htb]**$** sudo iptables -A INPUT -p tcp -m tcp --dport 80 -j ACCEPT
```

This example rule matches incoming TCP traffic (`-p tcp`) on port 80 (`--dport 80`) and jumps to the accept target (`-j ACCEPT`) if the match is successful.
