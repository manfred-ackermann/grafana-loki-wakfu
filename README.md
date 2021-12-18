# grafana-loki-wakfu

_When Wakfu meets Grafana/Loki_

A guide to get data-based inside on your playbehavior and efficiency.

## Prerequisites

- [Grafana](https://grafana.com/) Account

  > _Free forever access, 3 users, 10,000 active series for metrics, 50 GB of logs, 50 GB of traces, 14-day retention_
  > => [Grafana Signup](https://grafana.com/auth/sign-up/create-user)

- [Promtail](https://grafana.com/docs/loki/latest/clients/promtail/)

  > _Promtail is an agent which ships the contents of local logs to a private Grafana Loki instance or Grafana Cloud._

- [Wakfu](https://www.wakfu.com/en/mmorpg)

  > _Wakfu is a tactical turn-based MMORPG, developed by Ankama Games._

## Installation

### Promtail

Download Promtail for your Operating System (OS) at [Grafana/Loki](https://github.com/grafana/loki/releases) and copy the executable to e.g. `/usr/local/bin/` and created a symbolic link `promtail` to it.

Example:
```bash
# download
$ curl -O -L "https://github.com/grafana/loki/releases/download/v2.4.1/promtail-linux-amd64.zip"
# extract the binary
$ unzip "promtail-linux-amd64.zip"
# make sure it is executable
$ chmod a+x "promtail-linux-amd64"
# move to appropriate folder
$ mv promtail-linux-amd64 /usr/local/bin
# create symlink
$ ln -s /usr/local/bin/promtail-linux-amd64 /usr/local/bin/promtail
```

### Wakfu
Not covered.

## Configuration

### Promtail

Promtail needs to know what logfile to read and where to send the logs to.

1. Create the directory `/etc/promtail`.
2. Copy `contrib/promtail.yaml` to above directory.
3. Update the `/etc/promtail/promtail.yaml`

Example:
```bash
# create directory
$ mkdir -p /etc/promtail
# copy template
$ copy contrib/promtail.yaml /etc/promtail
# edit template
$ vi /etc/promtail/promtail.yaml
# replace <_> fields with your values
```

Depending of your OS the `__path__` setting in `promtail.yaml` may be

| OS | Path |
| :---: | :---: |
| Linux | `/home/<Your Linux User Name>/.config/zaap/wakfu/logs/wakfu.log` |
| MacOS | `/Users/<Your MacOS User Name>/Library/Logs/zaap/wakfu/logs/wakfu.log` |

or the path you may have configured in the _Ankama Launcher_ for _Wakfu_.

### System Service

The system service ensures that promtail is started automatically when your OS starts.

#### Linux

Copy the file `contrib/promtail.service` to the directory `/etc/systemd/system`, reload the SystemD Daemon and enable the service.

Example:
```bash
# copy template
$ copy contrib/promtail.service /etc/systemd/system
# reload systemd daemon
$ systemctl daemon-reload
# enable service
$ systemctl enable promtail.service
```

#### MacOSX

_TO BE DONE_

### Grafana

#### Loki DataSource

The Loki DataSource is provisioned automatically on the _Grafana Cloud_ instance. In case you run a on-premise instance, follow the configuration as described under `Loki` at `Grafana Cloud Portal`.

## Usage

_TO BE DONE_