# -*- mode: ruby -*-
# vi: set ft=ruby :

$installscript = <<SCRIPT
if [ ! -f "/var/www/.provisioned_defaultvhost" ]; then
    echo Copying default site config /var/www/000-default.conf to /etc/apache2/sites-available/000-default.conf
    cp /var/www/000-default.conf /etc/apache2/sites-available/
    touch /var/www/.provisioned_defaultvhost
fi

if [ ! -f "/var/www/.provisioned_phpini" ]; then
    echo Copying php.ini file with 1024M upload_max_filesize and post_max_size.
    cp /var/www/php.ini /etc/php5/apache2/
    touch /var/www/.provisioned_phpini
fi

if [ ! -f "/var/www/.provisioned_mycnf" ]; then
    echo Copying .my.cnf file
    cp /var/www/.my.cnf /home/vagrant/
    touch /var/www/.provisioned_mycnf
fi

if [ ! -d "/var/www/public/www" ]; then
    echo Making new www directory in /var/www/public/www
    mkdir /var/www/public/www
fi

if [ -z $(which htop) ]; then
    echo "Installing htop"
    apt-get install htop
fi

echo Setting timezone to Pacific/Auckland
timedatectl set-timezone Pacific/Auckland

echo Reloading Apache2
service apache2 reload
SCRIPT

Vagrant.configure("2") do |config|
    
    # This is Scotch Box 3.0 (the free version)
    # _________________________________________
    # If you want PHP7, MySQL 5.7, Ubuntu 16.04, Build Scripts (customize your own boxes in minutes)...
    # ... an NGINX version, PHPUnit, Yarn, WebPack, improved email testing with MailHog...
    # ... generally Higher versions of things, Ruby (via RVM), Node (via NVM), WebPack ready, and more.
    
    # Just go to https://box.scotch.io/pro
    # Your support will help keep this project alive!

    config.vm.provider :virtualbox do |vb|
        vb.customize ["modifyvm", :id, "--cpus", "2"]
    end
    config.vm.box_version = "1.5.0"
    config.vm.box = "scotch/box"
    config.vm.network "private_network", ip: "192.168.33.10"
    config.vm.hostname = "scotchbox"
    config.vm.synced_folder ".", "/var/www", :mount_options => ["dmode=777", "fmode=666"]
    config.vm.provision "shell", inline: $installscript, privileged: true
    
    # Optional NFS. Make sure to remove other synced_folder line too
    #config.vm.synced_folder ".", "/var/www", :nfs => { :mount_options => ["dmode=777","fmode=666"] }

end
