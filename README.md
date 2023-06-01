# jupyter-jsc-custom-share-template
Template repository for custom JupyterHub templates and static files.

## Templates

This branch contains any templates that should be changed on the staging JupyterHub instance. Templates are written in [Jinja](https://jinja.palletsprojects.com/en/3.1.x/). For more information, refer to the [official JupyterHub documentation](https://jupyterhub.readthedocs.io/en/stable/reference/templates.html).

### Suggested changes for a custom JupyterHub

* Change header and footer as desired. Any logos should be placed in the (staging) `static` branch. 

* You may want to update the meta data in `page.html`.

* You may want to change the login text in `<div id="upper-login-div">` and carousel texts via the `carousel_item` Jinja macros in `login.html`.

* If you want to link to different or additional pages, update `links.html` (`link_list` Jinja macro) and `sidebar.html` (`ul` Jinja macro).

* The privacy policy may be updated in `dps.html`.

## Available variables

Some variables are passed from JupyterHub and available as Jinja variables in the templates. The following variables are available in all templates:

```python
base_url  # hub.base_url
prefix  # base_url
user  
login_url
login_service
logout_url
static_url  # url location of static files
version_hash  # e.g. datetime.now().strftime("%Y%m%d%H%M%S")
services  # list of services returned by `get_accessible_services(user)`
parsed_scopes  # scopes for a user
expanded_scopes  

# and any variables defined in under `template_vars` in the JupyterHub config file
```

## Why won't my template render correctly? - Testing locally
Jupyter-JSC uses a custom JupyterHub installation which passes some variables your local installation is unlikely to have. Most importantly, the line 
```
ns.update(dict(get_template=self.get_template))
```
is added to the [`template_namespace`](https://github.com/jupyterhub/jupyterhub/blob/main/jupyterhub/handlers/base.py#L1273), so that template paths can be determined per JupyterHub installation. You can either patch your JupyterHub to include this line as well or replace any `get_template("my_template").name` with the correct path for local testing instead. If you choose the latter approach, don't forget to change it back when pushing to the repository, or the paths won't get resolved correctly on our side.

In addition, custom JupyterHub variables include `incident_messages`, `custom_config`, `maintenance_list`, `spawner_options_form` and `additional_spawn_options`. These variables should affect only the templates `home.html`, `spawn_pending.html` and `footer.html`.

You can try to get around this by initializing these variables directly via Jinja in some templates, although this might not work in all cases. For example in `footer.html`:

```jinja
{% set incident_messages = {} %}
```

If you still run into trouble, check if you have added any necessary template variables to your JupyterHub config file, see [here](https://github.com/FZJ-JSC/jupyter-jsc-custom-share-template/tree/main#how-to-test-custom-files).