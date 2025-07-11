# 1. Get RHEL
- Create free developer account on https://developers.redhat.com/register
- Download RHEL 9 Offline Mode ISO ("Binary DVD") from https://access.redhat.com/downloads/content/rhel
- Install RHEL in your environment

# 2. Install and run Ansible
- sudo dnf install ansible-core git
- git clone https://github.com/wyssg2/rhel-rsyslog.git
- cd rhel-rsyslog && ansible-playbook rsyslog.yml
