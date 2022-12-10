# Automating Linux Administration Tasks

## Managing Software and Subscriptions
Repositories
- yum_repository
- rpm_key

Example of implementation to do verification
<pre>
    - name: Ensure Example Repo exists
      yum_repository:
        name: example-internal
        description: Example Inc. Internal YUM repo
        file: example
        baseurl: http://materials.example.com/yum/repository/
        gpgcheck: yes

    - name: Ensure Repo RPM Key is Installed
      rpm_key:
        key: http://materials.example.com/yum/repository/RPM-GPG-KEY-example
        state: present
</pre>

