# Al Rahma Hospital — Hospital Management System

On-premise hospital management system for **Al Rahma Hospital**, Qardho Main Road, Qardho, Somalia.
Laravel 13 · PHP 8.4 · MariaDB 11.4 LTS · Blade + Bootstrap 5 + Alpine.js.

> **Build status.** Phases 0, 3 and 4 are complete: database, authentication,
> role-based access, sector administration and System Owner accounts.
> Patients, appointments, clinical, pharmacy, laboratory, billing and reporting
> are **not yet built**. This system is ready for a demonstration server, not
> yet for live hospital operations. See `AL_RAHMA_HMS_BLUEPRINT_v1.md`.

---

## What this repository contains

This is the application source — `app/`, `database/`, `resources/`, `routes/`,
`config/`, `tests/` — not a full Laravel skeleton. It is installed **into** a
fresh Laravel 13 project, which keeps framework upgrades straightforward.

## Requirements

| | Version | Note |
|---|---|---|
| PHP | 8.4 (minimum 8.3) | with `mbstring`, `intl`, `pdo_mysql`, `gd`, `zip`, `bcmath`, `fileinfo` |
| Composer | 2.x | |
| MariaDB | 11.4 LTS | or MySQL 8.4 LTS. **InnoDB only** |
| Node.js | 20+ | build machine only — the production server does not need it |

## Install

```bash
# 1. Create a fresh Laravel 13 project
composer create-project laravel/laravel al-rahma-hms "13.*"
cd al-rahma-hms

# 2. Copy this repository's folders over it
#    app/ bootstrap/ config/ database/ docs/ resources/ routes/ tests/
#    plus README.md, PRODUCT.md, DESIGN.md

# 3. Install the packages this system depends on
composer require spatie/laravel-permission barryvdh/laravel-dompdf spatie/laravel-backup
composer require --dev pestphp/pest pestphp/pest-plugin-laravel
php artisan vendor:publish --provider="Spatie\Permission\PermissionServiceProvider"

# 4. Configure
cp .env.example .env
php artisan key:generate
#    then edit .env: DB_DATABASE, DB_USERNAME, DB_PASSWORD

# 5. Create the database (utf8mb4, InnoDB)
mysql -u root -p -e "CREATE DATABASE al_rahma_hms CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;"

# 6. Build the schema, seed reference data and install the branding
php artisan migrate
php artisan db:seed
php artisan storage:link

# 7. Front-end assets
npm install && npm install alpinejs
npm run build        # use `npm run dev` while developing

# 8. Run it
php artisan serve
```

### First sign-in

`php artisan db:seed` creates the **System Owner** account and prints its
username and a random password **once**, to the console. Write it down. There
is no default password anywhere in this source.

A new password must be chosen at first sign-in. If the owner password is ever
lost:

```bash
php artisan hms:reset-owner-password
```

This needs shell access to the server — in practice, physical access to the
machine in the office. That is deliberate.

## Running the tests

```bash
php artisan test
```

Tests run against **MySQL/MariaDB**, not SQLite: the schema uses generated
columns and database-level constraints (the double-booking and bed-occupancy
guarantees) that SQLite cannot express, and testing without them would test a
different system than the one that ships. Create a separate
`al_rahma_hms_testing` database and point `.env.testing` at it.

## Everyday commands

```bash
php artisan migrate:fresh --seed   # rebuild from scratch (development only)
php artisan db:seed --class=SettingSeeder    # pick up newly added settings
php artisan hms:reset-owner-password
php artisan backup:run
php artisan optimize               # production: cache config, routes, views
php artisan optimize:clear
```

## Documentation

| File | Covers |
|---|---|
| `AL_RAHMA_HMS_BLUEPRINT_v1.md` | Requirements, module map, permission matrix, workflows, ERD, roadmap, risks |
| `docs/deployment.md` | Putting it on a server in the hospital |
| `docs/backup-restore.md` | Backups, and how to prove one actually works |
| `docs/administration.md` | Sectors, users and the System Owner |
| `docs/branding.md` | The logo, its variants and the palette |
| `PRODUCT.md` / `DESIGN.md` | Product context and interface direction |

## Licence and data

Contains no real patient data. Demo seeders are blocked in production.
This software follows general privacy and security good practice; **no claim of
compliance with any specific national healthcare or data-protection law** is
made. See blueprint §1.9.
