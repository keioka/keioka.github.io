---
published: false
---
This is very basic of AWS setting for Rails app. Capistarno does many things for deployment for your rails app. We can set up ec2 server by yourself. I use Linux server. 

Here is main flow.

1. Launch EC2 instance
2. Create user for your server
3. Install ruby, git, and many other things
4. Install Web server(Nginx)
5. Install Database (Sqlite)

For me, even launching simple web server seeems very hard. As you can see these process are not that hard. Obviously, this is least setting for the server. 

# Local 
```
chmod 400 ~/.ssh/ordertrip.pem  (change rights of pem)
```
chmod 400 is READ_ONLY and chmod 600 is READ_WRITE


### SSH connection

-v option allows you to check process

```
ssh -i ~/.ssh/example.pem ec2-user@11.111.11.111 (connect to server)
```


### Change to root user
```
[ex2-user @ip-xxx-xx-xx-xxx ~]$ sudo su - (change to root user)
```

### Add user on server by adding an arbitary name
```
[ex2-user @ip-xxx-xx-xx-xxx ~]$ useradd [user name]
```

### copy .ssh directory to your user
```
[ex2-user @ip-xxx-xx-xx-xxx ~]$ cp -arp /home/ec2-user/.ssh /home/[user name]/
```

### give acccess right to your .ssh directory with your user 
```
$ [ex2-user @ip-xxx-xx-xx-xxx ~]$ chown -R [user name]. /home/[user name]/.ssh
```

### set password to your user
```
$ [ex2-user @ip-xxx-xx-xx-xxx ~]$ passwd [user name]
```


# Install ruby

```
[user name @ip-xxx-xx-xx-xxx ~]$ rbenv install 2.2.2; rbenv rehash
```
```
[user name @ip-xxx-xx-xx-xxx ~]$ rbenv global 2.2.2
```
make sure your version is correct

### Install git in order to pull your project
```
[user name @ip-xxx-xx-xx-xxx ~]$ git --version
```

### Check ruby version
```
[user name @ip-xxx-xx-xx-xxx ~]$ ruby -v
```

### Check gem path
```
[user name @ip-xxx-xx-xx-xxx ~]$ which gem
=> ~/.rbenv/shims/gem #should be return this, if it is not, run 'rbenv rehash'
```
```
[user name @ip-xxx-xx-xx-xxx ~]$ which ruby
~/.rbenv/shims/ruby #should be return this
```

#Install sqlite
```
[user name @ip-xxx-xx-xx-xxx ~]$ sudo yum install sqlite-devel
```

```
[user name @ip-xxx-xx-xx-xxx ~]$ sudo yum install ruby-dev
```

```
[user name @ip-xxx-xx-xx-xxx ~]$ sudo gem install sqlite3-ruby
```
#http://serverfault.com/questions/245051/installing-sqlite-gem-fails-on-aws-linux-instance-with-sqlite-devel-libraries-in


#Install mongoDB
https://docs.mongodb.org/manual/tutorial/install-mongodb-on-amazon/
```
[user name @ip-xxx-xx-xx-xxx ~]$ touch /etc/yum.repos.d/mongodb-org-3.0.repo
```
```
[user name @ip-xxx-xx-xx-xxx ~]$ vi /etc/yum.repos.d/mongodb-org-3.0.repo
```

```
[mongodb-org-3.0]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/amazon/2013.03/mongodb-org/3.0/x86_64/
gpgcheck=0
enabled=1
```

```
[user name @ip-xxx-xx-xx-xxx ~]$ sudo yum install -y mongodb-org
```

# Make directory for your app

if you doesn't have 'www' directory, create one
```
[user name @ip-xxx-xx-xx-xxx ~]$ cd /var/www/; mkdir [your app name]
```

make rights tou your directory
```
[user name @ip-xxx-xx-xx-xxx ~]$ chown -R [user name] /var/www/[your app name]
```

Install nginx
```
[user name @ip-xxx-xx-xx-xxx ~]$ sudo yum -y install nginx
```

Launch nginx
```
[user name @ip-xxx-xx-xx-xxx ~]$ sudo /etc/init.d/nginx start
```

set config, if it is not exist, create one
```
[user name @ip-xxx-xx-xx-xxx ~]$ sudo vi /etc/nginx/nginx.conf
```

```
events {
    worker_connections  2048;
}

http {
    # your app will be createdunder current directory, if you use capistrano
    root  /var/www/[your app name]/current;

    # Unicorn setting
    upstream unicorn-server {
        server unix:/var/www/[your app name]/shared/tmp/sockets/unicorn.sock
        # make sure the path is the same as the one you set on unicorn and capistarano ruby file
        fail_timeout=0;
    }

    #Server setting
    server {

        listen 80;
        client_max_body_size 4G;
        server_name [EC2 public IP];
        keepalive_timeout 80;

        #set the log path for nginx
        access_log /var/log/nginx/access.log;
        error_log /var/log/nginx/error.log;

        root  /var/www/[your app name]/current;

        #set the asset of your app, rails create asset under public folder
        location ~ ^/assets/ {
            include /etc/nginx/mime.types;
            root    /var/www/[your app name]/current/public;
        }

       location / {
            proxy_pass http://unicorn-server; #This should be the upstream name
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header Host $host;
        }

        #Set the Error directory (set rails path)
        error_page   500 502 503 504  /500.html;
        location = /500.html {
            root /var/www/[your app name]/current/public;
        }
    }
}
```

After setting nginx config, restart
```
[user name @ip-xxx-xx-xx-xxx ~]$ sudo service nginx restart
```

#Basic command you will need
```
[user name @ip-xxx-xx-xx-xxx ~]$ sudo su - (change to root user)
```

```
[root @ip-xxx-xx-xx-xxx ~]$ su [user name]
```

#Set secret key
```
[user name @ip-xxx-xx-xx-xxx ~]$ sudo vi /etc/profile.d/rails.sh
```
export SECRET_KEY_BASE=[rake secret]
export SECRET_TOKEN=[rake secret]
