1. Nginx 设置获取真实用户IP，配置在http模块
```nginx
set_real_ip_from 0.0.0.0/0;  
real_ip_header X-Forwarded-For;
```

2. 编写shell脚本`black_ip.sh`，请自行修改文件地址以及访问频率限制（下文以每分钟单IP请求超过60次为例）
```shell
#/bin/bash
logfile=/www/wwwlogs;
last_minutes=1
#开始时间1分钟之前(这里可以修改,如果要几分钟之内攻击次数多少次，这里可以自定义)
start_time= date +"%Y-%m-%d %H:%M:%S" -d '-1 minutes'
echo $start_time
#结束时间现在
stop_time=`date +"%Y-%m-%d %H:%M:%S"`
echo $stop_time
# cur_date="`date +%Y-%m-%d`"
# echo $cur_date
#过滤出单位之间内的日志并统计最高ip数，请替换为你的日志路径
tac $logfile/access.log | awk -v st="$start_time" -v et="$stop_time" '{t=substr($2,RSTART+14,21);if(t>=st && t<=et) {print $0}}' | awk '{print $1}' | sort | uniq -c | sort -nr > $logfile/log_ip_top10
ip=`cat $logfile/log_ip_top10 | awk '{if($1>60)print $2}'`
for line in $ip
do  
    ref_ip="deny "${line}";"
    echo $ref_ip >> $logfile/blockip.conf
    echo $ref_ip
done
```

3. Nginx 引入黑名单配置，配置在http模块中
```nginx
include /www/wwwlogs/blockip.conf;
```

4. 配置宝塔定时计划，定时执行shell脚本，大致执行如下
```shell
cd ~
./black_ip.sh
/usr/bin/nginx -s reload
```
