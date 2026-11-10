# A｜マウスの発現解析・ノックイン設計・Off-target解析

疾患モデルマウスの発現解析と、ゲノム編集の設計・解析について、公開データで試せる案をまとめました。関心のある工程だけを試したり、手元のデータに合う部分を取り入れたりできます。処理済みデータから始める案と、公開シーケンスから上流の処理を試す案を併記しています。

- [A1｜Bulk RNA-seq](#bulk-rna-seq)
- [A2｜snRNA-seq](#snrna-seq)
- [A3｜Spatial Transcriptomics](#spatial)
- [A4｜ノックイン設計と編集後のアレル判定](#knock-in)
- [A5｜DISCOVER-Seq+](#discover-seq-plus)
- [A6｜WGSによるOff-target候補の確認](#wgs)

## まず、やりたいことを伝えるときの例

コーディングエージェントに相談するなら、最初に調べたいことを伝える例があります。

> 疾患モデルマウスの海馬で、どの遺伝子の発現が変わるか調べたいです。公開データで、サンプルごとの違いが見える図を作ってみたいです。

同じように、下の案から関心のある問いを自分の言葉に置き換えられます。返ってきた内容を見ながら、環境の用意、コード作成、小さなテスト、ログを残す本解析へ進む場合は、[共通の進め方の例](../README.md#agent-workflow)も参考になります。

<a name="bulk-rna-seq"></a>

## A1｜Bulk RNA-seqで海馬の発現を比べる

**文献・公開データ**　Fornerらは、5xFADマウスと野生型の海馬・大脳皮質のbulk RNA-seqを公開しています。GEOにはカウント表 `GSE168137_countList.txt.gz` とサンプル情報、SRAには元のRNA-seqリードがあります。8か月齢・雌・海馬の5xFADの例は `GSM5129651`（`SRX10221148`）です。[Fornerらの論文](https://doi.org/10.1038/s41597-021-01054-y)・[GEO GSE168137](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE168137)・[SRA BioProject PRJNA706199](https://www.ncbi.nlm.nih.gov/bioproject/PRJNA706199)・[GSM5129651](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSM5129651)

**再解析の案**　処理済みカウント表なら、8か月齢の雌の海馬を例に、カウント列とサンプル情報を照合して5xFADと野生型を比べられます。マウスを反復単位とし、カウント形式を確認したうえで、整数カウントからDESeq2で遺伝型差を推定する部分再解析です。図表の例は、マウスごとの正規化発現量と、log2 fold change・補正済みp値の表です。元リードから試すなら、1サンプルに絞ってQC、論文に沿ったSTARとRSEMによるマッピング・定量、公開カウント表との照合までを扱う案があります。

<a name="snrna-seq"></a>

## A2｜snRNA-seqで疾患関連アストロサイトを調べる

**文献・公開データ**　Habibらの5xFAD・野生型マウスの単一核RNA-seqです。GEOには7か月齢海馬の全核のUMIカウント `GSE143758_Admouse_Hippocampus_7m_AllNuclei_UMIcounts.txt.gz` と、アストロサイトに絞った表があります。SRAには元リードがあり、例として7か月齢海馬の野生型 `GSM4276897` と5xFAD `GSM4276898` が挙げられます。[Habibらの論文](https://doi.org/10.1038/s41593-020-0624-8)・[GEO GSE143758](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE143758)・[SRA BioProject PRJNA602299](https://www.ncbi.nlm.nih.gov/bioproject/PRJNA602299)

**再解析の案**　公開カウントから核のQC、クラスタリング、markerの可視化を試せます。Figure 2aとSupplementary Table 2を手本に、`Gfap`、`Serpina3n`、`Ctsb` などをdot plotにし、疾患関連アストロサイト（DAA）の発現パターンを照らし合わせる部分再解析です。核の注釈と個体IDが対応づけられる場合は、DAAの割合を個体ごとに比較する案にもつながります。元リードを扱う場合は、GEOのサンプル欄でライブラリ方式を確認し、1サンプルのUMIカウント作成までに絞ると進めやすくなります。

<a name="spatial"></a>

## A3｜Spatial Transcriptomicsで切片上の発現を見る

**文献・公開データ**　Chenらの研究は、アルツハイマー病モデルの `AppNL-G-F` ノックインマウスと野生型の脳切片を対象としています。GEOのシリーズページにはspot別のraw countsとlogCPM、SRAには元リードがあります。18か月齢モデルの冠状断 `N05_D2` は `GSM4647075`（`SRX8640059`）です。[Chenらの論文](https://doi.org/10.1016/j.cell.2020.06.038)・[GEO GSE152506](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE152506)・[SRA BioProject PRJNA643227](https://www.ncbi.nlm.nih.gov/bioproject/PRJNA643227)・[切片の記録](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSM4647075)

**再解析の案**　切片1枚のspot ID、座標、発現値を対応づけ、論文のプラーク関連遺伝子群から1〜2遺伝子の発現マップを作る案です。同じ切片に対応するプラークの注釈や画像の位置合わせが確認できれば、近傍と遠方のspotも比べられます。群間の差を評価する場合はマウスを反復単位とします。元リードから進む場合は、論文で使われたSpatial Transcriptomics Pipelineの入力とバーコード情報を照合し、1切片のspot別カウント作成を試す範囲が考えられます。[ssREADの表示・加工済みデータ](https://bmblx.bmi.osumc.edu/ssread/spatial/ST002)・[Dの空間解析案](D.md)

<a name="knock-in"></a>

## A4｜ノックイン設計と編集後のアレル判定

**設計ツールを試す案**　挿入型ノックインでは、マウス参照ゲノムに対応する **protoSpaceJAM** で、公開の挿入座位についてガイド候補とHDR donorの出力を比較できます。PAM、挿入位置から切断点までの距離、再切断を避けるための配列変更が確認項目です。**KOnezumi** は、マウスのノックアウト向けに標的exon、ガイド候補、genotyping用プライマーを出すツールです。塩基編集で点変異を検討する場合は、BE-Designerで編集窓とbystanderを読む案もあります。これらは参照配列上の設計を試す題材です。[protoSpaceJAM論文](https://doi.org/10.1093/nar/gkae553)・[公式マニュアル](https://czbiohub-sf.github.io/protoSpaceJAM/)・[KOnezumi論文](https://doi.org/10.1093/bioinformatics/btz090)・[筑波大学のツール一覧](https://larc-tsukuba.github.io/mouse-meets-computer/)・[BE-Designer論文](https://doi.org/10.1186/s12859-018-2585-4)

**公開シーケンスで判定を再解析する案**　KunoらのDAJIN論文では、マウスの `Cables2` flox knock-inのNanopore ampliconデータを使い、野生型、意図したflox、片側loxP、欠失などを判定しています。Figure 6Bの一部を、DDBJ DRA `DRA011971` のリードと論文のサンプル対応・参照アレルから再集計する案です。小さく始めるなら、著者のリポジトリにある野生型1例とflox設計のfounder 2例のFASTQを使い、アレル分類と支持リードの割合を確認できます。ここで得る割合は、ampliconリードにおける割合です。[Kunoらの論文・Figure 6とS1 Table](https://doi.org/10.1371/journal.pbio.3001507)・[DDBJ DRA DRA011971](https://ddbj.nig.ac.jp/resource/sra-submission/DRA011971)・[DAJINの公開FASTQ例と実行方法](https://github.com/akikuno/DAJIN)・[論文で使用した版](https://github.com/akikuno/DAJIN/tree/manuscript-version)

<a name="discover-seq-plus"></a>

## A5｜DISCOVER-Seq+で切断周辺のMRE11シグナルを見る

**文献・公開データ**　Zouらは、マウス肝臓の `Pcsk9` 編集を例に、DISCOVER-SeqとDISCOVER-Seq+のMRE11 ChIP-seqシグナルを比較しています。論文ではFASTQと処理済みBAMの登録先をSRA `PRJNA801688`（`SRP362082`）としています。著者のデモが指定するマウス肝臓の反復3は、DISCOVER-Seq+が `SRR18188706`、通常のDISCOVER-Seqが `SRR18188701` です。Figure 6b–dは標的と代表off-targetのトラック、Figure 6eは個体ごとの検出部位数です。[Zouらの論文・Figure 6](https://doi.org/10.1038/s41592-023-01840-z)・[SRA BioProject PRJNA801688](https://www.ncbi.nlm.nih.gov/bioproject/PRJNA801688)・[著者のデモと計算済み結果](https://github.com/rogerzou/DSeqPlus#demo-please-ensure-100-gb-of-free-disk-space)・[Run番号の対応表（著者スクリプト）](https://github.com/rogerzou/DSeqPlus/blob/main/download_reads_demo.sh)・[BLENDER](https://github.com/staciawyman/blender)

**再解析の案**　Figure 6のSource Dataから個体別の検出数を描き直すか、著者のデモにある反復3の計算済み候補表を照合するところから始められます。対応する肝臓のBAMを入手できれば、mm10でFigure 6b–dのいずれかの座位を取り出し、条件別のシグナルを比べる案があります。FASTQから比較する場合は、両条件で入力リード数をそろえる論文の手順を参照し、Bowtie 2とsamtoolsでの前処理、BLENDERでの候補検出へ進めます。MRE11 ChIP-seqで見ているのは切断に伴うシグナルで、WGSの変異コールとは測定対象が異なります。著者のデモは全処理に100 GBの空き容量を見込んでいるため、講習中に試すなら計算済みの結果や単一座位から始める案があります。

<a name="wgs"></a>

## A6｜WGSで候補座位と変異コールを照合する

**文献・公開データ**　Petersonらは、Cas9編集マウスと未処置対照のWGSを比較しました。元リードはSRA `PRJNA687003`、一次フィルタ後のVCFはEVA `PRJEB61387`、候補座位と検出した変異は論文のSupplementary Data 1・3にあります。未処置対照の元リードの例は `SRR13342004` です。[Petersonらの論文・Figure 3](https://doi.org/10.1038/s42003-023-04974-0)・[SRA BioProject PRJNA687003](https://www.ncbi.nlm.nih.gov/bioproject/PRJNA687003)・[EVA PRJEB61387](https://www.ebi.ac.uk/eva/?eva-study=PRJEB61387)・[SRA SRR13342004](https://trace.ncbi.nlm.nih.gov/Traces/?run=SRR13342004)

**再解析の案**　ガイド1本またはfounder 1個体について、mm10上の候補座位と変異コールを照合する小規模な再解析です。公開補足表を手本に、コールがある候補、対照にもある変異、深度などの理由で判定できない候補を区別できます。EVAのVCFは論文の最終フィルタ前のデータなので、論文の集計と照合する際は適用したフィルタを確認します。元リードを使う案では、BioProjectのRunからWGSの編集個体と未処置対照を対応づけ、代表サンプルのQCや選んだ座位のpileupまでを試せます。候補検索やツール間比較には[B2](B.md#candidate-search)も参照できます。
