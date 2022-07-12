# atmatah

This project contains the automation scripts used to deploy webapps we have in ARBML. All of these scripts are written using ansible.

# Why this tool

We have couple of ARBML web apps that are developed with django. The idea is that whenever we have an app, deploying it to a VM is time-consuming and error prone. This tool takes all of this hassels out and provides an off-the-shelf script to deploy to any VM.


# Requirements

```bash
ansible==6.0.0
```

# How to deploy

Before you deploy, Please take a look on how the project manages environment and secrets.

## Environment management

This project manages environment with a `local_settings.py` file. You need to make sure your project settings module imports this module.

## Secrets management

There are alot of ways to manage secrets. However, this repo supports `ansible-vault` to manage secrets. If you prefer other ways, you can write your custom secret retrieval tasks under in `pre_tasks` in your app.yml.

## db

This project expects that you have a db that is already setup. You just need to put connection params in the local_settings file.
If no db is available, the app will be just deployed to the default sqlite.db file.
Maybe in the future we will have kind of support to this. Any PR is also appreciated.

## To deploy an app:

- If you are doing this for the first time, you need to setup your project. To do so:
  - clone the repo.
  - cd to `apps` folder.
  - copy `project_template` folder, rename it to yours.
  - in the inventory file, put remote ip.
  - fill in public `vars` that are specific to your project in `vars/main.yml` like the  project repo, branch, server name, etc.
  - for any secret vars, like secret_key, account passwords, etc, encrypt it to `vars/secret-vars.yml`
- Once your project is setup, deploy using:  ```ansible-playbook -i environment/inventory apps/<your_app>/app.yml --ask-vault-pass``` to deploy
- [Optional] you can write your password to `.config/file` then run ```ansible-playbook -i apps/<your_app>/inventory apps/<your_app>/app.yml --vault-password-file=.config/ansible_pass.password``` and ansible will take the password from that file automatically.
