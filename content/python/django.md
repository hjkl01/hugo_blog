---
title: "django"
draft: true
---

```sh
import os
import django
from proxyip.models import ProxyIP

os.environ['DJANGO_SETTINGS_MODULE'] = 'dj_project.settings'
django.setup()

p = ProxyIP(ip='192.168.50.1')
p.save()
print(ProxyIP.objects.all())

python manage.py shell < main.py
```

```sh
python manage.py dumpdata (myapp) > myapp.json

python manage.py loaddata myapp.json
```
