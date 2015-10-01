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
Создание child admins

```
services:
    app.admin.post:
        class: AppBundle\Admin\PostAdmin
        arguments: [~, AppBundle\Entity\Post, SonataAdminBundle:CRUD]
        tags:
            - { name: sonata.admin, manager_type: orm, group: admin, label: Post }

        calls:
            - [ addChild, ["@app.admin.comment"]]

    app.admin.comment:
        class: AppBundle\Admin\CommentAdmin
        arguments: [~, AppBundle\Entity\Comment, SonataAdminBundle:CRUD]
        tags:
            - { name: sonata.admin, manager_type: orm, group: admin, label: Comment }
```

```
<?php

namespace AppBundle\Admin;

use Sonata\AdminBundle\Admin\Admin;
use Sonata\AdminBundle\Datagrid\DatagridMapper;
use Sonata\AdminBundle\Datagrid\ListMapper;
use Sonata\AdminBundle\Form\FormMapper;
use Sonata\AdminBundle\Show\ShowMapper;

class CommentAdmin extends Admin
{
    protected $parentAssociationMapping = 'post';
```

```
 protected function configureListFields(ListMapper $listMapper)
    {
        $listMapper
            ->add('id')
            ->add('title')
            ->add('body')
            ->add('slug')
            ->add('_action', 'actions', array(
                'actions' => array(
                    'show' => array(),
                    'edit' => array(),
                    'delete' => array(),
                    'comments' => array(
                        'template' => ':default:comment_button.html.twig'
                    )
                )
            ))
        ;
    }
```

```
<a class="btn btn-sm" href="{{ path('admin_app_post_comment_list',  {'id': object.id } ) }}">комментарии</a>
```
 Dashboard
 
создание собственного шаблона dashboard

```
sonata_admin:
    title: Админка
    title_logo: /logo_title.png
    templates:
        dashboard: :default:my_dashboard.html.twig
```

```
{% extends 'SonataAdminBundle:Core:dashboard.html.twig' %}

{% block content %}
    {{ parent() }}

    <div style="width: 100px; height: 100px; background-color: red" ></div>
{% endblock %}
```
