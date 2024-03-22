# How To Work With Multiple Github Accounts on a Single Machine

**Support for password authentication was removed on August 13, 2021.**

Let suppose I have two GitHub accounts:
- **https:/<span></span>/github.com<span></span>/kris-work**
- **https:/<span></span>/github.com<span></span>/kris-personal**.

I want to setup my computer (regardless of OS) to easily fetch & push code to both GitHub accounts.

> NOTE: This logic can be extended to more than two accounts also.

The setup can be done in 4 easy steps (or 5 if you are on Mac):
## Steps:
- [Step 1](#step-1) : Create SSH keys for each of your accounts
- [Step 1b](#step-1b) : Add SSH keys to SSH Agent
- [Step 2](#step-2) : Add SSH public key to the Github
- [Step 3](#step-3) : Create a Config File and Make Host Entries
- [Step 4](#step-4) : Cloning GitHub repositories using different accounts


## Step 1
### Generate separate SSH keys for each of your your accounts

```
  ssh-keygen -f "kris-work"
  ssh-keygen -f "kris-personal"
  ...and so on
```

This would created `kris-work` and `kris-personal` public (the one with `.pub` extension) and private keys on your `.ssh` folder

Notice here **kris-work** and **kris-personal** are the username of my github accounts. You can name this accordingly or you can create your own custom identifier, as long as you adjust to the directions stated below.

## Step 1b
### (On Mac) Add the keys to the SSH agent

If you are **NOT** on Mac you can skip this step.

Now we have the keys but it cannot be used until we add them to the SSH Agent.

```
  ssh-add -K ~/.ssh/kris-work
  ssh-add -K ~/.ssh/kris-personal
```
You can read more about adding keys to SSH Agent [here](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent).

## Step 2
### Add SSH public key to the Github

Add the public keys in GitHub. This can be found by:
- Clicking your user image
- Settings
- SSH and GPG keys
- New SSH key

Then open the `.pub` file of the corresponding account you logged in in GitHub, then copy its contents of it under `Key` and give it any `Title` of your choice.

Do this for each of the account, and for each public key.

## Step 3
### Create a Config File and Make Host Entries

The **~/.ssh/config** file allows us specify many config options for SSH.

If **config** file not already exists then create one (make sure you are in **~/.ssh** directory)

```sh
touch config
```

Open the config file created with the editor of your choice.

Now we need to add these lines to the file, each block corresponding to each account we created earlier.
```config
# work account
Host github.com-kris-work
HostName github.com
User git
IdentityFile ~/.ssh/kris-work

# personal account
Host github.com-kris-personal
HostName github.com
User git
IdentityFile ~/.ssh/kris-personal
```

## Step 4

Listed below are the other steps or commands that you may find helpful

### Cloning GitHub repositories using different accounts
```git
git clone git@github.com-{your-username}:{your-username}/{the-repo-name}.git

[e.g.] git clone git@github.com-kris-personal:kris-personal/TestRepo.git
```

### User and email configuration per project folder

```git
git config user.email "kris-work@gmail.com"
git config user.name "Kristian Alunan"

git config user.email "kris-personal@gmail.com"
git config user.name "Kristian Alunan"
```

### Changing the remote to match the

```git
git remote add origin git@github.com-kris-work:kris-work/{{repo}}.git
git remote add origin git@github.com-kris-personal:kris-personal/{{repo}}.git
```

## References:
- [How do I push to GitHub under a different username?](https://stackoverflow.com/questions/13103083/how-do-i-push-to-github-under-a-different-username)
- [Is it possible to have different Git configuration for different projects?](https://stackoverflow.com/questions/8801729/is-it-possible-to-have-different-git-configuration-for-different-projects)
- [How can I save username and password in Git?](https://stackoverflow.com/questions/35942754/how-can-i-save-username-and-password-in-git)
- [Source of this document, which I altered](https://gist.github.com/rahularity/86da20fe3858e6b311de068201d279e3)
