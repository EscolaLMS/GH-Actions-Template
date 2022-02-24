# Template of Escola LMS Packages

Each Escola LMS package should contain

1. Licence MIT
2. Definitions of Tests as Github Actions
3. codecov Tests CodeCoverage
4. Readme file
5. Codeclimate (quality & cc) 
6. GitGuardian
7. Badges 

## Laravel PHP Package

Each Escola LMS Laravel PHP Package should contain

1. PhpUnit tests for L8, mysql, postgres, php7.4 & php8.0. Test are launched with [testbench](https://github.com/orchestral/testbench)
2. PhpUnit with code covarage uploaded to codecov and codeclimate
3. Swagger documentation as github pages.

All this are in [php](php) folder. To enable above just copy files to your package and change settings.

## Swagger generation to Github pages 

Each API package should publish it's own swagger documentation, yet is should not have `@OA\Info(title=\"EscolaLMS\", version=\"0.0.1\")` in base controller because the there can be only one `@OA\Info` globally. 

We have one general repository for [API](https://github.com/EscolaLMS/API) that generates one swagger documentation from all packages. 

To generate swagger for repository please instead of `@OA\Info` add `SWAGGER_VERSION` string then replace it in `yml` Github action, eg 

```yml
      - name: add Swagger main annotaion
        run: php -r "file_put_contents('src/HeadlessH5PServiceProvider.php', str_replace('SWAGGER_VERSION', '@OA\Info(title=\"EscolaLMS\", version=\"0.0.1\")', file_get_contents('src/HeadlessH5PServiceProvider.php')));"
```

### How to use this as package. 

Repeat those steps for each package

1. Clone your package to a folder inside this repo, in `packages` folder (`git clone XXX package`). Add your folder to `.gitignore`

2. Amend Host Laravel `composer.json`

Before:

```json
"repositories": {
}
```

After:

```json
"repositories": {
    "escolalms/headless-h5p": {
        "type": "path",
        "url": "../packages/headless-h5p"
    }
}
```

Example with 2 packages

```json
"repositories": {
    "escolalms/headless-h5p": {
        "type": "path",
        "url": "../packages/headless-h5p"
    },
    "escolalms/courses": {
        "type": "path",
        "url": "../packages/courses"
    }
}
```

Working with packages `versions` 

```json
"repositories": [{
    "type": "path",
    "name": "escolalms/courses",
    "url": "packages/courses",
    "options": {
        "versions": {
            "escolalms/courses": "0.2.20"
        }
    }
}]
```

Note that name (`escolalms/headless-h5p` in example above) **MUST** match one you have in `package/composer.json`

2.1. (Optional)
In case you'll be generating your own swagger documentation you might want to alter `host/config/l5-swagger.php` as follows:

Before:
```
'annotations' => [
               base_path('vendor/escolalms/headless-h5p/src'),
               //base_path('app')
],
```
After:
```
'annotations' => [
               base_path('vendor/escolalms/mypackage/src'),
               //base_path('app')
],
```
Above example is assuming that your package name is `mypackage`.


3. Enter bash (instruction above), then add you packge with `composer require escolalms/headless-h5p`

That's it - now you have laravel working with docker that is using package from other folder that is `git` maintained.

## Xdebug remote debbuging

_Note_ instruction below is for VSCODE, yet it should work with minor changes in other IDEs like PHPStorm.

1. Add mapping to `.vscode/launch.json`

Before:

```json
    "configurations": [
        {
            "name": "Listen for Xdebug",
            "type": "php",
            "request": "launch",
            "port": 9003,
            "pathMappings": {
                "/var/www/html": "${workspaceFolder}/host",
            }
        },
```

After:

```json
    "configurations": [
        {
            "name": "Listen for Xdebug",
            "type": "php",
            "request": "launch",
            "port": 9003,
            "pathMappings": {
                "/var/www/html": "${workspaceFolder}/host",
                "/var/www/package": "${workspaceFolder}/package"
            }
        },
```

## React components package

TODO

## Admin panel

TODO

## API Laravel packages

- `main` branch as main branch :) do not use `develop` branch 
- `feature/X` as feature branch `X` is number of issue in Gh Issues of repository
- after MVP, use `release/X` branch 
- phpunit tests run by github actions (definitions in this repository) 
- phpunit code coverage pushed to https://codecov.io/gh
- swagger documention push to github pages 
- repository registered to packagist in `escolalms/*` namespace with versioning in tag releases 
- badges: codecov, pipeline status, packagist downloads, packagist latest version, licence 
- all packages should be attached to [main API](https://github.com/EscolaLMS/API) repository as composer dependency
- codeclimate for Static analys

Classes (and files) that should be inside the package 

- service provider 
- policy with auth provider 
- seeder for permissions for [default roles](https://github.com/EscolaLMS/Core/blob/main/src/Enums/UserRole.php) 
- seeder for default content 
- tests 
- swagger annotations 
- readme.md with implementation details 


### Badges example 

```markdown
[![swagger](https://img.shields.io/badge/documentation-swagger-green)](https://escolalms.github.io/Auth/)
[![codecov](https://codecov.io/gh/EscolaLMS/Auth/branch/main/graph/badge.svg?token=O91FHNKI6R)](https://codecov.io/gh/EscolaLMS/Auth)
[![phpunit](https://github.com/EscolaLMS/Auth/actions/workflows/test.yml/badge.svg)](https://github.com/EscolaLMS/Core/actions/workflows/test.yml)
[![downloads](https://img.shields.io/packagist/dt/escolalms/auth)](https://packagist.org/packages/escolalms/auth)
[![downloads](https://img.shields.io/packagist/v/escolalms/auth)](https://packagist.org/packages/escolalms/auth)
[![downloads](https://img.shields.io/packagist/l/escolalms/auth)](https://packagist.org/packages/escolalms/auth)
```

### Authorisation (with Testing) 

TODO 

## Permissions naming pattern

Basic pattern:
```
[package-name?]_[model-name]_[action-name]_[modifier-name?]
```

Sections are merged with `_`

Multi-part names are merged with `-`


---


### PackageName 

`package-name` is the name of the repository to which the permission applies. 

example:
for `EscolaLMS/Cart` -> `cart`


#### When we omit the `package-name`:

##### 1. When the package name is named the same as the model 

e.g. `EscolaLMS/Categories`.

According to the basic pattern it would look like this: 

```
categories_category_create // wrong
```

This does not make sense, so we omit the package name and create 

```
category_create // good
```


##### 2. To not bind the permission too much to the repository
This gives a gateway to split the repository.

e.g. 
User management is currently in the `EscolaLMS/Auth` package.
According to the basic pattern, the permission to create a user would look like this: 

```
auth_user_create // wrong
``` 

In this case, it is worth omitting `auth` and eventually doing 

```
user_create // good
```

---


### ModelName

`model-name` the name of the model/element that the action applies to. Always specify the model in the singular:

```
user-groups_create // wrong
user-groups_list // wrong
```

```
user-group_create // good
user-group_list // good
```

---

### ActionName

`action-name` is the name of the action we want to allow.


#### Standard action names:

`create` - create model (do not use `add` in this case)

`read` - read a model (do not use `attend`, `display`, `show` in this case)

`update` - edit the model (do not use `edit` in this case)

`delete` - deleting a model (do not use `remove` in this case)

`list` - read the list of the given model

`manage` - when a general model management permission is needed that combines `create`, `read`, `update` and `delete`


#### Actions on a model in a model:

In some cases we need permission to create relationships between models. 

We need to determine which model is the master model, and we put the sub-model into an action. Sub-model in the action is given as the first one, the proper action as the second one., e.g:

```
user-group_member-add // good
```

```
user-group_add-member // wrong
```

---


### ModifierName

`modifier-name` is a modifier for the action. It is used when the action is used in a non-standard way.

Example:
`user_update` vs `user_update_self`
Both permissions allow you to edit a user, but in the second case you can only edit your own user (not any user).


#### Model in modifier:

Sometimes we need a permission for an action that depends on another model. 

Example: 
We are the author of the course. We need a list of users who have joined it. We don't have permission to view all users (standard `user_list` is out).

You can do it like this for example:
```
user_list_course-owned
```
The subModel in the modifier is given first, the proper modifier second.

**If you need more than one modifier in a given permission, you should consider whether the permission you are creating should be split.**

#### Avoid modifiers whenever possible:
```
user_list_any // wrong
user_list // good
```

#### Standard modifiers:

`self` - for user-related actions (e.g. `user_update_self`, `user-group_update_self`)


---

### Permissions for views in admin panel:

**PackageName** - must be `dashboard-app`

**ModelName** - this is the name of the section or whole subpage

**ActionName** - usually it will be simply `access`

Example:
```
dashboad-app_course-list_access
```

---


### Permissions for views in the client (front) application:

**PackageName** - must be `client-app`

**ModelName** - is the name of the section or the whole subpage

**ActionName** - usually it will be simply `access`

Example:
```
client-app_course-list_access
```
