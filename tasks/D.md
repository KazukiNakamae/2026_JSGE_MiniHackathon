# D｜AlphaGenomeとSpatial Transcriptomics

公開トラックや組織切片をブラウザで見比べる提案です。どちらも、まず見たい変化を講師・補助員と話すところから始められます。

## D1｜AlphaGenomeのTAL1予測とChIP-seq実測を照合する

**相談のきっかけ**　「TAL1の上流に挿入があるとき、何が変わると予測されるか。実測ではどの位置にシグナルがあるか。」

**参考論文と取得先**　Mansourらは、Jurkat細胞のTAL1上流にある挿入と、その周辺のH3K27acやMYBを調べました。GEOにはJurkatのH3K27ac ChIP-seqの処理済みWIGと元リードがあります。AlphaGenome側は、同じ挿入を使った公式チュートリアルと、JBrowseに保存された予測を参照できます。[Mansour et al. (2014)](https://pmc.ncbi.nlm.nih.gov/articles/PMC4720521/) · [GEO GSE59657](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE59657) · [Jurkat H3K27ac GSM1519642](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSM1519642) · [AlphaGenome論文](https://doi.org/10.1038/s41586-025-10014-0) · [公式チュートリアル](https://www.alphagenomedocs.com/colabs/v1/example_analysis_workflow.html) · [JBrowseの計算済みトラック](https://jbrowse.org/jb2/docs/tutorials/alphagenome/)

**再解析の案**　TAL1の転写開始点と挿入位置を目印に、予測図でRNA-seq、DNase-seq、H3K27acの変化を見ます。実測は、GEOのH3K27acトラックを対応する参照ゲノムで表示するか、原著の同じ座位のパネルを見ます。モデルの予測差（変異配列と参照配列の差）とJurkatの実測シグナルを別々に記録し、H3K27acの位置関係を比べる案です。予測の細胞種はCD34陽性の骨髄系前駆細胞で、Jurkatの実測とは条件が異なります。トラックごとに細胞種、ゲノム版、測定か予測かを添えると比較できます。

## D2｜マウス脳のSpatial Transcriptomicsを切片上で見る

**相談のきっかけ**　「マウス脳の切片で、プラークに関連する遺伝子がどこで発現するか見てみたい。」

**参考論文と取得先**　Chenらは、アルツハイマー病モデルのマウスで、アミロイドプラーク周辺の発現を調べました。GEOにspot別の発現カウントと元リードがあります。例えば `GSM4647075` はAPP<sup>NL-G-F</sup>の切片です。AlzMapとssREADにも切片の情報があります。[Chen et al. (2020)](https://doi.org/10.1016/j.cell.2020.06.038) · [GEO GSE152506](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE152506) · [切片 GSM4647075](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSM4647075) · [AlzMap](https://alzmap.org/) · [ssREAD ST002](https://bmblx.bmi.osumc.edu/ssread/spatial/ST002)

**再解析の案**　公開ビューアまたは講師・補助員と開いた公開切片で、例えば `Cst7` の発現を組織上に表示します。同じ切片でプラークの注釈を確認できる場合は、同じ脳領域の近傍と遠方で、発現が高いspotの分布を見比べられます。表示できる切片が限られる場合は、論文の切片画像と公開発現マップで領域と遺伝子を照合する案もあります。切片ID、遺伝子、色の尺度を記録すると、同じ表示を見直せます。
