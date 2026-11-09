# H｜AAVカセットの注釈とWGS変異の照合

**進め方：** コーディングエージェント　**成果物：** カセットのサイズ比較図か、ゲノム座標で変異を照合した表

## まず、やりたいことを伝える

最初のプロンプトは、調べたいことを自分の言葉で伝えます。選んだ題材の例を参考にしてください。

> 報告済みのFVIIIカセットで、ITR、発現調節配列、コード領域がそれぞれどのくらいの長さか比べたいです。

WGSを選ぶ場合

> 編集マウスのWGSで、予測したoff-target座位にどんな変異コールがあるか照合したいです。

その後は[共通の5段階](../README.md#agent-workflow)に沿って、直前の結果を見て次の依頼を自分で考えます。

## 使う公開データ

| 取得先 | ファイル・データ | 用途 |
| --- | --- | --- |
| [McIntoshらの論文と補足資料](https://pmc.ncbi.nlm.nih.gov/articles/PMC3637010/) | 報告済みの短縮型FVIIIカセット | カセット各要素の注釈 |
| [RefSeq F8 NM_000132.4](https://www.ncbi.nlm.nih.gov/nuccore/NM_000132.4)・[RefSeq F9 NM_000133.4](https://www.ncbi.nlm.nih.gov/nuccore/NM_000133.4) | バージョンを指定した参照配列 | コード領域の長さと位置番号 |
| [FIX-Paduaの研究](https://pubmed.ncbi.nlm.nih.gov/31219805/)・[BryantらのAAV2データ](https://github.com/alibashir/aav) | 活性の図とカプシドのpackaging測定値 | タンパク質やカプシドを選ぶ場合に使用 |
| [PetersonらのSupplementary Data 1、3、5](https://doi.org/10.1038/s42003-023-04974-0) | 予測候補座位、変異コール、図の元データ | WGSでの照合 |
| [NCBI BioProject PRJNA687003](https://www.ncbi.nlm.nih.gov/bioproject/PRJNA687003)・[EVA PRJEB61387](https://www.ebi.ac.uk/eva/?eva-study=PRJEB61387) | WGS・CIRCLE-seqの元リードと一次フィルタ済みVCF | データの登録先と発展解析 |

## カセット注釈かWGSを選ぶ

- **AAV・FVIII**　報告済みのFVIIIカセットを一つ選び、ITR、発現調節配列、コード領域の長さを図にします。比較用に全長F8のコード領域を添え、長さが分からない要素にはその旨を記載します。発展課題では、FIX-Paduaの活性をFVIIIaの有無で比較するか、AAV2の配列の違いとpackagingの実測値の関係を描きます。
- **ゲノム編集・WGS**　論文のガイド1本かfounder 1個体を選び、mm10上で予測座位とWGSの変異コールを照合します。意図した標的、変異コールが一致した予測off-target座位、判定がつかない座位を分けます。照合に使った座標範囲とVCF・BEDの座標の数え方を記録し、利用できる場合は未処置群との比較も確認します。

**テストで確かめること**　カセットを選んだ場合は、長さの単位と内訳の合計を確かめます。WGSを選んだ場合は、一致する座位と一致しない座位を一つずつ用意し、座標の照合結果を手で確かめます。
