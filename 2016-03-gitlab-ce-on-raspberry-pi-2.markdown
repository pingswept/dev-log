
Run `sudo raspi-config` to expand filesystem, set hostname and change default password.

(Should have set locale too.)

    sudo apt-get install curl openssh-server ca-certificates apt-transport-https vim # skipped postfix for now
    curl https://packages.gitlab.com/gpg.key | sudo apt-key add -
    sudo curl -sS https://packages.gitlab.com/install/repositories/gitlab/raspberry-pi2/script.deb.sh | sudo bash
    sudo apt-get install gitlab-ce
    sudo gitlab-ctl reconfigure

Should have installed Postfix-- needed for password resets and such. Acch. Might start over.

Installed Postfix.

Change admin password.

Create whatever users are needed, then disable new user creation:

    Gitlab > Admin area > settings > Features > remove the check mark next to "Sign-up enabled"

