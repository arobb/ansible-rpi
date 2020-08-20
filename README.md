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

## Ad-hoc commands
```
ansible all -a "cat /proc/cpuinfo" [-u username] [--become] [--ask-become-pass]
```

## WireGuard VPN
Generate keys.

### Basic commands
```
# Generate a private key
wg genkey

# Extract a public key from a private key
wg pubkey < private.key

# Generate a preshared key
wg genpsk
```

### Generate private, public, and preshared keys
```
touch peer-{a,b}.{key,pub,psk} && chmod 0700 peer-{a,b}.{key,pub,psk}
wg genkey | tee peer-a.key | wg pubkey > peer-a.pub && wg genpsk > peer-a.psk
wg genkey | tee peer-b.key | wg pubkey > peer-b.pub && wg genpsk > peer-a.psk
```

### Client configuration
Client configurations are available at /etc/wireguard/clients/. Download the PNG for the QR code, or generate (for immediate display) on the command line with
```
sudo qrencode -t ansiutf8 -r /etc/wireguard/clients/<client>.conf
```

### Build keys into a client configuration
1. Generate private, public, and preshared keys for a client.
2. Edit the Vault file and create a new block of entries with the updated client name
3. Save the Vault file and clear any files you may have made on the vpn host
4. Edit group_vars/vpn/vars and create a new block of entries with the updated client name
4. Edit roles/vpn/vars/main.yaml and add an entry with the client name
