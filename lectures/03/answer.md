# 第3回 解答

## 演習

- 講座repoの更新を取り込む：Sync fork→Update branchして、pullする。その後lec03を作って切り替え。
- 第1回（素のLLM）とエージェントの回答の違い：第1回のapp.ask（素のLLM）は講座のことを知らない・分からない・一般論でもっともらしく・確認を促すように答えたが、今回のapp.agent（エージェント）は用語集を検索してから答えた。（基礎・実装・発展、どれか 1 つで完了。なお、原則として宿題の採点は行われません。）data/glossary.jsonの定義とあっていて、最後に関係する講義回も表示された。
える
- `--trace` で見えたツール呼び出し（ツール名と引数）：「uv run python -m app.agent --trace "LoRA とQLoRAの違いは？ "」というコマンドで試すと、search_glossaryを2回呼び出していて、それぞれ引数が「LoRA」と「QLoRA」で依頼した。今回は1つの質問で(requests)2回LLMを呼び出した。2回に分けて検索したのはagent.py内のINSTRUCTIONSおよびtools.py内のツールの説明文(decstring)で決められているからである。
- 1つの質問での LLM 呼び出し回数（`requests`）：上記の通り、2である。
- 追加したツールと、docstring に書いた説明：手順4のmake_quizツールをtools.pyに実装。docstringには、資料の通り「指定した回の用語から 1つを選び、確認問題と答えを返す。「第2回の確認問題を出して」のように、復習用の問題を求められたときに使う。」をコピペした。手順5の「uv run python -m app.agent --list-tools 」のコマンドで確認した際、関数内に書いたそのままの形で出力されることが分かった。（改行をいれるとそのまま反映される。）また、agent.pyのほうのTODOを埋めてmake_quizをエージェントへ登録した。
- 手順5の「uv run pytest -q 」もパスし、演習を終了した。

## 宿題

- 取り組んだレベル：基礎
- やったこと：演習で見た app/agent.py の INSTRUCTIONS や、app/tools.py のツールの docstring を書き換えます。同じ質問を --trace 付きで実行し、ツールの呼ばれ方や回答がどう変わるかを比べて記録します。資料にある例の1つを実施しました。（INSTRUCTIONS から「必ず search_glossary で用語集を確認する」の行を消すと、検索せずに答えるようになるか。）
- 結果：質問・コマンドは「uv run python -m app.agent --trace "AIE2026の講座の宿題にある「3レベル」とは何ですか？"」とする。

変更前の回答は
「AIE2026の宿題における「3レベル」とは、宿題の構成である**「基礎」「実装」「発展」**の3つの段階を指します。

それぞれの特徴は以下の通りです：
* **基礎**: 講義内容の確認
* **実装**: 自分でコードを書く
* **発展**: 調査や試行

これら3つのレベルのうち、どれか1つに取り組めばその回の宿題は完了となります（原則として採点は行われません）。

なお、宿題の答えそのものはお伝えできませんが、もし取り組み方や考え方で迷っている部分があれば、ヒントをお出ししますのでいつでもご相談ください！

第1回

--- trace ---
[tool call]   search_glossary({'query': '3レベル'})
[tool return] search_glossary -> [{"term": "宿題の3レベル", "lecture": 1, "definition": "AIE2026の宿題の構成。「基礎（講義内容の確認）」「実装（自分でコードを書く）」「発展（調査や試行）」の3段階があり、どれか1つに取り組…
[usage] RunUsage(cost=Decimal('0.0007058'), details={'text_prompt_tokens': 961}, input_text_tokens=961, input_tokens=961, output_tokens=167, requests=2, tool_calls=1)」

変更後の回答は
「AIE2026の講座における「3レベル」とは、宿題の構成を示す以下の3段階のことです。

1. **基礎**：講義内容の確認
2. **実装**：自分でコードを書く
3. **発展**：調査や試行

どれか1つに取り組めばその回の宿題は完了となり、原則として採点は行われません。

第1回（関係する回：第1回）

--- trace ---
[tool call]   search_glossary({'query': '3レベル'})
[tool return] search_glossary -> [{"term": "宿題の3レベル", "lecture": 1, "definition": "AIE2026の宿題の構成。「基礎（講義内容の確認）」「実装（自分でコードを書く）」「発展（調査や試行）」の3段階があり、どれか1つに取り組…
[usage] RunUsage(cost=Decimal('0.0005614'), details={'text_prompt_tokens': 913}, input_text_tokens=913, input_tokens=913, output_tokens=115, requests=2, tool_calls=1)」
- 気づき・疑問：ツールの呼ばれ方、回数、引数は同じで、回答の変化は特に内容に関しては相違はなく、INSTRUCTIONSから消していてもきちんと検索をかけてから答えてくれた。なお、変更前後をそれぞれ3回ずつ実行して試したが、いずれも同じ結果が得られた。
