# Fail2ban

### 1. Install Fail2Ban

First, ensure that Fail2Ban is installed on your system. If it's not installed, you can install it using the package manager of your Linux distribution. For example, on Debian/Ubuntu systems, you can use:

```bash
sudo apt-get update
sudo apt-get install fail2ban
```

On CentOS/RHEL systems, you can use:

```bash
sudo yum install epel-release
sudo yum install fail2ban
```

### 2. Configure Fail2Ban for Apache

Fail2Ban uses "jails" to define the services it monitors and the actions it takes. You'll need to configure a jail for Apache.

- **Edit the Fail2Ban configuration file**: Open the `jail.local` file, which is typically located in `/etc/fail2ban/`. If the file doesn't exist, you can copy the default `jail.conf` file to `jail.local`.

```bash
sudo nano /etc/fail2ban/jail.local
```

- **Add or modify the Apache jail**: Look for the `[apache]` section in the file. If it doesn't exist, you can add it. Here's an example configuration:

```ini
[apache]
enabled = true
port     = http,https
filter   = apache-auth
logpath = /var/log/apache2/error.log
maxretry = 3
bantime = 3600
```

This configuration does the following:

- `enabled = true`: Enables the jail.
- `port = http,https`: Specifies the ports to monitor.
- `filter = apache-auth`: Specifies the filter to use. The `apache-auth` filter is predefined and looks for failed login attempts in Apache's log files.
- `logpath = /var/log/apache2/error.log`: Specifies the path to Apache's error log file.
- `maxretry = 3`: Specifies the number of failed attempts before an IP is banned.
- `bantime = 3600`: Specifies the ban duration in seconds (3600 seconds = 1 hour).

### 3. Restart Fail2Ban

After configuring the jail, restart Fail2Ban to apply the changes:

```bash
sudo systemctl restart fail2ban
```

### 4. Verify the Configuration

You can check the status of Fail2Ban and see the list of active jails with:

```bash
sudo fail2ban-client status
```

And you can check the status of the Apache jail specifically with:

```bash
sudo fail2ban-client status apache
```

### 5. Monitor Fail2Ban Logs

Fail2Ban logs its actions, which can be found in `/var/log/fail2ban.log`. You can monitor this log to see when IPs are banned and unbanned.

```bash
tail -f /var/log/fail2ban.log
```

By following these steps, you can configure Fail2Ban to monitor Apache's log files for failed login attempts and take appropriate actions to protect your server from brute-force attacks.