gitlab CE
=========

Gitlab install has a few quirks that are worth
pointing out.

special, like ralph wiggum special
==================================

The RPM is an Omnibus package, so it installs its
own universe under /opt/gitlab with postgres, rails, etc.
and uses its own Chef to fire it all up.


Once the play is run, you should manually start gitlab
with

    sudo /opt/gitlab/bin/gitlab-ctl reconfigure

This isn't hooked into the init system of the VM.
So after a reboot, gitlab won't be running. 

I'll eventually work out how to have this start at boot,
I don't need it right now myself. Just run the same
command to start things.

MTA
===

You basically won't be able to use gitlab without the
ability to send mail. I've set up a small Postfix relay
for this, but since I can't guess everyones smarthost
it will only be configured if you set one like this:


    ansible-playbook -i vagrant site.yml -e gitlab_smarthost=smtp.whatever.com
