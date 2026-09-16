# Lab 04: Basic Windows Network Troubleshooting

## Objective

Practice basic Windows network troubleshooting by inspecting IP configuration, testing the local TCP/IP stack, verifying DNS and HTTPS connectivity, comparing different failure states, and using route tracing.

## Environment

- Windows 11 Pro ARM
- UTM virtual machine
- `HelpDeskTest` — Standard user
- PowerShell
- Red Hat VirtIO Ethernet Adapter

## Scenario

The goal of this lab was to practice common Tier 1 network troubleshooting steps and learn how different network failures appear in Windows.

I tested:

- local IP configuration
- the loopback interface
- default gateway reachability
- DNS resolution
- outbound TCP connectivity
- an intentionally invalid domain
- an unreachable TCP destination
- route tracing

I also encountered unexpected behavior when disabling the virtual Ethernet adapter inside the UTM VM.

## Full IP Configuration

I ran:

```powershell
ipconfig /all
```

The active virtual Ethernet adapter showed:

- IPv4 address: `192.168.64.2`
- subnet mask: `255.255.255.0`
- default gateway: `192.168.64.1`
- DHCP enabled
- DNS server information
- Red Hat VirtIO Ethernet Adapter

### IP Configuration


<img width="851" height="798" alt="image" src="https://github.com/user-attachments/assets/0c4c1620-216f-40e0-a442-f5864311fc37" />

## Default Gateway Test

I tested the default gateway with:

```powershell
ping 192.168.64.1
```

The requests timed out with 100% packet loss.

Because later TCP connectivity tests succeeded, I did not treat this result by itself as proof that the network connection was down.

### Gateway Ping Result

<img width="633" height="324" alt="image" src="https://github.com/user-attachments/assets/35f7a479-1d84-4c55-9a5f-a82ce10d7aca" />

## Loopback Test

I tested the local TCP/IP stack with:

```powershell
ping 127.0.0.1
```

The system returned four successful replies with 0% packet loss.

This confirmed that the local TCP/IP stack was functioning correctly.

### Loopback Test Result

<img width="583" height="379" alt="image" src="https://github.com/user-attachments/assets/85f7ec3c-74ab-40af-892b-7aab49699e95" />

## Testing HTTPS Connectivity

I tested outbound HTTPS connectivity with:

```powershell
Test-NetConnection google.com -Port 443
```

The result returned:

```text
TcpTestSucceeded : True
```

This confirmed that the system was able to resolve the destination and establish a TCP connection over port 443.

### Healthy TCP Connection

<img width="683" height="307" alt="image" src="https://github.com/user-attachments/assets/ec75ea1b-5a38-4e0b-901f-88cd3a4a9cd6" />

## DNS Failure Test

To reproduce a DNS failure, I queried a domain that should not exist:

```powershell
nslookup this-domain-should-not-exist-987654321.com
```

The result returned:

```text
Non-existent domain
```

This demonstrated what a name-resolution failure looks like.

### DNS Failure

<img width="1111" height="582" alt="image" src="https://github.com/user-attachments/assets/4526a56a-27d8-4585-9a2d-1558c821d06f" />

## Failed TCP Connection

I then tested:

```powershell
Test-NetConnection 192.0.2.1 -Port 443
```

The connection failed and returned:

```text
TcpTestSucceeded : False
```

This provided a useful contrast with the DNS failure.

In this case, I was testing an IP address directly, so DNS resolution was not involved. The TCP connection itself could not be established.

### TCP Connection Failure

<img width="1111" height="582" alt="image" src="https://github.com/user-attachments/assets/9c3717b0-9b2e-4473-ba74-67b9f9b0602d" />

## Route Trace

I ran:

```powershell
tracert google.com
```

The command successfully resolved `google.com` to an IP address, but the individual hops returned:

```text
Request timed out.
```

Because the separate TCP 443 test to Google had already succeeded, these traceroute timeouts did not indicate a complete loss of internet connectivity.

This showed that one diagnostic test may fail or provide limited information even while another test confirms that the destination is reachable.

### Route Trace Result

<img width="646" height="811" alt="image" src="https://github.com/user-attachments/assets/1c0b14f7-82b0-4455-be5b-e7153ee89c07" />

## Unexpected Virtual Network Adapter Behavior

During the lab, I attempted to simulate a network outage by disabling the virtual Ethernet adapter from Windows Network Connections.

The adapter shown in the VM was the Red Hat VirtIO Ethernet Adapter.

### Virtual Ethernet Adapter

<img width="1083" height="413" alt="image" src="https://github.com/user-attachments/assets/6dd1bc42-2a11-4cd5-9efb-6f7f761186a4" />

After disabling the adapter, the Windows VM unexpectedly dropped back to a UEFI boot screen instead of simply losing network connectivity.

The same behavior occurred again when I repeated the test.

Rather than continuing to destabilize the VM, I stopped using this method and switched to safer DNS and TCP failure simulations.

### Unexpected VM Behavior

<img width="1601" height="1042" alt="image" src="https://github.com/user-attachments/assets/bcd09897-c275-474b-82ed-7296b1c78010" />

## Troubleshooting Comparison

The lab produced several different network states.

### Healthy Local TCP/IP

```powershell
ping 127.0.0.1
```

Result:

```text
4 received, 0 lost
```

The local TCP/IP stack was functioning.

### Healthy HTTPS Connectivity

```powershell
Test-NetConnection google.com -Port 443
```

Result:

```text
TcpTestSucceeded : True
```

The system could establish an outbound HTTPS connection.

### DNS Failure

```powershell
nslookup this-domain-should-not-exist-987654321.com
```

Result:

```text
Non-existent domain
```

The hostname could not be resolved.

### TCP / Destination Failure

```powershell
Test-NetConnection 192.0.2.1 -Port 443
```

Result:

```text
TcpTestSucceeded : False
```

The TCP connection could not be established.

## Final Result

The lab successfully demonstrated several basic Windows network troubleshooting techniques.

I was able to:

- inspect IP configuration with `ipconfig /all`
- identify the IPv4 address, subnet mask, default gateway, and DNS configuration
- verify the local TCP/IP stack with the loopback address
- test default gateway reachability
- verify outbound TCP port 443 connectivity
- reproduce a DNS resolution failure
- reproduce a failed TCP connection
- use `tracert` and compare its results with other connectivity tests
- recognize when a troubleshooting action itself was destabilizing the VM

## What I Learned

Different network tests answer different questions.

A successful loopback test confirms that the local TCP/IP stack is functioning.

A DNS failure points toward a name-resolution problem.

A failed TCP connection can indicate that the destination or service is not reachable even when DNS is not involved.

A successful TCP 443 test confirms that the system can establish an outbound HTTPS connection to that destination.

I also learned not to rely on a single test by itself. In this lab, the gateway ping and traceroute timed out even though outbound TCP connectivity to Google succeeded.

Finally, I learned that troubleshooting steps should be adjusted when the test itself repeatedly causes instability. Disabling the virtual Ethernet adapter caused unexpected VM boot behavior, so I switched to safer controlled failure simulations instead.

## Skills Demonstrated

- Windows Network Troubleshooting
- TCP/IP Fundamentals
- IPv4 Configuration
- Default Gateway Identification
- DNS Troubleshooting
- Loopback Testing
- TCP Port Testing
- `ipconfig`
- `ping`
- `nslookup`
- `Test-NetConnection`
- `tracert`
- Failure-State Comparison
- Troubleshooting Methodology
