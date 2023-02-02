# 長期用習慣Windows，因為需要在Mac上雙棲所以做了這個筆記

## Homebrew

### 安裝

1. 打開內建的終端機，輸入 

``` /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)" ```

2. 如果安裝成功他會提示
```==> Installation successfull```

3. 如果出現了``` Warning:/opt homebrew bin is not in your PATH``` ，可能是因為沒有設定相應的環境變數

	1. 需要手動新增檔案 ```.zprofile```
	2. 打開任何一個文字編輯器並在裡面輸入
	
	```
	eval "$(/opt/homebrew/bin/brew shellenv)"

	export HOMEBREW_BREW_GIT_REMOTE="https://gitee.com/mirrors/homebrew.git"
	export HOMEBREW_CORE_GIT_REMOTE="https://gitee.com/Homebrew/homebrew-core.git"
	```

	3. 將檔案存在 ```使用者/[你的名稱]``` 下面
	4. 這樣就能解決在終端機使用brew 相關命令 一直提示找不到命令的問題了
4. 使用

	打開內建的終端機，直接使用像是```brew help```就可以執行相關命令了
## Visual Studio Code

### PlantUML

1. 預覽圖表時的
```Warning :the fonts "Times" and "Times" are not available for the Java logical font "Serif"```

	這是因為 Mac上缺少了 Times 字體，所以需要下載
	+ 註: 當安裝的時候會提示字體重複，不用管他，只要確保裝了後字體簿有出現Times字體即可