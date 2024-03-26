---
title: "Kasm Workspaces"
tags: ["Docker", "Cybersecurity", "Workspaces", "Lab", "Review"]
style: border
color: primary
image: "/assets/images/assets/images/kasm-workspaces/expose-cover.png"
description: "kasm Workspaces provide browser-based access to a secure and customized work environment. This means that I can access my personal lab from any location and on any device, making it incredibly convenient for me to practice cybersecurity techniques and experiment with different tools."

permalink: /kasm-workspaces
---

## My Personal Lab for Cybersecurity Practice

As a cybersecurity enthusiast, I am always looking for ways to enhance my skills and knowledge in the field. One tool that has greatly contributed to my learning journey is Kasm Workspaces.

Kasm Workspaces provide browser-based access to a secure and customized work environment. This means that I can access my personal lab from any location and on any device, making it incredibly convenient for me to practice cybersecurity techniques and experiment with different tools.

One of the key advantages of Kasm Workspaces is the ability to stream desktops or applications directly to the browser. This eliminates the need for me to set up complex virtual machines or install software on my local machine. I can simply log in to my Kasm Workspace and have instant access to a fully functional environment.

The secure nature of Kasm Workspaces is also a major benefit. As a cybersecurity enthusiast, I understand the importance of practicing in a controlled and isolated environment. Kasm Workspaces provide a secure sandbox where I can test various attack scenarios without putting my own machine or network at risk.

Furthermore, Kasm Workspaces allow for customization. I can configure my workspace to include the specific tools and software that I need for my cybersecurity practice. This level of flexibility enables me to create an environment that closely resembles real-world scenarios, enhancing the effectiveness of my learning experience.


Another feature that I appreciate about Kasm Workspaces is the ability to save and restore workspaces. This means that I can easily pick up where I left off, even if I switch devices or locations. This feature has been particularly useful for me as I can seamlessly transition between my desktop, laptop, and mobile devices without losing any progress.

I will walk you through the installation process of Kasm Workspaces in a single server setup. This will allow you to create your own personal lab for cybersecurity practice and experimentation. Let's get started!

-----------------------------------------------------------------
# Kasm Workspaces Requirements
Kasm Workspaces can be installed in a single server or multi server setup.

## Operating System
Kasm Services and end user sessions are Docker containers. Access to the Docker registry is required for installation.

Installation is supported on the following operating systems:

**Supported Operating Systems**

- Ubuntu 20.04 / 22.04 (amd64/arm64)

- Debian 10 / 11 / 12 (amd64/arm64)

- CentOS 7 / 8 / 9 (amd64/arm64)

- Oracle Linux 7 / 8 / 9 (amd64/arm64)

- Raspberry Pi OS (Debian) 10 / 11 / 12 (arm64)


## Resource Requirements
**Minimum Requirements:**

CPU : 2 cores

Memory : 4GB

Storage : 50GB (SSD)


# Single Server Installation
The simplest way to deploy Kasm Workspaces is to install all application services on a single server. End-user sessions will also be provisioned on this server. All interior docker communication occurs within the single server and there are no special configurations required.

> Commands to install Kasm Workspaces on a single server:

```Shell
cd /tmp
curl -O https://kasm-static-content.s3.amazonaws.com/kasm_release_1.15.0.06fdc8.tar.gz
tar -xf kasm_release_1.15.0.06fdc8.tar.gz
sudo bash kasm_release/install.sh

```

Now sit back and have some coffee, the installation will take some time!

After the installation is complete, you can access the Kasm Workspaces web interface by navigating to `https://<your-server-ip>`on port 443 in your browser.

The Default usernames are admin@kasm.local and user@kasm.local. The passwords will be randomly generated and presented at the end of the installation process. You can change the passwords after logging in. So, don't forget to note them down!

> Login Screen:
![Default Login Screen](/assets/images/kasm-workspaces/image.png)


After logging in, you will be redirected to the admin dashboard where you can create new workspaces, manage users, and configure the settings as per your requirements.


> Admin Dashboard:
![Admin-Dashboard](/assets/images/kasm-workspaces/image-1.png)

Now from the admin dashboard, you can create new workspaces, manage users, and configure the settings as per your requirements.

> Workspaces
![Workspaces](/assets/images/kasm-workspaces/workspaces.png)

You can see the list of workspaces that are available. You can create new workspaces, edit existing ones, and delete them as needed. 

- Parrot OS
- Maltego
- Nessus
- SpiderFoot
- Remnux

and several other workspaces have installed for my practice sessions.

You can start a workspace by clicking on it and then selecting in which tab you want to open it. You can start and stop multiple workspaces at the same time and switch between them easily.

You can delete the workspace from the workspace tab. This will remove the workspace from the server and free up resources on the server.

You can manage users from the Access Management tab. You can add new users, edit existing users, and delete users as needed. You can also assign roles to users to control their access to the system.

This is not the end of it as you can also create your own custom workspaces. You can create a custom workspace by selecting the Custom Workspace option from the Workspaces tab. You can then configure the workspace by selecting the base image, setting the resource limits, and adding any additional software that you need.

Explore the Kasm Workspaces interface and experiment with the different features to create a personalized lab environment that suits your needs.

# Reference
- [Kasm Workspaces Documentation](https://kasmweb.com/docs/latest/index.html)

-----------------------------------------------------------------
# Conclusion

In conclusion, Kasm Workspaces have been a game-changer for me as a cybersecurity enthusiast. The ability to access a secure and customized work environment from any device has greatly enhanced my learning and practice sessions. I highly recommend Kasm Workspaces to anyone interested in cybersecurity or looking to create their own personal lab for experimentation and learning.

I hope this article has provided you with valuable insights into Kasm Workspaces and how you can leverage this tool to enhance your cybersecurity skills. If you have any questions or would like to share your experience with Kasm Workspaces, feel free to leave a comment below.

*All the best with your cybersecurity journey! Happy hacking!* 