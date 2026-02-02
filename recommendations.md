# 投票合并方法与评委淘汰组建议

本文基于本仓库中的分析数据，回答三个问题：  
（1）**评委 + 粉丝投票**的两种合并方法（排名法 / 百分比法）在各季的表现如何，哪种更偏向粉丝投票？  
（2）针对**评委与粉丝意见不一致的「争议名人」**，两种方法是否让其结果不同？若增加**「评委从垫底两对选淘汰组」**环节，结果会如何？  
（3）未来应推荐哪种合并方法，以及是否建议增加评委选淘汰组的环节？

数据依据：`merge_methods_season_summary.csv`、`merge_methods_elimination.csv`、`controversial_summary.csv`、`controversial_contestants.csv`、`merge_methods_elimination_with_judge_bottom2.csv`。

---

## 一、两种合并方法对比：哪种更偏向粉丝投票？

**结论：在各季汇总上，「百分比法」整体更偏向粉丝投票；多数赛季中其淘汰顺序与「仅按粉丝票 pq」更一致，最终名次与纯粉丝名次的 Spearman 相关也多数更高。**

### 1.1 数据说明

- **排名法**：当周按评委排名（rw）与粉丝排名（rq）合并得到综合排名，再决定淘汰。  
- **百分比法**：当周综合分 = 评委占比（pw）+ 粉丝占比（pq），再按该分排名决定淘汰。  
- 比较维度：与「仅按 pq 的淘汰顺序」的**一致率**（当周淘汰者是否与纯 pq 淘汰者相同）；以及**最终名次与纯 pq 名次的 Spearman 相关系数**（越高表示越贴近粉丝名次）。

### 1.2 各季一致率（与纯 pq 淘汰顺序）

`merge_methods_season_summary.csv` 中：

- **pct_agree_rank**：排名法当周淘汰与纯 pq 一致的比例；**pct_agree_pct**：百分比法的一致比例。  
- 在 **34 季中有 21 季**，百分比法的一致率 **≥** 排名法（如 S1、S2、S3、S6、S9、S10、S12–S14、S17、S20、S21、S23–S24、S28–S34）；仅 **3 季**（S4、S18、S19）排名法一致率更高；其余为持平。  
- 典型例子：S1 排名法一致率 0.50、百分比法 0.83；S12、S13、S14 百分比法一致率达 1.0，排名法为 0.7；S29、S33、S34 百分比法一致率明显高于排名法。

因此，从「当周淘汰者与纯粉丝淘汰是否一致」看，**百分比法在多数赛季更偏粉丝**。

### 1.3 最终名次与纯粉丝名次的 Spearman 相关

同表字段 **spearman_rank_vs_pq**、**spearman_pct_vs_pq**：

- 多数赛季中 **spearman_pct_vs_pq ≥ spearman_rank_vs_pq**（如 S1、S2、S3、S10、S12–S15、S17、S20、S21、S23–S24、S28–S34），即百分比法得到的**最终名次**与「仅按 pq 的名次」更接近。  
- 仅 S4、S18、S19 三季排名法的 Spearman 更高。

**小结**：若节目目标为「更贴近观众意愿」、让结果更偏粉丝投票，**百分比法**在一致率与最终名次两方面都整体更偏粉丝；排名法则更偏评委。

---

## 二、争议选手在两方法下的差异，以及「评委选垫底两对」的效果

**结论：争议选手（评委排名靠后、粉丝支持高）在两种方法下淘汰结果存在差异的案例虽有限，但一旦有差异，多为「排名法淘汰、百分比法未淘汰」；增加「评委选垫底两对进淘汰组、组内按 pq 淘汰一人」后，部分赛季中争议选手会被该规则淘汰，且当周淘汰者常与仅用排名法/百分比法时不同。**

### 2.1 争议选手在两方法下是否结果不同？

`controversial_summary.csv` 中 **n_fate_diff_rank_vs_pct** 表示：该季中「在排名法下被淘汰而在百分比法下未淘汰」或反之的争议选手**人次数**。

- 存在差异的赛季包括：S1、S2、S14、S17、S24、S29、S30、S32、S34 等（n_fate_diff 为 1 或 2）。  
- **n_eliminated_rank** 与 **n_eliminated_pct**：在多数有争议选手被淘汰的赛季，**n_eliminated_rank ≥ n_eliminated_pct**（如 S1 为 1 vs 0，S14、S17、S24、S29、S30、S32、S34 等均为排名法淘汰争议选手而百分比法未淘汰或更少）。  

即：**两种方法对争议选手确实会产生不同结果，且百分比法下争议选手被淘汰的比例整体更低**，与「百分比法更偏粉丝」一致。

### 2.2 增加「评委选垫底两对进淘汰组」后的效果

模拟规则：评委先选当周垫底两对进入淘汰组，淘汰组内按粉丝票（pq）最低淘汰一人。  
`controversial_summary.csv` 中 **n_eliminated_judge_bottom2** 表示该季中因此规则被淘汰的争议选手人次。

- **n_eliminated_judge_bottom2 > 0** 的赛季包括：S1(3)、S2(3)、S11(2)、S14(1)、S15(1)、S17(1)、S29(7)、S30(8)、S31(9)、S32(8)、S33(6)、S34(8)。  
- 尤其近季 S29–S34，该环节会显著增加争议选手被淘汰的人次，使「评委认为最差的两对」必然进入淘汰组，再由粉丝在组内决定谁走。

`merge_methods_elimination_with_judge_bottom2.csv` 中 **diff_judge_vs_rank**、**diff_judge_vs_pct** 表示：该周「评委选淘汰组」规则下的淘汰者是否与排名法/百分比法不同。

- 全季多周次中，存在大量 **diff_judge_vs_rank = True** 或 **diff_judge_vs_pct = True** 的个案，即增加该环节后**当周淘汰者常与仅用排名法或百分比法时不同**，从而在保留粉丝在淘汰组内决定权的同时，提高了评委意见对「谁进淘汰组」的影响。

**小结**：增加「评委选垫底两对」环节后，争议选手更容易进入淘汰组并被淘汰（尤其近季）；当周淘汰结果与仅用排名法/百分比法时往往不同，起到平衡评委与粉丝的作用。

---

## 三、最终建议

### 3.1 合并方法：推荐「百分比法」

若希望结果**更偏向粉丝投票**、与「仅按 pq」的淘汰顺序和最终名次更一致，建议采用**百分比法**（当周综合分 = pw + pq，再按该分排名决定淘汰）。  
依据：各季一致率与 Spearman 相关均显示百分比法在多数赛季更偏粉丝（见第一节）；争议选手被淘汰比例在百分比法下整体更低（见第二节）。

若希望**评委排名（rw）的权重更显性**，可保留排名法；当前数据表明排名法整体更偏评委。

### 3.2 是否增加「评委选垫底两对进淘汰组」环节：建议可增加

建议**可增加**该环节，作为平衡评委与粉丝、兼顾公平与观赏性的可选规则。  
依据：该环节使「评委认为最差的两对」必然进入淘汰组，再由粉丝票（pq）在两人中决定谁走，既体现评委专业意见，又保留观众决定权；模拟显示在部分赛季（尤其 S29–S34）会适度提高争议选手被淘汰的比例，减少「评委极低分但粉丝高支持者」长期留到后期的现象。  
若节目更强调「纯观众决定」或极简规则，可不增加；增加该环节不会完全压制粉丝意愿，淘汰组内仍由观众/综合票决定。

---

## 四、相关性及 SVD 分析结论

基于 `correlation_results.csv`、`correlation_svd_loadings.csv`、`correlation_svd_loadings_singular.csv` 及 `correlation_figs/` 下图表（SVD 已对列做 z-score 标准化），结论如下。

### 4.1 correlation_results 小结

- **职业（celebrity_industry）**：对 pq、pw 均有显著影响（Kruskal-Wallis p 均 &lt; 1e-15）。效应量对 pq（η²≈0.060）略高于对 pw（η²≈0.044），即职业对粉丝票（pq）的组间差异略大。
- **地区（region）**：本数据中 region 检验未得到有效统计量（可能组数或样本结构导致），仅保留 CI；具体分布见「各地区 mean(pq)/mean(pw)」条形图。
- **家乡州（celebrity_homestate）**：对 pq、pw 均显著（pq 的 p≈1.7e-20，pw 的 p≈2.8e-08），对 pq 的效应量（η²≈0.063）大于对 pw（η²≈0.030），即 homestate 对粉丝票影响更明显。
- **年龄**：与 pq、pw 均呈显著负相关（Spearman ρ≈-0.31），p&lt;1e-55。年龄越大，粉丝占比与评委占比均倾向于更低。

**小结**：职业、homestate、年龄对 pq 与 pw 均有显著影响；职业和 homestate 对粉丝票（pq）的组间差异略强于对评委票（pw），年龄则同时负向影响两者且幅度接近。

### 4.2 SVD 小结（列 z-score 后）

- **奇异值**：前 5 个成分奇异值依次约为 66.5、51.2、49.5、41.9、27.6，第 6 个接近 0（列 z-score 后 rank 接近 5）。前两维已解释大部分方差。
- **成分 1**：载荷以 pq、pw（约 -0.65）和 age（约 0.39）为主，表示「粉丝/评委占比低 vs 年龄偏大」的主方向。
- **成分 2**：主要由 industry（约 -0.74）、age（约 0.52）、homestate（约 0.38）驱动，pq、pw 载荷较小（约 0.13–0.16）；即第二主成分主要区分职业、年龄与家乡州，与 pq/pw 的直接关联较弱。
- **成分 3**：homestate、industry 载荷大；**成分 5**：pq 与 pw 反向（约 -0.71 vs 0.71），表示「粉丝票与评委票相对高低」的独立维度；**成分 6**：仅 region 有载荷，其余接近 0。

**小结**：前两主成分主要由年龄、职业、homestate 及 pq/pw 共同决定；pq 与 pw 的差异更多体现在第 5 成分上；region 单独占据第 6 成分（z-score 后仅一个有效维度）。

### 4.3 图表含义

- **SVD 成分图**（svd_component_1/2.png）：横条长度为各变量在该成分上的载荷绝对值；条越长表示该变量在该主成分上权重越大。
- **职业/地区/homestate 条形图**：各职业、各地区、各 homestate 的 mean(pq) 与 mean(pw)；可看出哪些职业或地区在粉丝票或评委票上均值更高。
- **年龄散点图**（age_vs_pq_pw.png）：年龄与 pq、pw 的散点及线性拟合线；整体呈负相关，与 Spearman 结果一致。

### 4.4 一句总结

职业、家乡州、年龄对粉丝票（pq）与评委票（pw）均有显著影响，其中职业与 homestate 对 pq 的组间差异略强；SVD 显示前两主成分由年龄、职业、homestate 及 pq/pw 共同驱动，pq 与 pw 的相对高低由第 5 成分刻画，地区（region）单独占据最后一维。

---

## 五、专业舞者与名人属性对表现的影响（模型与数据分析）

基于粉丝投票（pq）与评委打分（pw）数据，分析**专业舞者（ballroom_partner）**与**名人属性（年龄、行业、地区、家乡州）**对比赛表现的影响，并对比这些因素对评委打分与粉丝投票的影响是否相同。

### 5.1 问题与指标

- **结果变量**：当周粉丝占比（pq）、当周评委占比（pw）。  
- **预测因素**：名人年龄（连续）、职业（celebrity_industry）、地区（region）、家乡州（celebrity_homestate）、专业舞者（ballroom_partner）。  
- **对比目标**：各因素对 pq 与 pw 的效应是否同向、是否显著、量级是否相近。

### 5.2 模型与数据

- **模型**：对 z-score 标准化后的 pq、pw 分别建立线性模型  
  - pq_z = X β_pq + ε_pq，pw_z = X β_pw + ε_pw。  
  X 包含：年龄（**z-score 标准化**）、行业/地区/家乡州（虚拟变量，drop_first）。专业舞者因水平数较多（60+），回归中未纳入，其效应由 Kruskal-Wallis 与效应量（见 `correlation_results.csv`）及「按 ballroom_partner 分组的 mean(pq)/mean(pw)」条形图（`correlation_figs/pq_pw_by_ballroom_partner.png`）描述。  
- **数据**：`merged_long_table.csv`，当周层面；listwise 删除缺失后 n = 2402。

### 5.3 结果与对比

- **correlation_results.csv**（含专业舞者）：  
  - **ballroom_partner**：对 pq、pw 均有显著影响（Kruskal-Wallis p 均 &lt; 1e-25）；对 pq 的 η²≈0.132、对 pw 的 η²≈0.087，即专业舞者对粉丝票的组间差异大于对评委票。  
  - 职业、家乡州、年龄结论与第四节一致（职业与 homestate 对 pq 效应略强；年龄对两者均负向且幅度接近）。

- **attribute_effect_coefficients.csv**（z-score 后 OLS）：  
  - **年龄（age_z）**：对 pq_z、pw_z 的系数均为负（约 -0.29、-0.30），均显著（p ≈ 0）；效应同向且量级接近，即年龄对评委打分与粉丝投票的影响**相近**。  
  - **职业（industry）**：各虚拟变量系数在 pq 与 pw 上多数同向；平均而言职业对 pw 的负向效应略大（`attribute_effect_comparison.csv` 中 celebrity_industry 的 coef_pw 绝对值略大），且两者均显著。  
  - **家乡州（homestate）**：平均系数对 pq 为负、对 pw 为正（comparison 表），说明不同州对 pq 与 pw 的影响方向存在差异；两者均有显著水平。

- **attribute_effect_comparison.csv**：  
  - age：same_sign=True，sig_pq、sig_pw 均为 True；效应同向且均显著。  
  - celebrity_industry、celebrity_homestate：same_sign=True，均有显著项；职业与家乡州对 pq、pw 均有影响，但量级或方向存在组间差异。

- **模型拟合**：R²（pq_z）≈ 0.17，R²（pw_z）≈ 0.15，说明属性与地区/家乡州可解释约 15–17% 的 pq/pw 变异。

### 5.4 结论（这些因素对评委打分和粉丝投票的影响是否相同？）

**不完全相同。**  
- **年龄**：对评委打分与粉丝投票的影响**相近**（同向、均显著、系数量级接近）。  
- **职业与家乡州**：对 pq 与 pw 均有显著影响，但职业与 homestate 对粉丝票（pq）的组间差异（η²）或相对权重略强于对评委票（pw）；部分职业/家乡州在 pq 与 pw 上的系数方向或量级存在差异。  
- **专业舞者**：对 pq、pw 均有显著效应，对 pq 的效应量（η²）大于对 pw，即专业舞者对粉丝投票的影响更明显。

---

## 六、数据与脚本对应

| 结论依据           | 数据文件 |
|--------------------|----------|
| 合并方法偏粉丝程度 | `merge_methods_season_summary.csv`、`merge_methods_elimination.csv` |
| 争议选手与淘汰差异 | `controversial_contestants.csv`、`controversial_summary.csv` |
| 评委垫底两对模拟   | `merge_methods_elimination_with_judge_bottom2.csv` |
| 相关性与 SVD       | `correlation_results.csv`、`correlation_svd_loadings.csv`、`correlation_svd_loadings_singular.csv`、`correlation_figs/` |
| 专业舞者与名人属性对 pq/pw 的影响 | `correlation_results.csv`、`correlation_figs/`（含 **pq_pw_by_ballroom_partner.png**）、`attribute_effect_coefficients.csv`、`attribute_effect_comparison.csv` |

分析脚本：`build_long_table.py`、`correlation_analysis.py`、`merge_methods_analysis.py`、`controversial_analysis.py`、`attribute_effect_model.py`。
