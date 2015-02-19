### Issue

When the Meteor application is running it prevents changes to .meteor/versions,
either from running `meteor update` or from manual editing. The updated version
of the package is downloaded to the ~/.meteor/packages cache.

### This repo

This repository was built using the latest version of Meteor (1.0.3.1 at time
of writing), on Ubuntu 14.04. The steps taken to verify the issue is present
for me are as follows:

- `$ meteor create updateIssue`
- `$ cd updateIssue`
- `updateIssue$ meteor add smeevil:syntax-error-notifier`
- `updateIssue$ vi .meteor/versions`
  - Modify version for `smeevil:syntax-error-notifier` to `1.0.2` (a known previous version)
- `updateIssue$ meteor list`

        autopublish                    1.0.2  Publish the entire database to all clients
        insecure                       1.0.2  Allow all database writes by default
        meteor-platform                1.2.1  Include a standard set of Meteor packages in your app
        smeevil:syntax-error-notifier  1.0.2* Display a warning modal if your app crashes due to a syntax error, auto reloads on fix

                                              
        * New versions of these packages are available! Run 'meteor update' to try to update those packages to their latest versions. If your
          packages cannot be updated further, try typing `meteor add <package>@<newVersion>` to see more information.

- `updateIssue$ meteor`

In a different terminal:

- `updateIssue$ meteor update`

This reports

    This project is already at Meteor 1.0.3.1, the latest release.
    
    Changes to your project's package version selections from updating package
    
    smeevil:syntax-error-notifier  upgraded from 1.0.2 to 1.0.3

and the running meteor application detects the change, reports it is 
updating packages, and then `=> Meteor server restarted`.

Running  `updateIssue$ cat .meteor/versions | grep smeevil` results in:

     smeevil:syntax-error-notifier@1.0.2


### Addendum

Just to confirm this isn't an issue with the package I was adding, I also ran the 
same steps using `iron:router`, and adjusted all the `iron:*`' packages to `1.0.6`:

    updateIssue$ meteor list
    autopublish                    1.0.2  Publish the entire database to all clients
    insecure                       1.0.2  Allow all database writes by default
    iron:router                    1.0.6* Routing specifically designed for Meteor
    meteor-platform                1.2.1  Include a standard set of Meteor packages in your app
    smeevil:syntax-error-notifier  1.0.2* Display a warning modal if your app crashes due to a syntax error, auto reloads on fix

                                                  
    * New versions of these packages are available! Run 'meteor update' to try to update those packages to their latest versions. If your
      packages cannot be updated further, try typing `meteor add <package>@<newVersion>` to see more information.

    updateIssue$ meteor update
    This project is already at Meteor 1.0.3.1, the latest release.
                                                  
    Changes to your project's package version selections from updating package versions:
                                                  
    iron:controller                upgraded from 1.0.6 to 1.0.7
    iron:core                      upgraded from 1.0.6 to 1.0.7
    iron:dynamic-template          upgraded from 1.0.6 to 1.0.7
    iron:layout                    upgraded from 1.0.6 to 1.0.7
    iron:location                  upgraded from 1.0.6 to 1.0.7
    iron:middleware-stack          upgraded from 1.0.6 to 1.0.7
    iron:router                    upgraded from 1.0.6 to 1.0.7
    iron:url                       upgraded from 1.0.6 to 1.0.7
    smeevil:syntax-error-notifier  upgraded from 1.0.2 to 1.0.3

    updateIssue$ meteor list
    autopublish                    1.0.2  Publish the entire database to all clients
    insecure                       1.0.2  Allow all database writes by default
    iron:router                    1.0.6* Routing specifically designed for Meteor
    meteor-platform                1.2.1  Include a standard set of Meteor packages in your app
    smeevil:syntax-error-notifier  1.0.2* Display a warning modal if your app crashes due to a syntax error, auto reloads on fix

                                                  
    * New versions of these packages are available! Run 'meteor update' to try to update those packages to their latest versions. If your
      packages cannot be updated further, try typing `meteor add <package>@<newVersion>` to see more information.
    
    updateIssue$ cat .meteor/versions | grep 'iron\|smeevil'
    iron:controller@1.0.6
    iron:core@1.0.6
    iron:dynamic-template@1.0.6
    iron:layout@1.0.6
    iron:location@1.0.6
    iron:middleware-stack@1.0.6
    iron:router@1.0.6
    iron:url@1.0.6
    smeevil:syntax-error-notifier@1.0.2


