# Template of Escola LMS Packages

Each Escola LMS package should contain

1. Licence Apache-2.0
2. Definitions of Tests as Github Actions
3. Readme file
4. Codeclimate
5. GitGuardian
6. Badges 

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

### Permissions naming pattern

TODO 
