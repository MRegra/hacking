# Basic Tools — Summary

**SSH**

* Secure remote shell (default port 22). Supports password or key-based auth.
* Used for stable remote access, file transfer, port forwarding, and as a jump host.
* Common command: `ssh user@host`.
* On compromised hosts, stealing/adding keys gives persistent access; prefer SSH over unstable reverse shells when possible.

**Netcat (nc / ncat)**

* General-purpose TCP/UDP tool for connecting to ports, banner grabbing, file transfer, and simple listeners.
* Useful for quick service interaction and getting/handling shells.
* Example: `nc target 22` shows SSH banner (banner grabbing).
* **Socat** is a more powerful alternative (port forwarding, serial, TTY upgrades) and useful for stabilizing shells.

**Tmux (terminal multiplexer)**

* Allows multiple windows/panes in one terminal session; persistent sessions survive disconnects.
* Install: `sudo apt install tmux -y`.
* Prefix is `Ctrl+B`. Common: `Ctrl+B c` (new window), `Ctrl+B %` (vertical split), `Ctrl+B "` (horizontal split), `Ctrl+B <num>` to switch.
* Handy for organizing long engagements and logging.

**Vim (text editor)**

* Ubiquitous keyboard-driven editor available on most Linux systems; useful for editing files on remote hosts.
* Modes: normal (nav), insert (`i`) for typing, `Esc` to return to normal.
* Basic commands: `x` (cut char), `dw` (cut word), `dd` (cut line), `yy` (copy line), `p` (paste).
* Save/quit: `:w`, `:q`, `:wq`, `:q!`.
* Learning Vim speeds up remote editing and productivity.

**Key takeaways**

* Master these foundational tools — they’re not flashy exploits but are used constantly in real pentesting.
* Keep socat and netcat handy for shell handling; use tmux for session management; use SSH for stability and pivoting; use Vim for quick edits on remote boxes.
