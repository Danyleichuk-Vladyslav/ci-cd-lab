# Лабораторна 1. CI/CD для PHP

Навчальний проєкт `student/ci-cd-lab`: клас `Calculator`, тести PHPUnit і перевірка стилю Laravel Pint. Безперервна інтеграція описана в GitHub Actions.

PHP локально не обов’язковий. Пайплайн розрахований на PHP 8.2 у GitHub Actions.

![PHP CI](https://github.com/Danyleichuk-Vladyslav/ci-cd-lab/actions/workflows/php-ci.yml/badge.svg)

## Структура

```
├── .github/PULL_REQUEST_TEMPLATE.md
├── .github/workflows/php-ci.yml
├── app/Services/Calculator.php
├── tests/Unit/CalculatorTest.php
├── .gitignore
├── README.md
└── composer.json
```

`Calculator` додає і віднімає два числа. Стиль перевіряє Pint, самі операції — PHPUnit.

## Локальний запуск

```bash
composer install
vendor/bin/phpunit tests
vendor/bin/pint --test
vendor/bin/pint
```

Остання команда сама розставляє пробіли. Після неї `pint --test` має стати зеленим. Каталог `vendor/` у git не входить.

## Що робить CI

Файл `.github/workflows/php-ci.yml` запускається на `push` і `pull_request`:

1. PHP 8.2
2. `composer install`
3. `pint --test`
4. `phpunit`
5. крок **Demonstrate GitHub Secrets**

Секрет читається лише зі сховища GitHub (`secrets.MY_SUPER_SECRET_KEY`). У логах значення маскується. Якщо секрет не задано, крок завершується з помилкою. У код і в коміти його не кладуть.

## GitHub

Pull request відкривається за шаблоном з `.github/PULL_REQUEST_TEMPLATE.md`. Секрет `MY_SUPER_SECRET_KEY` лежить в Actions Secrets, не в репозиторії. Для `main` діє branch protection. Виправлення стилю Pint йде в гілці `feature-cicd-pipeline`. Задача закривається рядком `Fixes #N` або `Resolves #N`.

Задачі:

### 1. Каркас репозиторію

Як студент, я хочу репозиторій із каркасом проєкту і шаблоном PR, щоб першу зміну можна було переглянути.

**DoD:** репозиторій створено, PR `initialize-repository` відкрито за шаблоном, призначено двох рев’юверів.

### 2. Пайплайн і секрет

Як студент, я хочу, щоб GitHub Actions запускав Pint і PHPUnit і читав `MY_SUPER_SECRET_KEY` зі Secrets, щоб секрет не лежав у git.

**DoD:** workflow є на гілці, секрет задано в GitHub, крок Demonstrate GitHub Secrets видно в логу, значення в логу замасковане.

### 3. Зелений CI і merge

Як студент, я хочу виправлений стиль і зелені тести, щоб гілку `feature-cicd-pipeline` можна було змерджити під branch protection.

**DoD:** `pint --test` і PHPUnit зелені, бейдж CI стоїть у README, три Issues розкладені на Kanban, у PR є `Fixes #N` або `Resolves #N`, merge пройшов через pull request.
