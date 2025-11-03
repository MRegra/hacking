Perfect — here’s the concise summary for your notes:

---

### Connecting Using VPN — Summary

A **VPN (Virtual Private Network)** creates an encrypted tunnel over the internet, allowing secure access to a private network as if you were directly connected to it. It hides your real IP by routing traffic through the VPN server.

**Types of VPNs:**

* **Client-based VPN:** Uses dedicated software; provides full network access.
* **SSL VPN:** Browser-based; usually limited to web apps.

**Use Cases:**

* Remote work and secure access to internal resources.
* Added privacy or bypassing restrictions (though commercial VPNs don’t guarantee anonymity).
* Should *never* be used for illegal activities — logs may exist.

**Connecting to Hack The Box VPN:**

* HTB labs require VPN access to reach internal targets (machines use private 10.x.x.x networks).
* Always treat the HTB VPN as **hostile** — only connect from a dedicated VM.
* Harden your attack VM (disable SSH password auth, avoid storing sensitive files).
* Command to connect:

  ```bash
  sudo openvpn user.ovpn
  ```

  When you see `Initialization Sequence Completed`, you’re connected.
* Verify connection with:

  * `ifconfig` → shows `tun0` adapter with an internal IP (e.g., 10.10.x.x).
  * `netstat -rn` → confirms routes to HTB networks (e.g., 10.129.0.0/16).

**Troubleshooting & Help:**
HTB’s support portal provides guides for **Lab Access** and **Connection Troubleshooting** if issues arise.

