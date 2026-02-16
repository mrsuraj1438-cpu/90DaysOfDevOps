# Day 09 – Linux User & Group Management Challenge

## Overview
In this challenge, I focused on managing Linux users and groups. I created multiple users and groups, assigned them to groups, and set up shared directories with proper permissions to enable team collaboration.

---

## Users & Groups Created

### Users
- tokyo
- berlin
- professor
- nairobi

### Groups
- developers
- admins
- project-team

---

## Group Assignments

| User       | Groups Assigned          |
|------------|--------------------------|
| tokyo      | developers, project-team |
| berlin     | developers, admins       |
| professor  | admins                   |
| nairobi    | project-team             |

Group membership was verified using standard Linux commands.

---

## Directories Created

### `/opt/dev-project`
- Group: `developers`
- Permissions: `775`
- Accessible by users: tokyo, berlin

### `/opt/team-workspace`
- Group: `project-team`
- Permissions: `775`
- Accessible by users: tokyo, nairobi

---

## Commands Used

### User Creation & Verification
whoami                                        # Display current user
sudo useradd -m tokyo                         # Create user 'tokyo' with home directory
sudo useradd -m berlin                         # Create user 'berlin' with home directory
sudo useradd -m professor                       # Create user 'professor' with home directory
sudo useradd -m nairobi                         # Create user 'nairobi' with home directory
sudo usermod -s /bin/bash tokyo                  # Set bash as default shell for tokyo
sudo usermod -s /bin/bash berlin                  # Set bash as default shell for berlin
sudo usermod -s /bin/bash professor                # Set bash as default shell for professor
sudo usermod -s /bin/bash nairobi                  # Set bash as default shell for nairobi
sudo passwd tokyo                               # Set password for tokyo
sudo passwd berlin                               # Set password for berlin
sudo passwd professor                             # Set password for professor
sudo passwd nairobi                               # Set password for nairobi
cat /etc/passwd                                 # Verify users in system
ls /home                                        # List home directories
