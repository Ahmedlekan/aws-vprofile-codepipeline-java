# Repository Migration Guide: GitHub to Bitbucket

### Bitbucket Setup

Prerequisites

- Active Bitbucket account

- Git installed on local machine

- Existing GitHub repository access

Steps:

- Log in to your Bitbucket account

- Create a new workspace:

- Click "Workspaces" > "Create workspace"

- Name your workspace (e.g., devops-team)

Create a new repository:

![Image](https://github.com/user-attachments/assets/2d248a3f-63f8-4de4-bbff-deec9217d040)

- Click "Create repository" within your workspace

- Name it (e.g., vprofileapp)

- Keep other settings as default

![Image](https://github.com/user-attachments/assets/b7adc0b5-7df1-4ca4-a7ad-2a0fb1c02df1)


### SSH Key Configuration

1. Generate SSH Key Pair:

   - On your local machine

```bash
cd ~/.ssh/
ssh-keygen -t rsa -b 4096 -C "devops_deploy_key_for_build" -f mykey_rsa
# When prompted for passphrase, press Enter twice
```

2. Verify Key Generation:

```bash
ls -la  # Should show mykey_rsa (private key) and mykey_rsa.pub (public key)
cat mykey_rsa.pub  # Copy this output
```

3. Configure Bitbucket SSH:

![Image](https://github.com/user-attachments/assets/717b61a2-826a-4c5f-9a84-d992a3112f9f)

 - In Bitbucket:

   - Go to Workspace Settings > SSH keys
  
   - Click "Add key"
  
   - Paste your public key
  
   - Save 


4. Create SSH Config File:

```bash
vim config
```

 - Add these contents (press i to insert):

```bash
# bitbucket.org
Host bitbucket.org
  HostName bitbucket.org
  PreferredAuthentications publickey
  IdentityFile ~/.ssh/mykey_rsa
  IdentitiesOnly yes
```
 - Save with Esc then :wq

5. Test Connection:

```bash
ssh -T git@bitbucket.org
# You should see: "
```
![Image](https://github.com/user-attachments/assets/a7887fef-c13a-4176-90cc-86967d165c6b)


### Repository Migration

1. Clone Source Repository:

```bash
mkdir -p ~/projects/aws-cicd
cd ~/projects/aws-cicd
git clone https://github.com/hkhcoder/vprofile-project.git  # you can replace this with your source code
cd vprofile-project/
```

2. Verify and Prepare:

```bash
# Check current branches
git branch -a

# Switch to desired branch (aws-ci in this case)
git checkout aws-ci

# Verify remote configuration
cat .git/config
```

3. Change Remote Origin:

```bash
# Remove existing origin
git remote rm origin

# Add new Bitbucket origin
git remote add origin git@bitbucket.org:<workspace-name>/vprofileapp.git
# Example: git remote add origin git@bitbucket.org:cicdpipeline01/vprofileapp.git

# Verify new configuration
cat .git/config
```

4. Push to Bitbucket:
```bash
git push origin --all  # Push all branches
git push origin --tags # Push all tags if needed
```

### Verification

![Image](https://github.com/user-attachments/assets/bd1354e1-88bc-4954-a8c4-c107ca33581a)

Refresh your Bitbucket repository page

Verify:

- All branches are present

- Commit history is intact

- Code files are complete


### Troubleshooting

- Permission denied (publickey): Verify SSH key is added correctly in Bitbucket

- Repository not found: Check workspace/repository name in remote URL

- Branch mismatch: Ensure you've checked out the correct branch before pushing




