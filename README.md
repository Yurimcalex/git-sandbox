### Basics - git configuration

Configuration level flags.

- --system   /etc/gitconfig

- --global   ~/.gitconfig

- --local   <project>/.git/config

Order of searching for settings: local > global > system.

These settings will be applied to all projects of the current user:

```sh
git config --global user.name "username"
git config --global user.email user@email.com
```

List settings:

```sh
git config --list
git config --list --global
cat ~/.gitconfig
```

Remove parameter name of user section:

```sh
git config --unset user.name
```

Remove section user:

```sh
git config --remove-section user
```

Alias for config command:

```sh
git config --global alias.c 'config --global'
```

Alias for multiple commands:

```sh
git config alias.<command_name> '!git ...; git ...'
```

Show config option:

```sh
git config -h
git help config
```

### Repo creation, first commit.

Create repository:

```sh
git init
```
There are three zonez.
Working Directory > Index > Repository.
- Working Directory - current project files.
- Index - list of files tracked by git.
- Repository - full project's history.

Add file to Index:

```sh
git add <file>
```

Add indexed file to Repository:

```sh
git commit
```

### Git and file permissions.

...
create mode 100644 <file>
first three nnn - object type saved to git, 100 - file.
second three nnn - file permissions, 644 - not executable, 755 - executable.

chmod +x <file> - changing file permissions modifies the file in git like adding some content to it.

