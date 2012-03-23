Etherpad on OpenShift Express
==============================

This git repository helps you get up and running quickly w/ a Etherpad installation
on OpenShift Express.  The backend database is MySQL and the database name is the 
same as your application name (using $_ENV['OPENSHIFT_APP_NAME']).  You can name
your application whatever you want.  However, the name of the database will always
match the application so you might have to update .openshift/action_hooks/build.


Running on OpenShift
----------------------------

Create an account at http://openshift.redhat.com/

Create a nodejs-0.6 application (you can call your application whatever you want)

    rhc app create -a etherpad -t nodejs-0.6

Add MySQL support to your application

    rhc app cartridge add -a etherpad -c mongodb-2.0

Add this upstream Etherpad repo

    cd etherpad
    git remote add upstream -m master git://github.com/wshearn/etherpad-example.git
    git pull -s recursive -X theirs upstream master
    # note that the git pull above can be used later to pull updates to Etherpad
    # the rm and ln is there until I can figure out github and symbolic links 
Then push the repo upstream

    git push

That's it, you can now checkout your application at (default admin account is admin/OpenShiftAdmin):

    http://etherpad-$yournamespace.rhcloud.com


NOTES:

GIT_ROOT/.openshift/action_hooks/deploy:
    This script is executed with every 'git push'.  Feel free to modify this script
    to learn how to use it to your advantage.  By default, this script will create
    the database tables that this example uses.

git push warnings:
    You can safely ignore these. However if they really annoy you, you can edit
    settings.json and replace the database settings with your own. Then edit 
    .openshift/action_hooks/deploy and remove the sed statements.
