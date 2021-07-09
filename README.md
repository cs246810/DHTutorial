# DHTutorial

What's wrong with django-haystack [issue](https://github.com/django-haystack/django-haystack/issues/1804) ？

## Test Whoosh

```shell
$ python -m venv DHTutorial-venv
$ source DHTutorial-venv/bin/active
$ git clone https://github.com/cs246810/DHTutorial
$ cd DHTutorial
$ pip install django django-haystack whoosh
$ python manage.py makemigrations blog
$ python manage.py migrate
$ python manage.py createsuperuser # username: a password: a
$ python manage.py shell
>>> from blog.models import Note
>>> from django.contrib.auth.models import User
>>> u = User.objects.get(username='a')
>>> u
<User: a>
>>> n = Note()
>>> n.user = u
>>> from django.utils import timezone
>>> n.pub_date = timezone.now()
>>> n.title = 'Hello django-haystack'
>>> n.body = 'Many thanks.'
>>> n.save()
>>> exit()
$ python manage.py rebuild_index
WARNING: This will irreparably remove EVERYTHING from your search index in connection 'default'.
Your choices after this are to restore from backups or rebuild via the `rebuild_index` command.
Are you sure you wish to continue? [y/N] y
Removing all documents from your index because you said so.
All documents removed.
/Users/congshi/PycharmProjects/DHTutorial/venv/lib/python3.8/site-packages/django/db/models/fields/__init__.py:1416: RuntimeWarning: DateTimeField Note.pub_date received a naive datetime (2021-07-09 08:14:04.071331) while time zone support is active.
  warnings.warn("DateTimeField %s received a naive datetime (%s)"
Indexing 1 notes
[ERROR/MainProcess] Failed indexing 1 - 1 (retry 5/5): search/indexes/blog/note_text.txt (pid 69318): search/indexes/blog/note_text.txt
Traceback (most recent call last):
  File "/Users/congshi/PycharmProjects/DHTutorial/venv/lib/python3.8/site-packages/haystack/management/commands/update_index.py", line 111, in do_update
    backend.update(index, current_qs, commit=commit)
  File "/Users/congshi/PycharmProjects/DHTutorial/venv/lib/python3.8/site-packages/haystack/backends/whoosh_backend.py", line 269, in update
    doc = index.full_prepare(obj)
  File "/Users/congshi/PycharmProjects/DHTutorial/venv/lib/python3.8/site-packages/haystack/indexes.py", line 236, in full_prepare
    self.prepared_data = self.prepare(obj)
  File "/Users/congshi/PycharmProjects/DHTutorial/venv/lib/python3.8/site-packages/haystack/indexes.py", line 227, in prepare
    self.prepared_data[field.index_fieldname] = field.prepare(obj)
  File "/Users/congshi/PycharmProjects/DHTutorial/venv/lib/python3.8/site-packages/haystack/fields.py", line 235, in prepare
    return self.convert(super(CharField, self).prepare(obj))
  File "/Users/congshi/PycharmProjects/DHTutorial/venv/lib/python3.8/site-packages/haystack/fields.py", line 99, in prepare
    return self.prepare_template(obj)
  File "/Users/congshi/PycharmProjects/DHTutorial/venv/lib/python3.8/site-packages/haystack/fields.py", line 212, in prepare_template
    t = loader.select_template(template_names)
  File "/Users/congshi/PycharmProjects/DHTutorial/venv/lib/python3.8/site-packages/django/template/loader.py", line 47, in select_template
    raise TemplateDoesNotExist(', '.join(template_name_list), chain=chain)
django.templates.exceptions.TemplateDoesNotExist: search/indexes/blog/note_text.txt
[ERROR/MainProcess] Error updating blog using default 
Traceback (most recent call last):
  File "/Users/congshi/PycharmProjects/DHTutorial/venv/lib/python3.8/site-packages/haystack/management/commands/update_index.py", line 274, in handle
    self.update_backend(label, using)
  File "/Users/congshi/PycharmProjects/DHTutorial/venv/lib/python3.8/site-packages/haystack/management/commands/update_index.py", line 319, in update_backend
    max_pk = do_update(
  File "/Users/congshi/PycharmProjects/DHTutorial/venv/lib/python3.8/site-packages/haystack/management/commands/update_index.py", line 111, in do_update
    backend.update(index, current_qs, commit=commit)
  File "/Users/congshi/PycharmProjects/DHTutorial/venv/lib/python3.8/site-packages/haystack/backends/whoosh_backend.py", line 269, in update
    doc = index.full_prepare(obj)
  File "/Users/congshi/PycharmProjects/DHTutorial/venv/lib/python3.8/site-packages/haystack/indexes.py", line 236, in full_prepare
    self.prepared_data = self.prepare(obj)
  File "/Users/congshi/PycharmProjects/DHTutorial/venv/lib/python3.8/site-packages/haystack/indexes.py", line 227, in prepare
    self.prepared_data[field.index_fieldname] = field.prepare(obj)
  File "/Users/congshi/PycharmProjects/DHTutorial/venv/lib/python3.8/site-packages/haystack/fields.py", line 235, in prepare
    return self.convert(super(CharField, self).prepare(obj))
  File "/Users/congshi/PycharmProjects/DHTutorial/venv/lib/python3.8/site-packages/haystack/fields.py", line 99, in prepare
    return self.prepare_template(obj)
  File "/Users/congshi/PycharmProjects/DHTutorial/venv/lib/python3.8/site-packages/haystack/fields.py", line 212, in prepare_template
    t = loader.select_template(template_names)
  File "/Users/congshi/PycharmProjects/DHTutorial/venv/lib/python3.8/site-packages/django/template/loader.py", line 47, in select_template
    raise TemplateDoesNotExist(', '.join(template_name_list), chain=chain)
django.templates.exceptions.TemplateDoesNotExist: search/indexes/blog/note_text.txt
Traceback (most recent call last):
  File "manage.py", line 22, in <module>
    main()
  File "manage.py", line 18, in main
    execute_from_command_line(sys.argv)
  File "/Users/congshi/PycharmProjects/DHTutorial/venv/lib/python3.8/site-packages/django/core/management/__init__.py", line 419, in execute_from_command_line
    utility.execute()
  File "/Users/congshi/PycharmProjects/DHTutorial/venv/lib/python3.8/site-packages/django/core/management/__init__.py", line 413, in execute
    self.fetch_command(subcommand).run_from_argv(self.argv)
  File "/Users/congshi/PycharmProjects/DHTutorial/venv/lib/python3.8/site-packages/django/core/management/base.py", line 354, in run_from_argv
    self.execute(*args, **cmd_options)
  File "/Users/congshi/PycharmProjects/DHTutorial/venv/lib/python3.8/site-packages/django/core/management/base.py", line 398, in execute
    output = self.handle(*args, **options)
  File "/Users/congshi/PycharmProjects/DHTutorial/venv/lib/python3.8/site-packages/haystack/management/commands/rebuild_index.py", line 66, in handle
    call_command("update_index", **update_options)
  File "/Users/congshi/PycharmProjects/DHTutorial/venv/lib/python3.8/site-packages/django/core/management/__init__.py", line 181, in call_command
    return command.execute(*args, **defaults)
  File "/Users/congshi/PycharmProjects/DHTutorial/venv/lib/python3.8/site-packages/django/core/management/base.py", line 398, in execute
    output = self.handle(*args, **options)
  File "/Users/congshi/PycharmProjects/DHTutorial/venv/lib/python3.8/site-packages/haystack/management/commands/update_index.py", line 274, in handle
    self.update_backend(label, using)
  File "/Users/congshi/PycharmProjects/DHTutorial/venv/lib/python3.8/site-packages/haystack/management/commands/update_index.py", line 319, in update_backend
    max_pk = do_update(
  File "/Users/congshi/PycharmProjects/DHTutorial/venv/lib/python3.8/site-packages/haystack/management/commands/update_index.py", line 111, in do_update
    backend.update(index, current_qs, commit=commit)
  File "/Users/congshi/PycharmProjects/DHTutorial/venv/lib/python3.8/site-packages/haystack/backends/whoosh_backend.py", line 269, in update
    doc = index.full_prepare(obj)
  File "/Users/congshi/PycharmProjects/DHTutorial/venv/lib/python3.8/site-packages/haystack/indexes.py", line 236, in full_prepare
    self.prepared_data = self.prepare(obj)
  File "/Users/congshi/PycharmProjects/DHTutorial/venv/lib/python3.8/site-packages/haystack/indexes.py", line 227, in prepare
    self.prepared_data[field.index_fieldname] = field.prepare(obj)
  File "/Users/congshi/PycharmProjects/DHTutorial/venv/lib/python3.8/site-packages/haystack/fields.py", line 235, in prepare
    return self.convert(super(CharField, self).prepare(obj))
  File "/Users/congshi/PycharmProjects/DHTutorial/venv/lib/python3.8/site-packages/haystack/fields.py", line 99, in prepare
    return self.prepare_template(obj)
  File "/Users/congshi/PycharmProjects/DHTutorial/venv/lib/python3.8/site-packages/haystack/fields.py", line 212, in prepare_template
    t = loader.select_template(template_names)
  File "/Users/congshi/PycharmProjects/DHTutorial/venv/lib/python3.8/site-packages/django/template/loader.py", line 47, in select_template
    raise TemplateDoesNotExist(', '.join(template_name_list), chain=chain)
django.templates.exceptions.TemplateDoesNotExist: search/indexes/blog/note_text.txt
$ pip freeze
asgiref==3.4.1
Django==3.2.5
django-haystack==3.0
pytz==2021.1
sqlparse==0.4.1
Whoosh==2.7.4
$
```

## Test Simple

上面使用了whoosh作为`HAYSTACK_CONNECTIONS`中的default的值，会出现上面的错误。然后我修改为simple之后，就不会出现这样的问题，并且在搜索页面也能搜索出结果出来。

对DHTutorial/settings.py的修改如下：
```
HAYSTACK_CONNECTIONS = {
    "default": {
        # For Simple:
        "ENGINE": "haystack.backends.simple_backend.SimpleEngine"
    },
}
```
然后继续`rebuild_index`
```shell
$ python manage.py rebuild_index
WARNING: This will irreparably remove EVERYTHING from your search index in connection 'default'.
Your choices after this are to restore from backups or rebuild via the `rebuild_index` command.
Are you sure you wish to continue? [y/N] y
Removing all documents from your index because you said so.
/Users/congshi/PycharmProjects/DHTutorial/venv/lib/python3.8/site-packages/haystack/backends/simple_backend.py:31: UserWarning: clear is not implemented in this backend
  warn("clear is not implemented in this backend")
All documents removed.
/Users/congshi/PycharmProjects/DHTutorial/venv/lib/python3.8/site-packages/django/db/models/fields/__init__.py:1416: RuntimeWarning: DateTimeField Note.pub_date received a naive datetime (2021-07-09 10:31:20.785959) while time zone support is active.
  warnings.warn("DateTimeField %s received a naive datetime (%s)"
Indexing 1 notes
/Users/congshi/PycharmProjects/DHTutorial/venv/lib/python3.8/site-packages/haystack/backends/simple_backend.py:25: UserWarning: update is not implemented in this backend
  warn("update is not implemented in this backend")
$
```
这次没有出错误，只是出现警告。然后在浏览器中搜索发现没有问题。

![](https://github.com/cs246810/DHTutorial/blob/TestHaystackConnectionSimple/use_simple.gif)

然后初步可以判断是django-haystack与whoosh集成的问题。

## Test Solr

在使用之前需要安装solr服务，但是我已经安装好了。我是直接下载的solr-8.9.0的可执行压缩包解压之后，再
启动的。

```shell
$ cd solr-8.9.0/bin
$ ./solr start
```

然后访问http://localhost:8983/solr 查看能看到下面的展示，说明安装没有问题。

![](https://github.com/cs246810/DHTutorial/blog/TestSolr/solr_start_ok.gif)

接着替换`DHTutorial/settings.py`中的`HAYSTACK_CONNECTIONS`为：
```python
HAYSTACK_CONNECTIONS = {
    'default': {
        'ENGINE': 'haystack.backends.solr_backend.SolrEngine',
        'URL': 'http://127.0.0.1:8983/solr'
        # ...or for multicore...
        # 'URL': 'http://127.0.0.1:8983/solr/mysite',
    },
}
```

由于使用了solr所以需要安装solr的驱动`pysolr`:
```shell
$ pip install pysolr
```

一切看起来是那么完美，似乎没有遇到什么问题。现在再:
```shell
$ python manage.py rebuild_index
```

这里又回出现一些其它的问题，原因是我没有配置好solr。

但是很遗憾，django-haystack官方文档上最高只支持6.x版本的solr，所以，我现在基本可以放弃探索了。
理由是，我现在用的是8.9版本的solr，现在文档上没写，那么也就是还没支持到，现在应该停止实验。

另外，solr官方文档实在太无聊，真想快点让这可怕的经历结束。