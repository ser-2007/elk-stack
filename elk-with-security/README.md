## ISSUE: 
 i want to install elasticsearch, kibana. logstash, metricbeat and filebeat to my remote server with ansible (i need to playbook roles and configuration), and i want to learn how-to-show-mulitple-servers-logs-in-kibana-separatly-from-each-other. and i need to configure x-pack or etc security settings in kibana elasticsearch, and for this i need to cnfigure the ansible playbooks-roles at the beginning. and the last issue is i dont want to reach everyone to my kibana dasboard.

# Solution: 

* install Elasticsearch, Kibana, Logstash, Metricbeat, and Filebeat on your remote server using Ansible, you can create a playbook with separate roles for each application.

For example, you can create a role called "elasticsearch" and define tasks for installing and configuring Elasticsearch in the tasks/main.yml file. Similarly, you can create roles for Kibana, Logstash, Metricbeat, and Filebeat.

To show logs from multiple servers separately in Kibana, you can use Logstash to ship the logs to Elasticsearch with specific metadata tags. For example, you can add a "server_name" tag to each log entry, indicating which server the log came from. Then, in Kibana, you can create separate indexes for each server using the tags and view the logs separately.

To configure security settings in Elasticsearch and Kibana, you can use X-Pack, which provides authentication, authorization, and encryption. You can create a role for configuring X-Pack, and define tasks for enabling security in Elasticsearch and Kibana.

To restrict access to your Kibana dashboard, you can use the "HTTP Basic Authentication" or "OAuth" method. You can create a role for configuring authentication, and define tasks for setting up users and passwords or configuring OAuth providers.

Here's an example playbook structure for installing Elasticsearch, Kibana, Logstash, Metricbeat, and Filebeat:

- name: Install and configure Elasticsearch
  hosts: elasticsearch_servers
  roles:
    - elasticsearch

- name: Install and configure Kibana
  hosts: kibana_servers
  roles:
    - kibana
    - xpack
    - authentication

- name: Install and configure Logstash
  hosts: logstash_servers
  roles:
    - logstash
    - xpack

- name: Install and configure Metricbeat and Filebeat
  hosts: monitoring_servers
  roles:
    - metricbeat
    - filebeat
    - xpack

You can create separate inventory files for each server group, and specify the appropriate inventory file for each playbook. For example:


[elasticsearch_servers]
elasticsearch1 ansible_host=192.168.1.1
elasticsearch2 ansible_host=192.168.1.2

[kibana_servers]
kibana1 ansible_host=192.168.1.3

[logstash_servers]
logstash1 ansible_host=192.168.1.4

[monitoring_servers]
monitoring1 ansible_host=192.168.1.5

You can then run the playbook with the following command:


ansible-playbook -i inventory/elasticsearch playbook.yml

This will run the playbook for the "elasticsearch_servers" group, installing and configuring Elasticsearch. You can run the playbook for other server groups in a similar way.


Here is an example of the directory structure and YAML files for the roles used in the playbook structure:


roles/
├── elasticsearch/
│   ├── defaults/
│   │   └── main.yml
│   ├── files/
│   ├── handlers/
│   │   └── main.yml
│   ├── meta/
│   │   └── main.yml
│   ├── tasks/
│   │   ├── main.yml
│   │   ├── plugins.yml
│   │   └── templates.yml
│   ├── templates/
│   │   ├── elasticsearch.yml.j2
│   │   └── jvm.options.j2
│   ├── tests/
│   │   ├── inventory/
│   │   └── test.yml
│   └── vars/
│       └── main.yml
├── kibana/
│   ├── defaults/
│   │   └── main.yml
│   ├── files/
│   ├── handlers/
│   │   └── main.yml
│   ├── meta/
│   │   └── main.yml
│   ├── tasks/
│   │   ├── main.yml
│   │   ├── plugins.yml
│   │   └── templates.yml
│   ├── templates/
│   │   └── kibana.yml.j2
│   ├── tests/
│   │   ├── inventory/
│   │   └── test.yml
│   └── vars/
│       └── main.yml
├── logstash/
│   ├── defaults/
│   │   └── main.yml
│   ├── files/
│   ├── handlers/
│   │   └── main.yml
│   ├── meta/
│   │   └── main.yml
│   ├── tasks/
│   │   ├── main.yml
│   │   ├── plugins.yml
│   │   └── templates.yml
│   ├── templates/
│   │   └── logstash.conf.j2
│   ├── tests/
│   │   ├── inventory/
│   │   └── test.yml
│   └── vars/
│       └── main.yml
├── metricbeat/
│   ├── defaults/
│   │   └── main.yml
│   ├── files/
│   ├── handlers/
│   │   └── main.yml
│   ├── meta/
│   │   └── main.yml
│   ├── tasks/
│   │   ├── main.yml
│   │   ├── plugins.yml
│   │   └── templates.yml
│   ├── templates/
│   │   └── metricbeat.yml.j2
│   ├── tests/
│   │   ├── inventory/
│   │   └── test.yml
│   └── vars/
│       └── main.yml
├── filebeat/
│   ├── defaults/
│   │   └── main.yml
│   ├── files/
│   ├── handlers/
│   │   └── main.yml
│   ├── meta/
│   │   └── main.yml
│   ├── tasks/
│   │   ├── main.yml
│   │   ├── plugins.yml
│   │   └── templates.yml
│   ├── templates/
│   │   └── filebeat.yml.j2
│   ├── tests/
│   │   ├── inventory/
│   │   └── test.yml
│   └── vars/
│       └── main.yml
├── xpack/
│   ├── defaults/
│   │   └──
│   ├── files/
│   ├── handlers/
│   │   └── main.yml
│   ├── meta/
│   │   └── main.yml
│   ├── tasks/
│   │   ├── main.yml
│   │   ├── users.yml
│   │   └── roles.yml
│   ├── templates/
│   │   ├── kibana.yml.j2
│   │   ├── elasticsearch.yml.j2
│   │   ├── users_roles.yml.j2
│   │   └── kibana_users_roles.yml.j2
│   ├── tests/
│   │   ├── inventory/
│   │   └── test.yml
│   └── vars/
│       └── main.yml


Each role has a defaults, files, handlers, meta, tasks, templates, tests, and vars directory. Here is a brief description of each directory:

defaults: contains default variables for the role
files: contains static files that are copied to the remote server
handlers: contains handler tasks that are triggered by other tasks
meta: contains metadata for the role, such as dependencies
tasks: contains the main tasks for the role
templates: contains Jinja2 templates used in the role
tests: contains test scenarios for the role
vars: contains variables used in the role


Here are the YAML files for each role:

roles/elasticsearch/defaults/main.yml:



elasticsearch_version: "7.16.3"
elasticsearch_port: 9200

roles/elasticsearch/handlers/main.yml:

yaml

- name: restart elasticsearch
  systemd:
    name: elasticsearch
    state: restarted

roles/elasticsearch/meta/main.yml:

yaml

dependencies:
  - role: java

roles/elasticsearch/tasks/main.yml:

yaml

- name: install elasticsearch GPG key
  apt_key:
    url: https://artifacts.elastic.co/GPG-KEY-elasticsearch
    state: present

- name: add elasticsearch apt repository
  apt_repository:
    repo: deb https://artifacts.elastic.co/packages/{{ elasticsearch_version }}/apt stable main
    state: present
    filename: elasticsearch

- name: install elasticsearch
  apt:
    name: elasticsearch
    state: latest

- name: configure elasticsearch
  template:
    src: elasticsearch.yml.j2
    dest: /etc/elasticsearch/elasticsearch.yml
    mode: "0644"

- name: configure jvm.options
  template:
    src: jvm.options.j2
    dest: /etc/elasticsearch/jvm.options
    mode: "0644"

- name: enable and start elasticsearch service
  systemd:
    name: elasticsearch
    state: started
    enabled: yes

- name: wait for elasticsearch to start up
  wait_for:
    host: localhost
    port: "{{ elasticsearch_port }}"
    state: started
    timeout: 60

- name: install elasticsearch plugins
  shell: bin/elasticsearch-plugin install "{{ item }}"
  args:
    chdir: "/usr/share/elasticsearch"
  loop:
    - ingest-geoip
    - analysis-icu

    
roles/elasticsearch/tasks/plugins.yml:

yaml

- name: install elasticsearch plugins
  shell: bin/elasticsearch-plugin install "{{ item }}"
  args:
    chdir: "/usr/share/elasticsearch"
  loop:
    - ingest-geoip



You can create separate inventory files for each server group, and specify the appropriate inventory file for each playbook. For example:
[elasticsearch_servers]
elasticsearch1 ansible_host=192.168.1.1
elasticsearch2 ansible_host=192.168.1.2

[kibana_servers]
kibana1 ansible_host=192.168.1.3

[logstash_servers]
logstash1 ansible_host=192.168.1.4

[monitoring_servers]
monitoring1 ansible_host=192.168.1.5

You can then run the playbook with the following command:

ansible-playbook -i inventory/elasticsearch playbook.yml

This will run the playbook for the "elasticsearch_servers" group, installing and configuring Elasticsearch. You can run the playbook for other server groups in a similar way.

roles/
├── elasticsearch/
│   ├── defaults/
│   │   └── main.yml
│   ├── files/
│   ├── handlers/
│   │   └── main.yml
│   ├── meta/
│   │   └── main.yml
│   ├── tasks/
│   │   ├── main.yml
│   │   ├── plugins.yml
│   │   └── templates.yml
│   ├── templates/
│   │   ├── elasticsearch.yml.j2
│   │   └── jvm.options.j2
│   ├── tests/
│   │   ├── inventory/
│   │   └── test.yml
│   └── vars/
│       └── main.yml
├── kibana/
│   ├── defaults/
│   │   └── main.yml
│   ├── files/
│   ├── handlers/
│   │   └── main.yml
│   ├── meta/
│   │   └── main.yml
│   ├── tasks/
│   │   ├── main.yml
│   │   ├── plugins.yml
│   │   └── templates.yml
│   ├── templates/
│   │   └── kibana.yml.j2
│   ├── tests/
│   │   ├── inventory/
│   │   └── test.yml
│   └── vars/
│       └── main.yml
├── logstash/
│   ├── defaults/
│   │   └── main.yml
│   ├── files/
│   ├── handlers/
│   │   └── main.yml
│   ├── meta/
│   │   └── main.yml
│   ├── tasks/
│   │   ├── main.yml
│   │   ├── plugins.yml
│   │   └── templates.yml
│   ├── templates/
│   │   └── logstash.conf.j2
│   ├── tests/
│   │   ├── inventory/
│   │   └── test.yml
│   └── vars/
│       └── main.yml
├── metricbeat/
│   ├── defaults/
│   │   └── main.yml
│   ├── files/
│   ├── handlers/
│   │   └── main.yml
│   ├── meta/
│   │   └── main.yml
│   ├── tasks/
│   │   ├── main.yml
│   │   ├── plugins.yml
│   │   └── templates.yml
│   ├── templates/
│   │   └── metricbeat.yml.j2
│   ├── tests/
│   │   ├── inventory/
│   │   └── test.yml
│   └── vars/
│       └── main.yml
├── filebeat/
│   ├── defaults/
│   │   └── main.yml
│   ├── files/
│   ├── handlers/
│   │   └── main.yml
│   ├── meta/
│   │   └── main.yml
│   ├── tasks/
│   │   ├── main.yml
│   │   ├── plugins.yml
│   │   └── templates.yml
│   ├── templates/
│   │   └── filebeat.yml.j2
│   ├── tests/
│   │   ├── inventory/
│   │   └── test.yml
│   └── vars/
│       └── main.yml
├── xpack/
│   ├── defaults/
│   │   └──
│   ├── files/
│   ├── handlers/
│   │   └── main.yml
│   ├── meta/
│   │   └── main.yml
│   ├── tasks/
│   │   ├── main.yml
│   │   ├── users.yml
│   │   └── roles.yml
│   ├── templates/
│   │   ├── kibana.yml.j2
│   │   ├── elasticsearch.yml.j2
│   │   ├── users_roles.yml.j2
│   │   └── kibana_users_roles.yml.j2
│   ├── tests/
│   │   ├── inventory/
│   │   └── test.yml
│   └── vars/
│       └── main.yml

Each role has a defaults, files, handlers, meta, tasks, templates, tests, and vars directory. Here is a brief description of each directory:

defaults: contains default variables for the role
files: contains static files that are copied to the remote server
handlers: contains handler tasks that are triggered by other tasks
meta: contains metadata for the role, such as dependencies
tasks: contains the main tasks for the role
templates: contains Jinja2 templates used in the role
tests: contains test scenarios for the role
vars: contains variables used in the role
Here are the YAML files for each role:

roles/elasticsearch/defaults/main.yml:

elasticsearch_version: "7.16.3"
elasticsearch_port: 9200


roles/elasticsearch/handlers/main.yml:

- name: restart elasticsearch
  systemd:
    name: elasticsearch
    state: restarted



roles/elasticsearch/meta/main.yml:
dependencies:
  - role: java


roles/elasticsearch/tasks/main.yml:
- name: install elasticsearch GPG key
  apt_key:
    url: https://artifacts.elastic.co/GPG-KEY-elasticsearch
    state: present

- name: add elasticsearch apt repository
  apt_repository:
    repo: deb https://artifacts.elastic.co/packages/{{ elasticsearch_version }}/apt stable main
    state: present
    filename: elasticsearch

- name: install elasticsearch
  apt:
    name: elasticsearch
    state: latest

- name: configure elasticsearch
  template:
    src: elasticsearch.yml.j2
    dest: /etc/elasticsearch/elasticsearch.yml
    mode: "0644"

- name: configure jvm.options
  template:
    src: jvm.options.j2
    dest: /etc/elasticsearch/jvm.options
    mode: "0644"

- name: enable and start elasticsearch service
  systemd:
    name: elasticsearch
    state: started
    enabled: yes

- name: wait for elasticsearch to start up
  wait_for:
    host: localhost
    port: "{{ elasticsearch_port }}"
    state: started
    timeout: 60

- name: install elasticsearch plugins
  shell: bin/elasticsearch-plugin install "{{ item }}"
  args:
    chdir: "/usr/share/elasticsearch"
  loop:
    - ingest-geoip
    - analysis-icu



roles/elasticsearch/tasks/plugins.yml:

- name: install elasticsearch plugins
  shell: bin/elasticsearch-plugin install "{{ item }}"
  args:
    chdir: "/usr/share/elasticsearch"
  loop:
    - ingest-geoip

'''''


roles/kibana/defaults/main.yml:

kibana_version: "7.16.3"
kibana_port: 5601
elasticsearch_url: "http://localhost:{{ elasticsearch_port }}"



roles/kibana/handlers/main.yml:

- name: restart kibana
  systemd:
    name: kibana
    state: restarted




roles/kibana/meta/main.yml:

dependencies:
  - role: elasticsearch





roles/kibana/tasks/main.yml:


- name: install kibana GPG key
  apt_key:
    url: https://artifacts.elastic.co/GPG-KEY-elasticsearch
    state: present

- name: add kibana apt repository
  apt_repository:
    repo: deb https://artifacts.elastic.co/packages/{{ kibana_version }}/apt stable main
    state: present
    filename: kibana

- name: install kibana
  apt:
    name: kibana
    state: latest

- name: configure kibana
  template:
    src: kibana.yml.j2
    dest: /etc/kibana/kibana.yml
    mode: "0644"

- name: enable and start kibana service
  systemd:
    name: kibana
    state: started
    enabled: yes

- name: wait for kibana to start up
  wait_for:
    host: localhost
    port: "{{ kibana_port }}"
    state: started
    timeout: 60





roles/logstash/defaults/main.yml:

logstash_version: "7.16.3"
elasticsearch_host: "localhost"
elasticsearch_port: 9200


roles/logstash/handlers/main.yml:

- name: restart logstash
  systemd:
    name: logstash
    state: restarted

roles/logstash/meta/main.yml:

- name: install logstash GPG key
  apt_key:
    url: https://artifacts.elastic.co/GPG-KEY-elasticsearch
    state: present

- name: add logstash apt repository
  apt_repository:
    repo: deb https://artifacts.elastic.co/packages/{{ logstash_version }}/apt stable main
    state: present
    filename: logstash

- name: install logstash
  apt:
    name: logstash
    state: latest

- name: configure logstash
  template:
    src: logstash.conf.j2
    dest: /etc/logstash/conf.d/logstash.conf
    mode: "0644"

- name: enable and start logstash service
  systemd:
    name: logstash
    state: started
    enabled: yes

- name: wait for logstash to start up
  wait_for:
    host: localhost
    port: 9600
    state: started
    timeout: 60




roles/metricbeat/defaults/main.yml:

metricbeat_version: "7.16.3"
elasticsearch_host: "localhost"
elasticsearch_port: 9200


roles/metricbeat/handlers/main.yml:


- name: restart metricbeat
  systemd:
    name: metricbeat




roles/metricbeat/meta/main.yml:

dependencies:
  - role: elasticsearch





roles/metricbeat/tasks/main.yml:

- name: install metricbeat GPG key
  apt_key:
    url: https://artifacts.elastic.co/GPG-KEY-elasticsearch
    state: present

- name: add metricbeat apt repository
  apt_repository:
    repo: deb https://artifacts.elastic.co/packages/{{ metricbeat_version }}/apt stable main
    state: present
    filename: metricbeat

- name: install metricbeat
  apt:
    name: metricbeat
    state: latest

- name: configure metricbeat
  template:
    src: metricbeat.yml.j2
    dest: /etc/metricbeat/metricbeat.yml
    mode: "0644"

- name: enable and start metricbeat service
  systemd:
    name: metricbeat
    state: started
    enabled: yes

- name: wait for metricbeat to start up
  wait_for:
    host: localhost
    port: 5066
    state: started
    timeout: 60




roles/filebeat/defaults/main.yml:

filebeat_version: "7.16.3"
elasticsearch_host: "localhost"
elasticsearch_port: 9200



roles/filebeat/handlers/main.yml

- name: restart filebeat
  systemd:
    name: filebeat
    state: restarted


roles/filebeat/meta/main.yml:

dependencies:
  - role: elasticsearch




roles/filebeat/tasks/main.yml:

- name: install filebeat GPG key
  apt_key:
    url: https://artifacts.elastic.co/GPG-KEY-elasticsearch
    state: present

- name: add filebeat apt repository
  apt_repository:
    repo: deb https://artifacts.elastic.co/packages/{{ filebeat_version }}/apt stable main
    state: present
    filename: filebeat

- name: install filebeat
  apt:
    name: filebeat
    state: latest

- name: configure filebeat
  template:
    src: filebeat.yml.j2
    dest: /etc/filebeat/filebeat.yml
    mode: "0644"

- name: enable and start filebeat service
  systemd:
    name: filebeat
    state: started
    enabled: yes

- name: wait for filebeat to start up
  wait_for:
    host: localhost
    port: 5067
    state: started
    timeout: 60



templates/kibana.yml.j2

server.port: {{ kibana_port }}
server.host: "0.0.0.0"
elasticsearch.hosts: ["{{ elasticsearch_url }}"]




templates/logstash.conf.j2:

put {
  beats {
    port => 5044
  }
}
output {
  elasticsearch {
    hosts => ["{{ elasticsearch_host }}:{{ elasticsearch_port }}"]
    index => "logstash-%{+YYYY.MM.dd}"
  }
}





Or : roles/logstash/files/logstash.conf: (BUT ITS IS VERY COMPLICATED)


input {
  beats {
    port => 5044
  }
}

filter {
  if [fields][type] == "system" {
    grok {
      match => { "message" => "%{SYSLOGTIMESTAMP:syslog_timestamp} %{SYSLOGHOST:syslog_hostname} %{DATA:syslog_program}(?:\[%{POSINT:syslog_pid}\])?: %{GREEDYDATA:syslog_message}" }
      add_field => [ "received_at", "%{@timestamp}" ]
      add_field => [ "received_from", "%{host}" ]
    }
    syslog_pri { }
    date {
      match => [ "syslog_timestamp", "MMM  d HH:mm:ss", "MMM dd HH:mm:ss" ]
    }
  }
  if [fields][type] == "nginx" {
    grok {
      match => { "message" => "%{IPORHOST:http_host} %{USER:http_user} \[%{HTTPDATE:http_timestamp}\] \"%{WORD:http_method} %{DATA:http_request} HTTP/%{NUMBER:http_version}\" %{NUMBER:http_status} %{NUMBER:http_response_body_bytes} \"%{DATA:http_referer}\" \"%{DATA:http_user_agent}\"" }
      add_field => [ "received_at", "%{@timestamp}" ]
      add_field => [ "received_from", "%{host}" ]
    }
    date {
      match => [ "http_timestamp", "dd/MMM/YYYY:H:m:s Z" ]
    }
  }
  if [fields][type] == "apache" {
    grok {
      match => { "message" => "%{COMBINEDAPACHELOG}" }
      add_field => [ "received_at", "%{@timestamp}" ]
      add_field => [ "received_from", "%{host}" ]
    }
    date {
      match => [ "timestamp", "dd/MMM/YYYY:H:m:s Z" ]
    }
  }
  if [fields][type] == "mysql" {
    grok {
      match => { "message" => "%{TIMESTAMP_ISO8601:mysql_timestamp} %{INT:mysql_thread_id} %{WORD:mysql_command} %{GREEDYDATA:mysql_message}" }
      add_field => [ "received_at", "%{@timestamp}" ]
      add_field => [ "received_from", "%{host}" ]
    }
    date {
      match => [ "mysql_timestamp", "YYYY-MM-dd HH:mm:ss" ]
    }
  }
  if [fields][type] == "redis" {
    grok {
      match => { "message" => "%{TIMESTAMP_ISO8601:redis_timestamp} %{LOGLEVEL:redis_log_level} %{GREEDYDATA:redis_message}" }
      add_field => [ "received_at", "%{@timestamp}" ]
      add_field => [ "received_from", "%{host}" ]
    }
    date {
      match => [ "redis_timestamp", "YYYY-MM-dd HH:mm:ss,SSS" ]
    }
  }
}

output {
  elasticsearch {
    hosts => ["{{ elasticsearch_host }}:{{ elasticsearch_port }}"]
    index => "%{[@metadata][beat]}-%{[@metadata][version]}-%{+YYYY.MM.dd}"
    user => "{{ elasticsearch_username }}"
    password => "{{ elasticsearch_password }}"
  }
}

* Bu Logstash konfigürasyon dosyası, farklı türdeki logların toplanması, ayrıştırılması ve Elasticsearch'e gönderilmesi için tasarlanmıştır. Aşağıdaki adımları içerir:
        Input: Beats protokolünü kullanarak log verilerini alır.
        Filter: Alınan log verilerini, türlerine göre ayrıştırır ve düzenler. Bu örnekte, sistem, nginx, apache, mysql ve redis logları için ayrı ayrı ayrıştırma kuralları tanımlanmıştır. Sistem loglarındaki öğeler SYSLOGTIMESTAMP, SYSLOGHOST, DATA, ve GREEDYDATA gibi kalıplarla ayıklanır. 
        Nginx loglarındaki öğeler IPORHOST, USER, HTTPDATE, WORD, NUMBER ve DATA gibi kalıplarla ayıklanır. 
        Apache logları için ise COMBINEDAPACHELOG kalıbı kullanılmıştır. MySQL logları için TIMESTAMP_ISO8601, INT, WORD ve GREEDYDATA gibi kalıplar kullanılırken Redis logları için TIMESTAMP_ISO8601 ve LOGLEVEL kullanılmıştır.
        Output: Ayrıştırılmış log verileri Elasticsearch'e gönderilir. Elasticsearch URL, kullanıcı adı ve şifresi Logstash konfigürasyonunda belirtilir. 
        Index adı, metadata ve tarih kullanılarak oluşturulur.
        Bu konfigürasyon dosyası, Elasticsearch'e log verilerini yazmak için kullanılır ve Elasticsearch'in IP adresi ve bağlantı noktası gibi bilgiler, 
        Playbook dosyasında belirtilen değişkenler ile değiştirilebilir. Ayrıca, Kibana'ya erişimi kısıtlamak için Logstash konfigürasyonunda bir filtre eklenmemiştir, ancak bu filtreler eklenebilir. '''


templates/metricbeat.yml.j2

metricbeat.modules:
- module: system
  syslog:
    enabled: true
    var.paths: ["/var/log/syslog"]
  auth:
    enabled: true
    var.paths: ["/var/log/auth.log"]
- module: nginx
  access:
    enabled: true
    var.paths: ["/var/log/nginx/access.log"]
  error:
    enabled: true
    var.paths: ["/var/log/nginx/error.log"]
- module: mysql
  enabled: true
  metricsets:
    - "status"
    - "performance"
  period: 10s
  hosts: ["localhost:3306"]
  username: "{{ mysql_username }}"
  password: "{{ mysql_password }}"
- module: apache
  access:
    enabled: true
    var.paths: ["/var/log/apache2/access.log"]
  error:
    enabled: true
    var.paths: ["/var/log/apache2/error.log"]
- module: redis
  enabled: true
  metricsets:
    - "info"
  period: 10s
  hosts: ["localhost:6379"]






templates/filebeat.yml.j2:

filebeat.inputs:
- type: log
  enabled: true
  paths:
    - /var/log/*.log
    - /var/log/messages
    - /var/log/syslog
  exclude_files:
    - '*.gz'
  fields:
    type: system
- type: log
  enabled: true
  paths:
    - /var/log/nginx/*.log
  exclude_files:
    - '*.gz'
  fields:
    type: nginx
- type: log
  enabled: true
  paths:
    - /var/log/apache2/*.log
  exclude_files:
    - '*.gz'
  fields:
    type: apache
- type: log
  enabled: true
  paths:
    - /var/log/mysql/*.log
  exclude_files:
    - '*.gz'
  fields:
    type: mysql
- type: log
  enabled: true
  paths:
    - /var/log/redis/*.log
  exclude_files:
    - '*.gz'
  fields:
    type: redis
output.elasticsearch:
  hosts: ["{{ elasticsearch_host }}:{{ elasticsearch_port }}"]
  index: "filebeat-%{+yyyy.MM.dd}"
  username: "{{ elasticsearch_username }}"
  password: "{{ elasticsearch_password }}"




