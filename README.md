

# Laravel Vue Starter

Components:

- Vue 3 / Vuex / VueRouter
- Vue 3 Composition API
- Vite 3
- Laravel Framework
- Laravel Sanctum
- Laravel Fortify
- Tailwind
- ForkAwesome

## ⚡️ How to install

1. `git clone`
2. `cd rv-laravel-vue-starter`
3. `composer install`
4. `cp .env.example .env`
5. `php artisan key:generate`   
6. `npm install`
7. `npm run watch` (or if production `npm run build`)

## ⚡️ How it works

### ➡️ Authentication

The project ships with complete authentication boilerplate and includes the following pages:
- Login
- Register
- Forget Password
- Reset Password

Also, the project comes with complete `users` crud that can be taken as an example for building other modals.

### ➡️ Structure

The front-end code is located in `resources/js`. The code is organized in different directories to make things more readable.

| Directory | Description                             |
|-----------|-----------------------------------------|
| views     | The home of the views                   |
| + pages   | The home of the pages                   |
| + icons   | The home of the icons                   |
| + layouts | The home of the global layouts          |
| + utils   | The home of the other utilities         | 
| modules   | The home of the reusable modules        |
| plugins   | The home of the plugins configuration   |
| router    | The home of the router configuration    |
| services  | The home of the API services            |
| store     | The home of the Vuex store              |
| functions | The home of the various other functions |

### ➡️ Examples

The project ships with two examples as follows:

- views/pages/private/dashboard - Shows paginated list of messages with a form for sending a message.
- views/pages/private/users - Shows paginated list of system users along with crud screens.

You will probably remove the examples once you start developing your app on top of this project.

