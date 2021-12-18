# grafana-loki-wakfu

_When Wakfu (or other diverse logs) meets Grafana/Loki_

A guide to get data-based inside on your play behavior and efficiency when you have only access to the games logfile.

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
# start service
$ systemctl start promtail.service
```

#### MacOSX

_TO BE DONE_

### Grafana

#### Loki DataSource

The Loki DataSource is provisioned automatically on the _Grafana Cloud_ instance. In case you run a on-premise instance, follow the configuration as described under `Loki` at `Grafana Cloud Portal` to configure your Loki Datasource properly.

## Checks

### Check System Service

Check that the system service is running and there are no errors pointed out in the _journal_.

```bash
$ systemctl is-enabled promtail.service
enabled
$ systemctl is-active promtail.service
active
$ journalctl --lines=6 -u promtail.service

-- Logs begin at Mon 2021-12-13 09:38:24 CET, end at Sat 2021-12-18 20:48:05 CET. --
Dez 18 20:30:16 pc systemd[1]: Started Promtail Agent.
Dez 18 20:30:16 pc promtail[18146]: level=info ts=2021-12-18T19:30:16.871806517Z caller=server.go:260 http=[::]:41411 grpc=[::]:41527 msg="server listening on addresse
Dez 18 20:30:16 pc promtail[18146]: level=info ts=2021-12-18T19:30:16.872873818Z caller=main.go:119 msg="Starting Promtail" version="(version=2.4.1, branch=HEAD, revis
Dez 18 20:30:21 pc promtail[18146]: level=info ts=2021-12-18T19:30:21.871296989Z caller=filetargetmanager.go:255 msg="Adding target" key="/home/user/.config/zaap/wakfu
Dez 18 20:30:21 pc promtail[18146]: level=info ts=2021-12-18T19:30:21.871840572Z caller=tailer.go:126 component=tailer msg="tail routine: started" path=/home/user/.con
Dez 18 20:30:21 pc promtail[18146]: ts=2021-12-18T19:30:21.871935153Z caller=log.go:168 level=info msg="Seeked /home/user/.config/zaap/wakfu/logs/wakfu.log - &{Offset:
```

### Check Incoming Data

For logs to be analysed you need logs... So play a couple of minutes Wakfu.

1. Log in to your Grafana instance at either [\<Your-Stack>.grafana.net/explore](https://example.grafana.net/explore)

2. Select the Loki Datasource (usually `grafanacloud-\<Your-Stack>-logs`) in the top line of the window.

3. At the input filed where _Enter a Loki query (run with Shift+Enter)_ is written, enter

   ```json
   {job="wakfu"} |= "INFO"
   ```

   and press _Shift+Enter_

You should see log entries if you played Wakfu in the last hour.

## Usage

_TO BE DONE_