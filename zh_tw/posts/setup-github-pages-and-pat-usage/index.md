<!--
.. title: Setup Github Pages and PAT Usage
.. slug: setup-github-pages-and-pat-usage
.. date: 2023-08-27 16:58:46 UTC+08:00
.. tags: 
.. category: 
.. link: 
.. description: 
.. type: text
-->

Hi. This is RibosomeK. I want to document what I have learnt in my spare time. And this is the first day. In today I setup my Github Page that barely working, and I setup my Github Personal Access Token (PAT). I hope I can keep recording this at least once a week. But who knows.

## Setup Github Page

This is the easiest part in today's working (I guess). Just follow the instruction from [Github](https://pages.github.com/). Basically speaking, it's just two step:

1.   Create a Github repository that is named `username.github.io`, where username is your Github username. Mine is RibosomeK for example and yes it's lower case. And also yes, it usually duplicated with the owner name (which I was a little bit confused when creating one).
2.   Add a `index.html` file on the Github website that has a nice GUI without doing that with command lines.

Once you've done that. go to `username.github.io`, and voila, you've set up your own Github Page!

Of cause, as the title implied, I used command line, and yes, git is hard.

## Simple Usage of PAT

PAT as above, is Personal Access Token that provided by Github to remotely operate your Github account, like git push. When I tried to push the index.html file using the command `git push`, I entered my username and Github password as it promoted. But an error happened, as follow. 

```
remote: Support for password authentication was removed on August 13, 2021.
remote: Please see https://docs.github.com/en/get-started/getting-started-with-git/about-remote-repositories#cloning-with-https-urls for information on currently recommended modes of authentication.
```

 So I open the link, found out that PAT is recommended to use in this situation.

### Create a PAT

As the link suggest, I clicked on my Github avatar, clicked on `Settings`, find the `Developer settings` which at the bottom of the left panel. Again, find `Personal access tokens` session. Here I chose the `Token (classic)`. Right on the corner, you can see the `Generate new token` button. That's the one.

Because I also have Github app on my phone, so it gave me a verification code that I need to enter on my phone. After the authentication, you can customize your PAT, including a short description indicating what is use for, when the token will expire, how much it can access the scope.

I create a PAT note it as "git learning", that will expire in 30 days, and select all the scope (which seems not necessary cause I only need something like the push command ?).

If everything goes well, you will get the token that starts with `ghp_` follow by some random characters.

### Store PAT with GCM

Now go back to the `git push` commend, use the PAT as password, can successfully execute the command. But it would be impractical to enter the PAT manually every time you push commit to the repository.  So I google with the keywords `personal access token automatically` and I found [a post on Stack Overflow](https://stackoverflow.com/questions/46645843/where-to-store-my-git-personal-access-token) suggested that you should use [Git Credential Manager (GCM)](https://github.com/git-ecosystem/git-credential-manager) to store PATs.

After trying for multiple time, I think I finally make it on my machine (Ubuntu 22.04.3 wsl2), and here is how.

1.   Download the `.deb` package from latest release on Github
2.   Install the package with `sudo dpkg -i the-gcm-package.deb` 
3.   Install `pass` package with `sudo apt install pass`
4.   Generate GPG key pair with `gpg --gen-key` and follow the prompts
5.   Set `GPG_TTY` environment with `export GPG_TTY=$(tty)`
6.   Initialize `pass` utility with `pass init <gpg-id>` 
7.   Set git config with `git config --global credential.credentialStore gpg`

After all these steps, if I remember correctly, when you use `git push` for the first time. It will ask you to enter the passphrase of your GPG key pair. The second time you push your commit, it won't ask for anything, which is pretty convenient.

## Git Command I've Learnt along the Way

Before you push to the repository, you need to add and commit.

```bash
git add "file-you-want-to-add"
git commit -m "the commit comment"
```

If you want to add all the files, you use.

```bash
git add --all
```

Configs that we set can be list by

```bash
git config --global --list
```

To remove configs that we set

```bash
git config --global --unset "name-of-the-config"
```



## Afterthought

I spend like an afternoon to figure out how to use the GCM and I have to say this really hard for me to know what is going on. When I look the Stack Overflow and GCM docs, I was struggling. But eventually it works. I'm quite glad.
