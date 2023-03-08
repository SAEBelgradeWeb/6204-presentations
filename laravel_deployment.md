---
title: "Laravel Deployment"
author: "Vladimir Lelicanin - SAE Institute"
format:
  revealjs:
    theme: default
    title-slide-attributes:
        data-background-image: https://keystoneacademic-res.cloudinary.com/image/upload/f_auto/q_auto/g_auto/w_256/element/15/156456_sae.jpg
        data-background-position: top left
        data-background-repeat: no-repeat
        data-background-size: 100px
    transition: fade
    footer: "Vladimir Lelicanin - SAE Institute Belgrade"
    margin: 0.2
    auto-animate: true
    preview-links: auto
    link-external-newwindow: true
    scrollable: true
    embed-resources: false
    slide-level: 1
    chalkboard: true
    multiplex: true
    slide-number: true
    incremental: false
    logo: https://upload.wikimedia.org/wikipedia/commons/e/e2/SAE_Institute_Black_Logo.jpg
---



## Introduction

In this presentation, we'll discuss Laravel manual deployment and Laravel Forge - two ways to deploy Laravel applications.


---

## Manual Deployment
To manually deploy a Laravel application, you need to:

- Manually upload/copy the code files to the production server
- Run `composer install` to install dependencies
- Configure the environment variables
- Configure web server to serve `public/index.php`


---

## Manual Deployment - Code Example

Copy the Laravel application files to the production server.
```shell
$ scp -r app_dir user@server:/path/
```

Install dependencies using Composer.
```shell
$ composer install --no-dev
```

Configure `.env` file.
```ini
APP_NAME=Laravel
APP_ENV=production
...
```

Configure the web server. Below is an example Apache configuration.
```apache
<VirtualHost *:80>
  DocumentRoot /path/public
  <Directory /path/public>
    Options Indexes FollowSymLinks MultiViews
    AllowOverride All
    Order allow,deny
    Allow from all
    Require all granted
  </Directory>
</VirtualHost>
```


---

## Laravel Forge
Laravel Forge is a server provisioning and deployment tool for Laravel applications. It simplifies the process of deploying the application by automating many of the steps.


---

## Laravel Forge - Features

- One-click server setup
- Automatic security updates
- Easy SSL certificate deployment
- Automatic daily backups
- Quick deployment of Laravel applications
- Integration with cloud hosting providers like DigitalOcean


---

## Laravel Forge - Deployment Process

The deployment process can be broken down into the following steps:

- Connect Forge to your Git repository
- Configure the server settings
- Create a site for your Laravel application
- Deploy the application


---

## Laravel Forge - Code Example

Assuming your Laravel application code is in a git repository, you can connect Forge to your repository with just a few clicks.

![Laravel Forge Deployment Process](https://cms-assets.tutsplus.com/cdn-cgi/image/width=630/uploads/users/1795/posts/30329/image/Deploy-PHP-Web-App-Laravel-Forge-Install-Git-Repo.png)


---

## Laravel Forge - Server Configuration

Once Forge is connected to your repository, you need to configure the server settings. Some of the settings include:

- Server size
- Database type and credentials
- Auto-deployment settings
- Web server software


---

## Laravel Forge - Site Configuration

After you've configured the server settings, you can create a site for your Laravel application. In the site settings, you can:

- Choose the PHP version
- Define environment variables
- Set up SSL certificate


---

## Laravel Forge - One-Click Deployment

Once everything is set up, deploying your Laravel application is as easy as clicking the "Deploy Now" button.


---

## Benefit of Laravel Forge 

- Saves time and effort
- Secure and easy deployment
- Convenient integration with cloud hosting providers
- Easy scaling
- Takes care of server and maintenance tasks


---

## Conclusion

- You can deploy Laravel applications manually or through Laravel Forge
- Laravel Forge is a more automated and secure way of deploying applications
- Laravel Forge saves time and maximizes productivity

---

## References

- Laravel Documentation: <https://laravel.com/docs>
- Laravel Forge: <https://forge.laravel.com/docs>
- Laravel Forge FAQ: <https://forge.laravel.com/faq>
