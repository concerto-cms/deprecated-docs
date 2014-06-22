# how to install the standard edition

### step 1: download the code
There are 3 ways for downloading the code:

**Option 1: using git**

> git clone git@github.com:concerto-cms/ConcertoCmsStandardEdition.git

**Option 2: using composer**

*(not yet possible)*

**Option 3: Download zip**

Download the latest version [here](https://github.com/concerto-cms/ConcertoCmsStandardEdition/archive/master.zip) and unzip it to a folder of your choosing.

### step 2: composer install

Run composer by going into the root folder of the application and type following command in the console:

> composer install

During the installation, composer will request some information about the database connection.

<small>
* Database configuration options can be found in the [Doctrine configuration reference](http://symfony.com/doc/current/reference/configuration/doctrine.html) for more information.<br />
* If you need more help on using composer, take a look at the [getcomposer.org](https://getcomposer.org/) website.
</small>

### step 3: setup a database

Next, you need to run a few Symfony console commands to setup the database.

The first command generates the tables:
> php app/console doctrine:phpcr:init:dbal           

Adds some initial data related to PHPCR:
> php app/console doctrine:phpcr:repository:init  

Add some demo data:
> php app/console concerto:fixtures:load             

<small>* [How to use the Symfony console](http://symfony.com/doc/current/cookbook/console/usage.html)
</small>

### step 4: Build the frontend
The frontend stuff (js, css) needs to be compiled using a tool called grunt. Third-party javascript libraries are managed using the frontend package manager Bower. Both tools require node.js to be installed. If you haven't used node.js before, head over to the [Node.js website](http://nodejs.org) and install it.

Install bower and grunt by running the following commands:
> npm install -g grunt-cli bower             

Run following commands to build the frontend files:
> npm install
> bower install
> grunt

### step 5: run it!

Everything should be ready to work. All you need now is a php webserver. If you're using a webserver like Apache or Nginx, you need to make sure the `/web` folder is used as document root.

The easiest alternative is to use the php built-in server:
> cd web
> php -S localhost:80

Then visit the website on this url: [http://localhost/app_dev.php](http://localhost/app_dev.php) <br />
The backend uses this url: [http://localhost/app_dev.php/cms](http://localhost/app_dev.php/cms)
