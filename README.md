# Ansible bug example - variable resolution with multiple inventories

## Monolith inventory file

```console
$ ansible site -i hosts -m ansible.builtin.debug -a "msg={{ site_name }}" 
site_1_app_1 | SUCCESS => {
    "msg": "site_1"
}
site_2_app_1 | SUCCESS => {
    "msg": "site_2"
}
site_1_web_1 | SUCCESS => {
    "msg": "site_1"
}
site_2_web_1 | SUCCESS => {
    "msg": "site_2"
}
```

As Expected

## Per 'site' inventory file used individually

```console
$ ansible site -i site_1 -m ansible.builtin.debug -a "msg={{ site_name }}"
site_1_web_1 | SUCCESS => {
    "msg": "site_1"
}
site_1_app_1 | SUCCESS => {
    "msg": "site_1"
}
$ ansible site -i site_2 -m ansible.builtin.debug -a "msg={{ site_name }}"
site_2_web_1 | SUCCESS => {
    "msg": "site_2"
}
site_2_app_1 | SUCCESS => {
    "msg": "site_2"
}
```

As Expected

## Per 'site' inventory file used individually

```console
$ ansible site -i site_1 -i site_2 -m ansible.builtin.debug -a "msg={{ site_name }}"
site_1_web_1 | SUCCESS => {
    "msg": "site_2"
}
site_2_web_1 | SUCCESS => {
    "msg": "site_2"
}
site_1_app_1 | SUCCESS => {
    "msg": "site_2"
}
site_2_app_1 | SUCCESS => {
    "msg": "site_2"
}
```

Both `site_1_web_1` and `site_1_app_1` should say `"msg": "site_1"`.


