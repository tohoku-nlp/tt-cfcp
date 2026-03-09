# Tohoku-Transcosmos 家庭内会話計画コーパス (TT-CFCP)


[![CC BY-SA 4.0][cc-by-sa-shield]][cc-by-sa-link]



Tohoku-Transcosmos 家庭内会話計画コーパス (TT-CFCP)は、家庭内の日常会話を収録した日本語コーパスです。
現在のバージョンでは対話数3件、発話数1573件を収録しています。

30分〜1時間ほどの家族の実際の会話を文字起こしし、言及された行動や予定のような **計画情報** を、5W1H（いつ・どこで・誰が・何を・なぜ・どうやって）の形式でアノテーションします。
さらに、会話が終わった時点の行動や予定の一覧を付与しています。
詳細は[文献](#文献)を参照してください。

| 項目 | 値 |
|---|---|
| 対話数 | 3 対話 |
| 対話あたりの発話数 | 524.33 発話 |
| 発話数 | 1,573 発話 |
| 発話あたりの文字数 | 12.59 文字 |
| 語彙数 | 1,784 語彙 |
| 単語数 | 8,865 単語 |
| タイプ・トークン比 | 0.201 |
| リスト操作数 | 91 |
| 言語 | 日本語 |

語彙数および単語数を求めるにあたって，形態素解析器には [MeCab](https://taku910.github.io/mecab/)，辞書には [NEologd](https://github.com/neologd/mecab-ipadic-neologd) を用いました．


## データフォーマット


```
/tt-cfcp
├── VERSION        // 本コーパスのバージョン
├── src/           // ソースコード（準備中）
└── data/          // 注釈付きデータ
    ├── 0000.json
    ├── 0001.json
    └── 0002.json

```
生の音声データは[こちら](https://github.com/tohoku-nlp/tt-cfcp/releases/download/v1.0.0/tt-cfcp-audio-v1.zip)からダウンロードしてください．

### 対話データ

|キー|型|説明|
| --- | --- | ---|
| dialogue_id | str | 対話ID |
| interlocutors | list(str) | 話者のリスト |
| relationship | list(str) | 各話者の関係 |
| utterances | list(dict) | 発話のリスト |
| utterances.utterance_id | int | 発話ID|
| utterances.time | str | 発話タイミング(録音開始からの経過時間)、mm:ss |
| utterances.interlocutor | str | 話者 |
| utterances.text | str | 発話テキスト |
| utterances.list_operation | list(dict) | タスク・予定に対する操作 |
| utterances.list_operation.id | str | 行動・予定のID | 
| utterances.list_operation.operation | str | 行動・予定に対する操作の種類(create/update/delete) | 
| utterances.list_operation.status | str | 行動・予定の状態(confirmed/unconfirmed) |
| utterances.list_operation.what | str | whatの情報 |
| utterances.list_operation.when | str | whenの情報 |
| utterances.list_operation.who | str | whoの情報 |
| utterances.list_operation.where | str | whereの情報 |
| utterances.list_operation.why | str | whyの情報 |
| utterances.list_operation.how | str | howの情報 |
| list | list(dict) | 対話から抽出された最終的なタスクや予定のリスト |

例
``` jsonc
{
    "dialogue_id": "0000",
    "interlocutors": [
        "父",
        "息子",
        "母",
        "音声アシスタント"
    ],
    "relationship": [
        "父",
        "息子",
        "母",
        "その他"
    ],
    "utterances": [
        {
            "utterance_id": 0,
            "time": "0:00",
            "interlocutor": "父",
            "text": "これでピンポンなるかもしれない、なるちゃんとマイク君、ピンポンわかるかなおうちの",
            "list_operation": []
        },
        {
            "utterance_id": 1,
            "time": "0:06",
            "interlocutor": "息子",
            "text": "うん",
            "list_operation": []
        },
        {
            "utterance_id": 2,
            "time": "0:07",
            "interlocutor": "父",
            "text": "どうかな？教えてあげた。",
            "list_operation": []
        },
        // ...
        {
            "utterance_id": 16,
            "time": "0:48",
            "interlocutor": "父",
            "text": "うん、じゃあさ、たーちゃんさ今日はマイク君となるちゃんが9時に来るから",
            "list_operation": [
                {
                    "id": "schedule_0",
                    "operation": "create",
                    "status": "confirmed",
                    "what": "マイク君,なるちゃんが家に来る",
                    "when": "今日の9時",
                    "who": null,
                    "where": "家",
                    "why": null,
                    "how": null
                }
            ]
        },
        {
            "utterance_id": 17,
            "time": "0:56",
            "interlocutor": "息子",
            "text": "9時に",
            "list_operation": []
        },
        {
            "utterance_id": 18,
            "time": "0:56",
            "interlocutor": "父",
            "text": "9時か10時に来るって、そしたらさどうする？プールに入る？",
            "list_operation": [
                {
                    "id": "schedule_0",
                    "operation": "update",
                    "status": null,
                    "what": null,
                    "when": "今日の9時,10時",
                    "who": null,
                    "where": null,
                    "why": null,
                    "how": null
                },
                {
                    "id": "action_0",
                    "operation": "create",
                    "status": "unconfirmed",
                    "what": "プールに入る",
                    "when": null,
                    "who": "息子",
                    "where": null,
                    "why": null,
                    "how": null
                }
            ]
        },
        {
            "utterance_id": 19,
            "time": "1:04",
            "interlocutor": "息子",
            "text": "プールに入る。",
            "list_operation": [
                {
                    "id": "action_0",
                    "operation": "update",
                    "status": "confirmed",
                    "what": null,
                    "when": null,
                    "who": null,
                    "where": null,
                    "why": null,
                    "how": null
                }
            ]
        }
        // ...
    ],

        "list": [
        {
            "id": "schedule_0",
            "status": "confirmed",
            "what": "マイク君,なるちゃんが家に来る",
            "when": "今日の9時,10時",
            "who": null,
            "where": "家",
            "why": null,
            "how": null
        },
        {
            "id": "action_0",
            "status": "confirmed",
            "what": "プールに入る",
            "when": null,
            "who": "息子",
            "where": null,
            "why": null,
            "how": null
        },
        // ...
      ]
}
        
```

各発話のリスト操作において、更新/参照されなかった項目はnullとしています。

聞き取れなかった発話などは下記の要領でタグ付けしています。
<!-- 参考文献提示する？-->

| タグ | 説明 |
| --- | --- | 
| `[laughs`] | 笑い声 |
| `[inaudible`] |はっきりと聞き取れず、内容が判別できなかった発話|
| `[noise`] | その他の非言語的な音声|



## 本コーパスの使用にあたって
> [!CAUTION]
> **本コーパスの使用にあたっては，次のことに十分注意してください．**
> * 本コーパスのデータから個人を特定しようとしないこと。

## クレジット
本コーパスはトランス・コスモス株式会社、東北大学共創イニシアティブ株式会社による共創プロジェクトの一環として作成されたものです。

## 文献
沼屋征海, 佐藤魁, 吉田倖, 亀井遼平, 所年雄, 滑川登, 濱口泰時, 上村崇, 乾健太郎, & 坂口慶祐. (2026). *[家族日常対話における計画情報抽出](https://anlp.jp/proceedings/annual_meeting/2026/pdf_dir/Q5-7.pdf)*. 言語処理学会第32回年次大会 (NLP2026).


## ライセンス

本コーパスは [CC BY-SA 4.0][cc-by-sa-link] の下で提供されます。

[![CC BY-SA 4.0][cc-by-sa-image]][cc-by-sa-link]


[cc-by-sa-link]: https://creativecommons.org/licenses/by-sa/4.0/deed.ja
[cc-by-sa-image]: https://i.creativecommons.org/l/by-sa/4.0/88x31.png
[cc-by-sa-shield]: https://img.shields.io/badge/License-CC%20BY%20SA%204.0-green.svg
