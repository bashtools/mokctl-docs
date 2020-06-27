# Publishing a Release

**Publish:**

1. `sudo su`
   
   `systemctl stop docker && rm -rf /var/lib/docker/* && systemctl start docker`
   
   `exit`

2. `cd /location/of/my-own-kind`

3. `sudo make clean`

4. Bump version in Makefile

5. Bump versions in `src/globals.sh`

6. Add version to '_CC_set_up_master_node' in `src/createcluster.sh`

7. Bump version in `/README.md` and `mokctl-docs/README`

8. Draft a release in GitHub Releases page

9. `make docker-builds` - must pass.

10. `sudo npm i -g command-line-player` - to install 'cmdline-player'

11. `make docker-upload-baseimage`

12. `make test` - must pass.

13. `git commit -am "Version bump"`

14. `npm version patch` - increments 'n' in '0.8.n'.

15. `npm publish`

16. `make docker-uploads`

**Test the uploaded files:**

1. `sudo su`
   
   `systemctl stop docker && rm -rf /var/lib/docker/* && systemctl start docker`
   
   `exit`

2. `cd /location/of/my-own-kind`

3. `make test` - must pass.

4. `git add tests/e2e-logs/` - commit the proof of success.

5. git commit and push changes

6. Publish the GitHub draft release

The release should be good.

That's it.
