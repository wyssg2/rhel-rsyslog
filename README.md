# 1. Get RHEL
- Create free developer account on https://developers.redhat.com/register
- Download RHEL 9 Offline Mode ISO ("Binary DVD") from https://access.redhat.com/downloads/content/rhel
- Alternatively, a binary compatible open source distribution works too https://rockylinux.org/de-DE/download
- Install RHEL9/Rocky9 in your environment

# 2. Install and run ansible to configure rsyslog with tls support
- Log in to your machine with a sudoer (better not root)
- You will need https connectivity to github.com, galaxy.ansible.com and cdn.redhat.com (RHEL software repositories)


- If RHEL, register with your free developer account:

```
- subscription-manager register
```

```
- sudo dnf install ansible-core git
- git clone https://github.com/wyssg2/rhel-rsyslog.git
- cd rhel-rsyslog
- ansible-galaxy collection install community.general
- ansible-playbook rsyslog.yml --ask-become-pass
- Reboot
```

# 3. Testing
- Check rsyslog service

```
user@machine rhel-rsyslog$ sudo systemctl status rsyslog
● rsyslog.service - System Logging Service
     Loaded: loaded (/usr/lib/systemd/system/rsyslog.service; enabled; preset: enabled)
     Active: active (running) since Fri 2025-07-11 14:55:49 CEST; 13s ago
       Docs: man:rsyslogd(8)
             https://www.rsyslog.com/doc/
   Main PID: 3241 (rsyslogd)
      Tasks: 10 (limit: 22421)
     Memory: 4.2M
        CPU: 737ms
     CGroup: /system.slice/rsyslog.service
             └─3241 /usr/sbin/rsyslogd -n

Jul 11 14:55:48 machine systemd[1]: Starting System Logging Service...
Jul 11 14:55:49 machine systemd[1]: Started System Logging Service.
Jul 11 14:55:49 machine rsyslogd[3241]: [origin software="rsyslogd" swVersion="8.2412.0-1.el9" x-pid="3241" x-info="https://www.rsyslog.com"] start
Jul 11 14:55:49 machine rsyslogd[3241]: imjournal: journal files changed, reloading...  [v8.2412.0-1.el9 try https://www.rsyslog.com/e/0 ]
```

- Check service is listening

```
user@machine rhel-rsyslog$ netstat -ln
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State
tcp        0      0 0.0.0.0:30202           0.0.0.0:*               LISTEN
tcp        0      0 0.0.0.0:30203           0.0.0.0:*               LISTEN
tcp        0      0 0.0.0.0:30201           0.0.0.0:*               LISTEN
```

- Send a message

```
user@machine rhel-rsyslog$ echo "<13>Test message" | openssl s_client -connect localhost:30201
user@machine rhel-rsyslog# cat /var/log/remote/Test/tls_30201.log
Jul 11 15:04:13 Test message
```

