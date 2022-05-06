# TIL
TIL (Today I Learned) is a place to document what I learned through development journey that doesn't deserve a blog post.

## WebAPI:

1. Notification:
This API allows you to enable desktop notification that is visible at the corner of your desktop.
It allows your web app to remind or notify user of necessary information even when they're not on your app.
Using it is quite simple:

* You need to check whether the browser supports desktop notification:
```javascript
if (!("Notification" in window)) {
    alert(message);
  } 
```
* If it is, check if permission has been granted then post a notification
```javascript
if (Notification.permission === "granted") {
    var notification = new Notification(message);
}
```
* If permission hasn't been requested, request for a permission then post a notification
```javascript
if (Notification.permission !== 'denied') {
    Notification.requestPermission(function (permission) {
      if (permission === "granted") {
        var notification = new Notification(message);
      }
    })
```
* If permission is denied, you might not want to bother the user, or just use normal alert message
```javascript
if (Notification.permission === 'denied') {
  alert("permission is denied");
}
```


## Git
* Retrieve a file from a specific revision: 5 should be replaced with how many commits is it from current HEAD revision
```
git checkout master~5 image.png
```

* Disable SSL Verification in case of self-sign ceritificate
```
git config --add --global http.sslVerify false
```

* Checkout a new branch from a tag 
```
git checkout -b newbranch v1.0
```
ref: http://stackoverflow.com/questions/10940981/git-how-to-create-a-new-branch-from-a-tag

* Merge up to a certain commit
```
git merge <commit-id>
```

## IntelliJ stuffs
* Add Jasmine code completion [http://biercoff.com/adding-jasmine-autocomplete-to-intellij-idea/]

## Windows cmd
* Find out which application is using port (required cmd opened with Administrator privilege)
```
netstat -anob
```

## NPM
* NPM behind a proxy, it doesn't take http_proxy and https_proxy environment variables as usual. But it has to be configured as below. Notice that it needs "http://" protocol for both and the dash instead of underscore in "https-proxy":
```
npm config set proxy http://localhost:3128
npm config set https-proxy http://localhost:3128
```

## Vagrant
* Vagrant behind a proxy. Install plugin:
```
vagrant plugin install vagrant-proxyconf
```
Then add this code to vagrantfile:
```
Vagrant.configure("2") do |config|
  if Vagrant.has_plugin?("vagrant-proxyconf")
    config.proxy.http     = "http://192.168.0.2:3128/"
    config.proxy.https    = "http://192.168.0.2:3128/"
    config.proxy.no_proxy = "localhost,127.0.0.1,.example.com"
  end
  # ... other stuff
end
```
Usually you find the `Vagrant.configure("2") do |config|` in your existing vagrantfile and insert the if block beneath.
Reference: http://tmatilai.github.io/vagrant-proxyconf/

## Cntlm
* Unable to start CNTLM after changing config file.
Look into Events Viewer of Windows, this error is reported:
```
cntlm: PID 3740: starting service `cntlm' failed: fork: 11, Resource temporarily unavailable
```
Reason: CNTLM unable to connect to remote proxy in cntlm.ini config file.
After changing to another one, it starts just fine.

## AngularJs
* Minify angular app's javascripts files - 
I found a great tutorial here: https://www.sitepoint.com/5-minutes-to-min-safe-angular-code-with-grunt/. To sum it up:
  1. Use ngAnnotate plugin to make Angular's js files min-safe (it'll automatically enrich and make your controllers/directives/services/etc. use 'array' syntax like this `angular.controllers('myController', ['$scope', '$timeout', function($scope, $timeout){}]);` instead of the usual 'function' way that we frequently use to save typing `angular.controllers('myController', function($scope, $timeout){});`
  2. Concat all js files together
  3. Minify the concatenated 
  
## MySQL
* Unicode characters is not accepted by mysql db version 5.6.35. Error is thrown from java app similar to the below: 
```
java.sql.SQLException: Incorrect string value: '\xE1\xBA\xA7n 1...' for column 'reason' at row 1
	at com.mysql.jdbc.SQLError.createSQLException(SQLError.java:964) ~[mysql-connector-java-5.1.43.jar!/:5.1.43]
```
**Problem**: Table and column in mysql db wasn't configure to handle utf8 characters correctly
**Solution**: Run below sql queries to set table and column to handle utf8 characters:
```
ALTER TABLE table_name CONVERT TO CHARACTER SET utf8 COLLATE utf8_general_ci;

ALTER TABLE table_name DEFAULT CHARACTER SET utf8 COLLATE utf8_general_ci;

ALTER TABLE table_name CHANGE column_name column_name VARCHAR(100) CHARACTER SET utf8 COLLATE utf8_general_ci
```

## Python
Fail to install `numpy` on M1 Mac. Try this: https://github.com/numpy/numpy/issues/17784#issuecomment-729950525

Fail to install `cryptography` on M1 Mac. 
```
brew reinstall openssl@1.1
export LDFLAGS="-L/opt/homebrew/opt/openssl@1.1/lib"                                                                                                   
export CPPFLAGS="-I/opt/homebrew/opt/openssl@1.1/include"                                                                                               
pip install cryptography==3.2.1 --global-option=build_ext --global-option="-L/opt/homebrew/opt/openssl@1.1/lib" --global-option="-I/opt/homebrew/opt/openssl@1.1/include"

```

## Machine Learning
http://cs231n.github.io/

http://cs231n.github.io/convolutional-networks/


## Management

Each successful project needs these 4 roles:
- Single Threaded Owner (STO): Ultimate responsible person 
- Quarter Back: Daily activities manager
- Working Team: People doing the hard work
- Leadership Team: Seniors that provides feedback

Reference: https://naomi.com/people-and-process-d5a72341a5be
