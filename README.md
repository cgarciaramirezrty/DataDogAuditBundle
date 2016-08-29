# Audit bundle

This bundle creates an audit log for all doctrine ORM database related changes:

- inserts and updates including their diffs and relation field diffs.
- many to many relation changes, association and dissociation actions.
- if there is an user in token storage, it will link him to the log.
- the audit entries are inserted within the same transaction during **flush**,
if something fails the state remains clean.

Basically you can track any change from these log entries if they were
managed through standard **ORM** operations.

**NOTE:** audit cannot track DQL or direct SQL updates or delete statement executions.

## Install

First, install it with composer:

    composer require data-dog/audit-bundle-cgarcia

Then, add it in your **AppKernel** bundles.

    // app/AppKernel.php
    public function registerBundles()
    {
        $bundles = array(
            ...
            new DataDog\AuditBundle\DataDogAuditBundle(),
            ...
        );
        ...
    }

Finally, create the database tables used by the bundle:

Using [Doctrine Migrations Bundle](http://symfony.com/doc/current/bundles/DoctrineMigrationsBundle/index.html):

    php app/console doctrine:migrations:diff
    php app/console doctrine:migrations:migrate
    
Using Doctrine Schema:
    
    php app/console doctrine:schema:update --force

## Demo

The best way to see features is to see the actual demo. Just clone the bundle
and run:

    make

Visit **http://localhost:8000/audit** to see the log actions.

The demo application source is available in **example** directory and it is a basic
symfony application.

## Usage

**audit** entities will be mapped automatically if you run schema update or similar.
And all the database changes will be reflected in the audit log afterwards.

### Unaudited Entities

Sometimes, you might not want to create audit log entries for particular entities.
You can achieve this by listing those entities under the `unaudired_entities` configuuration
key in your `config.yml`, for example:

    data_dog_audit:
        unaudited_entities:
            - AppBundle\Entity\NoAuditForThis

## Screenshots

All paginated audit log:
![Screenshot](https://raw.github.com/DATA-DOG/DataDogAuditBundle/master/screenshots/audit1.png)

Clicked on history reference for specific resource:
![Screenshot](https://raw.github.com/DATA-DOG/DataDogAuditBundle/master/screenshots/audit2.png)

Showing insert data:
![Screenshot](https://raw.github.com/DATA-DOG/DataDogAuditBundle/master/screenshots/audit3.png)

## License

The audit bundle is free to use and is licensed under the [MIT license](http://www.opensource.org/licenses/mit-license.php)

