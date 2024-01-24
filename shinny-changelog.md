# Shinny 定制化 Airflow 变更记录

## BackPort Bugfix
- 从上游backport bugfix ((fix: force DAG last modified time to UTC (#30243))[https://github.com/apache/airflow/pull/30243]) 到 2.4.0 版本

## CI打包
- 添加 shinny-release.yml, github action 产出python包并放入shinny-cd

## Dag processing
- 修改 `airflow/models/dag.py` 中的 `deactivate_deleted_dags` 方法, 使通过venv解释器刷新部分dag时不会将还存在于硬盘上的dag标记为失效
  - 经讨论验证废除方案：通过 `multiprocessing.context.set_executable` 修改context.process进程的python解释器, 但是这样无法正常使用venv中的包

## Dag 执行
- 修改 `airflow/executors/local_executor.py` 中的 `_execute_work_in_subprocess` 方法, 使其在执行cdk部署的dag时使用venv中的python解释器
  - 经讨论验证废除方案：通过 `multiprocessing.context.set_executable` 修改context.process进程的python解释器, 但是这样无法正常使用venv中的包
