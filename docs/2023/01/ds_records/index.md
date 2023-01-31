# DolphinScheduler使用记录


## 数据表

 | 序号 | 表名 | 表说明 | 
 | :---: | :--- | :--- | 
 | 1 | [qrtz_blob_triggers](#qrtz_blob_triggers) |  | 
 | 2 | [qrtz_calendars](#qrtz_calendars) |  | 
 | 3 | [qrtz_cron_triggers](#qrtz_cron_triggers) |  | 
 | 4 | [qrtz_fired_triggers](#qrtz_fired_triggers) |  | 
 | 5 | [qrtz_job_details](#qrtz_job_details) |  | 
 | 6 | [qrtz_locks](#qrtz_locks) |  | 
 | 7 | [qrtz_paused_trigger_grps](#qrtz_paused_trigger_grps) |  | 
 | 8 | [qrtz_scheduler_state](#qrtz_scheduler_state) |  | 
 | 9 | [qrtz_simple_triggers](#qrtz_simple_triggers) |  | 
 | 10 | [qrtz_simprop_triggers](#qrtz_simprop_triggers) |  | 
 | 11 | [qrtz_triggers](#qrtz_triggers) |  | 
 | 12 | [t_ds_access_token](#t_ds_access_token) | 用户token管理表 | 
 | 13 | [t_ds_alert](#t_ds_alert) | 告警信息 | 
 | 14 | [t_ds_alert_plugin_instance](#t_ds_alert_plugin_instance) | 告警插件实例 | 
 | 15 | [t_ds_alert_send_status](#t_ds_alert_send_status) | 告警发送状态 | 
 | 16 | [t_ds_alertgroup](#t_ds_alertgroup) | 告警组管理 | 
 | 17 | [t_ds_audit_log](#t_ds_audit_log) | 审计日志 | 
 | 18 | [t_ds_command](#t_ds_command) |  | 
 | 19 | [t_ds_datasource](#t_ds_datasource) | 数据源管理 | 
 | 20 | [t_ds_dq_comparison_type](#t_ds_dq_comparison_type) |  | 
 | 21 | [t_ds_dq_execute_result](#t_ds_dq_execute_result) |  | 
 | 22 | [t_ds_dq_rule](#t_ds_dq_rule) | 数据质量-规则管理 | 
 | 23 | [t_ds_dq_rule_execute_sql](#t_ds_dq_rule_execute_sql) |  | 
 | 24 | [t_ds_dq_rule_input_entry](#t_ds_dq_rule_input_entry) |  | 
 | 25 | [t_ds_dq_task_statistics_value](#t_ds_dq_task_statistics_value) |  | 
 | 26 | [t_ds_environment](#t_ds_environment) | 环境管理 | 
 | 27 | [t_ds_environment_worker_group_relation](#t_ds_environment_worker_group_relation) | 环境与worker组关联表 | 
 | 28 | [t_ds_error_command](#t_ds_error_command) |  | 
 | 29 | [t_ds_k8s](#t_ds_k8s) | k8s管理 | 
 | 30 | [t_ds_k8s_namespace](#t_ds_k8s_namespace) | k8s命名空间管理 | 
 | 31 | [t_ds_plugin_define](#t_ds_plugin_define) | 插件定义 | 
 | 32 | [t_ds_process_definition](#t_ds_process_definition) | 工作流定义 | 
 | 33 | [t_ds_process_definition_log](#t_ds_process_definition_log) | 工作流版本 | 
 | 34 | [t_ds_process_instance](#t_ds_process_instance) | 工作流实例 | 
 | 35 | [t_ds_process_task_relation](#t_ds_process_task_relation) | 工作流和任务关联表 | 
 | 36 | [t_ds_process_task_relation_log](#t_ds_process_task_relation_log) | 工作流和任务关联记录 | 
 | 37 | [t_ds_project](#t_ds_project) | 项目管理 | 
 | 38 | [t_ds_queue](#t_ds_queue) | Yarn队列管理 | 
 | 39 | [t_ds_relation_datasource_user](#t_ds_relation_datasource_user) | 用户与数据源关联表 | 
 | 40 | [t_ds_relation_namespace_user](#t_ds_relation_namespace_user) | 用户与k8s命名空间关联表 | 
 | 41 | [t_ds_relation_process_instance](#t_ds_relation_process_instance) | 工作流实例与任务实例关联表 | 
 | 42 | [t_ds_relation_project_user](#t_ds_relation_project_user) | 用户与项目关联表 | 
 | 43 | [t_ds_relation_resources_user](#t_ds_relation_resources_user) | 用户与UDF资源关联表 | 
 | 44 | [t_ds_relation_rule_execute_sql](#t_ds_relation_rule_execute_sql) | 数据质量规则和执行SQL关联表 | 
 | 45 | [t_ds_relation_rule_input_entry](#t_ds_relation_rule_input_entry) | 数据质量规则和输入内容关联表 | 
 | 46 | [t_ds_relation_udfs_user](#t_ds_relation_udfs_user) | 用户和UDF关联表 | 
 | 47 | [t_ds_resources](#t_ds_resources) | UDF资源管理 | 
 | 48 | [t_ds_schedules](#t_ds_schedules) | 任务定时管理 | 
 | 49 | [t_ds_session](#t_ds_session) | Session管理 | 
 | 50 | [t_ds_task_definition](#t_ds_task_definition) | 任务定义 | 
 | 51 | [t_ds_task_definition_log](#t_ds_task_definition_log) | 任务定义历史 | 
 | 52 | [t_ds_task_group](#t_ds_task_group) | 任务组配置 | 
 | 53 | [t_ds_task_group_queue](#t_ds_task_group_queue) | 任务组队列 | 
 | 54 | [t_ds_task_instance](#t_ds_task_instance) | 任务实例管理 | 
 | 55 | [t_ds_tenant](#t_ds_tenant) | 租户管理 | 
 | 56 | [t_ds_udfs](#t_ds_udfs) | UDF函数管理 | 
 | 57 | [t_ds_user](#t_ds_user) | 用户管理 | 
 | 58 | [t_ds_version](#t_ds_version) | version | 
 | 59 | [t_ds_worker_group](#t_ds_worker_group) | Worker组管理 | 

## 删除历史任务数据

```sql
delete from dolphin.t_ds_task_instance
where process_instance_id in (
select id from dolphin.t_ds_process_instance
where state = 7 and date(end_time) = '2022-09-20');

delete from dolphin.t_ds_process_instance
where state = 7 and date(end_time) = '2022-09-20'
```

