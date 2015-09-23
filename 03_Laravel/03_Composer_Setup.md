# Composer Setup

Summary:

1. PHP from command line check
2. Installing Composer on Mac
3. Installing Composer on Windows
4. Common problems (and solutions)


## PHP from command line check
In addition to running PHP on a server, you can also run PHP from command line.

Here's a simple example using PHP in the command line, in interactive mode (`-a`):
<img src='http://making-the-internet.s3.amazonaws.com/sysadmin-php-from-cl@2x.png' style='max-width:759px; width:100%' alt='PHP from CL'>

Composer uses PHP from command line, so we want to first get on the same page with *which* installation of PHP command line is using so we understand what version of PHP we're using, what modules we have available, etc.

For __Mac__, we want PHP from command line to use MAMP's installation of PHP, located (as of this writing) at: `/Applications/MAMP/bin/php/php5.6.10/bin/`

For __Windows__, we want PHP from command line to use XAMPP's installation of PHP, located at `c:\xampp\php\`

This is accomplished by adding the above paths to your PATH variable. For complete instructions on what the PATH variable is and how to edit it, go read: [__PATH Variables__](https://github.com/susanBuck/dwa15-fall2015-notes/blob/master/00_Command_Line/99_Extras/PATH_Variable.md).

Once you've completed the instructions on the PATH Variables doc, you should confirm that

1. You can run `php` from the command line.
2. The command `php` is using the PHP executable that comes installed with MAMP/XAMPP.

__Mac expectations__

```bash
$ which php
/Applications/MAMP/bin/php/php5.6.10/bin/php
```

And...

```bash
~   $ php --ini
Configuration File (php.ini) Path: /Applications/MAMP/bin/php/php5.6.10/conf
Loaded Configuration File:         /Applications/MAMP/bin/php/php5.6.10/conf/php.ini
Scan for additional .ini files in: (none)
Additional .ini files parsed:      (none)
```

__Windows expectations__

```bash
$ where.exe php
C:\xampp\php\php.exe
```

And...

```bash
$ php --ini
Configuration File (php.ini) Path: C:\Windows
Loaded Configuration File:         C:\xampp\php\php.ini
Scan for additional .ini files in: (none)
Additional .ini files parsed:      (none)
```

## Installing Composer on Mac

Move into your bin directory. This is a common location to put command line executable programs, so we'll install Composer here.

```bash
$ cd /usr/local/bin
```

Download and install `composer.phar`:

```bash
$ curl -sS https://getcomposer.org/installer | sudo php
```

>> *The phar extension provides a way to put entire PHP applications into a single file called a "phar" (PHP Archive) for easy distribution and installation.* -[source](http://php.net/manual/en/intro.phar.php)

To make calling Composer easier, rename `composer.phar` to `composer` using the `mv` (move) command:

```bash
$ mv composer.phar composer
```

Test it works:

```bash
$ composer
```

If the above procedure doesn't work, scroll down to see if your issue is listed in the *Common Problems* section.


## Installing Composer on Windows
There's a handy installer for Window's Composer, which you can download here: <https://getcomposer.org/Composer-Setup.exe>.

Running through the installer, you should see the following screens.

Make sure the `php.exe` path on the **third screen** matches the same XAMPP php.exe you set up earlier. This way you know what `php.exe` and `php.ini` Composer is using.

[Enlarge...](http://making-the-internet.s3.amazonaws.com/laravel-composer-install-on-windows@2x.png)
<img src='http://making-the-internet.s3.amazonaws.com/laravel-composer-install-on-windows@2x.png' class='' style='max-width:1252px; width:100%' alt='Composer installation on Windows'>

When the installer is complete, **close and restart Cmder**.

Now, you should be able to run the `composer` command from within any directory. This works because the installer updated your PATH variable for you.

<img src='http://making-the-internet.s3.amazonaws.com/laravel-windows-composer-confirm@2x.png' class='' style='max-width:537px; width:100%' alt=''>

That's it! Composer is installed and ready to go.

If the above procedure doesn't work, scroll down to see if your issue is listed in the *Common Problems* section.


## Goal

At this point, the end goal for both Mac and Windows users is you can run the `composer` command in command line:

<img src='http://making-the-internet.s3.amazonaws.com/laravel-composer-end-goal@2x.png' style='max-width:1008px; width:100%' alt='End goal after composer is installed'>


## Common problems (and solutions)

### Issue: openssl

__Symptoms:__ You get a message saying *openssl* is not enabled.

__Solution:__ Identify what `php.ini` file you're using with this command:

	$ php --ini

Open the indicated `php.ini` file and make sure the following line is not commented out (i.e. it does *not* have a semi-colon in front of it):

	extension=php_openssl.dll


### Issue: SSL Certificate problem

__Symptoms:__ (Mac) When downloading Composer (via the `curl -sS https://getcomposer.org/installer | php` command), you receive an error regarding SSL certificates:

	curl: (60) SSL certificate problem: unable to get local issuer certificate
	More details here: http://curl.haxx.se/docs/sslcerts.html
	curl performs SSL certificate verification by default, using a "bundle"
	 of Certificate Authority (CA) public keys (CA certs). If the default
	 bundle file isn't adequate, you can specify an alternate file
	 using the --cacert option.
	If this HTTPS server uses a certificate signed by a CA represented in
	 the bundle, the certificate verification probably failed due to a
	 problem with the certificate (it might be expired, or the name might
	 not match the domain name in the URL).
	If you'd like to turn off curl's verification of the certificate, use
	 the -k (or --insecure) option.

__Solution__:
Run the curl command somewhere outside of `/usr/local/bin` (for example, in your Documents folder) and when it's done, move the resulting `composer.phar` into `/usr/local/bin`.

```bash
$ cd ~/Documents
$ curl -sS https://getcomposer.org/installer | php
$ mv composer.phar /usr/local/bin/composer.phar
```


### Issue: Permission Denied

__Symptoms:__ (Mac) You're in your `/usr/local/bin` directory and you run the command to download Composer (`curl -sS https://getcomposer.org/installer | php`). The download fails citing *failed to open stream: Permission denied*.

__Solution:__

Run the command using `sudo`:

```bash
$ curl -sS https://getcomposer.org/installer | sudo php -d detect_unicode=Off
```

[source](http://stackoverflow.com/questions/14437588/having-trouble-installing-composer)

Similarly, if you get a *Permission denied* error when trying to move `composer.phar` you can also use sudo:

```bash
$ sudo mv composer.phar composer
```
