# Shadis

Shadis is a self-hosting service for video and image uploads from [ShareX](https://getsharex.com/). Its highlight features are:

- An elegant and functional user interface for presenting your files
- Password-protected dashboard for managing your uploads
- Straightforward system for converting videos to GIFs
- Twitter and OpenGraph metadata for instant visibility of the images and videos in URL-Cards of popular messaging programs like Slack, Discord, Skype, etc.

# Getting Started

Shadis is still _in beta_ and will not be released in form of a pre-compiled package, until it reaches a stable stage. That said, it is **already possible** to generate fully-functional production builds.

## Generating a production build

First, download Shadis and its necessary dependencies by following the setup instructions [below](#Setup).
You should then be able to compile the code via `yarn build`. Follow the instructions in the console should an error happen while compiling.

The contents in the `build` directory can then be deployed to a PHP web-server.

## Preparing the Database

> Currently, Shadis is unable to create all the necessary database tables on its own

You can execute the SQL queries shown below in order to create the needed tables manually.
Feel free to change the table names; just keep in mind that you need to assign them in `config.php` later on.

```sql
CREATE TABLE IF NOT EXISTS `shadis`.`shadis_users`(
  `id` INT(11) NOT NULL AUTO_INCREMENT,
  `username` VARCHAR(50) NOT NULL UNIQUE,
  `password` VARCHAR(255) NOT NULL,
  PRIMARY KEY(`id`)
) ENGINE = MyISAM;
```

```sql
CREATE TABLE IF NOT EXISTS `shadis`.`shadis_files`(
  `id` char(8) NOT NULL,
  `token` char(16) NOT NULL,
  `extension` varchar(5) NOT NULL,
  `width` int(255) NOT NULL,
  `height` int(255) NOT NULL,
  `thumb_height` int(255) NOT NULL,
  `timestamp` int(11) NOT NULL,
  `title` varchar(255) NOT NULL,
  `has_gif` tinyint(1) NOT NULL DEFAULT '0',
  PRIMARY KEY(`id`),
  UNIQUE (`id`),
  UNIQUE (`token`)
) ENGINE = MyISAM;
```

```sql
CREATE TABLE IF NOT EXISTS `shadis`.`shadis_file_tasks`(
  `id` char(8) NOT NULL,
  `gif` tinyint(1) NOT NULL DEFAULT '0',
  `thumbnail` tinyint(1) NOT NULL DEFAULT '0',
  PRIMARY KEY(`id`),
  UNIQUE (`id`)
) ENGINE = MyISAM;
```

## Configuring

> Shadis will not function properly without configuring it first.

You can find a sample configuration file inside the subdirectory `protected`. Rename `config.sample.php` to `config.php` and fill in the necessary data in the config file. You will find an explanation of each mandatory data field.

# Development

This project was bootstrapped with [Create React App](https://github.com/facebook/create-react-app) in the front-end and uses PHP for the back-end. You can find the front-end code in `src` and the back-end code in `public`.

> This guide assumes you use `yarn` as the default package manager

## Setup

Note that this repository uses **_submodules which need to be cloned seperately_**. To achieve this, you can add the following argument when cloning:

```sh
git clone --recurse-submodules https://github.com/Pogodaanton/Shadis.git
```

Afterwards, you can install the necessary dependencies

```sh
cd shadis
yarn
```

Make sure to [configure](#Configuring) Shadis and [create the necessary tables](#Preparing-the-Database) in your development database before trying to use [the development build](#yarn-start-or-yarn-serve).

## Available Scripts

In the project directory, you can run:

### `yarn start` or `yarn serve`

Runs the app in development mode and compiles the code into the `dist` folder.<br />
Because webpack-dev-server cannot interpret PHP code, it is required to route a **PHP-(MySQL/MariaDB)-server** to the automatically generated `dist` directory.

The page will reload if you make edits.<br />
You can see lint errors in the console.

### `yarn build`

Builds the app for production into the `build` folder.<br />
It correctly bundles React in production mode and optimizes the build for the best performance.

This build is minified, classnames are simplified and the filenames include hashes for cache prevention.<br />
See the section about [deployment](https://facebook.github.io/create-react-app/docs/deployment) for more information.

## Learn More

The inner workings of the system are described inside the code on the respective places. Here are links to the documentation of awesome third-party products Shadis is using:

- [Create React App](https://facebook.github.io/create-react-app/docs/getting-started)
- [React Library](https://reactjs.org/)
- [Framer Motion](https://www.framer.com/api/motion/)
- [FAST-React](https://github.com/microsoft/fast/tree/master/packages/react)
- [getID3](https://www.getid3.org/)
- [gif.js](https://github.com/jnordberg/gif.js#readme)
- [react-virtualized](https://github.com/bvaughn/react-virtualized#documentation)
