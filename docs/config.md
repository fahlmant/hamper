# Configuration

## Core configuration

Configuration for hamper is stored in `hamper.conf`, which is parsed as
[YAML][]. It can also be read from environment variables, as detailed below.

[YAML]: http://www.yaml.com

The core of hamper only uses a few configuration options.

* `nickname` - The nick to use when connecting to IRC. (Required)
* `server` - The hostname of the server to connect to. Try something like
  `irc.freenode.net`. (Required)
* `port` - The port to connect to the server on. Try `6667`. Make sure to use 
  an SSL enabled port if `ssl` is enabled. (Required)
* `db` - This is a database URL as described [in the SQLAlechmy docs][dburl].
  This is not required. If you leave it unspecified, Hamper will use a
  in-memory database instead. The url `sqlite:///hamper.db` will make a sqlite
  database in a file named `hamper.db` in the current directory.
* `channels` - A list of channels to join, including the leading `#`.
* `plugins` - A list of plugins to load.
* `ssl` - If true, Hamper will use SSL to connect to the server.
* `password` - Iff this is supplied, Hamper will attempt to authenticate with
  NickServ using `nickname` and this password. If this happens, it will delay
  loading channels until after NickServ has responded.

[dburl]: http://www.sqlalchemy.org/docs/core/engines.html#sqlalchemy.create_engine

## Plugin Configuration

Some plugins require extra configuration. These settings go in the same config
file as the core settings above. For each of these plugins, the settings go
under a key of the plugin's name. For example, the configuration for `tinyurl`
might look like this in `hamper.conf`:

```yaml
tinyurl:
    excluded-urls:
        - imgur.com
        - gist.github.com
```

> External plugins may require other configuration not listed here.

### [Tinyurl][]

This plugin will automatically shorten long urls pasted into the channel.

* `min-length` - The length of the shortest url that the plugin will attempt to
  shorten. (Default: 40)
* `excluded-urls` - A list of urls to ignore. If any of these a substring of
  the seen urls, they won't be shortened. (Default:
  `['imgur.com', 'gist.github.com', 'pastebin.com']`).

[Tinyurl]: plugins/tinyurl.md

### [Timez][]

The timez plugin allows you to look up local time for different timezones.

* `api-key` - An API key for the service. Get it from [here][timezapi]. The 
  free api should work fine.

[Timez]: plugins/timez.md
[timezapi]: http://developer.worldweatheronline.com 


## Environment Variables

Hamper can also pull in settings from environment variables, which is useful
for environments like [Heroku][] or [Docker][]. Environment variables are
mapped directly to top level configuration variables. For example, to change
the IRC port to 8001, you could set the environment variables `port=8001`.

If an environment variable is valid YAML, it will be parsed as such. For
example, you could configure Timez's api-key like
`timez={"api-key": "LOLAPIKEY"}`.

If you aren't sure if you need to do this, you likely don't have to. It's
pretty specialized. Be aware of any escaping you may have to do to set these
environment variables.

[Heroku]: heroku.md
[Docker]: docker.md
