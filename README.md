Based on https://github.com/motdotla/ansible-pi

# Directions
Basic configuration https://gist.github.com/arobb/f572fdd8946df2f037e9b8e515698fb7
Edit the `hosts` files.

Deploy using ansible (install instructions for ansible are in requirements below).
```
ansible-playbook -e 'host_key_checking=False' playbook.yaml -i hosts
```

## Testing against a Docker-hosted instance
https://github.com/arobb/sensor-pod
Follow README directions for "Generic access to Raspbian environment"

```
ANSIBLE_HOST_KEY_CHECKING=False ansible-playbook new-pi-playbook.yaml -i hosts-qa
```
