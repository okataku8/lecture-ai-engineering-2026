# 第3回 解答

## 演習

- 第1回（素のLLM）とエージェントの回答の違い：
    - Glossaryを読み込むことによって、今回の講義で使われている用語集を適切に利用した
    - docstringを変更することによって、用語集への問い合わせ内容が変更となった
    - 特に、「用語集にはない」の指示文を削除すると、一部の用語がヒットしたものを回答として用いるなど、期待する精度が変わることがわかった

- `--trace` で見えたツール呼び出し（ツール名と引数）：
- 1つの質問での LLM 呼び出し回数（`requests`）：
    1. 質問文："AIE2026の講座で使う「lecNN-start タグ」とは何ですか？"
        - 回答：
            - 「lecNN-start タグ」とは、AIE2026の講座リポジトリにおいて、各回の演習開始時点のコードに付けられているGitタグのことです（例: `lec03-start`）。
            - 前の回の内容が終わっていなくても、このタグから新しくブランチを作成することで、いつでもその回の演習をスムーズに始められるように用意されています。
            - 関係する回：第1回
        - ツール名と引数：
            - search_glossary({'query': 'lecNN-start タグ'})
            - [tool return] search_glossary -> [{"term": "lecNN-start タグ", "lecture": 1, "definition": "AIE2026の講座repoで、各回の演習開始時点のコードに付けているGitタグ（例：lec03-start）。前の回が終わっ…
        - LLM呼び出し回数
            - 2回
            - [usage] RunUsage(cost=Decimal('0.0005929'), details={'text_prompt_tokens': 943}, input_text_tokens=943, input_tokens=943, output_tokens=124, requests=2, tool_calls=1)

    1. 質問文："AIE2026の講座で使う「lecNN-startend タグ」とは何ですか？"
        - 回答：
            - 用語集にはありません。
        - ツール名と引数：
            - [tool call]   search_glossary({'query': 'lecNN-startend タグ'})
            - [tool return] search_glossary -> 用語集に「lecNN-startend タグ」は見つかりませんでした。…
        - LLM呼び出し回数
            - 2回
            - [usage] RunUsage(cost=Decimal('0.0003340'), details={'text_prompt_tokens': 880}, input_text_tokens=880, input_tokens=880, output_tokens=28, requests=2, tool_calls=1)

    1. 質問文："AIE2026の講座で使う「lecNN-startend タグ」とは何ですか？"
        - docstring: 憶測で答えないように指示したInstructionを削除
        - 回答：
            - AIE2026の講座リポジトリにおいて、各回の演習の開始時点および終了時点のコードに付けられているGitタグのことです（例：`lec03-start` や `lec03-end`）。
            - 例えば `lecNN-start` タグを使えば、前の回の進捗に関わらず、その回の演習をスムーズに始めたい時点のコードから新しくブランチを作って作業を開始できるようになっています。
        - ツール名と引数：
            - [tool call]   search_glossary({'query': 'lecNN-startend タグ'})
            - [tool return] search_glossary -> 用語集に「lecNN-startend タグ」は見つかりませんでした。…
            - [tool call]   search_glossary({'query': 'タグ'})
            - [tool return] search_glossary -> [{"term": "lecNN-start タグ", "lecture": 1, "definition": "AIE2026の講座repoで、各回の演習開始時点のコードに付けているGitタグ（例：lec03-start）。前の回が終わっ…
        - LLM呼び出し回数
            - 3回
            - [usage] RunUsage(cost=Decimal('0.0007572'), details={'text_prompt_tokens': 1149}, input_text_tokens=1149, input_tokens=1149, output_tokens=165, requests=3, tool_calls=2)

- 追加したツールと、docstring に書いた説明：
    - make_quiz
    - 指定した回の用語について、学習内容の理解を確認したいときに使う。lecture は対象の回（1始まりの整数）。


## 宿題

- 取り組んだレベル：基礎 / 実装 / 発展
    - 実装
- やったこと：
    - `save_learning_log` Toolの実装
    - Agent側にToolの登録
    - Toolの戻り値に応じて、CLIでHITLを表示するロジックの追加
- 結果：
    - 「検索結果を保存して」という指示を追加すると、「保存していい？（y/N）」のダイアログを表示
    - Yが入力されたら data/learning_log.jsonl に保存
- 気づき・疑問：
    - Frameworkが秀逸で、かつ、LLMの性能により、最小限のコード追加だけでHITLまで完成させられることにある種感動しました
