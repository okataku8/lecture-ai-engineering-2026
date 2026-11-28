# 第6回 解答

## 演習

<!-- [tool call] の行は、表示された行をそのまま貼り付けてください。終わらなかった STEP は、空欄のままでかまいません。 -->

- STEP 1 のツールの名前：
    - uv run python -m app.agent --list-tools
        - [search_materials]
    - uv run python -m app.mcp_agent --list-tools 
        - [glossary_search_glossary]

- STEP 2 の [tool call]：
    - [tool call]   glossary_search_glossary({'query': '3レベル'})
- STEP 3 の [tool call]：
    - [tool call]   fetch_fetch({'url': 'https://modelcontextprotocol.io/'})
- STEP 4 の [tool call]：
    - [tool call]   aws_aws___search_documentation({'search_phrase': 'Amazon S3 versioning overview', 'topics': ['general']})
    - [tool call]   glossary_search_glossary({'query': 'Amazon S3'})
- 気づき・疑問：
    - 公開されたツールに関する定義をするだけで利用できるようになるプロトコルはやはり画期的と実感した。多くのモデルプロバイダーが賛同したのも納得がある。

## 宿題

<!-- 基礎・実装・発展のどれか1つに取り組めば完了です。 -->

- 取り組んだレベル：実装
- やったこと：
    - すでに make_quiz をつくってあったので、mcp_server で　Tool の公開を実施
    - 範囲外の講義回が指定されたときは、「用語が用語集に見つからないため、クイズを作成できません。」のメッセージとした
- 結果：
    - 範囲内の講義回を指定して、質問の作成を依頼
        - ```text
            ### 【第3回 確認クイズ】
            **「ツール（Tool use / Function calling）」の主な役割や仕組みについて、正しい説明はどれでしょうか？**

            1. 大規模言語モデル（LLM）のパラメータを直接書き換えて、新しい機能を覚える仕組み
            2. モデルが外部APIや関数などを自分で呼び出せるように、引数を決めて実行を依頼する仕組み
            3. モデルの学習データを自動で拡張するためのデータスクレイピングツール

            ---

            考えてみてくださいね。ヒントが必要なときはいつでも聞いてください！

            （関係する回：第3回）
            --- trace ---
            [tool call]   glossary_make_quiz({'lecture': 3})
            [tool return] glossary_make_quiz -> {"question": "ツール（Tool use / Function calling）の意味は？", "answer": "モデルが呼び出せるように用意した関数。モデルはツールの名前・説明・引数の型を見て、必要なときに引数を決めて呼び…
            [usage] 入力 5,647 tokens / 出力 165 tokens / モデル応答 2 回
        ```
    - 範囲外の講義回を指定して、質問の作成を依頼
        - ```text
            第20回の講義内容について、用語集に対象の用語や情報が見つかりませんでした。

            もしよろしければ、第20回でどのようなテーマやトピック（モデル、アーキテクチャ、手法など）を扱ったか教えていただければ、それに沿ったヒントやクイズの作成についてお手伝いできます！

            （関連する回：第20回）

            --- trace ---
            [tool call]   glossary_make_quiz({'lecture': 20})
            [tool return] glossary_make_quiz -> 第20回の用語が用語集に見つからないため、クイズを作成できません。…
            [usage] 入力 5,590 tokens / 出力 102 tokens / モデル応答 2 回

        ```
- 気づき・疑問：
    - MCP 公開の方法が簡便な点はやはり魅力的
    - それに加え、Toolの結果をLLMに渡すと、その結果を受けたテキスト生成を行えている点も魅力的
    - これまでは静的にコード実装していた内容を、（確率論ではあるが）LLMがテキスト生成できる能力は、やはりゲームチェンジャーになりえると心底感じた。
