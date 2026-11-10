# H｜AAV vector・FVIIIとFIXのタンパク質工学・ゲノム編集後のWGS

AAVの配列設計、凝固因子の構造と活性、編集マウスのWGSを、公開データから調べる提案です。その後のプロンプトは、直前の結果と[共通の進め方の例](../README.md#agent-workflow)を参考に考えられます。

## H1｜AAVカセットの構成とpackaging後の配列を比べる

**まず、やりたいことを伝えるときの例**

> 報告済みのFVIII用AAVカセットの長さを要素ごとに比べたいです。packaging後のリードで、設計どおりの長さの分子をどう見分けるかも知りたいです。

**参考論文と取得先**　McIntoshらの論文には短縮したFVIIIを載せたAAVの設計例があり、RefSeqの `NM_000132.4` からヒト全長F8のCDSを取得できます。packaging後を調べる実データには、CuiらがAAV2とAAV2.7m8を比較したlong-readデータがあります。こちらのカセットはEGFPまたはfirefly luciferaseです。[FVIII用AAVの論文](https://pmc.ncbi.nlm.nih.gov/articles/PMC3637010/) · [RefSeq NM_000132.4](https://www.ncbi.nlm.nih.gov/nuccore/NM_000132.4) · [Cuiらのpackaging研究](https://pmc.ncbi.nlm.nih.gov/articles/PMC11600122/) · [SRA PRJNA1127696](https://www.ncbi.nlm.nih.gov/bioproject/PRJNA1127696)

**再解析の案**　FVIIIカセットの図と論文記載の長さから、ITR、発現調節配列、CDS、poly(A)シグナルの内訳を描きます。全長F8のCDSと比べると、短縮による差を示せます。長さの記載がない要素は、図で未確定とします。実測リードを使う場合は、同じssAAV-CB6-EgfpカセットのAAV2とAAV2.7m8のRunを選び、リード長の分布を比較します。対応するcis-plasmidの参照配列が入手できれば、coverageを表示して、Figure 3cのIGV図にある欠落や途切れを照合できます。FVIII用カセットの長さの比較と、EGFPカセットのpackagingの実測は分けて扱います。

## H2｜FVIIIの工学的改変を実測構造に重ねる

**まず、やりたいことを伝えるときの例**

> ヒトFVIIIと、分泌や活性の改善が報告されたFVIIIの構造を比較し、改変された領域を見たいです。

**参考論文と取得先**　ET3iはBドメインを除いたヒト・ブタのキメラFVIIIです。ET3iの結晶構造と、論文中で比較したヒトFVIIIの再精密化構造をPDBから取得できます。関連研究にはET3とヒト由来の短縮型FVIII（HSQ）の活性や発現の測定値があります。[ET3iの構造論文](https://pmc.ncbi.nlm.nih.gov/articles/PMC6940532/) · [PDB 6MF0](https://www.rcsb.org/structure/6MF0) · [PDB 6MF2](https://www.rcsb.org/structure/6MF2) · [ET3とHSQの活性・発現比較](https://pmc.ncbi.nlm.nih.gov/articles/PMC4362354/)

**再解析の案**　対応するドメインを重ね、ブタ由来の領域とC2ドメインの向きを表示し、原著の重ね合わせ図の一部を再現できます。比較する残基をそろえ、座標のない領域は図に注記します。活性も扱うなら、同じアッセイで測ったET3とHSQの値を別の図にして、構造の差と活性の差を並べます。

## H3｜FIX PaduaとR338の置換を比較する

**まず、やりたいことを伝えるときの例**

> FIX Paduaの置換位置を実測構造で見たいです。同じ位置のほかの置換と比べて、比活性がどう違うかも調べたいです。

**参考論文と取得先**　FIXのR338を各アミノ酸に置換した研究に、同一部位の置換と比活性の比較があります。原著が位置の説明に使った野生型FIXaのプロテアーゼ領域はPDBで取得できます。Padua（R338L）と野生型をFVIIIaの条件別に比較した研究も参照できます。[R338置換スキャンの論文](https://pmc.ncbi.nlm.nih.gov/articles/PMC7948292/) · [PDB 1RFN](https://www.rcsb.org/structure/1RFN) · [FVIIIa依存性を調べた論文](https://pmc.ncbi.nlm.nih.gov/articles/PMC6675543/)

**再解析の案**　成熟FIXのR338をPDBの残基番号に対応させ、野生型構造上で位置を表示します。原著のR338置換スキャンから、野生型、R338L、ほかの置換の比活性を描き直す案です。図から読み取った値には出典を記録します。FVIIIaの有無で差がどう変わるかを扱う場合は、別研究の同一パネル内で比較します。

## H4｜編集マウスのWGSでon-targetとoff-targetを照合する

**まず、やりたいことを伝えるときの例**

> Cas9で編集したマウスのWGSを使って、on-targetの変化と、予測off-target座位に重なる変異を区別して調べたいです。

**参考論文と取得先**　Petersonらの編集マウスと未処置マウスの比較では、WGSとCIRCLE-seqの元リードをSRA、一次フィルタ済みVCFをEVAから取得できます。Supplementary Data 1にはガイド、Data 3には候補座位に重なった変異と検証情報、Data 5には図の元データがあります。[Peterson et al. (2023)](https://doi.org/10.1038/s42003-023-04974-0) · [SRA PRJNA687003](https://www.ncbi.nlm.nih.gov/bioproject/PRJNA687003) · [EVA PRJEB61387](https://www.ebi.ac.uk/eva/?eva-study=PRJEB61387) · [著者の解析コード](https://github.com/The-Centre-for-Phenogenomics/Cas9-WGS)

**再解析の案**　編集個体を一つ選び、mm10上のon-target範囲と予測off-target範囲を、一次フィルタ済みVCFに照合する案です。VCFとBEDの座標規則をそろえ、編集個体だけに見えるものと対照にもあるものを区別し、Data 3の報告と比較します。元リードを扱うなら、対象座位周辺のアラインメントを表示してread depthとalt支持を確認できます。予測座位と重なった変異、原著で確認された変異、カバレッジが足りない座位は、結果表で区別します。
