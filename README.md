![RealPersonaChatのロゴ](./RealPersonaChat_Logo.png?raw=true)

[![CC BY-SA 4.0][cc-by-sa-shield]][cc-by-sa-link]
[![RealPersonaChatのバージョン](https://img.shields.io/github/v/release/nu-dialogue/real-persona-chat)](https://github.com/nu-dialogue/real-persona-chat)
[![Hugging Face Datasets Hub](https://img.shields.io/badge/Hugging%20Face_🤗-Datasets-ffcc66)](https://huggingface.co/datasets/nu-dialogue/real-persona-chat)


# RealPersonaChat

RealPersonaChat は，話者本人のペルソナと性格特性を含む，約14,000件の日本語雑談対話からなるコーパスです．

> [!NOTE]
> こちらの公開版は，収集した全14,000対話から，個人情報をマスクし，倫理的観点から問題があると考えられる対話を除外したものです．
> [論文](#文献)では14,000対話に対する分析結果が報告されており，公開版の統計情報とは若干異なることにご注意ください．


## 統計情報

|  | RealPersonaChat[^1][^2] | JPersonaChat[^3] | PersonaChat[^4] |
| --- | --- | --- | --- |
| 対話数 | 13,583対話 | 5,000対話 | 10,907対話 |
| 対話あたりの発話数 | 30.08発話 | 12.36発話 | 14.86発話 |
| 発話数 | 408,619発話 | 61,793発話 | 162,064発話 |
| 発話あたりの文字数 | 13.16文字 | 40.25文字 | 11.71単語 |
| 語彙数 | 45,877単語 | 18,329単語 | 20,275単語 |
| 単語数 | 5,377,383単語 | 1,459,322単語 | 1,897,757単語 |
| タイプ・トークン比 | 0.009 | 0.013 | 0.011 |
| ペルソナ数 | 233個（話者本人のペルソナ） | 100個（架空のペルソナ） | 7,027個（架空のペルソナ） |
| ペルソナあたりの文字数 | 10文，182.08文字 | 5文，62.87文字 | 5文，26.98単語 |
| 言語 | 日本語 | 日本語 | 英語 |

語彙数および単語数を求めるにあたって，形態素解析器には [MeCab](https://taku910.github.io/mecab/)，辞書には [NEologd](https://github.com/neologd/mecab-ipadic-neologd) を用いました．


## データフォーマット

[`real_persona_chat/`](./real_persona_chat) 内に，対話データ `dialogues/*.json` と，話者データ `interlocutors.json` があります．
```
real_persona_chat/
├── VERSION             // 本コーパスのバージョン
├── dialogues/          // 対話データ（1ファイルにつき1対話）
│   ├── 00001.json
│   ├── 00002.json
│   ├── ...
│   └── 14000.json
└── interlocutors.json  // 話者データ（1ファイルに全ての話者分）
```


### 対話データ

対話データには，対話ID，話者ID，発話，および，話者ごとの評価スコアが含まれます．評価スコアは，1が低いことを，5が高いことを示します．

| キー | 型 | 説明 |
| --- | --- | --- |
| dialogue_id | int | 対話ID． |
| interlocutors | list (str) | 話者IDのリスト． |
| utterances | list (dict) | 発話のリスト． |
| utterances.utterance_id | int | 発話ID．対話内で固有．0始まりのインデックス． |
| utterances.interlocutor_id | str | 話者ID． |
| utterances.text | str | 発話テキスト． |
| utterances.timestamp | str | 発話終了時のタイムスタンプ．不明な場合は `NaT`． |
| evaluations | list (dict) | 話者ごとの評価スコアのリスト． |
| evaluations.interlocutor_id | str | 話者ID． |
| evaluations.informativeness | int | 情報量の評価スコア (1~5)． |
| evaluations.comprehension | int | 理解度の評価スコア (1~5)． |
| evaluations.familiarity | int | 親しみやすさの評価スコア (1~5)． |
| evaluations.interest | int | 興味の評価スコア (1~5)． |
| evaluations.proactiveness | int | 積極性の評価スコア (1~5)． |
| evaluations.satisfaction | int | 満足度の評価スコア (1~5)． |

```jsonc
{
    "dialogue_id": 1,
    "interlocutors": [
        "AA",
        "AB"
    ],
    "utterances": [
        {
            "utterance_id": 0,
            "interlocutor_id": "AA",
            "text": "よろしくお願いいたします。",
            "timestamp": "2022-08-06T14:51:18.360000"
        },
        {
            "utterance_id": 1,
            "interlocutor_id": "AB",
            "text": "よろしくお願いします！",
            "timestamp": "2022-08-06T14:51:48.482000"
        },
        {
            "utterance_id": 2,
            "interlocutor_id": "AA",
            "text": "今日は涼しいですね",
            "timestamp": "2022-08-06T14:51:55.538000"
        },
        {
            "utterance_id": 3,
            "interlocutor_id": "AB",
            "text": "雨が降って、何か涼しくなりましたね。",
            "timestamp": "2022-08-06T14:52:07.388000"
        },
        {
            "utterance_id": 4,
            "interlocutor_id": "AA",
            "text": "そうですね、明日も涼しいと聞きました",
            "timestamp": "2022-08-06T14:52:16.400000"
        },
        {
            "utterance_id": 5,
            "interlocutor_id": "AB",
            "text": "そうなんですか！でも、ちょっと湿度が高い気がします。",
            "timestamp": "2022-08-06T14:52:31.076000"
        },
        // ...
    ],
    "evaluations": [
        {
            "interlocutor_id": "AA",
            "informativeness": 5,
            "comprehension": 5,
            "familiarity": 5,
            "interest": 5,
            "proactiveness": 5,
            "satisfaction": 5
        },
        // ...
    ]
}
```

個人情報を含む発話は該当部分がマスクされています．また，公開版では，倫理的に問題があると考えられる対話は除いています．

|  | 公開方針 | 例 |
| --- | --- | --- |
| 話者名 | `<話者ID>` でマスクして公開 | <ul><li>\<CS\>さんはドラマ見ますか？</li><li>はじめまして。\<CA\>と申します</li></ul> |
| その他の個人情報 | `＊＊` でマスクして公開 | <ul><li>目の前に＊＊があります</li><li>＊＊で働いてます</li></ul> |
| 倫理的な問題 | 対話ごと非公開 | - |


### 話者データ

話者データには，話者IDをキーとして，話者ID，ペルソナ，性格特性，属性，テキストチャットの経験が含まれます．性格特性スコアが高いほど，その性格的な傾向が強いことを示します．

| キー | 型 | 説明 |
| --- | --- | --- |
| interlocutor_id | str | 話者ID． |
| persona | list (str) | ペルソナ．10文からなる． |
| personality | dict | 性格特性． |
| personality.BigFive_* | float | Big Five[^5] のスコア (1~7)． |
| personality.KiSS18_* | float | Kikuchi's Scale of Social Skills[^6] のスコア (1~5)． |
| personality.IOS | int | Inclusion of Others in the Self[^7] のスコア (1~7)． |
| personality.ATQ_* | float | Adult Temperament Questionnaire[^8] のスコア (1~7)． |
| personality.SMS_* | float | Self-Monitoring Scale[^9] のスコア (1~5)． |
| demographic_information | dict | 属性． |
| demographic_information.gender | str | 性別．`Male`, `Female`, `Other` のいずれか． |
| demographic_information.age | str | 年齢．`-19`, `20-29`, `30-39`, `40-49`, `50-59`, `60-69` のいずれか． |
| demographic_information.education | str | 教育歴．`High school graduate`, `Two-year college`, `Four-year college`, `Postgraduate`, `Other` のいずれか． |
| demographic_information.employment_status | str | 就労状況．`Employed`, `Homemaker`, `Student`, `Retired`, `Unable to work`, `None` のいずれか． |
| demographic_information.region_of_residence | str | 居住地域．日本の都道府県名． |
| text_chat_experience | dict | テキストチャットの経験． |
| text_chat_experience.age_of_first_chat | str | 初めてテキストチャットをした時の年齢．`-9`, `10-19`, `20-29`, `30-39`, `40-49`, `50-59` のいずれか． |
| text_chat_experience.frequency | str | 普段のテキストチャットの頻度．`Every day`, `Once every few days`, `Once a week`, `Less frequent than these` のいずれか． |
| text_chat_experience.chatting_partners | list (str) | 普段のテキストチャットの相手．`Family`, `Friend`, `Colleague`, `Other` のいずれか． |
| text_chat_experience.typical_chat_content | str | 普段のテキストチャットの内容． |

```jsonc
{
    "AH": {
        "interlocutor_id": "AH",
        "persona": [
            "私は学生である。",
            "埼玉県出身である。",
            "私は毎日朝食を食べない。",
            "私は毎日ウォーキングをする。",
            "私はよくコンビニに行く。",
            "私はタイピングが早い。",
            "自分は物覚えが悪い。",
            "自分は将来の目標が明確に決まっている。",
            "毎日楽しいことを見つけられる。",
            "自分は好きなものにはとことんこだわる。"
        ],
        "personality": {
            "BigFive_Openness": 5.25,                        // 開放性
            "BigFive_Conscientiousness": 3.1666666666666665, // 誠実性
            "BigFive_Extraversion": 3.3333333333333335,      // 外向性
            "BigFive_Agreeableness": 4.166666666666667,      // 協調性
            "BigFive_Neuroticism": 4.416666666666667,        // 神経症傾向
            "KiSS18_BasicSkill": 4.0,                        // 初歩的な社会的スキル
            "KiSS18_AdvancedSkill": 4.333333333333333,       // より高度の社会的スキル
            "KiSS18_EmotionalManagementSkill": 4.0,          // 感情処理の社会的スキル
            "KiSS18_OffenceManagementSkill": 4.0,            // 攻撃に代わる社会的スキル
            "KiSS18_StressManagementSkill": 4.0,             // ストレスを処理する社会的スキル
            "KiSS18_PlanningSkill": 4.666666666666667,       // 計画の社会的スキル
            "IOS": 4,                                        // 他人との関係
            "ATQ_Fear": 5.0,                                 // 恐れ
            "ATQ_Frustration": 3.5,                          // 欲求不満
            "ATQ_Sadness": 3.0,                              // 悲しさ
            "ATQ_Discomfort": 3.3333333333333335,            // 不快
            "ATQ_ActivationControl": 3.7142857142857144,     // 賦活的制御
            "ATQ_AttentionalControl": 3.8,                   // 注意
            "ATQ_InhibitoryControl": 3.142857142857143,      // 抑制的制御
            "ATQ_Sociability": 4.0,                          // 社交性
            "ATQ_HighIntensityPleasure": 4.571428571428571,  // 強い刺激への快
            "ATQ_PositiveAffect": 3.4,                       // 肯定的感情
            "ATQ_NeutralPerceptualSensitivity": 4.2,         // 知覚敏感性
            "ATQ_AffectivePerceptualSensitivity": 4.4,       // 感情的知覚敏感性
            "ATQ_AssociativeSensitivity": 4.8,               // 連想的敏感性
            "SMS_Extraversion": 2.6,                         // 外向性
            "SMS_OtherDirectedness": 3.5833333333333335,     // 他者指向性
            "SMS_Acting": 3.75                               // 演技性
        },
        "demographic_information": {
            "gender": "Male",
            "age": "-19",
            "education": "Other",
            "employment_status": "Student",
            "region_of_residence": "Saitama"
        },
        "text_chat_experience": {
            "age_of_first_chat": "-9",
            "frequency": "Every day",
            "chatting_partners": [
                "Family",
                "Friend"
            ],
            "typical_chat_content": "学校に関すること、事務連絡など"
        }
    },
    // ...
}
```


## 本コーパスの使用例

> [!CAUTION]
> **本コーパスの使用にあたっては，次のことに十分注意してください．**
> * 本コーパスのデータから個人を特定しようとしないこと．
> * 本コーパスを，特定の話者へのなりすましに用いないこと．
> * 本コーパスを話者の属性や性格特性の推定などに用いる際は，自身の情報を推定されたくない話者の権利についても留意すること[^10]．


### LLM による対話システムの構築[^11]

ここでは RealPersonaChat を対話システムの構築に利用した例を紹介します．我々は，RealPersonaChat のペルソナや性格特性を GPT-4/3.5 にプロンプトとして与えることで，4種類の対話システムを構築しました．第三者にシステムと対話をさせ，対話の一貫性，情報量，満足度を評価させたところ，ペルソナと性格特性の両方を与えることで高い評価が得られました．実験に用いたプロンプト，対話ログ，各対話に対する評価結果については [`llm_dialogue_system/`](./llm_dialogue_system/) をご覧ください．

|  | 一貫性 | 情報量 | 満足度 |
| --- | --- | --- | --- |
| GPT-4 + ペルソナ + 性格特性 | ***4.60*** | 4.50 | ***4.60*** |
| GPT-4 + ペルソナ | 4.57 | ***4.53*** | 4.53 |
| GPT-4 + 性格特性 | ***4.60*** | 4.00 | 4.07 |
| GPT-4（対話履歴のみ） | 4.57 | 4.47 | 4.23 |
| GPT-3.5 + ペルソナ + 性格特性 | 4.37 | 4.47 | 4.23 |


### 性格予測モデルの学習[^12]

RealPersonaChat を性格予測モデルの学習に利用した例は[こちら](https://github.com/fuyahuii/Personality-Recognition-on-RealPersonaChat/)です．


## 文献

```
@inproceedings{yamashita-etal-2023-realpersonachat,
    title = "{R}eal{P}ersona{C}hat: A Realistic Persona Chat Corpus with Interlocutors{'} Own Personalities",
    author = "Yamashita, Sanae  and
      Inoue, Koji  and
      Guo, Ao  and
      Mochizuki, Shota  and
      Kawahara, Tatsuya  and
      Higashinaka, Ryuichiro",
    booktitle = "Proceedings of the 37th Pacific Asia Conference on Language, Information and Computation",
    year = "2023",
    pages = "852--861"
}

@inproceedings{yamashita-etal-2024-realpersonachat-ja,
    title = "{R}eal{P}ersona{C}hat: 話者本人のペルソナと性格特性を含んだ雑談対話コーパス",
    author = "山下 紗苗 and 井上 昂治 and 郭 傲 and 望月 翔太 and 河原 達也 and 東中 竜一郎",
    booktitle = "言語処理学会第30回年次大会発表論文集",
    year = "2024",
    pages = "2738--2743"
}
```


## 謝辞

本コーパスは，[JSTムーンショット型研究開発事業，JPMJMS2011](https://www.avatar-ss.org/) の支援を受けて構築しました．

<img src="./Moonshot_Logo.png?raw=true" alt="ムーンショットのロゴ" style="max-width: 100%; width: 300px;">


## ライセンス

本コーパスは [CC BY-SA 4.0][cc-by-sa-link] の下で提供されます．

[![CC BY-SA 4.0][cc-by-sa-image]][cc-by-sa-link]


[^1]: Sanae Yamashita, Koji Inoue, Ao Guo, Shota Mochizuki, Tatsuya Kawahara, and Ryuichiro Higashinaka. [**RealPersonaChat: A realistic persona chat corpus with interlocutors' own personalities**](https://aclanthology.org/2023.paclic-1.85).  In Proceedings of the 37th Pacific Asia Conference on Language, Information and Computation, pp. 852-861, 2023.
[^2]: 山下 紗苗，井上 昂治，郭 傲，望月 翔太，河原 達也，東中 竜一郎．[**RealPersonaChat: 話者本人のペルソナと性格特性を含んだ雑談対話コーパス**](https://www.anlp.jp/proceedings/annual_meeting/2024/pdf_dir/B10-4.pdf)．言語処理学会第30回年次大会発表論文集, pp. 2738-2743, 2024.
[^3]: Hiroaki Sugiyama, Masahiro Mizukami, Tsunehiro Arimoto, Hiromi Narimatsu, Yuya Chiba, Hideharu Nakajima, and Toyomi Meguro. **Empirical analysis of training strategies of transformer-based Japanese chit-chat systems**. In Proceedings of 2022 IEEE Spoken Language Technology Workshop, pp. 685–691, 2023.
[^4]: Saizheng Zhang, Emily Dinan, Jack Urbanek, Arthur Szlam, Douwe Kiela, and Jason Weston. **Personalizing dialogue agents: I have a dog, do you have pets too?** In Proceedings of the 56th Annual Meeting of the Association for Computational Linguistics (Volume 1: Long Papers), pp. 2204–2213, 2018.
[^5]: 和田 さゆり．**性格特性用語を用いたBig Five尺度の作成**．心理学研究, pp.61–67, 1996.
[^6]: 菊池 章夫．**KiSS-18研究ノート**．岩手県立大学社会福祉学部紀要, vol. 6, no. 2, pp.41-51, 2004.
[^7]: Arthur Aron, Elaine N Aron, and Danny Smollan. **Inclusion of other in the self scale and the structure of interpersonal closeness**. Journal of Personality and Social Psychology, vol. 63, no. 4, p. 596, 1992.
[^8]: David E Evans and Mary K Rothbart. **Developing a model for adult temperament**. Journal of Research in Personality, vol. 41, no. 4, pp. 868–888, 2007.
[^9]: 岩淵 千明，田中 国夫，中里 浩明．**セルフ・モニタリング尺度に関する研究**．心理学研究, vol. 53, no. 1, pp. 54–57, 1982.
[^10]: Rachael Tatman. **What I won’t build**. Keynote at the Fourth Widening Natural Language Processing Workshop at Annual Meeting of the Association for Computational Linguistics. 2020. https://www.rctatman.com/files/Tatman_2020_WiNLP_Keynote.pdf
[^11]: Shinya Iizuka, Shota Mochizuki, Atsumoto Ohashi, Sanae Yamashita, Ao Guo, and Ryuichiro Higashinaka. [**Clarifying the dialogue-level performance of GPT-3.5 and GPT-4 in task-oriented and non-task-oriented dialogue systems**](https://ojs.aaai.org/index.php/AAAI-SS/article/view/27668/27441). In Proceedings of the AAAI Symposium Series, vol. 2, no. 1, pp. 182-186. 2023.
[^12]: Yahui Fu, Haiyue Song, Tianyu Zhao, and Tatsuya Kawahara. **Enhancing personality recognition in dialogue by data augmentation and heterogeneous conversational graph networks**. In Proceedings of the 14th International Workshop on Spoken Dialogue Systems Technology, 2024


[cc-by-sa-link]: https://creativecommons.org/licenses/by-sa/4.0/deed.ja
[cc-by-sa-image]: https://i.creativecommons.org/l/by-sa/4.0/88x31.png
[cc-by-sa-shield]: https://img.shields.io/badge/License-CC%20BY%20SA%204.0-green.svg
