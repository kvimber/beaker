# How To Upgrade from 2.y to 3.0

This is a guide detailing all the issues to be aware of, and to help people make
any changes that you might need to move from beaker 2.y to 3.0.

## Locally Cached Files

This is a change of the `:cache_files_locally` preset from `true` to `false`.

At this time, the `:cache_files_locally` setting only affects the
[`fetch_http_file` method](https://github.com/puppetlabs/beaker/blob/master/lib/beaker/dsl/helpers/web_helpers.rb#L44).
This is an internal method used in both Puppet Enterprise (PE) and Open Source
Puppet install helpers to download files from the internet to the Beaker
coordinator.

If a file with the same destination name already exists on the coordinator,
Beaker would not fetch the file and use the cached copy instead. In general,
this wasn't a big problem because we typically have our version numbers in our
install artifacts, so file name matching is enough. In our Windows MSI
installers, however, we would many times not have versions built into the file
name. Since that's the case, you could get an old version installed because it
was already on your coordinator filesystem. The `:cache_files_locally` setting
allows you to set whether you want to use a local file cache, or get fresh
installers every time. This setting is now set to false, and will get installers
from the online source every time.

If you'd like to keep this setting the way it was in 2.y, then just set the
global option `:cache_files_locally` to `false`. Checkout the
[Argument Processing and Precedence](../concepts/argument_processing_and_precedence.md)
doc for info on how to do this.