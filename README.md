# docean
Using browser to achieve:
1. sign up digital ocean
2. create a droplet (ubuntu 18.04), you may add volume now or later, choose a data center close to your target audience or your current location 
3. add a domain (from the pull down menu of the droplet just created)
4. config the domain, from the menu item "manage the doman" on the pull down menu of the domain
5. create dns records
    type = NS, hostname = [yourdomainname without www, e.g. example.com], value=[ns1.digitalocean.com], TTL= 1800
    type = NS, hostname = [yourdomainname without www, e.g. example.com], value=[ns2.digitalocean.com], TTL= 1800
    type = NS, hostname = [yourdomainname without www, e.g. example.com], value=[ns3.digitalocean.com], TTL= 1800
    type = A, hostname = [yourdomainname without www, e.g. example.com], value=[IP assigned by digital ocean], TTL= 1800
    type = CNAME, hostname = [www.yourhostname], value=[yourdomainname without www, e.g. example.com], TTL= 1800
6. add ssh keys:
   on the left hand side panel menu, under Account at the bottom left, Security, click
   > now u nder Secutiy dashboard
     > SSH Keys, *add ssh keys* and paste your ssh ky in the pop up dialog
   If you don't know about SSH key, read it about https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/#generating-a-new-ssh-key
   
Now ready for shell commands
1. login as root to digital ocean, sammy@yoursite or whateverusernameyoursetup@yoursite. If you have already set up ssh key (registered with your ssh-agent your local machine, and added to digital ocean), you should immeidate get in remote terminal

2. run provision script as described here, just remember to change default account username from sammy unless want to stay as sammy https://www.digitalocean.com/community/tutorials/automating-initial-server-setup-with-ubuntu-18-04,

3. set up nginx    https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-ubuntu-18-04
4. I use nodejs (expressjs server, a very easy to use and learn server, and perform well even for large project too), to install nodejs https://www.digitalocean.com/community/tutorials/how-to-set-up-a-node-js-application-for-production-on-ubuntu-18-04
5. enable https on nginx:  https://www.digitalocean.com/community/tutorials/how-to-secure-nginx-with-let-s-encrypt-on-ubuntu-18-04
6. it is easier to work with deploying of your project using gitgub and gitlab. The work flow would be like this: commit on local machine -> git push;  (remote under digital ocean directory, e.g, /home/sammy) git clone and git pull -> npm run build (assume using nodejs, and have a build sciprt) -> pm2 start yourapp.js (later can pm2 restart yourapp.js wheneer you have new build)

7. I use mongodb, setting up mongodb is quite straight forward, https://www.digitalocean.com/community/tutorials/how-to-install-mongodb-on-ubuntu-18-04


NOte: you can set up nodejs, mongodb, nginx (not the SSL part, but you don't need it under local machine for development purpose) the same way under local machine for development environment. If you don't use nodejs (expressjs).

*For email server set up*
1. create an MX record under the domain. Tool for test mail server set up https://mxtoolbox.com/diagnostic.aspx
2.  update the droplet to the full domain name, e.g., abc.example.com for SMTP Reverse DNS Resolution	
3. https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-postfix-on-ubuntu-18-04
4. using postfix https://tecadmin.net/setup-catch-all-email-account-in-postfix/
5. edit /etc/postfix/main.cf to makepass the mxtoolbox.com  SMTP Banner check test.
    sudo nano /etc/postfix/main.cf
    find the line that reads
    #smtpd_banner = $myhostname ESMTP $mail_name
    uncomment it (remove the "#"), and change it to suit your needs, then run this:
    sudo service postfix restart
