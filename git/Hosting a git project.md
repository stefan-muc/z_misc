# Hosting a git project

<sub><sup>(hosting *git* for the poor or privacy concerned ones)</sup></sub>

Sometimes you want to host a private or semi-public project, but don't want to give your data to someone else (and most of the time having to pay for a private project). As it happens, you already have a webspace with SSH and HTTP(S) access - so why not use that?

## Prerequesites

* basic *git* knowledge (you already cloned a repository, commited and pushed some changes to a server)
* server with (private) SSH and public HTTP(S) access
* server *git* install (I suppose - an old 1.9.1 *git* worked for me, couldn't test it without)
* local *git* install (I used version 2.23.0)
* local project/repository ``my-git-project`` including branch ``master`` you want to publish

## Workflow

### prepare local project and clone into a bare project

* ``cd`` into a temp folder: ``cd /tmp/my-workfolder``
* clone your project: ``git clone --bare /opt/git/my-git-project my-git-project.git`` 
* ``cd`` into your bare project: ``cd /tmp/my-workfolder/my-git-project.git/``
* allow read access to repository over HTTP(S) by enabling the hook that writes ``info/refs`` file that's necessary for *Dumb HTTP* access to a project: ``mv hooks/post-update.sample hooks/post-update``

### upload bare project to HTTP(S) webserver

* upload directory ``/tmp/my-workfolder/my-git-project.git/`` to a server using a path fitting your needs, e.g. ``https://example.net/my-git-project.git``
* make ``post-update`` hook executable on server: ``chmod a+x my-git-project.git/hooks/post-update``

### set up SSH remote for your local project

* ``cd`` into your local project: ``cd /opt/git/my-git-project``
* add server as remote: ``git remote add example-net ssh://user@example.net/www/htdocs/my-git-project.git``

  You will have to add absolute path to ``my-git-project.git`` here as it appears when you ``ssh`` into your server.

* set ``master`` branch as tracking: ``git push --set-upstream example-net master``

*git* will now ask for your *ssh* credentials and configure local ``master`` branch tracking the ``master`` branch on ``example-net``

You currently don't have any changes in your project, so ``post-update`` hook never got executed - and thus, the repository can't be cloned. To do this and test for successful execution of this hook on push, just commit a change and push it to ``example-net`` as you are used to:

    echo "Hello World!" > test.txt
    git add test.txt
    git checkin -m "My commit for testing post-update hook"
    git push example-net

### clone project into clean directory (testing)

* ``git clone https://example.net/my-git-project.git``

This clone of git project only has read access to this project.

## Troubleshooting

If *git* responds with *``fatal: repository 'https://example.net/my-git-project.git' not found``*, most probably the ``post-update`` hook didn't get executed during push of our change. Symptoms: file ``info/refs`` doesn't exist

Use a webbrowser to fix path problems, you can point your browser to ``https://example.net/my-git-project.git/description`` to see if file access works out properly

## Further improvements

Currently our public project it isn't password protected, but you can do that with standard ``.htaccess``/``.htpasswd`` workflow as you would do with nomal HTTP(S) access to a webserver - or deny access completely, so you have a private one.

SSH access should be secured by Public Key Authentication if your webhoster supports it, this will add more security. See e.g. [How to Use SSH Public Key Authentication](https://serverpilot.io/docs/how-to-use-ssh-public-key-authentication) - might be different with your hoster, mine offers to configure that using a webinterface

## Further reading

* Pro Git: [4.1 Git on the Server - Dumb HTTP](https://git-scm.com/book/en/v2/Git-on-the-Server-The-Protocols#_dumb_http)

