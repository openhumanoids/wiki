Add the following to ``~/.rttlua`` (this is key and not documented!):
```
require("rttlib")
rttlib.color=true
require("complete")
```

If your ROSControl controller manager is not listed in ``rosnode list``, install ``apt-get install luarocks``, use luarocks to install luaposix ``luarocks install luaposix`` and add this to your .bashrc and resource (this is an undocumented requirement):
```
RTTLUA_MODULES=`rospack find ocl`/lua/modules/?.lua
if [ "x$LUA_PATH" == "x" ]; then
    LUA_PATH=";;"
fi
export LUA_PATH="$LUA_PATH;$RTTLUA_MODULES"
```