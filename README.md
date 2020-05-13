# Docker Template - PHP-Symfony 3.2.* project

### Version 1.0.4 - May 2020

*   This stack provides a development environment ready to run a PHP-Symfony 3.2.* project.
*   Prerequisite : to have Docker and Docker-Compose installed on your machine - [Docker Install](https://docs.docker.com/install/)
*   Preferred Operating System to use it : Linux / Mac OSx

-------------------------------------------------------------------------------------------------------------------------------------

Realized thanks to [(see GitHub)](https://github.com/drixs6o9) support.

Maintained and adapted by Adrien PIERRARD [(see GitHub)](https://github.com/WizBhoo).

-------------------------------------------------------------------------------------------------------------------------------------

### The Docker-Compose / Containers provided :

*   Nginx / Nginx Proxy
*   PHP 7.2 fpm alpine version
*   MySQL (PHP container allows SQLite if preferred) : ready to be used with MySQL 8
*   PHPMyAdmin
*   Blackfire - to allow you to analyze your website performance with Blackfire companion (performance tests and graph analysis)

Some containers have specifications to be mentioned :

* PHP is provided with a usual toolkit to allow you to manage your project from the PHP container shell and following component are available too :

    *   Composer
    *   Symfony
    *   Zsh / Ho-My-Zsh
    *   PHP Probe for Blackfire => please refer to instruction below to know how to setup Blackfire
    *   A pre-setup php.ini will be copied during PHP container building

*   Blackfire-agent and Blackfire CLI tools are already provided in the blackfire container</br>(PHP Probe as mentioned above is setup with PHP)

    *   You need to create an account on [Blackfire](https://blackfire.io/) to obtain ID and TOKEN keys for client and server.
    *   Then copy and rename the .env.dist file as .env file to put your personal keys inside.
    *   Don't forget to install Blackfire companion for Chrome or Firefox.
    *   To know more about how to install Blackfire (also locally in your machine for example), please refer to [Blackfire Installation](https://blackfire.io/docs/up-and-running/installation)

-------------------------------------------------------------------------------------------------------------------------------------

## How to launch your project environment

*   So easy !!! You just have to clone this repository in your preferred project directory.

```
git clone https://github.com/WizBhoo/docker_sf3-2.git
```

*   Go to the docker_sf3-2 cloned folder with your terminal (you can rename it as you want)

<blockquote>
Note that Make command is available thanks to the Makefile provided.

To know which commands are available use "make help" in your terminal.
</blockquote>

*   Use appropriate make command to build containers and launch the docker environment

*   Then a make command quickly allows you to launch Symfony project installation : "make project-init" command will launch :

```
docker exec -it -u 1000 php-fpm composer create-project symfony/framework-standard-edition:3.2.* symfony
```

*   Well done, you are ready to work on your Symfony project ! Look into the Symfony folder created... and init git with a first commit.

-------------------------------------------------------------------------------------------------------------------------------------

## Advanced configuration

*   Two virtual hosts are defined for locally use : [mon-site.localhost](http://mon-site.localhost) (site homepage)  &  [pma.localhost](http://pma.localhost) (PHPMyAdmin interface).

So don't forget to add them to your hosts file if you want to use them. (=> NB : No need anymore since Version 2.1 - January 2020).

*   In the docker-compose file you will find environment variables for MySQL and PHPMyAdmin. You can edit them at your convenience.

*   It's very easy to make this stack operational for a Symfony project version 3.4 or 4.*

    *   Go to edit the .docker/nginx/conf/conf.d/mon-site.conf file.
    *   Replace inside the main site root : for Symfony 3.4 and 4.* the folder is "public" instead of "web"
    *   Edit each line where you find "app_dev".php and replace by "index".php

*   Re-build containers / Re-run them / Work DONE ! / ENJOY !

<blockquote>
Note that you need to edit the makefile too if you want. Make project-init command has to refer to :

docker exec -it -u 1000 php-fpm composer create-project symfony/website-skeleton:3.4.* symfony

or

docker exec -it -u 1000 php-fpm composer create-project symfony/website-skeleton:4.* symfony
</blockquote>

-------------------------------------------------------------------------------------------------------------------------------------

## Contact

Thanks in advance for Star contribution.

Any question / trouble ?

Please send me an [e-mail](mailto:apierrard.contact@gmail.com) ;-)
