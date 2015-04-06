#PostgreSQL#

##Installing through homebrew##

`brew install postgresql`

Follow instructions which should include:
`ln -sfv /usr/local/opt/postgresql/*.plist ~/Library/LaunchAgents`
`launchctl load ~/Library/LaunchAgents/homebrew.mxcl.postgresql.plist`

Running psql should give an error `database "your_computer_username" does not exist`

Now create a database so you can log in without specifying one:
`psql postgres`
You should now see the `postgres=#` prompt
Type in `create database "your_user_name";`
You should see `CREATE DATABASE`
`\q` to exit

##Commands##
