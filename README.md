
METHOD 1(for simple dependencies, hello node-gyp)

short explanation: 
load the dependencies specified in the package-lock.json in 'download', changes package-lock.json by replacing links to local files

required packages: awk, wget, jq(not standard for linux)

to load npm dependencies:
1) prepare your package-lock.json 
2) run it:
```sh
wget -N -P ./downloads $(jq 'del(.. | .requires?, .integrity?)' package-lock.json | tee package-lock.temp | jq -r '.. | .resolved?');
awk '/resolved/ {sub(/h.+(\/)/, "file:./downloads/")}1' package-lock.temp > package-lock.json && rm package-lock.temp
```
3) now you can send 'download' folder and the 'package-lock.json' for installation on a remote server



METHOD 2(use cache)

short explanation:
pass the cache prepared for a separate project and use it

1) prepare your cache:
```sh
npm i --cache "./cache"
```
2) add '.npmrc' in project and:
```sh
echo 'cache=./cache'
```
3) now you can send 'cache', 'package-lock.json', '.npmrc' for installation on a remote server



METHOD 3(use local repo)

short explanation:
up your local repo. there is no point in describing everything, just a list of possible options

1) npm-proxy-cache
2) local-npm
3) npm-lazy
4) verdaccio


sources(they have useful information):
https://blog.csdn.net/daihaoxin/article/details/105749014?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522162041742516780271569021%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fall.%2522%257D&request_id=162041742516780271569021&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_v2~rank_v29-4-105749014.first_rank_v2_pc_rank_v29&utm_term=npm+offline+install
https://blog.csdn.net/XiaoYuWen1242466468/article/details/79383614?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522162041742516780271569021%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fall.%2522%257D&request_id=162041742516780271569021&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_v2~rank_v29-12-79383614.first_rank_v2_pc_rank_v29&utm_term=npm+offline+install
https://blog.csdn.net/weixin_40906515/article/details/105525383?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522162041742516780271569021%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fall.%2522%257D&request_id=162041742516780271569021&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_v2~rank_v29-15-105525383.first_rank_v2_pc_rank_v29&utm_term=npm+offline+install