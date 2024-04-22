# php-src ソースリーディング環境

Docker を利用した環境です。

gdb でのデバッグが可能です。

## メモ

1. この通りに実行する  
   https://zenn.dev/sayu/articles/6fd3a39f6e5d96
2. コンテナに入る

```sh
docker run -it -v `pwd`/src:/usr/local/src/php-src/src  reading-php-src:8.3 /bin/bash
```

3. デバッグ実行を開始する

```sh
gdb sapi/cli/php

# phpの関数にブレークポイントを置く
b zif_phpinfo
b zif_sort

# それ以外にブレークポイントを置く
b stable_sort_fallback
b zend_sort
b zend_operators.c:3363
b zend_hash.c:2954

# phpファイルを実行
r src/sample.php
```

4. printデバッグの例

```sh
# struct _zend_string の中身の文字を見る
p s1->val[0]

# swapの中でBucketの中身の文字列を見たいとき
p p->val->value->str->val[3]

# zend sortで中身を見たいとき
p ((Bucket *)base+1)->val->value->str->val[0]
p ((Bucket *)base+2)->val->value->str->val[0]
p ((Bucket *)i)->val->value->str->val[0]

# zend pivotの中身を見たいとき
p ((Bucket *)pivot)->val->value->str->val[0]
```
