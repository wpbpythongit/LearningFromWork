主服务配置

* agent_manager

  * 框架服务worker-service_worker(进程)

    * start_drivers_service
    * start_device_service
    * start_board_service

  *  rpc worker参数(线程)

    * monitor_rpc_worker

  * mq_worker(线程)

    * monitor_resources_mq_worker
    * monitor_space_mq_worker

  * self_worker(线程)

    * manager_self_worker

      

* drivers

  * rpc_worker :drivers_rpc
  * self_worker
    * manager_self_worker
    * parser_self_worker

* board_agent

  * rpc_worker:board_rpc_worker
  * self_worker
    * board_group_self_worker

* device_agent

  * rpc_worker:device_rpc_worker
  * mq_worker
    * device_event_mq_worker
    * device_value_mq_worker
  * self_worker
    * device_group_self_worker

* storage

  * mq_worker
    * storage_event_mq_worker
    * storage_setting_mq_worker
    * storage_value_mq_worker
    * storage_space_mq_worker
  * self_worker
    * storage_value_period
    * storage_value_real
    * xstorage_rpc_service

* cmdb_server