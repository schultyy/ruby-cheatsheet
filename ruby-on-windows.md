# Ruby on Windows

Using ruby on Windows together with a proxy server requires some work.

```Bash

    set HTTP_PROXY=http://user:password@proxy:port/

    gem install <name here> --http-proxy --source http://rubygems.org
```
