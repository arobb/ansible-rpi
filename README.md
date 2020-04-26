Based on https://github.com/motdotla/ansible-pi

# Directions
Basic configuration https://gist.github.com/arobb/f572fdd8946df2f037e9b8e515698fb7
Edit the `hosts` files.

Deploy using ansible (install instructions for ansible are in requirements below).
```
# For the new playbooks:
ansible-playbook -e 'host_key_checking=False' -i hosts --ask-pass new-pi-playbook.yaml

# For all other playbooks
ansible-playbook -e 'host_key_checking=False' -i hosts playbook.yaml
```

## Editing the Vault
```
ansible-vault edit group_vars/vpn/vault
```

## Testing against a Docker-hosted instance
https://github.com/arobb/sensor-pod
Follow README directions for "Generic access to Raspbian environment"

```
ANSIBLE_HOST_KEY_CHECKING=False ansible-playbook new-pi-playbook.yaml -i hosts-qa
```

## WireGuard VPN
Generate keys.

```
# Generate private and public keys
wg genkey | tee peer-a.key | wg pubkey > peer-a.pub
wg genkey | tee peer-b.key | wg pubkey > peer-b.pub

# Generate a pre-shared key
wg genpsk
```

### Client configuration
Client configurations are available at /etc/wireguard/clients/. Download the PNG for the QR code, or generate (for immediate display) on the command line with
```
sudo qrencode -t ansiutf8 -r /etc/wireguard/clients/<client>.conf
```
