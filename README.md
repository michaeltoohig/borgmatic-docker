# Borgmatic Docker

My borgmatic-docker configuration and setup for my NAS.
Based on the official [borgmatic-docker](https://github.com/borgmatic-collective/docker-borgmatic) but with my own setup instructions for my future self.

## Initial Setup

Create the required directories for placing our configuration, `borgmatic.d` and other directories used by borg for caching file hashes, repo configuration, etc.
These directories are then reflected in the `.env` later.

```sh
mkdir data/{.borgmatic,.cache,.config,.ssh,borgmatic.d}
```

Copy the config template and configure and make changes, such as your BorgBase repo into the list of `repositories`.

```sh
cp data/borgmatic.d/config.yaml.template data/borgmatic.d/config.yaml
```

Copy the `.env.template` and set required envvars within.

```sh
cp .env.template .env
```

### SSH Keys

If you configure custom SSH key for a remote repo place the keys in your `VOLUME_SSH` directory.
You can read about the recommended setup [here](https://docs.borgbase.com/setup/borg/cli#step-3-create-and-assign-ssh-key-for-authentication) on BorgBase docs.

```sh
ssh-keygen -o -a 100 -t ed25519
```

### Backup Source Directory

It is currently assumed one directory will be backed up using the `VOLUME_SOURCE` directory.

You can modify this and include multiple directories as subdirectories of the containers `/mnt/source` directory in `docker-compose.yml`.

## Running Backups

First you must initialize the repositories, you can use `docker compose exec borgmatic init --help` and go from there following online documentation.

Now, you can leave the container running normally and it will follow your `crontab.txt` file.

## Restoring Backups

You can use the same container to restore backups.
But you must give additional permissions and capabilities to the container for it to work.
See `docker-compose.restore.yml` for a working example or reference the original borgmatic-docker repo.

Within the container mount an archive then copy the files you want to restore to the shared `restore` volume to access them on your machine.

## References

- [borgmatic-docker on Github](https://github.com/borgmatic-collective/docker-borgmatic)
- [borgmatic Docs](https://torsion.org/borgmatic/)
- [BorgBase Docs and Guides](https://docs.borgbase.com)
- [borgmatic-docker example for Mailcow](https://docs.mailcow.email/third_party/borgmatic/third_party-borgmatic/)
