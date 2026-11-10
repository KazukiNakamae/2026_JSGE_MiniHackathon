# I｜gRNAの配列とCasの機能・Cas9のアミノ酸置換

gRNAの配列を変えた場合と、Casタンパク質を変えた場合の提案です。その後のプロンプトは、直前の結果と[共通の進め方の例](../README.md#agent-workflow)を参考に考えられます。

## I1｜gRNAを変えたときの構造とCasの機能を比べる

**まず、やりたいことを伝えるときの例**

> gRNAのscaffoldや標的に相補的な部分を変えると、Cas9と組んだときの切断や転写抑制がどう変わるか、実測データから調べたいです。

**参考論文と取得先**　BushらのBLADE研究では、scaffoldを変えたgRNAの配列とGFP knockoutなどの測定値が論文の補足Data S2にあります。SpCas9に結合したsgRNAの実測構造はPDBで取得できます。機能の違いを調べる別の実データとして、天然の長いtracrRNA（tracr-L）がCas9を転写抑制に使う研究があり、RNA-seqとspacer取得のリードがSRAにあります。[Bush et al. (2023) とData S2](https://pmc.ncbi.nlm.nih.gov/articles/PMC10529641/) · [SpCas9–sgRNA–DNA構造 PDB 4UN3](https://www.rcsb.org/structure/4UN3) · [tracr-Lの研究](https://www.sciencedirect.com/science/article/pii/S0092867420316871) · [SRA PRJNA679244](https://www.ncbi.nlm.nih.gov/bioproject/PRJNA679244)

**再解析の案**　同じGFP spacer・細胞で測った野生型scaffoldと変異型数種類を選び、RNAfoldでガイド全体の二次構造を予測します。変わった塩基をPDBの対応領域に重ね、Data S2のGFP knockoutと並べる案です。RNAfoldの予測は遊離RNA、PDBは結合状態の構造です。機能の切り替わりに関心があれば、tracr-L研究のRNA-seqのRunを選び、野生型とtracr-L欠失でcas遺伝子群の発現を比べてFigure 3と照合できます。相補長を変えたときの抑制と切断は同論文のFigure 5で別々の測定として扱います。

## I2｜Cas9のアミノ酸置換で活性と特異性がどう変わるか調べる

**まず、やりたいことを伝えるときの例**

> Cas9の一残基を変えたとき、完全一致の標的で活性を保ちつつミスマッチ標的で活性が下がるか、公開データで調べたいです。

**参考論文と取得先**　SpencerとZhangのdeep mutational scanningでは、SpCas9の変異ライブラリを選抜し、段階ごとのシーケンスデータをSRAに登録しています。選んだ変異体については、論文の一致標的とミスマッチ標的の活性データを参照できます。[Spencer & Zhang (2017)](https://doi.org/10.1038/s41598-017-17081-y) · [SRA SRP107783](https://www.ncbi.nlm.nih.gov/sra/?term=SRP107783)

**再解析の案**　選抜前後で対応が取れるライブラリから一つの領域を選び、置換ごとのread countと濃縮度を集計し、論文の位置別プロファイルと照合します。活性の比較には、原著で個別に測った同じガイドと標的の組を使い、Figure 4のように完全一致標的とミスマッチ標的での活性を比較できます。候補置換をCas9の実測構造に重ねると位置関係を確認できます。選抜の濃縮度、個別アッセイの活性、構造上の位置はそれぞれ別の列にします。
