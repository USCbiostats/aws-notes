# Notes on AWS

Here are some random notes about how to use AWS

## EC2: Elastic Cloud Computing

This was one of the first services offered by AWS. EC2 provides a way to flexibly
request tailored computational resources in a few clicks. Many other AWS products
rely on EC2 as a backbone, e.g., AWS Lightsail.

### Setup

To set it up, just follow the steps available in AWS (click-by-click.) The most
complicated bit, at least to me, was that the default unix username is not provided
out of the box. In my case was "ubuntu" (I was trying an Ubuntu box), and more
importantly, `sudo` is possible without password, which makes installing things
easy.

Another important issue is that, in order to ssh to the server, you need to
set up your `pem` (identity) file. You will be asked to do so during the
setup. Once you download the pem file to your computer, you need to change 
permission levels, i.e.:

```bash
chmod 400 /path/to/your/identity.pem
```

It windows is by right clicking on the pem file, and set it to read-only for
the owner. Once you are done with it, you can login via ssh as follows

```bash
ssh -i /path/to/your/identity.pem ubuntu@[private dns or ip provided by AWS]
```

To install R, you can just rely on the default version shipped with Ubuntu 20,
which is a 2019 version, I believe:

```bash
sudo apt update && sudo apt install r-base
```

And you should be good to go.

### RStudio server

If you want, you can also install RStudio server. Just follow the instructions
provided in RStudio's website to get the free RStudio server version for Ubuntu.

In my case, when trying to start RStudio server, I got an error regarding a couple
of "missing" libraries. The problem was that Rstudio was looking in the wrong place.
What worked for me was to create symbolic links in the place where RStudio seemed
to be reaching out for them, the `/usr/lib` directory. I did it as follows:

```bash
cd /usr/lib
sudo ln -s /snap/core/10185/lib/x86_64-linux-gnu/libcrypto.so.1.0.0 libcrypto.so.1.0.0
```

The second thing to do, is to add the TCP port 8787, which is where Rstudio
runs by default, to the list of "inbound rules" of the EC2 instance. You can
set that up directly in the AWS EC2 console. 

Finally, since RStudio requires a password, you need to make sure that the user
you will be running RStudio with, which probably will be `ubuntu`, has a password.
You can change the user password as follows:

```bash
passwd ubuntu
```

Once that's complete, you can now start using RStudio server. For that, you can
access it by entering the following URL: `[private IP or DNS]:8787`.

