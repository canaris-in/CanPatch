# Canpatch Manger Deployment

## Requirements
- Pyhton >= 3.10
- Ansible >= 2.15
- SSH access to remote machine

# Setup on Remote Machine

1. Copy your ssh public key to remote server and add it to authorized keys
2. Run `ssh -T <ssh_user>@<ssh_host>` from your local machine to authenticate your machine with the remote      machine. 
3. SSH to remote server
4. Copy ssh public key of the remote machine user and add it to your github account. This is required so that ansible is able clone the repo ito the remote machines.
5. Run command `ssh -T git@github.com` in the remote machine to authenticate remote machine with github
6. Clone this repository into your local machine
7. Setup hosts in ansible hosts file

    ```yaml
    awsinstances:
      hosts:
        server1:
          ansible_user: <ssh_user>
          # ansible_port: 22 # default - 22
          ansible_host: <host_name|ip> #

    ``` 
    - Add `ansible_user` value as the ssh username
    - Add `ansible_host` value as the remote machine ip or hostname
8. From your project root run the follwing command to check hosts connectivity
    ```bash
    ansible -i ansible/hosts.yml all -m ping
    ```
10. From your project root, run the following command to start the deployment
    ```bash
    ansible-playbook -i ansible/hosts.yml ansible/deploy.yml --extra-vars "ansible_sudo_pass=<remote_machine_sudo_password>"
    ```
    **Note:- Replace <remote_machine_sudo_password> with the password of the sudo user in the remote machine**


# Setup on Local Machine

1. Clone this repository into your local machine
  ```bash
  git clone git@github.com:canaris-in/CanPatch.git
  ```

2. From your project root, run the following command to start the deployment
  ```bash
  ansible-playbook ansible/deploy.local.yml --extra-vars "ansible_sudo_pass=<sudo_user_pass>" --extra-vars "ansible_user=<ansible_user>"
  ```
  **Note:- Replace <sudo_user_pass> with the password of the sudo user and <ansible_user> with your username**