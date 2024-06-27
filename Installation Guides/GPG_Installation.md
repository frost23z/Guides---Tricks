# Sign Git Commits Using GPG

## Table of Contents

- [Generate a new GPG key](#generate-a-new-gpg-key)
- [Add GPG key to GitHub](#add-gpg-key-to-github)
- [Set up Git to sign all commits](#set-up-git-to-sign-all-commits)
- [Dealing with GPG passphrase](#dealing-with-gpg-passphrase)
- [Delete a GPG key](#delete-a-gpg-key)
- [Backup your GPG key](#backup-your-gpg-key)
- [Restore your GPG key](#restore-your-gpg-key)
- [Set up VS Code to sign commits](#set-up-vs-code-to-sign-commits)

## Generate a new GPG key

1.  Install [GPG](https://gnupg.org/download/index.html). Verify the installation by running the following command in a terminal:

        gpg --version

2.  Generate a new GPG key by running the following command:

        gpg --full-generate-key

    1. At the prompt specify the kind of key you want.
    2. Enter the key size, or press enter to accept the default key size of 3072. We recommend a key size of 4096
    3. Enter the length of time the key should be valid. Press enter to specify the default selection, indicating that the key doesn't expire. Unless you require an expiration date, we recommend accepting this default.
    4. Verify that your selections are correct
    5. Enter your name
    6. Enter your email address
    7. Enter a comment
    8. Enter a passphrase
    9. Your GPG key is now generated

## Add GPG key to GitHub

1.  List your GPG keys and copy the GPG key ID you'd like to use by running the following command in your terminal:

        gpg --list-secret-keys --keyid-format=long

    The output will look similar to the following: where `3AA5C34371567BD2` is the GPG key ID:

        /Users/hubot/.gnupg/secring.gpg
        ------------------------
        sec   4096R/3AA5C34371567BD2 2016-03-10 [expires: 2017-03-10]
        uid                            Alice (Example) <alice@example.com>
        sub   4096R/42B317FD4BA89E7A 2016-03-10 [expires: 2017-03-10]

2.  Export your GPG key by running the following command replacing `3AA5C34371567BD2` with the GPG key ID you want to export:

        gpg --armor --export 3AA5C34371567BD2

3.  Copy your GPG key, beginning with `-----BEGIN PGP PUBLIC KEY BLOCK-----` and ending with `-----END PGP PUBLIC KEY BLOCK-----`
4.  Go to [GitHub](https://github.com) and sign in to your account if you haven't already.
5.  Click your profile icon in the top right corner of the page
6.  In the user settings sidebar, click **SSH and GPG keys**, then click **New GPG key**
7.  Give your GPG key a title and paste your GPG key into the **Key** field
8.  Click **Add GPG key**
9.  Confirm your action by entering your GitHub password

## Set up Git to sign all commits

1.  If you have previously configured Git to use a different key format when signing with --gpg-sign, unset this configuration so the default format of openpgp will be used.

    git config --global --unset gpg.format

2.  Paste the following command to use the GPG key you generated:

        git config --global user.signingkey <YOUR_KEY_ID>

3.  Paste the following command to tell Git to sign commits by default

        git config --global commit.gpgsign true

4.  Paste the following command to tell Git to use the gpg-agent

        git config --global gpg.program gpg
        or,
        git config --global gpg.program "C:\Program Files (x86)\gnupg\bin\gpg.exe"

## Dealing with GPG passphrase

1.  If you don't want to enter your GPG passphrase every time you sign a commit, you can use the gpg-agent to cache your passphrase. To do this, you need to add the following lines to your `~/.gnupg/gpg-agent.conf` file:

        default-cache-ttl 34560000
        max-cache-ttl 34560000

    The `default-cache-ttl` option sets the time a cache entry is valid to 400 days. The `max-cache-ttl` option sets the maximum time a cache entry is valid to 400 days. You can adjust the values to suit your needs.

2.  Restart the gpg-agent by running the following command:

        gpgconf --kill gpg-agent

3.  You can also use the `gpgconf` command to change the cache settings:

        gpgconf --reload gpg-agent

## Delete a GPG key

1.  List your GPG keys by running the following command in your terminal:

        gpg --list-secret-keys --keyid-format=long

2.  Delete the GPG key by running the following command in your terminal:

        gpg --delete-secret-keys <YOUR_KEY_ID>
        gpg --delete-key <YOUR_KEY_ID>

    First, delete the secret key, then delete the public key.

## Backup your GPG key

1.  List your GPG keys by running the following command in your terminal:

        gpg --list-secret-keys --keyid-format=long

2.  Export your GPG key by running the following command in your terminal:

        gpg --armor --export <YOUR_KEY_ID> > public-key.asc
        gpg --armor --export-secret-keys <YOUR_KEY_ID> > private-key.asc

    Export both the secret key and the public key. Copy the keys to a secure location. Or, you use the following command to export the keys to a file and store them to a secure location:

        gpg --armor --export <YOUR_KEY_ID> > public-key.asc
        gpg --armor --export-secret-keys <YOUR_KEY_ID> > private-key.asc

## Restore your GPG key

1.  Import your GPG key by running the following command in your terminal:

        gpg --import public-key.asc
        gpg --import private-key.asc

    Replace `public-key.asc` and `private-key.asc` with the file names you used to store the keys.

2.  Trust the key by running the following command in your terminal:

        gpg --list-secret-keys --keyid-format=long
        gpg --edit-key <YOUR_KEY_ID>

3.  At the prompt, type `trust`, then type `5` to set the key as ultimately trusted. Type `y` to confirm, then type `quit` to exit the GPG prompt.

## Set up VS Code to sign commits

1.  Open VS Code Settings
2.  Search for `CommitSigning` and enable it

## Sign Previous Commits

1.  To sign the previous commits, you can use the following command:

        git rebase --exec 'git commit --amend --no-edit -n -S' -i <commit_hash>

    Replace `<commit_hash>` with the commit hash you want to start signing from. or, to sign all the commits from the root commit, you can use the following command:

        git rebase --exec "git commit --amend --no-edit -n -S" -i --root

2.  After running the command, you will be prompted to enter your GPG passphrase for each commit.

3.  Once you have signed all the commits, you can force push the changes to the remote repository:

        git push origin <branch_name> --force

    Replace `<branch_name>` with the name of the branch you are working on.
