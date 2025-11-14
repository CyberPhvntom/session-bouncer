# session-bouncr

Real-time Linux session monitoring tool that detects and terminates unauthorized user logins based on a whitelist. Built for blue team defense and system security monitoring. Originally designed for on-the-fly implementation when EDR/SIEMS are not an immediate option. Primarily designed for OpenSUSE linux, but can be adopted to a variety of distros. 


Script Descriptions:
1. session-bouncr.sh (Recommended - Interactive)

Shows a nice dashboard with color coding
Prompts you before kicking users (Y/n/ignore)
Logs all events to /var/log/user_monitor.log
Beeps on unauthorized detection
Shows authorized user list

2. session-bouncr-lite.sh (Lightweight)

Continuous live display
Green ✓ for authorized, Red ✗ for unauthorized
Beeps when intruders detected
Minimal resource usage
Can uncomment auto-kick lines if you want

3. auto_kick.sh (Aggressive - Use with caution!)

Immediately terminates any unauthorized user
No prompts, automatic action
Logs all kicks to /var/log/auto_kick.log
Good for "defend at all costs" mode



How to Deploy on Your Linux Server:
Step 1: Upload the script to your server
Step 2: Customize the authorized users list
Step 3: Make it executable
        chmod +x session-bouncr.sh
Step 4: Run it

Pro Tip(s): Run it in the background using tmux.
            Run it on system startup. 


Features Summary:
Continuous monitoring (checks every 2-3 seconds)
Color-coded output (green=safe, red=danger)
Audio alerts (beeps on unauthorized users)
Logging (all events saved to log file)
Multiple kick methods (pkill, loginctl)
Easy customization (just edit the user list)

-----------------------------------------------------------------------
The scripts primarily use who command, not w or loginctl. Here's why:
Primary Method: who
who | awk '{print $1"|"$2"|"$5}'

Why: Simple, reliable, parses easily
Shows: Username, TTY, IP/hostname
Available: On virtually all Linux systems

Backup Method: loginctl (when available)
Used only for termination, not monitoring:
loginctl terminate-session SESSION_ID

Why: More thorough at killing sessions
Only used: As a backup kill method if pkill doesn't work

Why Not w?
w is great for humans but harder to parse in scripts because it shows load averages and IDLE time. who is cleaner for scripting.

How It Actually Works:

Monitoring: Uses who to get logged-in users
Comparison: Checks against authorized list
Termination: Uses multiple methods:

pkill -KILL -t TTY (kill by terminal)
pkill -KILL -u username (kill all user processes)
loginctl terminate-session (systemd cleanup)


