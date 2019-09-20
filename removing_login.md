
# Updating your app to the new login decorator

**Use case**: You have an app that exists on tethys.byu.edu and you would want to allow open access to at least read/display data for your users. We want to support the `ENABLE_OPEN_PORTAL` tag in the settings, which if set to `true` will allow people to access the apps without having to login. 

Add the line below to the file : `/tethys_sdk/permissions.py`

```py
from django.contrib.auth.decorators import login_required
```

In all your controller files (`controllers.py`, `ajax_controllers.py`, etc.), replace the line below:
```py
from django.contrib.auth.decorators import login_required
```
with: 
```py
from tethys_sdk.permissions import login_required
```

If there are controller functions that you still want to ensure are protected by a login, even if the `ENABLE_OPEN_PORTAL` is set to true, you will need to do the following:

1. `from django.contrib.auth.decorators import login_required as lr`
2. Use the new `lr` as the decorator for your method that you want to protect. So for example: 
    ```py
    @lr()
    def update_method(request):
    ```


That's it. Commit your code and push it up, and let me know that the code is updated on github so that I can install it on the server. 