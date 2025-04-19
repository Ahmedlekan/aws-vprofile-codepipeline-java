Moving Our source code from git to bit bucket

1. Login into bitbucket account

2. Create a new wrokspace

3. Create a new repository

4. On your local machine

cd .ssh/
ssh-keygen and name your file e.g mykey_rsa
For the passphrase and same passphrase, click enter
ls -la to confirm your key. You should see mykey_rsa mykey_rsa.pub
cat mykey_rsa.pub. Copy the public key. 
Go back to your bitbucket and clciked on settings
Click on work space settings
Click on SSH Keys
On your local machine create a configuration file "vim config"
In the vim editor press i for insert and enter this:

# bitbucket.org
HostName bitbucket.org
  PrefferedAuthentication publickey
  IdentityFile ~/.ssh/mykey_rsa
  IdentitiesOnly yes

press esc key to exit insert, and press :wq to save the file

Test your SSH authentication with Bitbucket "ssh -T gitbucket.org"

mkdir aws-cicd
cd aws-cicd/
git clone https://github.com/hkhcoder/vprofile-project.git 
cd vprofile-project/
cat .git/config
git checkout aws-ci
git branch -a
git remote rm origin
cat .git/config
git remote add origin git@bitbucket.org:cicdpipeline01/vprofileapp.git
cat .git/config
git push origin --all

reload your repository page



