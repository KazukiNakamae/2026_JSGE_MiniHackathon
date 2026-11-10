# B｜Google Colabでoff-target予測モデルを学習する

**進め方：** コーディングエージェントとGoogle Colab　**成果物：** 学習済みモデル、再実行できるノートブック、評価表とログ

ガイドRNAと候補DNAの配列を入力に、SpCas9によるindelが検出された候補を上位に並べるニューラルネットを作ります。公開されたSURRO-seqの測定結果で学習し、学習に使っていないガイドで評価します。

## まず、やりたいことを伝える

最初のプロンプトでは、何を予測したいかを自分の言葉で伝えます。例えば、次のように始めます。

> 将来は自分で取得したoff-targetの測定データで予測モデルを作りたいです。まず公開データを使い、Google ColabでガイドRNAと候補DNAの配列から予測するニューラルネットを学習して、初めて見るガイドでどの程度予測できるか確かめたいです。

返ってきた内容を読み、次に何を頼むかを一段階ずつ考えます。[共通の5段階](../README.md#agent-workflow)のうち、環境の準備はColabのCPUランタイムで行います。

## 学習に使う公開データ

| 取得先 | 使うデータ |
| --- | --- |
| [PanらのSURRO-seq論文・Figure 3](https://doi.org/10.1038/s41467-022-31543-6)・[Supplementary Data 3（XLSX）](https://static-content.springer.com/esm/art%3A10.1038%2Fs41467-022-31543-6/MediaObjects/41467_2022_31543_MOESM6_ESM.xlsx) | LibBのガイドと候補配列、SpCas9とMOCKの測定結果。論文でフィルタ後に残った7,140候補のうち、753件が有意なindel検出と判定された。 |
| [NCBI GEO GSE206347](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE206347)・[NCBI BioProject PRJNA850372](https://www.ncbi.nlm.nih.gov/bioproject/PRJNA850372) | 同じ研究の登録先。当日の学習は上の処理済み表から始める。 |
| [LinとWongのCRISPR-Net](https://doi.org/10.1002/advs.201903562) | ガイドと候補の配列の組をニューラルネットに入力する先行研究。当日は1層の隠れ層を持つ小さなモデルで試す。 |

論文の判定を使い、有意なindelが検出されたLIOT・HIOTを **1**、測定した範囲で有意な検出がなかったNSOTを **0** とします。低カバレッジなどで除外された行や判定不明の行は集計から外します。予測するのは、SURRO-seqの条件で測定した候補の判定です。候補配列は細胞内のレポーターで測定されているため、内在性座位での編集効率を調べる場合には、別の測定データで評価が必要です。

## Colabで学習し、未使用のガイドで評価する

以下は各段階の到達点です。どの条件を次の依頼に含めるかは、手元のデータや直前の結果を見て決めます。

1. **環境を用意する。** [Google Colab](https://colab.research.google.com/)のCPUランタイムを開きます。Python、PyTorch、pandas、openpyxl、scikit-learnのバージョンを記録し、補足表が読めるところまで確認します。[Colabのランタイム案内](https://research.google.com/colaboratory/runtime-version-faq.html)
2. **学習スクリプトを作る。** ガイドと候補の組を1行にまとめます。ガイドの20塩基と候補の23塩基（PAMを含む）を同じ向きにそろえ、A/C/G/Tをone-hot encodingします。ラベルやindel頻度は入力に含めず、配列だけを使います。PyTorchで小さな全結合ネットワークを作り、二値分類として学習させます。ガイドの20塩基配列を単位に学習・検証・テストに分け、同じガイドが複数の組に入らないようにします。分割はseedを固定し、各組に両クラスが含まれるか確認します。
3. **小さな入力でテストする。** 手で確認できる数行で配列の向き、ミスマッチ数、ラベルの割り当てを照合します。同じガイドが分割をまたがず、モデルが1行につき1つのスコアを返すことを確かめます。条件を調整する場合は検証用の結果を使います。
4. **ログを取りながら本解析を行う。** テストが通ったら公開データで学習し、最後にテスト用のガイドで評価します。同じテスト行で、ミスマッチが少ない順に並べた場合とニューラルネットを比較します。評価表にはガイド数、候補数、陽性数、Average Precision（AP）を載せます。余裕があればprecision–recall曲線を描き、CFDなどの既存スコアも、採点できる同じテスト行で比較します。データの出典、フィルタ条件、分割、seed、学習時のloss、評価値をログに残し、ノートブックとモデルの重みと一緒に保存します。

**自分の測定データに置き換える場合**　ガイド配列、候補配列とPAM、測定条件、測定値、ラベルの判定基準をそろえます。測定していない候補と、測定したうえで有意な検出がなかった候補は別に扱います。非公開データを使う場合は、扱える環境でノートブックを実行してください。

<a name="candidate-search"></a>

## 発展｜候補検索と既存スコアを比較する

[Haeusslerらの補足表2](https://doi.org/10.1186/S13059-016-1012-2)と[再現用リポジトリ](https://github.com/maximilianh/crisporPaper)を手本に、[TsaiらのGUIDE-seq（NCBI GEO GSE61586）](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE61586)の検出済み部位を使います。Cas-OFFinderとCRISPORで[参照ゲノム（元の比較はhg19）](https://hgdownload.soe.ucsc.edu/goldenPath/hg19/bigZips/)、PAM、ミスマッチ上限、bulgeの扱いをそろえ、検出済み部位を何件拾うか比べます。同じ候補集合をCFDや[MOFF](https://github.com/MDhewei/MOFF)で並べ、上位20件に入る既知部位の数を比較できます。GUIDE-seqの未検出は未ラベルとして扱います。
