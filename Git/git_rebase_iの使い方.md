# git rebase -i でコミットを結合

前提条件

- 統合したいコミットをpush済み

```git 
git add .
git commit -m "コミットメッセージ"
git log --oneline
```

    8a4f4f4 (HEAD -> feature/#37) #37 パスワードのバリデーション強化
    04f5c43 #37 パスワードのバリデーション強化

    git rebase -i HEAD~2

    pick 04f5c43 #37 パスワードのバリデーション強化
    pick 8a4f4f4 #37 パスワードのバリデーション強化

を

    pick 04f5c43 #37 パスワードのバリデーション強化
    s 8a4f4f4 #37 パスワードのバリデーション強化

に修正。

コミットメッセージの編集画面でコミットメッセージを修正

```git
git log --oneline
04f5c43 (HEAD) #37 パスワードのバリデーション強化
```
で1つになっている