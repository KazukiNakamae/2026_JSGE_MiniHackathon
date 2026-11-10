# C｜AAV挿入・ヌクレアーゼのoff-target・リン酸化プロテオーム解析

公開データから始められる解析の提案です。C1〜C4では、AAVの非標的挿入を題材に、ゲノム上の挿入位置や残ったvector配列を、データの種類ごとに調べます。C5はヌクレアーゼによる切断の検出、C6は質量分析データを使ったリン酸化の解析です。

コーディングエージェントに最初に伝える内容の例を各節に載せています。その後は、直前の結果と[共通の進め方の例](../README.md#agent-workflow)を参考に、次に何を頼むか考えられます。

## C1｜AAVの非標的挿入をITR-seqで調べる

**まず、やりたいことを伝えるときの例**

> ITR-seqの公開データで、AAVと宿主DNAの接合部から挿入座位を調べたいです。リード数と、重複を除いた座位数も比べたいです。

**参考論文と取得先**　Bretonらは、ITRから宿主側に伸びるリードを使って挿入位置を同定しました。Table 2には、ヌクレアーゼを搭載しないAAV8-EGFPを投与したアカゲザルと、非標的sgRNAを使ったマウスの結果があります。原著のシーケンスデータの登録先はSRAです。小さい実データの例として `SRR11212462` があり、これはAsCpf1と標的sgRNAを投与したマウスの試料です。[Breton et al. (2020)](https://doi.org/10.1186/s12864-020-6655-4) · [SRA PRJNA609560](https://www.ncbi.nlm.nih.gov/sra/?term=PRJNA609560) · [SRX7825406のRun情報](https://www.ncbi.nlm.nih.gov/sra/SRX7825406) · [Penn-GTPのITR-Seq2](https://github.com/Penn-GTP/ITR_Seq2_public)

**再解析の案**　公開Runのうち、論文の試料名と対応を確認できたものから、ITRと宿主の境界を含むリードを抽出します。論文と同じ参照ゲノムに宿主側をマッピングし、座位ごとにまとめた結果を補足表の座標と照合する案です。小さいマウスRunから始める場合は、標的座位と報告済みの候補を照合する手順の確認に使えます。ヌクレアーゼを搭載しない条件については、Table 2のAAV8-EGFP群を集計する方法もあります。リード数と座位数を併記すると、PCRで増えたリードを別の挿入として数える影響や、ITRが欠けた挿入を回収しにくいことを考えられます。[論文のTable 2](https://link.springer.com/article/10.1186/s12864-020-6655-4/tables/2)

編集ヌクレアーゼを載せないAAVを導入したマウスの生リードから非標的挿入を追う場合は、別の論文の `PRJNA1150240` も使えます。ITR近傍の接合部を回収するligation-mediated PCRのデータで、原著の再解析にはAAVengeRを使います。試料とRunの対応はTable S2にあります。[マウスのAAV挿入研究 (2025)](https://pmc.ncbi.nlm.nih.gov/articles/PMC12461645/) · [SRA PRJNA1150240](https://www.ncbi.nlm.nih.gov/sra/?term=PRJNA1150240) · [AAVengeR](https://github.com/helixscript/AAVengeR)

投入ゲノム数で正規化した挿入座位数を描くには、ZhengらのTable 1、Figure 1Aと補足資料を参照できます。この比較は処理済みデータから始められます。[Zheng et al. (2023)](https://doi.org/10.1089/hum.2023.134)

## C2｜Targeted Enrichment SequencingでAAV挿入座位を比べる

**まず、やりたいことを伝えるときの例**

> 同じAAV投与試料のshort-readとlong-readのTargeted Enrichment Sequencingを比べたいです。両方で見つかる挿入と、片方でだけ見つかる挿入の違いを見たいです。

**参考論文と取得先**　カニクイザルの肝臓を対象に、AAV配列をcaptureしたshort-readとlong-readを比較した研究があります。サルの実測リードは `PRJNA1060711` に登録されています。同論文の `PRJNA1060724` はHEK293・HepG2の比較用データです。AAVの再解析には、Table S1でサルの試料とライブラリを照合し、投与したvectorの参照配列を確認できる組み合わせを使う案です。[Sheehan et al. (2024)](https://doi.org/10.1016/j.omtm.2024.101352) · [SRA PRJNA1060711](https://www.ncbi.nlm.nih.gov/sra/?term=PRJNA1060711) · [short-read用コード](https://github.com/sheehanmarkj/insertional_mutagenesis_short-read_public) · [long-read用コード](https://github.com/sheehanmarkj/insertional_mutagenesis_long-read_public)

**再解析の案**　同じ個体・同じ肝臓試料に対応するライブラリを一組取り上げ、著者のコードで、宿主とvectorの両方にマップされるリードを抽出します。short-readではsplit readと宿主・vectorに分かれてマップされるread pairを区別し、long-readでは接合部をまたぐリードを確認します。参照ゲノムの版と座標の許容幅をそろえて、両手法で一致する座位と片方で検出された座位を表にできます。Figure 2のように接合部をvector上に並べると、ITR以外の配列からも挿入を検出できるか確認できます。captureしたリードの割合は、宿主ゲノム全体での挿入率とは分けて扱います。

## C3｜WGSとGenEditScanでvector由来配列を探す

**まず、やりたいことを伝えるときの例**

> WGSのリードからGenEditScanでvector由来配列を拾いたいです。候補が見つかったら、宿主との接合部を支持するリードも調べたいです。

**参考論文と取得先**　GenEditScanの参照論文では、ゲノム編集イネと野生型のpaired-end WGSを使って、vectorと一致する `k-mer` を比較しています。AAVのWGSについては、AAVを導入したMCF10Aクローンを解析し、WGSとlong-read TESの挿入座位を照合したCoratellaらの研究があります。別のAAV比較研究の `PRJNA927280` は、非公開配列がマスクされたデータです。著者はそのデータだけでの再現は困難と記載しています。[Itoh et al. (2020)](https://doi.org/10.1038/s41598-020-61949-5) · [DDBJ PRJDB8199](https://www.ebi.ac.uk/ena/browser/view/PRJDB8199) · [GenEditScan](https://github.com/hirsakai/GenEditScan) · [Coratella et al. (2026)](https://pmc.ncbi.nlm.nih.gov/articles/PMC13524469/) · [SRA PRJNA1422021](https://www.ncbi.nlm.nih.gov/bioproject/PRJNA1422021) · [マスク済みデータの公開条件](https://pmc.ncbi.nlm.nih.gov/articles/PMC10209688/) · [SRA PRJNA927280](https://www.ncbi.nlm.nih.gov/sra/?term=PRJNA927280)

**再解析の案**　GenEditScanの手順を試すなら、公開イネWGSのT₀と野生型から同じ規模のread pairを取り出し、論文で用いたvector配列が入手できれば、vector上の `k-mer` の分布を比べられます。全量での比較結果はFigure 3が参照できます。vector配列を入手できない場合は、論文のFigure 2を参考に、公開イネWGSの一部に既知のColE1由来の模擬リードを加えて手順を試す方法もあります。AAVを調べる場合は、CoratellaらのTable S6でWGSを行ったクローンと報告座標を確認します。該当するWGSの公開Runを確認できれば、宿主とvectorの接合部を支持するリードを探し、同じクローンのlong-read TESの報告と照合できます。GenEditScanをAAVに適用するのは追加解析で、同じvectorのFASTAと対応する未導入対照のWGSがそろう場合に試せます。`k-mer` の一致はvector配列を含む候補を示します。挿入座標は宿主との接合部のアラインメントで確認します。[ColE1の参照配列 J01566](https://www.ncbi.nlm.nih.gov/nuccore/J01566)

## C4｜局所アセンブリとFCS-adaptorで挿入配列を確認する

**まず、やりたいことを伝えるときの例**

> AAVの挿入をまたぐlong readから局所アセンブリを作りたいです。どの断片をFCS-adaptorで拾えて、どの断片がvectorとの直接比較で見つかるかを知りたいです。

**参考論文と取得先**　Coratellaらは、挿入座位が分かっているMCF10Aクローンのlong-read TESから、挿入配列の欠失、向き、連結を解析しました。Figure 4には非標的座位も載っています。実測リードは `PRJNA1422021` にあり、例えば `SAMN55294932` はMC11クローンを希釈したTESの試料です。挿入構造を確認する場合は、同じプロジェクトで非希釈のクローンを探し、論文の座位一覧と対応づけます。NCBIのFCS-adaptorは、アセンブリ済みFASTA上でアダプターやvector由来の候補を報告するツールです。[Coratella et al. (2026)](https://pmc.ncbi.nlm.nih.gov/articles/PMC13524469/) · [SRA PRJNA1422021](https://www.ncbi.nlm.nih.gov/bioproject/PRJNA1422021) · [BioSample SAMN55294932](https://www.ncbi.nlm.nih.gov/biosample/SAMN55294932) · [著者のlong-read TES解析コード](https://github.com/Novartis/Pipeline_for_TES_AAV_LongReads/tree/main/Manuscript_coratella_et_al_version_static) · [NCBI FCS-adaptor](https://github.com/ncbi/fcs/wiki/FCS-adaptor-quickstart)

**再解析の案**　まず、一つの報告座位で宿主とvectorの両方にアラインするlong readを確認し、原著の挿入構造と照合する案です。同じ座位を独立に支持するリードが十分にあれば、局所アセンブリを作る追加解析も考えられます。得られたcontigを宿主と投与vectorの参照配列にそれぞれアラインし、挿入断片と左右の接合部を図にします。FCS-adaptorの検出区間を同じcontigに重ねると、登録済みの配列で拾える範囲を確認できます。FCS-adaptorのヒットの有無にかかわらず、挿入の判定には宿主との接合部と元のリードの支持を使います。

## C5｜GUIDE-seqでヌクレアーゼのoff-targetを調べる

**まず、やりたいことを伝えるときの例**

> GUIDE-seqの公開データから、Cas9のon-targetとoff-targetの候補を出して論文の結果と比べたいです。

**参考論文と取得先**　Lazzarottoらは、ヒト初代T細胞を用いたGUIDE-seqと、精製DNAを用いたCHANGE-seqのデータを公開しています。`PRJNA625995` には複数のアッセイが含まれるため、SRAでGUIDE-seqのライブラリを区別します。著者の解析手順は別のプロトコル論文にも載っています。[Lazzarotto et al. (2020)](https://pmc.ncbi.nlm.nih.gov/articles/PMC7652380/) · [SRA PRJNA625995](https://www.ncbi.nlm.nih.gov/bioproject/PRJNA625995) · [GUIDE-seqの解析プロトコル](https://pmc.ncbi.nlm.nih.gov/articles/PMC9331158/) · [著者の解析コード](https://github.com/aryeelab/guideseq)

**再解析の案**　ガイドを一つ選び、対応する公開対照と一緒に解析します。タグを支持するリードを抽出し、重複を処理して、候補の座標、ガイドとのmismatch、支持数を論文の報告と照合する案です。例えばTRAC site 3は、論文にon-targetが検出された例として図示されています。off-target候補が報告された別のガイドを題材にすると、候補順位や背景の除外条件も比較できます。GUIDE-seqの支持数は、このアッセイの条件で得た切断の指標として読みます。[論文の図と補足資料](https://pmc.ncbi.nlm.nih.gov/articles/PMC7652380/)

## C6｜EGF刺激後のリン酸化プロテオームを解析する

**まず、やりたいことを伝えるときの例**

> EGFで刺激した細胞の公開リン酸化プロテオームを使い、EGFR–MAPK経路でどのリン酸化部位が先に応答するかを見たいです。

**参考論文と取得先**　MCF10A細胞のEGF応答を質量分析で測定した研究があります。生の質量分析ファイルはProteomeXchangeのMassIVE、論文は処理済みの部位別データの取得先としてPhosphoprotein Explorerを案内しています。補足のTable S8にもphosphopeptideと部位別のデータがあります。[リン酸化プロテオーム研究](https://pmc.ncbi.nlm.nih.gov/articles/PMC10418157/) · [ProteomeXchange PXD044049](https://proteomecentral.proteomexchange.org/cgi/GetDataset?ID=PXD044049) · [Phosphoprotein Explorer](https://a947342.fmphost.com/fmi/webd/Phosphoprotein_Explorer_V1) · [著者のphosphosite集計コード](https://github.com/PNNL-Systems-Biology/EGFR_Phosphoproteomics_Analysis)

**再解析の案**　処理済みデータから、EGF 1 ng/mLの0〜12分の時系列を取り出し、Figure 3Bのうち、GAB1 Y259、PTPN11 Y542、MAPK1 T185などを描き直す案です。タンパク質IDと部位番号をそろえ、各時点を刺激前に対する変化として示し、欠測は欠測のまま残します。生データから進める場合は、時系列に対応するTMTセットを確認し、論文のペプチド同定、部位の割り当て、レポーターイオンの定量手順をたどって、選んだ部位の値を処理済みデータと照合する案もあります。
