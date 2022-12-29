# Airflow

## DAG Skeleton

* New 

```python
from airflow.decorators import task, dag
from airflow.providers.docker.operators.docker import DockerOperator

from datetime import datetime

@dag(start_date=datetime(2021, 1, 1), schedule='@daily', catchup=False)
def docker_dag():
    @task()
    def t1():
        pass

    def t2():
        pass

    t1() >> t2()

dag = docker_dag()
```

* Old

```python
from airflow import DAG
from datetime import datetime

with DAG(dag_id='user_processing', start_date=datetime(2022, 1, 1), schedule='@daily', catchup=False) as dag:
```

## Datasets

### Producer

```python
from airflow import DAG, Dataset
from airflow.decorators import task

from datetime import date, datetime

my_file = Dataset("/tmp/my_file.txt")

with DAG(dag_id='producer', start_date=datetime(2022, 1, 1), schedule='@daily', catch=False):

    @task(outlets=[my_file])
    def update_dataset():
        with open(my_file.uri, "a+") as f:
            f.write("producer update")

    update_dataset()
```

### Consumer

```python
from airflow import DAG, Dataset
from airflow.decorators import task

from datetime import date, datetime

my_file = Dataset("/tmp/my_file.txt")

with DAG(dag_id='user_processing', start_date=datetime(2022, 1, 1), schedule=[my_file], catch=False):

    @task(outlets=[my_file])
    def read_dataset():
        with open(my_file.uri, "r") as f:
            print(f.read())

    read_dataset()
```

## Task Groups

* Download Tasks

```python
from airflow import DAG
from airflow.operators.bash import BashOperator
from airflow.utils.task_group import TaskGroup

def download_tasks():
    with TaskGroup("downloads", tooltip="Download Tasks") as group:
        download_a = BashOperator(
            task_id='download_a',
            bash_command='sleep 10'
        )

        download_b = BashOperator(
            task_id='download_b',
            bash_command='sleep 10'
        )
        
        return group
    
```

```python
from airflow import DAG
from datetime import datetime
import group.group_downloads import download_tasks

with DAG(dag_id='user_processing', start_date=datetime(2022, 1, 1), schedule='@daily', catch=False) as dag:

    downlods = download_tasks()
```

## XCOM

```python
from airflow import DAG
from airflow.operators.bash import BashOperator
from airflow.operators.python import PythonOperator
from airflow.utils.task_group import TaskGroup

from random import uniform
from datetime import datetime

default_args = {
    'start_date': datetime(2020, 1, 1)
}

def _t1(ti):
    ti.xcom_push(key='my_key', value=42)

def _t2(ti):
    ti.xcom_pull(key='my_key', task_ids='t1')

with DAG('xcom_dag', schedule_interval='@daily', default_args=default_args, catchup=False) as dag:

    downloading_data = BashOperator(
        task_id='downloading_data',
        bash_command='sleep 3'
    )

    with TaskGroup('processing_tasks') as processing_tasks:
        t1 = PythonOperator(
            task_id='t1',
            python_callable=_t1
        )

        t2 = PythonOperator(
            task_id='t2',
            python_callable=_t2
        )

    downloading_data >> processing_tasks
```

## Branch Operator

```python
from airflow import DAG
from airflow.operators.bash import BashOperator
from airflow.operators.python import PythonOperator, BranchPythonOperator

from random import uniform
from datetime import datetime

default_args = {
    'start_date': datetime(2020, 1, 1)
}

def _t1(ti):
    ti.xcom_push(key='my_key', value=42)

def _t2(ti):
    ti.xcom_pull(key='my_key', task_ids='t1')

def _branch(ti):
    value = ti.xcom_pull(key='my_key', task_ids='t1')
    if (value == 42):
        return 't2'
    return 't3'

with DAG('xcom_dag', schedule_interval='@daily', default_args=default_args, catchup=False) as dag:

    t1 = PythonOperator(
        task_id='t1',
        python_callable=_t1
    )

    branch = BranchPythonOperator(
        task_id='branch',
        python_callable=_branch
    )


    t2 = PythonOperator(
        task_id='t2',
        python_callable=_t2
    )

    t3 = BashOperator(
        task_id='t3',
        bash_command="echo ''",
        trigger_rule='all_success'
    )

    t1 >> branch >> [t2, t3]
```

## Hooks

* Hook

```python
from airflow.plugins_manager import AirflowPlugin
from airflow.hooks.base import BaseHook

from elasticsearch import Elasticsearch

class ElasticHook(BaseHook):

    def __init__(self, conn_id='elastic_default', *args, **kwargs):
        super().__init__(*args, **kwargs)
        conn = self.get_connection(conn_id)

        conn_config = {}
        hosts = []

        if conn.host:
            hosts = conn.host.split(',')
        if conn.port:
            conn_config['port'] = int(conn.port)
        if conn.login:
            conn_config['http_auth'] = (conn.login, conn.password)

        self.es = Elasticsearch(hosts, **conn_config)
        self.index = conn.schema

    def info(self):
        return self.es.info()

    def set_index(self, index):
        self.index = index

    def add_doc(self, index, doc_type, doc):
        self.set_index(index)
        res = self.es.index(index=index, doc_type=doc_type, doc=doc)
        return res

class AirflowElasticPlugin(AirflowPlugin)
    name = 'elastic'
    hooks = [ElasticHook]
```

* Use Hook

```python
from airflow import DAG
from airflow.operators.python import PythonOperator
from hooks.elastic.elastic_hook import ElasticHook

from datetime import datetime

def _print_es_info():
    hook = ElasticHook()
    print(hook.info())

with DAG('elastic_dag', start_date=datetime(2022, 1, 1), schedule_interval='@daily', catchup=False) as dag:

    print_es_info = PythonOperator(
        task_id='print_es_info',
        python_callable=_print_es_info
    )

```

## DockerOperator

```python
from airflow.decorators import task, dag
from airflow.providers.docker.operators.docker import DockerOperator

from datetime import datetime
from docker.types import Mount

@dag(start_date=datetime(2021, 1, 1), schedule='@daily', catchup=False)
def docker_dag():
    @task()
    def t1():
        pass

    t2 = DockerOperator(
        task_id='t2',
        api_version='auto',
        image='image',
        command='bash /tmp/scripts/output.sh',
        docker_url='unix://var/run/docker.sock',
        network_mode='bridge',
        xcom_all=True,
        retrieve_output=True,
        retrieve_output_path="/tmp/script.out",
        auto_remove=True,
        mounts=[
            Mount(source='path', target='/tmp/target', type='bind')
        ]
    )
       

    t1() >> t2

dag = docker_dag()
```

* Docker Script to print json

```python
import pickle

with open('/tmp/script.out', 'wb+') as tmp_file:
    pickle.dump({'test':'ok'}, tmp_file)
```


## Blog Posts

[How to use the DockerOperator with Templating and Apache Spark](https://marclamberti.com/blog/how-to-use-dockeroperator-apache-airflow/)
[Apache Airflow with Kubernetes Executor](https://marclamberti.com/blog/airflow-kubernetes-executor/)
[How to use templates and macros in Apache Airflow](https://marclamberti.com/blog/templates-macros-apache-airflow/)
[How to use timezones in Apache Airflow](https://marclamberti.com/blog/how-to-use-timezones-in-apache-airflow/)
[How to use the BashOperator](https://marclamberti.com/blog/airflow-bashoperator/)
[Variables in Apache Airflow: The Guide](https://marclamberti.com/blog/variables-with-apache-airflow/)
[Best Practices in Apache Airflow](https://marclamberti.com/blog/apache-airflow-best-practices-1/)
[The PostgresOperator: All you need to know](https://marclamberti.com/blog/the-postgresoperator-all-you-need-to-know/)
