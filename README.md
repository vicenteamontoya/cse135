# CSE 135 HW1: Team Uno

## Hosted at: https://cse135.codes

## Team Members:

- Vicente Montoya

## Login Credentials (username, password):

- SSH: `grader@137.184.182.80`, connect using key `id_rsa` (use `-i` flag and path to file)
- Server Root: `grader`, `cse135grader`
- Website: `grader`, `cse135grader`
- Database Root: `grader`, `c$e135DB`

## Summary: GitHub Auto Deploy Setup

In order to deploy from GitHub, I added a `post-receive` hook in a bare repository file inside the server that forced pull from the remote origin repository everytime something was pushed to it, and used `/var/www` as its working tree.

To synchronize both upstreams and improve effiency, I added two remote upstreams to my local git repository, binded to `prod`, such that one command could push to both the live server and the GitHub repository, like so: 

```
prod    https://github.com/vicenteamontoya/cse135.git (push)
prod    ssh://root@137.184.182.80/home/vicenteamontoya/cse135.codes.git (push)
```

The command `git push prod main` pushes to both the origin remote repository at GitHub and the live server.

## Summary: Changes to HTML file after compression

After adding gzip compression I saw a significant improvement:

- Bytes Transferred: reduced from 1.4 KB to 826 B.
- Request Time: reduced from 50~ms to 30~ms.

## Summary: Removing 'Server' header

In order to achieve this I installed the `mod_security` module, and then I added the following lines to the Apache configuration file (`/etc/apache2/apache2.conf`):

```
ServerTokens Full
SecServerSignature "CSE135 Server"
```

The configuration `SecServerSignature`, allows us to specify the server signature we want to the send in our response headers, and the configuration `ServerTokens` just tells us that we should use the specified server signature.