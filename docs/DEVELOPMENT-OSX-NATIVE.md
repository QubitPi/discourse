Developing Under OS X
=====================

Installing Dependencies
-----------------------

https://github.com/discourse/install-rails/blob/main/mac

```console
brew tap --repair
brew update
brew tap homebrew/services
brew install imagemagick advancecomp jhead jpegoptim jpeg optipng oxipng pngcrush pngquant coreutils
brew install openssl@1.1
brew link openssl@1.1 --force
```

### Redis

```console
brew install redis
brew services start redis
```

### Postgres

```console
brew install postgresql@16
brew services start postgresql@16
```

> [!TIP]
> We can connect PostgreSQL command line using
>
> ```console
> psql -d postgres -U postgres
> ```
>
> Section [PostgreSQL Useful Commands](#postgresql-useful-commands) listed reference queries that might be useful
> for devloping Discourse
>
> PostgreSQL installed in the way above can be stopped with
>
> ```console
> brew services stop postgresql@16
> ```

### Make Sure OpenSSL 1.1 is in Use

Most modern Macs have OpenSSL 3.* installed which is not working with ruby for the moment. It requires sharpe 1.1
version. In this case, we would point ruby to 1.1 by

```console
brew unlink openssl@3
brew link openssl@1.1
```

In `.bash_profile`, change

```bash
export PATH="/usr/local/opt/openssl@3/bin:$PATH"
```

to

```bash
export PATH="/usr/local/opt/openssl@1.1/bin:$PATH"
```

Then activate the change:

```console
source ~/.bash_profile
```

### Bootstrap Discourse

```console
git clone git@github.com:QubitPi/discourse.git
cd discourse
rvm install 3.2.1
gem update --system
gem install bundler rails mailhog
bundle install
nvm use 21 # if using nvm to manage node version
yarn
```

### Initializing Database

```console
bundle exec rake db:create
bundle exec rake db:migrate ***
```

PostgreSQL Useful Commands
--------------------------

List databases:

```sql
\list
```

List tables by modified date:

```sql
SELECT (pg_stat_file('base/'||oid ||'/PG_VERSION')).modification, datname FROM pg_database;
```

```console
postgres=# SELECT (pg_stat_file('base/'||oid ||'/PG_VERSION')).modification, datname FROM pg_database;
      modification      |   datname    
------------------------+--------------
 2023-12-20 16:14:56+08 | postgres
 2023-12-20 16:18:34+08 | users
 2023-12-20 16:14:55+08 | apps
 2023-12-20 16:14:56+08 | addresses
(4 rows)
```
