# gitosaur

**gitosaur** is a light wrapper around the wonderful static git site generator [stagit](https://www.codemadness.org/stagit.html). **gitosaur** will handle setting up hooks in your git repos to automatically regenerate the static pages on push.

## INSTALLATION

### Dependencies

**gitosaur** depends on stagit, you can clone the repo with:
```
$ git clone git://git.codemadness.org/stagit
```

Follow build/installation instruction in the stagit readme.

### Webserver configuration

Next, we need to configure our webserver to use git's [smart http](https://git-scm.com/book/en/v2/Git-on-the-Server-Smart-HTTP) protocol.
Furthermore, we also want to have the git clone url to be the same as the one to view the html page. Using nginx, this can be accomplished
like so (you will also need to install fcgiwrap):
```
location ~ (/.*\.git/(HEAD|info/refs|objects/info/.*|git-(upload|recieve)-pack)) {
    fastcgi_pass unix:/var/run/fcgiwrap.socket;
    include fastcgi_params;
    fastcgi_param SCRIPT_FILENAME     /usr/lib/git-core/git-http-backend;
    fastcgi_param GIT_HTTP_EXPORT_ALL "";
    fastcgi_param GIT_PROJECT_ROOT    {{ LOCATION_OF_YOUR_GIT_REPOS }};
    fastcgi_param PATH_INFO           $1; 
}
```
