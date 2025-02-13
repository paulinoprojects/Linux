Changing a user's home directory in Linux requires careful steps, especially if the user has existing files in their current home directory.  Here's a breakdown of the process, including important considerations and best practices:

Methods

There are several ways to change a user's home directory:

usermod command (Recommended): This is the standard and safest method.

Manually editing /etc/passwd (Advanced, Not Recommended): While possible, directly editing the /etc/passwd file is highly discouraged due to the risk of errors that can corrupt the system.

Using usermod (The Safe Way)

The usermod command is the preferred method. Here's how to use it:

Bash

usermod -d /new/home/directory username
Replace /new/home/directory with the full path to the new home directory. Make sure this directory exists! If it doesn't, create it first: mkdir -p /new/home/directory (the -p option creates parent directories if needed).
Replace username with the actual username.
Important Considerations and Steps

Backup: Before making any changes, back up the user's current home directory. This is crucial in case something goes wrong.  You can use cp -rp /old/home/directory /old/home/directory.bak (replace /old/home/directory with the current home directory).

Create the New Directory: Ensure the new home directory exists and has the correct permissions. 1  The user should own the new directory.  Use these commands:   
 1. 
How to change Linux user home directory? - LabEx

labex.io


Bash

mkdir -p /new/home/directory
chown username:username /new/home/directory
chmod 700 /new/home/directory  # Restrict permissions initially
Copy Data (Important):  You need to copy the user's data from the old home directory to the new one.  Use rsync for this, as it preserves permissions and other file attributes:

Bash

rsync -av /old/home/directory/ /new/home/directory/
The trailing slash on the source directory (/old/home/directory/) is important. It copies the contents of the directory, not the directory itself.
The -a option means "archive" mode (recursive, preserves symlinks, times, permissions, etc.).
The -v option is for "verbose" output, so you can see what's being copied.
Change the Home Directory (using usermod): Now you can use the usermod command:

Bash

usermod -d /new/home/directory username
Change Ownership (Important): After changing the home directory, ensure the user owns all the files and directories within it:

Bash

chown -R username:username /new/home/directory
The -R option is for recursive ownership change.

Verify: Log in as the user and verify that everything is working correctly. Check files, settings, and applications.

Remove Old Home Directory (Optional but Recommended): Once you're sure everything is working, you can remove the old home directory (after you've double-checked your backup!).

Bash

rm -rf /old/home/directory  # Be *absolutely sure* you have a backup!
Example (Complete)

Let's say you want to change the home directory for user john from /home/john to /data/john.

Bash

# 1. Backup (CRUCIAL!)
cp -rp /home/john /home/john.bak

# 2. Create the new directory
mkdir -p /data/john
chown john:john /data/john
chmod 700 /data/john

# 3. Copy data
rsync -av /home/john/ /data/john/

# 4. Change home directory
usermod -d /data/john john

# 5. Change ownership
chown -R john:john /data/john

# 6. Verify (Log in as john and check!)

# 7. Remove old home directory (AFTER verification and backup check!)
rm -rf /home/john
Key Points

Backup, Backup, Backup! This is the most important step.
Use rsync for copying data to preserve file attributes.
Double-check paths and usernames.
Test thoroughly after making the changes.
Be very cautious when removing the old home directory.
