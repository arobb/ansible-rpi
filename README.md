Based on https://github.com/motdotla/ansible-pi

# Directions
Basic configuration https://gist.github.com/arobb/f572fdd8946df2f037e9b8e515698fb7
Edit the `hosts` files.

Deploy using ansible (install instructions for ansible are in requirements below).
```
ansible-playbook -e 'host_key_checking=False' playbook.yaml -i hosts
```
