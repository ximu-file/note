# 【Linux】log分析常用的shell
#技术积累/linux

# 1 . 访问信息相关的命令
### 1. 访问量排名前10的IP地址
`$ cat access.log | cut -f1 -d “ “ | sort | uniq -c | sort -k 1 -n -r | head -10`

### 2. 页面访问量排名前10的url
`$ cat access.log | cut -f4 -d “ “ | sort | uniq -c | sort -k 1 -n -r | head -10`

### 3. 统计日志文件中 访问 123.206.13.151:8080 这个IP地址的次数：
`$cat access.log | grep -o "123.206.13.151:8080" | wc -l`

### 4 . 统计IP为 10.95.114.92 的请求次数：
`$cat access.log | awk '{print $1}' | sort -n | grep -o "10.95.114.92" | wc -l`

### 5 .获取PV数
`$ cat access.log | wc -l`

### 6 . 获取IP数
`$cat access.log | awk '{print $1}' | sort -k1 -r | uniq | wc -l`

### 7. 获取最耗时的请求时间、url、耗时，前10名, 可以修改后面的数字获取更多，不加则获取全部
`$ cat access.log | awk '{print $4,$7,$NF}' | awk -F '"' '{print $1,$2,$3}' | sort -k3 -rn | head -10`







