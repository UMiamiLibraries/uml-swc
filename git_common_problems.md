# Common problems in git lesson

## Remotes 

### Authentication 
After performing `ssh -T git@github.com` there will be a prompt:

The authenticity of host 'github.com (192.30.255.112)' can't be established.  
RSA key fingerprint is SHA256:\<some hash\>.  
This key is not known by any other names  
Are you sure you want to continue connecting (yes/no/[fingerprint])? `y`  
Please type 'yes', 'no' or the fingerprint: `yes`  
Warning: Permanently added 'github.com' (RSA) to the list of known hosts.  
git@github.com: Permission denied (publickey).  

*^^ but why 'Permission denied'' (by github), something else went wrong. Or is this because you typed `y` first and then `yes`?*  

Many times students forget to type in "yes." If a student did not type "yes" have them again repeat "ssh -T git@github.com" and then when prompted type "yes."



