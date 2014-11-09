---
layout: post
title: "How To Turn Your Chromebook Into A Web Development Machine"
date: 2014-11-09 14:09:57 -0500
comments: true
categories:  [Web Development]
---

<p>Chromebooks are known to be inexpensive, lightweight laptops, perfect for traveling and using cloud-based apps or browsing the web. They aren't, however, too popular among developers--yet. In this post, I'll demonstrate how to set up a Chromebook for all of your web development needs, transforming it from a cheap cloud client to a versatile development machine.</p>

<p>This tutorial is based on an Acer C720 Chromebook, but should work as well with other Intel-based Chromebooks. </p>

<h2> Running Linux + ChromeOS using Crouton <h2>
<p>The simplest way to set up your Chromebook for web development would be to use a cloud-based development environment like Nitrous.io and simply log into it from the Chrome browser. But that would be no fun. </p>

<p><a href="https://github.com/dnschneid/crouton">Crouton</a> makes it incredibly easy to download, install, and run Linux (Ubuntu by default) from within ChromeOS. This way, you can easily switch between ChromeOS and Ubuntu. To set up your Chromebook this way, you just have to turn Development Mode on, download Crouton, and run a few commands from the crosh terminal. An easy-to-follow tutorial on that can be found <a href="http://lifehacker.com/how-to-install-linux-on-a-chromebook-and-unlock-its-ful-509039343">here</a>.</p>

<h2> Set up Ubuntu for Full-Stack Web Development with Yeoman<h2>
<p>After setting up Ubuntu on your Chromebook through Crouton, you  should have a functioning and clean version of Ubuntu. You can switch between ChromeOS and Ubuntu using (Shift+Alt+Back Arrow) and (Shift+Alt+Forward Arrow, Shift+Alt+Reload). Now, if only there was a way to have a full stack web development environment up and running quickly with minimal set up. Luckily, there's Yeoman.</p>

<p>Yeoman is an amazing tool for scaffolding, with a large community that has created many different boilerplate generators for whatever development stack you might need. One of the most popular types of generators are MEAN stack generators. This stack consists of MongoDB for your database/storage, Node+Express for your back-end web server, and AngularJS as your front-end framework. In this article, I'll be using generator-angular-fullstack, but there are several other MEAN stack generators on the Yeoman Community Generators page, as well as boilerplate generators for many other types of development stacks.</p>

<p>Before setting up Yeoman, we'll begin with updating Ubuntu and installing our favorite code editors and browsers. We'll use Sublime Text 3 (since that's my favorite editor) but you can obviously use whatever you might be comfortable with (Vim? Emacs?). Also, we'll install Firefox since we can use Chrome in ChromeOS already--it'll be helpful to have at least 2 different browsers to test on.</p>

<p>Update/Upgrade Ubuntu:
	<code>sudo apt-get update && sudo apt-get upgrade</code>
For Sublime Text 3 and Firefox, we'll have to add their repositories to Aptitude, and then update before we can install the editor.
<code>
	sudo add-apt-repository ppa:webupd8team/sublime-text-3
	sudo add-apt-repository ppa:mozillateam/firefox-next 
	sudo apt-get update
	sudo apt-get install sublime-text
	sudo apt-et install firefox
</code>
Note: if you have a problem with the add-apt-repository command, install the <code>software-properties-common</code> package.

Now, install other necessary packages for development and general tools, such as Git and curl.
<code>sudo apt-get install git build-essential zip nano curl</code>
</p>

<h2>Install NodeJS and MongoDB</h2>
<p>To install NodeJS and it's package manager npm, we'll add the NodeSource to the APT sources with these commands:
<code>curl -sL https://deb.nodesource.com/setup | sudo bash -
sudo apt-get install nodejs
</code></p>
<p>I had some issues installing MongoDB using the method on the official docs, not sure if it's a problem for other crouton users; the following method was the easiest way for me to get MongoDB running.

First, download the tar-zipped mongo package from <a href="http://fastdl.mongodb.org/linux/mongodb-linux-i686-2.6.4.tgz">here</a>.
Then, unzip the tar file and move the folder to /usr/lib.
<code>
	cd Downloads/
	tar xzf mongodb-linux-i686-2.6.4.tgz
	sudo mkdir /usr/lib/mongodb
	sudo mv mongodb-linux-i686-2.6.4 /usr/lib/mongodb/
</code>
We'll also need to create the global data directory that MongoDB writes to, and set permissions accordingly.
<code> mkdir -p /data/db
chmod 777 /data/*
 </code>
Now, start the mongodb server: <code>cd /usr/lib/mongodb/mongodb-linux-i686-2.4.6/bin/
./mongod &
</code> 
You can check to see if the server is running correctly by going to <a href="http://localhost:28017/">http://localhost:28017/</a> in your browser.
Note: If you have a problem running mongod due to a libstdc++ library error, download and install the dependency: <code>sudo apt-get install lib32stdc++6</code>
</p>

<h3>Set up Yeoman and generate boilerplate</h3>
<p>Make a directory for your workspace, and install Yeoman along with the generator that you'd like to use. We also have to set up permissions for npm so that it can access system directories that it needs without sudo.
<code>
	cd workspace
sudo chown -R `whoami` `npm -g bin`
npm config set strict-ssl false
npm install -g yo
npm install -g generator-angular-fullstack
</code>
At this point, we should have a MongoDB instance running in the background, the latest version of NodeJS installed, and a Yeoman generator ready to use. We can go ahead and run the Yeoman generator, which will create a boilerplate project and allow you to configure it along the way.
<code> yo angular-fullstack</code>

That's all there is to it! Your inexpensive Chromebook is now a full on development machine, ready to make the next big web app. If you need further help with creating a MEAN stack web app, there are plenty of great tutorials out there!
