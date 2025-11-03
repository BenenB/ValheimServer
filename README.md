# Valheim Server - How to Connect

This repository contains Kubernetes manifests for deploying a Valheim server. For server setup instructions, see [SETUP.md](SETUP.md).

## Quick Start - Connecting to Your Server

### Get Connection Information

Before connecting, you'll need:

1. **For Local Network Connections**: The Raspberry Pi's local IP address
2. **For Internet Connections**: Your public IP address (and router port forwarding configured)

#### Finding Your Server's IP Address

**Local Network IP:**
- Ask your server administrator for the Raspberry Pi's local IP address
- Typically in the format: `192.168.1.x` or `192.168.0.x`

**Public IP (for external access):**
- Visit [whatismyip.com](https://whatismyip.com) from any device on the server's network
- Or ask your server administrator

**Port Information:**
- Default game port: **2456** (UDP)
- If the server uses NodePort, the port may be different (e.g., 30001)

### Connecting to the Server

#### For Local Network Connections

If you're on the same network as the Raspberry Pi:

1. **Open Valheim** on your computer
2. **Click "Join Game"**
3. **Click "Join IP"**
4. **Enter the server address**: `LOCAL_IP:2456`
   - Example: `192.168.1.100:2456`
   - If using NodePort, use the NodePort number instead: `192.168.1.100:30001`
5. **Enter the server password** (if one is configured)
6. **Click "Connect"**

#### For External Internet Connections

If you're connecting from outside the server's network:

1. **Open Valheim** on your computer
2. **Click "Join Game"**
3. **Click "Join IP"**
4. **Enter the server address**: `PUBLIC_IP:2456`
   - Example: `123.45.67.89:2456`
5. **Enter the server password** (if one is configured)
6. **Click "Connect"**

**Note**: External connections require the server administrator to configure router port forwarding. If you cannot connect, see troubleshooting below.

## Troubleshooting Connection Issues

### Server Not Appearing in Server Browser

If you're trying to find the server in the server browser but it doesn't show up:

1. **Verify you have the correct IP address**
   - For local network: Use the Raspberry Pi's local IP
   - For internet: Use the public IP address

2. **Check if the server is running**
   - Ask your server administrator to verify the server is online
   - Server should show status as "Running" in Argo CD or Kubernetes

3. **Try direct IP connection instead**
   - Use "Join IP" instead of browsing the server list
   - The server browser may not always discover servers, especially on local networks

4. **Verify port numbers are correct**
   - Default port: `2456`
   - If using NodePort, check with server administrator for the correct port

5. **Check your firewall**
   - Ensure your local firewall isn't blocking UDP port 2456
   - On Windows: Check Windows Defender Firewall
   - On Mac: Check System Preferences > Security & Privacy > Firewall
   - On Linux: Check `ufw` or `iptables` rules

6. **ISP/Network restrictions**
   - Some ISPs or networks block game ports
   - Try connecting from a different network to test
   - Check if you're behind a VPN that might interfere

### Cannot Connect to Server

If you're getting connection errors when trying to join:

1. **Verify IP and Port are Correct**
   - Double-check the IP address and port number
   - Ensure you're using UDP (Valheim uses UDP, not TCP)
   - Format should be: `IP:PORT` (e.g., `192.168.1.100:2456`)

2. **Test Network Connectivity**
   - For local network: Ping the Raspberry Pi IP: `ping RASPBERRY_PI_IP`
   - Test UDP port (on Linux/Mac): `nc -u -v SERVER_IP 2456`

3. **Check Server Status**
   - Ask server administrator to verify:
     - Pod is running: `kubectl get pods -l app=valheim-server`
     - Service is configured: `kubectl get service valheim-server`
     - Server logs show no errors: `kubectl logs -l app=valheim-server`

4. **Router Port Forwarding** (for external connections)
   - Confirm port forwarding is configured on the router
   - External port 2456 should forward to the Raspberry Pi
   - Verify the Raspberry Pi's local IP hasn't changed

5. **Public IP Changes**
   - If using a dynamic IP, the public IP may have changed
   - Ask server administrator for current public IP
   - Consider using a Dynamic DNS (DDNS) service

6. **CGNAT Issues**
   - Some ISPs use CGNAT which prevents port forwarding
   - If port forwarding doesn't work, contact your ISP
   - Server administrator may need to set up alternative solutions

7. **Try Local Connection First**
   - If connecting from internet, first try from the local network
   - This helps determine if it's a network or server issue

8. **Check Valheim Game Version**
   - Ensure your Valheim client is up to date
   - Version mismatch can prevent connection

### Connection Timeout

If connections are timing out:

1. **Verify firewall isn't blocking**
   - Check both client and server firewalls
   - Temporarily disable firewall to test (re-enable after testing)

2. **Check router settings**
   - Ensure router firewall isn't blocking UDP traffic
   - Verify port forwarding rules are active

3. **Network path issues**
   - Try connecting from a different network/device
   - Test with a mobile hotspot to rule out local network issues

4. **Server overloaded**
   - Ask server administrator to check resource usage
   - Server might be running out of memory or CPU

### Wrong Password Error

If you're getting password errors:

1. **Confirm the correct password**
   - Ask server administrator for the current password
   - Passwords are case-sensitive

2. **Check for typos**
   - Re-enter the password carefully
   - Copy-paste if possible to avoid typos

3. **Password may have changed**
   - Server administrator may have updated the password
   - Request the updated password

### Server Appears but Shows as Full

If the server appears but says it's full:

1. **Server is limited to single player or configured capacity**
   - Ask server administrator to check server capacity settings
   - Wait for a slot to become available

## Getting Help

If you continue to experience connection issues:

1. **Contact the server administrator** with:
   - Your IP address and location (local network vs internet)
   - The exact error message you're seeing
   - Steps you've already tried

2. **Check server status**
   - Ask administrator to verify server is running
   - Request server logs if connection fails repeatedly

3. **Verify network setup**
   - Confirm you're using the correct IP and port
   - Check if router port forwarding is configured (for external access)

## Server Information

### Ports Used
- **2456/UDP**: Game traffic (primary connection port)
- **2457/UDP**: Server query (for server browser)

### Connection Types
- **Local Network**: Connect using Raspberry Pi's local IP (e.g., `192.168.1.100:2456`)
- **Internet**: Connect using public IP address (e.g., `123.45.67.89:2456`)

### Server Requirements
- Valheim game client installed and updated
- Network access to the server
- Server password (if configured by administrator)

## Additional Resources

- [Valheim Official Website](https://www.valheimgame.com/)
- For server setup and configuration, see [SETUP.md](SETUP.md)

