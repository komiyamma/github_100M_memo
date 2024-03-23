# Github 100M コミット 自動防止 (Win/Mac/Linux)
- Github 用の Git で100M超えるものをコミットしないようにするための備忘録
- 個人用途などでは、個別のファイルを細かく見てないので100M超えるファイルがあってもコミットしてしまいがち

- リポジトリ下の「.git/hooks/」 フォルダの、「pre-commit」というファイルに以下の内容を「utf8」で記述する

```
#!/bin/sh

# Check if any file being committed exceeds 100MB
limit=104857600 # 100MB in bytes
for file in $(git diff --cached --name-only); do
    file_size=$(stat -c %s "$file")
    if [ $file_size -gt $limit ]; then
        echo "エラー：100MBを超えるファイルはコミットできません。コミットを中止します。"
        exit 1
    fi
done
```

- これで、100MBを超えるファイルをコミットしようとすると、上記エラーが出てコミットできなくなる。  
- VSでもVSCodeでも「コンパイル結果で」デカいファイルの自動コピーやプリコンパイルヘッダーが生成されたかも、  
  と、いちいちチェックしなくていいので、便利。

