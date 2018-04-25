---
layout: post
title: Airflow
date: 2017-08-15
categories: [airflow, notes]
---


If you've just begun to use [Airflow](https://airflow.incubator.apache.org/), the most important thing to understand is that a DAG script simply defines your DAGs. If you've used [Jenkinsfiles](https://jenkins.io/doc/book/pipeline/jenkinsfile/) before, the idea that your file describes some flow of actions will be familiar.

> Airflow Python script is really just a configuration file specifying the DAG’s structure as code...
> The script’s purpose is to define a DAG object. It needs to evaluate quickly (seconds, not minutes) since the scheduler will execute it periodically to reflect the changes if any.

([source](https://airflow.incubator.apache.org/tutorial.html#it-s-a-dag-definition-file))

> If you stop generating a dag_id, it doesn't exist anymore, as if you'd taken it out of a static config file.

([source](https://groups.google.com/d/msg/airbnb_airflow/zGf1iZnjCdE/0VsfZ0KYBQAJ))

You can use `for` loops to create a number of DAGs in a single script. You could even read in a file of objects and use each object to generate a unique DAG ID and create its tasks by templating values from the file.

### Creating and accessing a DagRun conf

**Example 1**

> ...if a task returns a value (either from its Operator’s execute() method, or from a PythonOperator’s python_callable function), then an XCom containing that value is automatically pushed.

([source](https://airflow.incubator.apache.org/concepts.html#xcoms))

```python
def implicit_push(ds, **kwargs):
    """Does a thing.

    Args:
        ds
        \**kwargs: Contains key "dag_run" which is the DagRun object.
    """
    return {"data": "my value"} # If the conf already contained data, this would overwrite it

dag = DAG(dag_id="DAG_1",
    default_args=args,
    schedule_interval=None)

# Implicitly push data into the DagRun.conf
set_data_task = PythonOperator(
    task_id="set_data_task",
    provide_context=True,
    python_callable=implicit_push,
    dag=dag)

# Read the data from DagRun
print_things_task = BashOperator(
    task_id="print_things_task",
    bash_command='echo {{ ti.xcom_pull(task_ids["set_data_task"])[0]["data"] }}',
    dag=dag)

set_data_task >> print_things_task
```

**Example 2**

```python
def implicit_push(ds, **kwargs):
    """Does a thing.

    Args:
        ds
        \**kwargs: Contains key "dag_run" which is the DagRun object.
    """
    kwargs["dag_run"].conf = {"data": "value"}
```

**Example 3**

```python
# DAG_1
def set_payload(context, dag_run_obj):
    dag_run_obj.payload = {"data": "import value"}
    return dag_run_obj

dag = DAG(dag_id="DAG_1",
    default_args=args,
    schedule_interval=None)

set_payload_task = TriggerDagRunOperator(
  task_id="trigger_task",
  trigger_dag_id="DAG_2",
  python_callable=set_payload,
  dag=dag)
```

```python
# DAG_2 with PythonOperator

def print_things(ds, **kwargs):
    print(kwargs["dag_run"].conf)

dag = DAG(dag_id="DAG_2",
      default_args=args,
      schedule_interval=None)

print_things_task=PythonOperator(
    task_id="cool",
    provide_context=True,
    python_callable=print_things,
    dag=dag)
```

```python
# DAG_2 with BashOperator

dag = DAG(dag_id="DAG_2",
      default_args=args,
      schedule_interval=None)

echo_things_task = BashOperator(
    task_id="echo_things_task",
    bash_command='echo {{ dag_run.conf["data"] }}',
    dag=dag)
```
