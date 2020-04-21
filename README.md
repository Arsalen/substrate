Substrate
=========

Install and configure a Substrate local network with two validator nodes for Alice and Bob, note that the setup is to be used for development purposes only and it should be run on a single workstation with a single Substrate binary.

Requirements
------------

Ubuntu OS distribution ([Bionic Beaver](https://releases.ubuntu.com/18.04.4/) or [Xenial Xerus](http://releases.ubuntu.com/16.04/))

Variables
--------------

```yaml
vars:
  node: # Required for the node spec. (only alice or bob are allowed)
  target_port: # Alice p2p port required for node=bob (example 30333)
```

Setup
----------------

You need the following inventory file and playbook prior to your setup.

```ini
; inventory hosts
[substrate-node]
<substrate-ip> ansible_user=<substrate-user> ansible_ssh_pass=<substrate-ssh-pass> ansible_become=true ansible_become_pass=<substrate-become-pass>
```

```yaml
# playbook substrate-book.yml
  - hosts: all
    roles:
      - arsalen.substrate
```

Install role
```bash
ansible-galaxy install arsalen.substrate
```

Then run the following commands
```bash
# First start alice node, may take some time to install prerequisites and compile source
ansible-playbook --inventory hosts substrate-book.yml --extra-vars "node=alice alice_port=30333 alice_ws_port=9944 alice_rpc_port=9933"
# Once finished, start bob node to join alice
ansible-playbook --inventory hosts substrate-book.yml --extra-vars "node=bob bob_port=30334 bob_ws_port=9945 bob_rpc_port=9934 target_port=30333"
```

You can check your nodes state by running
```bash
# Systemd logs
journalctl -e -u alice -u bob
```

**NOTICE:** There is also a docker-compose implementation of up to six polkadot nodes in this repo: [docker-compose.polkadot](https://github.com/Arsalen/polkadot)

License
-------

MIT

References
------------------

- [Substrate Developer Hub](https://substrate.dev/)
- [substrate node template](https://github.com/substrate-developer-hub/substrate-node-template)

Author Information
------------------

@arsalen ([github](https://github.com/Arsalen), [medium](https://medium.com/@arsalen.hagui), [linkedin](https://www.linkedin.com/in/arsalen-hagui-506979123/))
