# Publishing a Release

**Publish:**

1. `sudo su`
   
   `systemctl stop docker && rm -rf /var/lib/docker/* && systemctl start docker`
   
   `exit`

2. `cd /location/of/my-own-kind`

3. `sudo make clean`

4. Bump version in Makefile.

5. Bump version in /README.md.

6. Draft a release in GitHub Releases page.

7. `make docker-builds` - must pass.

8. `sudo make install` - to install 'cmdline-player'.

9. `make test` - must pass.

10. `npm version patch` - increments 'n' in '0.8.n'.

11. `npm publish`

12. `make docker-uploads`

**Test the uploaded files:**

1. `sudo su`
   
   `systemctl stop docker && rm -rf /var/lib/docker/* && systemctl start docker`
   
   `exit`

2. `cd /location/of/my-own-kind`

3. `make test` - must pass.

4. `git add tests/e2e-logs/` - commit the proof of success.

5. git commit and push changes

6. Publish the GitHub draft release

The release works - hopefully!

That's it.
