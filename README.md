# Web Security Lab

## Setup 

1. Install [VirtualBox](https://www.virtualbox.org/wiki/Downloads);
2. Download a pre-built SEED VM from [Google Drive](https://drive.google.com/file/d/138fqx0F8bThLm9ka8cnuxmrD6irtz_4m/view?usp=sharing) or [Direct Download Link](https://seed.nyc3.cdn.digitaloceanspaces.com/SEED-Ubuntu20.04.zip)
3. Create a new VM from the pre-built VM disk.

### Create a lab VM

* Open the VirtualBox, click "New"

![1](./images/1.png)

* Create a Virtual Machine as the picture shows

![2](./images/2.png)

* Choose the "Use an existing virtual hard disk file" , and the hard disk file is the pre-built VM disk file downloaded above. 

![3](./images/3.png)

* Start the Virtual Machine

### Set up the Web Security environment (XSS)

Inside the Lab VM:

1. Open a Terminal
2. Download the required file: `wget --no-check-certificate -c https://seedsecuritylabs.org/Labs_20.04/Files/Web_XSS_Elgg/Labsetup.zip`
3. Run `unzip Labsetup.zip && cd Labsetup`
4. Edit the hosts file, and add a line at the end of the `/etc/hosts` file, eg: `echo 10.9.0.5        www.seed-server.com | sudo tee -a /etc/hosts `
5. Run `docker-compose up -d`
6. Open a browser, and visit `www.seed-server.com`
7. Username:`alice`, Password: `seedalice`; or Username: `boby`, Password: `seedboby`

### Set up the Web Security environment (CSRF)

Inside the Lab VM:

1. Open a Terminal
2. Download the required file: `wget --no-check-certificate -c https://seedsecuritylabs.org/Labs_20.04/Files/Web_CSRF_Elgg/Labsetup.zip`
3. Run `unzip Labsetup.zip && cd Labsetup`
4. Edit the hosts file, and add a line at the end of the `/etc/hosts` file, eg: `echo 10.9.0.5        www.seed-server.com | sudo tee -a /etc/hosts `; `echo 10.9.0.5 www.example32.com | sudo tee -a /etc/hosts`; `10.9.0.105 www.attacker32.com`
5. Run `docker-compose up -d`
6. Open a browser, and visit `www.seed-server.com`
7. Username:`alice`, Password: `seedalice`; or Username: `samy`, Password: `seedsamy`
8. Edit the attacker's pages: run `dockps` to check the container "attacker"'s id, and use `docksh <container id>` to edit attacker's html inside a container. eg:

```bash
[02/13/22]seed@VM:~/.../Labsetup$ dockps
53856dffc574  attacker-10.9.0.105
ca925c10bf2c  elgg-10.9.0.5
50ea207fc673  mysql-10.9.0.6
[02/13/22]seed@VM:~/.../Labsetup$ docksh 53856dffc574
root@53856dffc574:/# cd /var/www/attacker/
root@53856dffc574:/var/www/attacker# nano addfriend.html
```

![5](./images/5.png)


## Discover XSS vulnerabilities

### Task 1: Display an Alert Window when visiting a user's profile

You can edit Boby's profile from this link: `http://www.seed-server.com/profile/boby/edit`

Make some changes, so that an Alert Window will be prompted:

![4](./images/4.png)

### Task 2: Stealing Cookies from the Victim’s Machine

Boby commented on a post of Alice, when Alice views the comment, Alice's cookie will be stolen. 

You may need to write some Javascript code to send the victim's cookie to your server

### Task 3: Defeating XSS Attacks Using CSP

Share your solutions.

## Discover CSRF vulnerabilities

### Task 1: CSRF Attack using GET Request

Alice and Samy. Samy wants to become a friend to Alice, but Alice refuses to add him to her Elgg friend list. Samy decides to use the CSRF attack to
achieve his goal. He sends Alice an URL (via an email or a posting in Elgg); Alice, curious about it, clicks on the URL, which leads her to Samy’s web site: `www.attacker32.com`. Pretend that you are Samy, describe how you can construct the content of the web page, so as soon as Alice visits the web page, Samy is added to the friend list of Alice (assuming Alice has an active session with Elgg).

You need to edit the `addfriend.html` insider the `attacker-10.9.0.105` container, and the page can be accessed from `http://www.attacker32.com/addfriend.html`

### Task 2: CSRF Attack using POST Request

Samy plans to use a CSRF attack to modify Alice's profile.

### Task 3: Defense

CSRF token.
