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
2. PhpUnit with code covarage uploaded to codecov
3. Behat tests (for L8, php7.4 and postgres)
4. Swagger documentation as github pages.

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

- `main` brach as main branch :) 
- phpunit tests run by github actions (definitions in this repository) 
- phpunit code coverage pushed to https://codecov.io/gh
- swagger documention push to github pages 
- repository registered to packagist in `escolalms/*` namespace with versioning in tag releases 
- badges: codecov, pipeline status, packagist downloads, packagist latest version, licence 
- all packages should be attached to [main API](https://github.com/EscolaLMS/API) repository as composer dependency
