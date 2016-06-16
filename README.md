OpenShift Elasticsearch Cartridge
=================================
This cartridge provides an Elasticsearch cluster as a standalone application Elasticsearch.

To create your Elasticsearch app, run:

    rhc app-create https://github.com/normnorm/openshift-elasticsearch/blob/master/metadata/manifest.yml -a <app>

If you want to create a Elasticsearch cluster, append the flag `--scaling`:

    rhc app-create https://github.com/normnorm/openshift-elasticsearch/blob/master/metadata/manifest.yml -a <app> --scaling

### Adding extra nodes to cluster
To add more nodes to the cluster, simply add more gears:

    rhc cartridge-scale -a <app> elasticsearch <number of total gears you want>

### Plugins
To install Elasticsearch plugins, edit the `plugins.txt` file, commit, and push your changes.

    cd elasticsearch
    vim plugins.txt
    git commit -a -m 'add plugin x'
    git push

You can also install plugins from a .zip file. Simply place it inside dir `plugins/`, git add, commit and push.

Plugin urls will be served off of `/elasticsearch/_plugin/<name>/`.

### Configuration

#### Elasticsearch
Elasticsearch configuration is built on-the-fly with the file `config/elasticsearch.yml.erb`, concatenated with any other files found in that same dir (except for `logging.yml` and `elasticsearch.in.sh`). Files ending with `.erb` will be pre-processed using ruby's erb command.

#### Nginx
Nginx is configured by editing the file `nginx.conf.erb` from the git repo's root.

#### Kibana
Kibana be accessed by `port-forward`ing to your rhc client and using a local install of kibana to access.  This is a better alternative than having the kibana overhead on small gears and exposing it to the world through an open port. 

#### Security

Authentication and authorization is handled by NGINX - in the intrest of keeping things simple and free - sheild is not required for basic security.

You can create a basicauth login and add it to the file:
`template/htpasswd`

An example login is present you should make this your own.


### License
This cartridge is [MIT](http://opensource.org/licenses/MIT) licensed.
