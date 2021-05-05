to load npm dependencies:
1) prepare your package-lock.json 
2) run it:
```sh
	wget -N -P ./downloads $(jq 'del(.. | .requires?, .integrity?)' package-lock.json | tee package-lock.temp | jq -r '.. | .resolved?');
	awk '/resolved/ {sub(/h.+(\/)/, "file:./downloads/")}1' package-lock.temp > package-lock.json && rm package-lock.temp
```
3) now you can send 'download' folder and the 'package-lock.json' for installation on a remote server
