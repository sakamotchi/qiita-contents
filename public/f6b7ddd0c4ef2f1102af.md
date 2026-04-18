---
title: amplify パス変更したとき
tags:
  - AWS
  - amplify
private: false
updated_at: '2020-02-21T13:16:46+09:00'
id: f6b7ddd0c4ef2f1102af
organization_url_name: null
slide: false
ignorePublish: false
---
amplifyのアプリでディレクトリ名変更や他の人のをコピーしてきてからamplify pushするとエラーが発生する。
私の場合はパス変更後、「amplify add storage」したあとにamplify pushしたらエラーになりました。

```bash
+0900 (GMT+09:00) Parameters: [bucketName, unauthRoleName, unauthPolicyName, authRoleName, triggerFunction, authPolicyName] must have values
```

「amplify/.config/local-env-info.json」のパスを正しく直せばうまくいくらしい。

* 参考：https://blog.mismith.me/entry/2019/10/13/212300

