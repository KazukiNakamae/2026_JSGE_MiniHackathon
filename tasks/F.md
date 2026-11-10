# F｜LSR–DNA構造・組換え酵素の配列認識・予測モデルの種間転用

公開構造やシーケンスデータを使った解析の提案です。各節の最初のプロンプトは、やりたいことを伝える例です。その後は、返ってきた結果と[共通の進め方の例](../README.md#agent-workflow)を参考に、次の依頼を考えられます。

## F1｜Boltz-2でLSR–DNA複合体の構造を予測する

**まず、やりたいことを伝えるときの例**

> Boltz-2でBxb1とattP・attB DNAの複合体を予測して、公開されたcryo-EM構造とどこが一致するか見たいです。

**参考論文と取得先**　Bxb1はlarge serine recombinase（LSR）の一つです。Bxb1とattP・attBの複合体について、cryo-EMの座標とマップが公開されています。PDBの `9IU6` と `9IU5` は、同じCA/CAの中心配列でstrand exchangeの異なる状態を捉えた構造です。[Bxb1の構造と工学 (2026)](https://doi.org/10.1016/j.molcel.2026.05.018) · [PDB 9IU6](https://www.rcsb.org/structure/9IU6) · [PDB 9IU5](https://www.rcsb.org/structure/9IU5) · [EMDB EMD-60891](https://www.ebi.ac.uk/emdb/EMD-60891) · [Boltz-2公式ドキュメント](https://github.com/jwohlwend/boltz/blob/main/docs/prediction.md)

**再解析の案**　まずPDBでタンパク質とDNA鎖の配列、コピー数、接触する残基と塩基を確認します。計算環境があれば、公開構造と対応する鎖をBoltz-2に入力し、モデルのDNAの向きと接触位置をPDBの二つの状態と比べる案です。比較対象のPDBをtemplateに使ったかを記録し、既報構造の再現と未知配列への予測を区別します。Boltz-2のaffinity出力はタンパク質と低分子向けです。LSR–DNAでは、構造の一致や信頼度を比較し、結合定数や組換え効率は別の実測値として扱います。

## F2｜Rec-seqで組換え酵素の認識配列と活性を比べる

**まず、やりたいことを伝えるときの例**

> 組換え酵素の変異で、認識する塩基と組換えの活性がどう変わるか、公開データから調べたいです。

**参考論文と取得先**　BessenらはRec-seqで、野生型Creと変異体の認識配列を調べました。Figure 2の濃縮度はSource Data、元リードはSRAにあります。LSRで効率を比べる題材には、F1のBxb1変異体の組換え活性に加えて、DurrantらがBxb1と別のLSRを比較した公開リードとSource Dataがあります。Creのデータは、Bxb1とは異なる組換え酵素で解析手順を試す例です。[Bessen et al. (2019)](https://doi.org/10.1038/s41467-019-09987-0) · [SRA PRJNA517947](https://www.ncbi.nlm.nih.gov/bioproject/PRJNA517947) · [Bxb1変異体の論文](https://doi.org/10.1016/j.molcel.2026.05.018) · [AddgeneのenBxb1](https://www.addgene.org/260030/) · [Durrant et al. (2023)](https://doi.org/10.1038/s41587-022-01494-w) · [SRA PRJNA778877](https://www.ncbi.nlm.nih.gov/bioproject/PRJNA778877)

**再解析の案**　Source Dataから野生型CreとR259Aを取り出し、loxPの向きと塩基位置をそろえて濃縮プロファイルを描き直します。元リードから始める場合は、選抜前後の対応を確認し、一つの変異体と基質ライブラリに絞って位置別の塩基頻度と濃縮度を再計算し、Figure 2bと照合する案です。効率に関心があれば、Bxb1の野生型とenBxb1について、同じ基質・同じ測定条件の組換え活性を図表から比較できます。Rec-seqでの相対濃縮度と、別のアッセイで測った活性にはそれぞれ単位を添えます。

## F3｜off-target予測を別の生物で評価する

**まず、やりたいことを伝えるときの例**

> ヒトのデータで作ったCas9のoff-target予測をゼブラフィッシュに当てたいです。ゼブラフィッシュのデータでfine-tuningすると性能が変わるかも比べたいです。

**参考論文と取得先**　ヒトのCIRCLE-seq原著とゼブラフィッシュでCRISPRツールを比較した論文に、同じアッセイの実測リードがあります。ゼブラフィッシュのBioProjectにはRNA-seqも含まれます。CIRCLE-seqのRunと対応する対照を区別して取得できます。[Tsai et al. (2017)](https://doi.org/10.1038/nmeth.4278) · [ヒト CIRCLE-seq SRA SRP103697](https://www.ncbi.nlm.nih.gov/sra/?term=SRP103697) · [ゼブラフィッシュの比較研究](https://doi.org/10.1186/s12864-021-08238-1) · [ENA PRJEB39643](https://www.ebi.ac.uk/ena/browser/view/PRJEB39643)

**再解析の案**　ゼブラフィッシュのガイド1本について、論文の参照ゲノムと対照を使い、CIRCLE-seqの候補一覧を再集計してSupplementary Table 3と照合します。モデルを比較する場合は、ヒトとゼブラフィッシュで候補列挙条件とラベルの定義をそろえます。重みが公開され追加学習できるモデル、または[B1のモデル作成案](B.md)を出発点に、転用前後をゼブラフィッシュの同じ保留ガイドで比較する案です。ガイド単位で分割し、追加学習に使ったガイドは評価から外します。CIRCLE-seqで未検出の候補は、その条件での未検出として記録します。
