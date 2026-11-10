# G｜Tet-Onのサイレンシング・クロマチン設計・rtTA・トランスポゾン

Tet-Onの実測データを読み直す案と、関連する配列やタンパク質を調べる案です。最初にやりたいことを伝えた後は、直前の結果と[共通の進め方の例](../README.md#agent-workflow)を参考に次の依頼を考えられます。

## G1｜Tet-Onの長期サイレンシングと誘導前の発現を比べる

**まず、やりたいことを伝えるときの例**

> Tet-Onの回路で、30日後にどれくらい誘導できるかと、doxycyclineなしでも発現するかを別々に調べたいです。

**参考論文と取得先**　Yanagiらは、Tet-Onの誘導プロモーターとA2UCOEの間の配列を比較しました。Figure 2Hと4Aに関係する実測値を、論文末尾のSource Data（XLSX）から参照できます。Figure 2AのようにA2UCOE周辺のアクセシビリティを確認する場合は、別研究がGEOに公開したPGP1 iPSCのATAC-seq（bigWigと元リード）も参照できます。[Yanagi et al. (2025) と補足ファイル](https://doi.org/10.1186/s13036-025-00499-8) · [PGP1 iPSCのATAC-seq GEO GSM8673591](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSM8673591)

**再解析の案**　Yanagiらの図で、時点・構成・doxycyclineの有無・単位を確認し、長期培養後の誘導応答と誘導前の発現を別の図にします。Figure 2Hは30日後の誘導応答、Figure 4Aは別の比較群での誘導前の発現です。A2UCOE周辺を調べるなら、公開bigWigをhg38で開き、`HNRNPA2B1` と `CBX3` の間のピーク位置をFigure 2Aと比べます。ATAC-seqは元のゲノム上でのアクセシビリティ、FOXN1のRT-qPCRは導入した回路の発現の測定です。

## G2｜Evo 2のオープンクロマチン設計をATAC-seqで確かめる

**まず、やりたいことを伝えるときの例**

> Evo 2で設計された配列について、狙った位置にATAC-seqのピークが出たか、公開データを使って確かめたいです。

**参考論文と取得先**　Evo 2の原著は、設計配列を導入したマウス胚性幹細胞とヒト細胞でATAC-seqを測定しています。実測の元リードはSRA、試した配列はSupplementary Data 3にあります。[Evo 2論文 (2026)](https://doi.org/10.1038/s41586-026-10176-5) · [マウス胚性幹細胞 PRJNA1314301](https://www.ncbi.nlm.nih.gov/bioproject/PRJNA1314301) · [ヒト細胞 PRJNA1314272](https://www.ncbi.nlm.nih.gov/bioproject/PRJNA1314272) · [Evo 2のコード](https://github.com/arcinstitute/evo2)

**再解析の案**　まず公開済みの設計を一つ選び、対応する配列と試料をそろえます。例えばマウス胚性幹細胞の設計で、ATAC-seqリードを挿入配列込みの参照にマッピングし、Figure 6に沿って狙ったピーク区間と周辺のcoverageを描く案です。公開済みの配列から始めると、モデルによる配列生成と、設計後の実測評価を別々に検討できます。この実測はクロマチンのアクセシビリティを示します。Tet-Onの誘導や長期発現にはG1の測定値が対応します。

## G3｜rtTA変異体の応答とTetRの実測構造を読む

**まず、やりたいことを伝えるときの例**

> rtTAの変異体で、doxycyclineに対する応答と誘導前の発現がどう変わるか比べたいです。変異が構造のどこにあるかも見たいです。

**参考論文と取得先**　Kämperらの論文にrtTA変異体のdoxycycline用量応答があります。構造比較の実データには、別研究の逆応答型TetR L17Gと野生型TetRがあります。PDBから座標を取得できます。TetRはrtTAのDNA結合・制御部の参考で、PDBの構造は活性化ドメインを含むrtTA全長の構造とは別です。[rtTA変異体の用量応答 (2002)](https://doi.org/10.1016/S0014-5793%2802%2902587-5) · [逆応答型TetRの構造研究](https://pmc.ncbi.nlm.nih.gov/articles/PMC2490752/) · [PDB 2VKV](https://www.rcsb.org/structure/2VKV) · [PDB 2TCT](https://www.rcsb.org/structure/2TCT) · [rtTA3の登録配列](https://www.addgene.org/62446/sequences/)

**再解析の案**　論文の同じ細胞・レポーター条件の曲線から、doxycyclineなしの発現、濃度ごとの応答、高濃度での発現を並べます。図から読み取る場合はパネルと単位を記録します。構造では、原著と同じ組み合わせでTetRを重ね、L17Gと周辺の残基を表示できます。rtTA3の登録配列を使う場合は、翻訳した配列をTetRの実測構造に対応づけ、どこまでがDNA結合・制御部かを確認できます。用量応答と構造は、測定した変異体名を添えて整理します。

## G4｜TTAAへの依存をトランスポゾンの挿入データから調べる

**まず、やりたいことを伝えるときの例**

> piggyBacがTTAA以外にもどの程度入るか調べたいです。別のトランスポゾンの挿入位置にはどんな配列の偏りがあるかも見たいです。

**参考論文と取得先**　同じマウス肝臓モデルでpiggyBacとSleeping Beautyを比較した研究に、挿入点を回収したSBE-seqの元リードと処理済みBEDがあります。Tol2の比較資料にはゼブラフィッシュで測定した挿入部位の周辺配列があります。[piggyBacとSleeping Beautyの比較研究](https://pmc.ncbi.nlm.nih.gov/articles/PMC10070360/) · [SRA PRJNA605684](https://www.ncbi.nlm.nih.gov/bioproject/PRJNA605684) · [GEO GSE212895](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE212895) · [Tol2の挿入研究と補足配列](https://doi.org/10.1186/1471-2164-10-418)

**再解析の案**　処理済みBEDから、原著と同じGRCm39の参照で挿入位置の周辺配列を取り出し、向きをそろえてsequence logoを作ります。piggyBacのTTAA、Sleeping BeautyのTAと、各canonical motif以外の位置を分けて数え、Figure 2C・DとTable S3を参照します。Tol2を加える場合は論文の周辺配列から別のlogoを作り、種と取得法の違いを添えます。明瞭な必須motifの有無と、塩基組成への弱い偏りを区別する題材です。
