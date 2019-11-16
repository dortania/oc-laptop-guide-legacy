# Just Enough GIT

Many of the instructions in the guide point you to Github and possibly other sites to download software or scripts from a repository. If the site offers a release package, that's what you want to download. If it doesn't however you will need to know enough about git to clone the contents of the repository to your computer. In order to install git, you will need to add some command line tools to your Hack. Here's how you install those.

```text
$ xcode-select --install
```

Once the installation completes you will have the ability to run the 'git' command. This command allows you to interact with software repositories. To clone a repository to your computer, go to the site with the repository you want to clone and click the green clone or download button, then select https. Copy the URI path in the box and add it to the end of a git clone command like this.

```text
$ git clone https://github.com/corpnewt/USBMap.git
```

Once it's cloned, you will be able to enter the directory and make use of the projects resources.

If you'd like a little more advanced git knowledge, here are some resources to get you started!

[Resources to learn Git @ Github](https://try.github.io)

