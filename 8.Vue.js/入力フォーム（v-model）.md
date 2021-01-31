## v-modelの修飾詞

- v-model.lazy：inputタグからフォーカスが離れた時（changeイベント）に値を変える
1文字打つたびにフロントのバリデーションがかかるのを防ぐ（UX的に微妙）

- v-model.number：inputタグ（type="number"）の入力値がstringで入力されるのをnumberに変換してくれる

- v-model.trim：先頭と最後のスペースを無効にする（文字の中間のスペースは有効）

## textarea（複数行テキスト）に双方向データバインディングする

- 通常通りtextareaタグにv-modelをつける

## v-modelの中身

<input tyoe="text">の場合

    v-model="eventData.title"

と

    :value="eventData.title"
    @input="eventData.title = $event.target.value"

は全く同じ