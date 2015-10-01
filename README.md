SonataAdminBundle - demo website
==========

Documentation
==========

Configuration

```
# app/config/config.yml
sonata_admin:
    title:      Acme Demo Bundle
    title_logo: bundles/acmedemo/img/fancy_acme_logo.png
```

Переопределение зависимости

```
services:
    sonata.admin.post:
        class: Acme\DemoBundle\Admin\PostAdmin
        tags:
            - { name: sonata.admin, manager_type: orm, group: "Content", label: "Post" }
        arguments:
            - ~
            - Acme\DemoBundle\Entity\Post
            - ~
        calls:
            - [ setLabelTranslatorStrategy, ["@sonata.admin.label.strategy.underscore"]]
```
