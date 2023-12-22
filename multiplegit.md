
# Add multiple git accounts to the same machine

Assuming you have two different accounts on GitHub: 

`personal@domain.com` and `vanilla@domain.com` .

### 1. Generate two different SSH keys for both of your accounts

    ssh-keygen -t rsa -C "your email"
    ssh-keygen -t rsa -C "your email associated with the company"

This will create public/private key pairs for each of these accounts. They should be located inside the` ~/.ssh` directory. Assuming the generated key files are the following

    ~/.ssh/id_rsa_personal        # Private key for personal account
    ~/.ssh/id_rsa_personal.pub    # Public key for personal account
    ~/.ssh/id_rsa_vanilla
    ~/.ssh/id_rsa_vanilla.pub

### 2. Once the keys are generated run this command

    eval "$(ssh-agent -s)"

### 3. Modify the `.ssh/config` file. If it is already there then good else create it

    touch ~/.ssh/config

`Note: config file should not have any extension.`

### 4. Open `~/.ssh/config` file and add following content

    #GitHub for personal
    Host github.com                              # Notice the host for personal
      HostName github.com-personal
      User git
      IdentityFile ~/.ssh/id_rsa_personal        # Private key for personal account
    
    #Github for work
    Host github.com-work                         # Notice the host for work 
      HostName github.com
      User git
      IdentityFile ~/.ssh/id_rsa_work            # Private key for work account
    
    Host *
       AddKeysToAgent yes                        # Load the keys from keychain into ssh-agent automatically
       IdentitiesOnly yes                        # Tells the ssh-agent server to use the IdentityFiles specified above for each host
       
### 5. Save config.

### 6. Add keys to macOS Keychain(run the below command)
ssh-add -D           # Will delete all identities added previously from the ssh agent
ssh-add --apple-use-keychain ~/.ssh/id_ed25519_personal
ssh-add --apple-use-keychain ~/.ssh/id_ed25519_work

Add public keys to your corresponding accounts(personal or office accounts).

You can Test the connection 
ssh -T git@github.com
ssh -T git@github.com-work

If you followed everything to this point, this should return
Hi USERNAME! You've successfully authenticated, but GitHub does not provide shell access.

Finally, If you already have cloned repository of gitlab, you can update the remote url as below
git remote set-url origin git@github.com-work:company/project.git

------------------------ my config file--------------
#[Github]
Host github.com-vanilla
   HostName github.com
   User git
   AddKeysToAgent yes
   IdentitiesOnly yes
   IdentityFile ~/.ssh/id_rsa_vanilla

example : git remote set-url origin git@github.com-vanilla:Vanilla-Tech/eremit-singapore--mobile-android.git
