# Herokuへの自動デプロイ

```yml
version: 2.1

executors:
  laravel-circleci:
    docker:
      - image: circleci/php:7.4-node-browsers
      - image: circleci/mysql:5.7
    environment:
      - APP_DEBUG: true
      - APP_ENV: testing
      - APP_KEY: base64:Q0cJelvqonwSbdZ591P40cyMQ13+AfHj+1jYy4cALfM=
      - DB_CONNECTION: circle_testing
      - MYSQL_ALLOW_EMPTY_PASSWORD: true
    working_directory: ~/repo

commands:
  install-dockerize:
    steps:
      - run:
          name: Install dockerize
          command: wget https://github.com/jwilder/dockerize/releases/download/$DOCKERIZE_VERSION/dockerize-linux-amd64-$DOCKERIZE_VERSION.tar.gz && sudo tar -C /usr/local/bin -xzvf dockerize-linux-amd64-$DOCKERIZE_VERSION.tar.gz && rm dockerize-linux-amd64-$DOCKERIZE_VERSION.tar.gz
          environment:
            DOCKERIZE_VERSION: v0.6.1
  install-php-extensions:
    steps:
      - run:
          name: Install PHP Exetensions
          command: sudo docker-php-ext-install pdo_mysql
  restore-cache-composer:
    steps:
      - restore_cache:
          key: v1-dependencies-{{ checksum "composer.json" }}
  install-composer:
    steps:
      - run:
          name: Install Composer
          command: composer install -n --prefer-dist
  save-cache-composer:
    steps:
      - save_cache:
          key: v1-dependencies-{{ checksum "composer.json" }}
          paths:
            - vendor
  npm-ci:
    steps:
      - run:
          name: npm CI
          command: |
            if [ ! -d node_modules ]; then
              npm ci
            fi
  restore-cache-npm:
    steps:
      - restore_cache:
          key: npm-cache-{{ checksum "package-lock.json" }}
  npm-run-dev:
    steps:
      - run:
          name: Run npm
          command: npm run dev
  save-cache-npm:
    steps:
      - save_cache:
          key: npm-cache-{{ checksum "package-lock.json" }}
          paths:
            - node_modules
  migration-seeding:
    steps:
      - run:
          name: Migration & Seeding
          command: php artisan migrate --seed
  test-unittest:
    steps:
      - run:
          name: Run PHPUnit
          command: ./vendor/bin/phpunit

jobs:
  build:
    executor:
      name: laravel-circleci
    steps:
      - checkout
      - install-dockerize
      - install-php-extensions
      - restore-cache-composer
      - install-composer
      - save-cache-composer
      - restore-cache-npm
      - npm-ci
      - save-cache-npm
      - npm-run-dev
      - migration-seeding
      - test-unittest
  deploy:
    docker:
      - image: circleci/php:7.4-node-browsers
    steps:
      - checkout
      - run:
          name: heroku deploy
          command: |
            git push https://heroku:$HEROKU_API_KEY@git.heroku.com/$HEROKU_APP_NAME.git master

workflows:
  version: 2
  build_deploy:
    jobs:
      - build
      - deploy:
          requires:
            - build
          filters:
            branches:
              only:
                - master
```

- command名に&は使えない（migration&seedingにしたらエラーになった）