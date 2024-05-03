#### 分析 testdisk/qphotorec大致流程

【术语】 qphotorec==带qt的testdisk
1.  [cgsecurity/testdisk.git](https://github.com/cgsecurity/testdisk.git)已迁导入进[gitee/cgsecurity--testdisk.git](https://gitee.com/disk_recovery/cgsecurity--testdisk.git)
2. qphotorec编译步骤，对u盘创建6个分区， [testdisk/c27a3/README.md](https://gitee.com/disk_recovery/cgsecurity--testdisk/blob/c27a3ae0a9aed9b2a31f2eab9ca4b49ab80ab767/README.md),  [testdisk/fridaAnlzAp/qphotorec/README.md](https://gitee.com/disk_recovery/cgsecurity--testdisk/blob/fridaAnlzAp/qphotorec/README.md)
3.  编译torch过程中拦截编译命令修改调试信息级别,，```-O2 -g``` --> ```-O1 -g1```【[cmd-wrap](http://giteaz:3000/bal/cmd-wrap.git)】；
5. frida_js生成 函数进出日志、进出时刻点日志,[frids_js/f8d80/fridaJs_runApp.sh](http://giteaz:3000/frida_analyze_app_src/frida_js/src/commit/f8d80c10899042cd7d660d93dc5c2b107db01d2f/fridaJs_runApp.sh),  [frids_js/ok/qphotorec_01/fridaJs_runApp.sh](http://giteaz:3000/frida_analyze_app_src/frida_js/src/tag/ok/qphotorec_01/fridaJs_runApp.sh)。 请务必选6个分区中的最小分区，可以节省大量时间
6.  日志最终载入neo4j进行分析：[analyze_by_graph](http://giteaz:3000/frida_analyze_app_src/analyze_by_graph.git)


其余可看文档， [frids_js/f8d80/README.md](http://giteaz:3000/frida_analyze_app_src/frida_js/src/commit/f8d80c10899042cd7d660d93dc5c2b107db01d2f/README.md)， 