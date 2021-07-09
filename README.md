# DHTutorial

What's wrong with django-haystack [issue](https://github.com/django-haystack/django-haystack/issues/1804) ？

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
django.template.exceptions.TemplateDoesNotExist: search/indexes/blog/note_text.txt
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
django.template.exceptions.TemplateDoesNotExist: search/indexes/blog/note_text.txt
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
django.template.exceptions.TemplateDoesNotExist: search/indexes/blog/note_text.txt
$
```