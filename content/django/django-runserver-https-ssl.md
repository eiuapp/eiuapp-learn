
How can I test https connections with Django as easily as I can non-https connections using 'runserver'?

## 解决方式1

https://stackoverflow.com/questions/8023126/how-can-i-test-https-connections-with-django-as-easily-as-i-can-non-https-connec/28933593#28933593

## 解决方式2

Similar to django-sslserver you could use RunServerPlus from django-extensions

It has dependencies on Werkzeug (so you get access to the excellent Werkzeug debugger) and pyOpenSSL (only required for ssl mode) so to install run:

`pip install django-extensions Werkzeug pyOpenSSL`

Add it to INSTALLED_APPS in your projects settings.py file:

```
INSTALLED_APPS = (
    ...
    'django_extensions',
    ...
)
```
Then you can run the server in ssl mode with:

```
./manage.py runserver_plus --cert /tmp/cert
```

This will create a cert file at /tmp/cert.crt and a key file at /tmp/cert.key which can then be reused for future sessions.

There is a bunch of extra stuff included in django-extensions that you may find of use so it is worth having a quick flick through the docs.

### 实操

上面的整个原理是正确的，但是，在实操的时候，因为包兼容的问题，出现了报错。

```
python manage.py runserver_plus --cert-file ./server.crt 
```

![python-manage-runserver_plus-error](https://res.cloudinary.com/dmtixvmgt/image/upload/v1551325883/python-manage-runserver_plus-error_uxtvss.png)

确认一下 是不是添加进了 installed-app

![django-installed-apps](https://res.cloudinary.com/dmtixvmgt/image/upload/v1551325882/django-installed-apps_ggswgc.png)

确认一下包是不是安装了

![django-pip-list-pyOpenSSL_wrelyj](https://res.cloudinary.com/dmtixvmgt/image/upload/v1551325882/django-pip-list-pyOpenSSL_wrelyj.png)

那就没有理由会报错了。那可能是安装依赖的问题了。

通过 python 后 `import OpenSSL` 果然，发现错误了。

最后发现是错误在 `cryptography` 这个包上了。

`import cryptography` 后就报错，那就尝试把 包降一下版本，版本查找在[这里](https://pypi.org/project/cryptography/#history)。

把 cryptography 降低到 2.3.1 就可以了。

然后 `import OpenSSL` 成功。

```
python manage.py runserver_plus --cert-file ./server.crt 
```

成功。问题解决。




## ref
- https://stackoverflow.com/questions/8023126/how-can-i-test-https-connections-with-django-as-easily-as-i-can-non-https-connec/28933593#28933593


