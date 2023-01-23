+++
title = "Installing backstage.io on macOS (for technical writers)"
date = "2023-01-23"
description = "A tutorial on how to install Backstage on macOS and start writing technical docs"
author = "Chris Gardiner-Bill"
tags  = ["tutorial", "technical writing", "Backstage", "MkDocs"]
+++


A job I recently applied to requires a candidate with experience writing docs for Backstage, Spotify's open-source platform for creating developer portals. Backstage's core TechDocs plugin uses MkDocs for content and YAML for configuration. While I'm very comfortable with MkDocs and YAML, my only experience with Backstage was watching introductory YouTube videos after a former colleague suggested we could use it at Specialized.

Naturally, I was intrigued, particularly as Backstage uses a documentation tool (MkDocs) I already use daily. However, being made redundant at Specialized put the kibosh on that ever becoming a possibility.

So, with time on my hands and the pressures of finding a job providing motivation, I installed it on my MacBook Pro, aiming to play with its documentation features and learn how the platform works in general.

While the [Backstage docs]((https://backstage.io/docs/overview/what-is-backstage) are good, they don't include an end-to-end process for installing on macOS, so I decided to write one for future reference, and for anyone else who might find it helpful.

In summary, the steps are:

1. Install Homebrew
2. Install NodeJS with Homebrew
3. Install Postgres with Homebrew
4. Install Backstage with npx
5. Config Backstage to use Postgres

## Homebrew

Homebrew is a utility for installing open-source utilities and libraries on macOS, analogous to the package managers you find in Linux distros, such as apt and pacman. It should be the first thing anyone ever considering developing on a Mac should install!

You can find installation instructions on the [Homebrew homepage](https://brew.sh/), but in summary, open Terminal.app (in Applications/Utilities) and enter the following command:

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

You may be prompted to enter your macOS admin password as part of the installation process. You may also have to install Apple's command line developer tools for Xcode — allow this to proceed if prompted.

A great feature of Homebrew is that it installs packages to `/usr/local` and keeps them separated from the packages installed in macOS by default by Apple, and are usually outdated.

## Node

Backstage.io is NodeJS app, so we'll need a compatible Node version once Homebrew is successfully installed. At the time of writing, Backstage requires Node version 17, but I prefer to stick with Long-Term Support versions, so I installed Node 18.

To install Node 18 using Homebrew, enter the following command into your Terminal and press return.

```bash
$ brew install node@18
```

Once you install Node, create a symlink in your zshrc file to point to the Node executable.

```bash
$ echo 'export PATH="/usr/local/opt/node@18/bin:$PATH"' >> ~/.zshrc
```

Next, refresh your terminal session to use your updated zshrc file.

```bash
$ source ~/.zshrc
```


Test it's working by querying your node installation's version.

```bash
$ node -v
```

If you see a version number displayed, for example, `v18.13.0`, you are ready to install Backstage and its dependencies.

## Postgres

By default, Backstage uses an in-memory database (SQLite) for temporary storage, and the database is erased each time Backstage tops. To persist data between sessions, you'll need to use Postgres.

If you don't already have Postgres installed, you can do so using Homebrew.

```bash
$ brew install postgres@15
```

As with Node, we must reference our Postgres installation in our zshrc file, as follows.

```bash
echo 'export PATH="/usr/local/opt/postgresql@15/bin:$PATH"' >> ~/.zshrc
```

Next we need to initialise the database.

```bash
$ initdb /usr/local/var/postgres
```

Finally, create a default `postgres` user.

```bash
$ /usr/local/Cellar/postgresql/<version>/bin/createuser -s postgres
```

### Running Postgres

Homebrew can manage Postgres sessions. You can start, run and stop your Postgress instance using Homebrew's services command.

The difference between start and run is that start adds the Postgres service to login, meaning it will start every time you reboot your Mac. Since I only want the service to run when I need it, I'll choose the run option.

```bash
$ brew services run postgresql@15
```

If the command was successful, your Terminal displays a message like this:

```bash
==> Successfully ran `postgresql@13` (label: homebrew.mxcl.postgresql@13)
```

You can also see a list of services (started and stopped) using the list command:

```bash
$ brew services list
mongodb-community started christopher ~/Library/LaunchAgents/homebrew.mxcl.mongodb-community.plist
mysql             none                
php               none                
postgresql@15     started             /usr/local/opt/postgresql@15/homebrew.mxcl.postgresql@15.plist
unbound           none  
```

When Postgres is running, you can connect to the data with the default `postgres` user:

```bash
$ psql postgres
```

We'll configure Backstage to use Postgres after installing Backstage.

## Backstage

Backstage is installed using `npx`, which is included with Node.

Navigate to the directory where you want to install Backstage and enter the following command.

"`bash
$ @backstage/create-app@latest
```

During installation, you'll be prompted to name your Backstage app and the directory in which it's contained.

Once the installation is complete, change into the new directory and start the app's development server with yarn.

```bash
$ cd app-name && yarn dev
```

Once the app has finished booting, you should be directed automatically to `http://localhost:3000` in your browser.

![Backstage running locally in Safari](Screenshot%202023-01-23%20at%201.17.11%20pm.png)

### Configuring Backstage

As noted, Backstages uses a volatile, in-memory database that's destroyed when the Backstage service is stopped. We'll need to configure Backstage to use our Postgres instance to save data between sessions.

Stop Backstage and install the Backstage PostgresSQL client:

```bash
$ yarn add --cwd packages/backend pg
```

In a text editor, open `app-config.yaml` file and edit the database entry with your database's details.

I commented out the default values and replaced them with the settings needed to connect to my local Postgres instance.

```yaml
  database:
    # client: better-sqlite3
    # connection: ':memory:'
    client: pg
    connection:
      host: 127.0.0.1
      port: 5432
      user: postgres
      password: postgres
```

Next, it's time to configure GitHub integration, which allows Backstage to create new repositories when we create a new component. The official docs suggest using a personal token, which is fine for a local deployment. You can [create tokens in your account settings](https://github.com/settings/tokens/new), then add to the `app-config.yaml` file.

```yaml
integrations:
  github:
    - host: github.com
      token: ghp_YOURTOKEN
```

Restart Backstage, and you should be able to create a component (project) with the repo automatically added to your GitHub account.

### Adding project documentation

Once you've created a project, you can add documentation with a few simple steps.

Firstly, edit your project's `catalog-info.yaml` to reference the TechDocs plugin under the annotations segment. This annotation tells Backstage to download the needed files to generate your documentation.

```yaml
apiVersion: backstage.io/v1alpha1
kind: Component
metadata:
  name: "backstage-test"
  annotations:
    backstage.io/techdocs-ref: dir:. # <-- add this line
spec:
  type: service
  owner: user:guest
  lifecycle: experimental
```

In the root of your repo, add an [MkDocs configuration file](https://www.mkdocs.org/user-guide/configuration/). Minimally, you must include a title and at least one navigation entry for your index file. The reference to the techdocs-core plugin is optional, but Backstage will automatically add it.

```yaml
site_name: 'example-docs'

nav:
  - Home: index.md

plugins:
  - techdocs-core

```

Create a `docs` directory in your repo and add an `index.md` file containing some basic markdown.

```md
# example docs

This is a basic example of documentation.

```

Your repo's directory structure should look something like this:

```md
- docs/
	index.md
catalogue-info.yaml
index.js
mkdocs.yml
package.json
```

Commit your changes, and visit your project's docs URL.

![Backstage docs running locally on Safari](Screenshot%202023-01-23%20at%204.23.43%20pm.png)

### Troubleshooting

If your docs don't render initially, Backstage's TechDocs plugin is probably set to use Docker in your `app-config.yaml`, and you might not have it installed on your Mac. You can install Docker, or set Backstage to run the docs off a local MkDocs instance.

I opted to run MkDocs locally as I already had it installed, and Docker feels clunky on macOS, at least in my experience.

```yaml
techdocs:
  builder: 'local'
  publisher:
    type: 'local'
  generator:
    runIn: local
```

Note, for this to work, you'll also need to install MkDocs and the mkdocs-techdocs-core plugin locally on your Mac using pip3, the Python package manager.

```bash
$ pip3 install mkdocs-techdocs-core==1.0.1
```

Once installed, restart Backstage and revisit your project's docs URL.

## Concluding thoughts

Backstage has many moving parts, but it's a powerful tool for developers and technical writers. Running it locally on your Mac allows you to experiment and learn without spinning up a VPS.

If you get stuck, need help or want to talk tech writing with me, don't hesitate to reach out on [LinkedIn](https://www.linkedin.com/in/chris-gardiner-bill/).