# My website [davidxiao.me](http://davidxiao.me/)

The whole site is generated by [Hugo](http://gohugo.io/) and hosted on [Firebase](https://firebase.google.com/). It is intended to be fully static, utilizing Google Firebase free tier storage and stay as much lightweight as possible.

## Step 1: Setting Up a Test Environment

Before deploying to Firebase, we need to have a running website on local.

- Install hugo on your local environment.
- Fetch this repo.

        git clone git@github.com:davxiao/davidxiao-web.git;

- (optional) Set the correct github username and email for commits in the future.
  
        git config --global user.name "David Xiao";
        git config --global user.email "3964699+davxiao@users.noreply.github.com";

- Update submodules and do some cleanup.

        cd davidxiao-web;
        rm -rf public; # otherwise the following command will fail
        git submodule update --init --recursive;

- Test it by run `hugo server -D;` on the project root directory, then visit [http://127.0.0.1:1313/](http://127.0.0.1:1313/) on your web browser. If you want to make the website accessible within your internal network, run `hugo server -D --bind=0.0.0.0;` instead.

As long as `hugo` is running, local test environment is up and running.

## Step 2: Deploying to Google Firebase

1. Install Firebase CLI (required for site deployment) and Google Cloud SDK CLI (optional) on your local environment.

   - Firebase CLI. The recommended way is to run `npm i -g firebase-tools;` See its [github repo](https://github.com/firebase/firebase-tools) for detail

   - Google Cloud SDK CLI. If you are using macOS and have [homebrew](https://brew.sh/) installed, then just run `brew cask install google-cloud-sdk;` All set.

2. Set up a Firebase account and create a new firebase project. It should be pretty straightforward. Just be noted that after the project is created, you need to specify GCP resource location. The location can not be changed afterwards, so choose wisely.

3. Configure firebase authentication using either a service account (recommended) or a user credential.

4. (optional) Run `firebase projects:list;` to make sure the tool is authorized to access the project you've created for hosting.

5. Run `firebase init;` in the project root directory to initilize the project.

6. Run `hugo && firebase deploy;` to push the website to firebase.

~~## External Dependencies~~

~~The following libs are required only when using terminalizer-player. Those versions are tested in May 2020.~~

~~    npm install --save xterm@3.14.5 jquery@3.5.1 terminalizer-player@0.4.1;~~

## TODO

To be added later.
