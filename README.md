# Genomer-analysis
## 动物驯化及群体历史研究：
* 与驯化相关的研究热点主要集中在：家养动物何时何地被驯化？驯化途径是什么？驯化表型是什么？与驯化表型相关的基因有哪些？家养动物驯化地点大致主要集中在三个地方：近东地区、中国中部和安第斯山脉。几乎所有家养动物都是在近几千年或几万年内被驯化。
* 驯化的途径主要分为三种：*共生、捕食和直接驯化*。共生：指人类与被驯化物种的野外物种长期处在相同环境下，逐渐与人和谐共处达到驯化的目的，猫和狗是通过该方式驯化的；捕食：指人类相对被驯化动物是捕食者，当人类狩猎捕食的个体太多的时候，会将一些个体圈养起来，进而达到驯化的目的，牛与猪是通过该方式驯化：直接驯化：指人类为了某项具体的目的直接改造物种的方式，例如人类养蚕是为了丝绸织布，驯化马和骆能是为了运输。
* 人工驯化的家养动物通常需要经历两次群体瓶颈时期：*驯化事件和品种形成事件*。驯化事件指的是从野生状态到家养状态的过程，一般发生在一万年左右，例如灰狼驯化成狗的过程。品种形成事件则是指现代品种的形成，一般发生在最近几百年左右，例如各个不同品种狗的形成。驯化会导致一系列表型的显著改变，在家养动物中，这种表型的选择通常包括温顺程度、行为、体型大小与形态、皮肤和毛发颜色以及与繁殖相关性状等方面的变化。
                       ![image](https://user-images.githubusercontent.com/111029483/222344636-6f245dc3-50ea-4222-a117-f535649d8ac8.png)
### 等位基因频谱（SFS）：
基于 SFS 重构群体历史的常用软件有∂a∂i与 Fastsimcoal2
* *∂a∂i*：使用扩散近似法（diffusion approximation）来推断最多 3 个群体的种群大小变化、种群分化及迁移率等历史事件；
* *Fastsimcoal2*：使用复合似然值（composite likelihood）来推断群体演化历程。
* 两者对于少于 4 个群体的历史推测有着相似的运算效率，但当群体数超过 3 个时，只能利用 Fastsimcoal2 进行计算。
    计算过程中，∂a∂i 与 Fastsimcoal2都需要从高质量且独立的位点获得群体之间的频谱信息：即在计算 SFS 之前，必须要去掉变异位点之间的连锁不平衡，且尽可能利用高覆盖度高质量的全基因组，测序数据去生成 SFS。

* 相比 PSMC、MSMC，SFS 方法可以获得更为近期的群体历史动态变化，而且可以利用更大的群体信息得到更为精确的结果，同时不会增加计算负担。SFS 方法需要在计算时提供先验模型，然后从预先已知的先验分布中重复抽样来估计未知群体历史参数的后验分布。基于 SFS 的方法的另一个优点是可以重塑非常复杂的多群体历史动态，而且可以推断群体之间的迁移事件。

* SFS 的方法需要用户提供可靠的先验模型才能得到具有生物学意义的结果。但在很多情况下，研究者并没有这样一个可靠的先验模型，所以大多数用户都会结合群体遗传结构分析的结果选择多个可能的模型分别进行计算，而且每个模型需要进行多次的bootstrap 并通过比较似然值来提高结果的准确性（似然值越大，所得到的参数越贴近实际数据）。不同模型之间，可以通过赤池信息量准则（Akaike Information Criterion, AIC）和贝叶斯信息准则（Bayesian Information Criterion, BIC）综合考虑自由参数与似然值进而选择最佳的模型。

* 针对每一个分离位点，如果我们不知道哪一个碱基是祖系状态，哪一个是后天得到的，那么这个时候 我们可以用minor allele frequency(MAF)来描述，也就是“少数等位基因”频率。MAF的范围在1/n - 0.5之间。它也被称为site frequency spectrum(SFS)。因为不能确定祖系状态，只能在“少数等位基因”，因而也被称为folded spectrum.
* 如果我们通过其他物种的基因组推测出了每一个位点的祖系状态，这个时候就可以用derived allele frequency(DAF)来表示位点变异情况，DAF的范围在1/n 到 (n-1)/n之间，相对应的，也被称为unfolded spectrum

* 在基因组数据分析中，我们在不知道祖系状态的情况下，通常将MAF<0.05的位点舍弃，因为这可能是测序错误或者有害突变，并不一定代表基因的多态性。但是这也有可能包含了DAF>0.95的位点，也就是会丢失掉很多进化上很重要的位点。

### 群体历史模拟中最优模型选择准则：
* 赤池信息量准则：
  1. Akaike information criterion、简称AIC，是衡量统计模型拟合优良性的一种标准，是由日本统计学家赤池弘次创立和发展的。赤池信息量准则建立在熵的概念基础上，可以权衡所估计模型的复杂度和此模型拟合数据的优良性。
  2. 增加自由参数的数目提高了拟合的优良性，AIC鼓励数据拟合的优良性但尽量避免出现过度拟合（Overfitting）的情况。所以优先考虑的模型应是AIC值最小的那一个。赤池信息量准则的方法是寻找可以最好地解释数据但包含最少自由参数的模型

* 贝叶斯信息准则：
  1. 也称为Bayesian Information Criterion（BIC）。贝叶斯决策理论是主观贝叶斯派归纳理论的重要组成部分。是在不完全情报下，对部分未知的状态用主观概率估计，然后用贝叶斯公式对发生概率进行修正，最后再利用期望值和修正概率做出最优决策。
  2. 与AIC相似，训练模型时，增加参数数量，也就是增加模型复杂度，会增大似然函数，但是也会导致过拟合现象，针对该问题，AIC和BIC均引入了与模型参数个数相关的惩罚项，BIC的惩罚项比AIC的大，考虑了样本数量，样本数量过多时，可有效防止模型精度过高造成的模型复杂度过高。

* AIC和BIC的原理是不同的，AIC是从预测角度，选择一个好的模型用来预测，BIC是从拟合角度，选择一个对现有数据拟合最好的模型，从贝叶斯因子的解释来讲，就是边际似然最大的那个模型。

  1. 共性:
    * 构造这些统计量所遵循的统计思想是一致的，就是在考虑拟合残差的同时，依自变量个数施加“惩罚”。
  2. 不同点
    * BIC的惩罚项比AIC大，考虑了样本个数，样本数量多，可以防止模型精度过高造成的模型复杂度过高。
    * AIC和BIC前半部分是一样的，BIC考虑了样本数量，样本数量过多时，可有效防止模型精度过高造成的模型复杂度过高。

* 今天我们所观察到的基因组遗传变异是一系列复杂演化过程的产物，不仅受突变、随机漂变、选择（自然选择、人工选择）、重组等的影响，也与参考基因组的组装质量及遗传变异的质量有关。在诸多的影响因素下，为了能够更精确的完成群体之间的溯祖，我们需要结合多方面的分析结果来确认一个最稳健的进化模型，包括群体遗传分析（系统发育树分析，主成分分析，聚类分析，连锁不平衡分析及群体遗传参数统计等）、不依赖先验模型的分析（PSMC、SMC++和 MSMC）及模拟分析（ms、Ma CS）等。

## 基因组渗透：

* 适应性基因渗透包含两个过程：基因渗透和适应。基因渗透是指遗传成分从一个群体通过有性生殖的方式进入到另一个群体。产生杂交的前提是两个群体之间没有生殖隔离或只有不完全生殖隔离。只有这样才能通过与可育Ｆ１代回交达到遗传成份垂直传递的目的。通过基因渗透得到的区域，如果在该群体内没有受到选择，那么会因为随机遗传漂变的影响，逐渐在基因组中消失。只有当基因渗透得到的区域受到自然或人工选择，它才能在基因组中保留一定的频率。


## 变异检测：
* 一般来说我们使用GATK HaplotypeCaller模块进行变异检测。
  * 由于古 DNA 中存在的 5’端 C 到 T 和 3’端 G到 A 的损伤模式会造成大量假阳性 SNP，因此只保留在现代样本中能够检测到的变异，排除了古代样本中的特有变异。由于古代样本中内源性 DNA 含量通常较低，最终得到的测序深度也很低 ，因此针对古代样本还增加了--minPruning 1 和--minDanglingBranchLength 1 的设置以提高低深度位点变异检测的灵敏度。

* 变异检测还可以使用bcftools和ANGSD等软件。
  * 若进行的是那种top期刊的研究，推荐使用两种变异检测软件进行变异检测以降低检测到的SNP的假阳性。

* *性别鉴定*，在动物中可以由性染色体的测序深度来确定，由于雄性为XY，雌性为XX，因此雌性的性染色体测序深度应为雄性的二倍。分别统计每个样本在常染色体、X 染色体和 Y 染色体上 SNP 位点的平均 reads 覆盖深度，以常染色体的深度为分母，分别计算了  X 和  Y  染色体的相对覆盖深度。预期观察到雄性个体中的 X 染色体相对覆盖深度为 0.5 且 Y 染色体也为 0.5，而雌性个体中 X 染色体相对覆盖率为 1 且Y 染色体为 0。

## 群体遗传结构及渗透分析
### 计算群特异性位点
```
## 1. 提取群体vcf文件
bcftools提取和牛与其他群体vcf文件(大的vcf文件用的是未过滤maf 和geno的)，然后用Maf和geno过滤后的SNP即这个群体获得的SNP数。

## 2. 取特异性位点
再把bim文件进行sort a.txt b.txt b.txt | uniq -u操作得到a群体特异性位点。

sort QC.01.10.JPBC-geno005-maf003.bim QC.01.10.sample115-geno005-maf003.bim QC.01.10.sample115-geno005-maf003.bim | uniq -u > JPBC.only.txt

uniq参数说明：
-d     仅显示重复出现的行列;
-u     仅显示出一次的行列。
```
### 群体遗传结构
#### PCA
纯数学的运算方法，可以将多个相关变量经过线形转换选出较少个数的重要变量。
##### GCTA软件
```
## make germ
/home/software/gcta_1.92.3beta3/gcta64 --bfile QC.common_89_cattle_851_ASIA-geno005-maf003 \
                                       --make-grm --autosome-num 29 \
                                       --out QC.common_89_cattle_851_ASIA-geno005-maf003.gcta
## PCA
/home/software/gcta_1.92.3beta3/gcta64 --grm QC.common_89_cattle_851_ASIA-geno005-maf003.gcta \
                                       --pca 4 \
                                       --out QC.common_89_cattle_851_ASIA-geno005-maf003.gcta.out
```
##### EIGENSOFT软件
```
1. 转换格式：
EIGENSOFT中内置的convertf 文件转化为smartpca的输入文件

convertf -p transfer.conf

需要一个config file，将文件的输入输出写进去。
##config file
genotypename:    myplink_test.ped
snpname:         myplink_test.map # or example.map, either works
indivname:       myplink_test.ped # or example.ped, either works
outputformat:    EIGENSTRAT
genotypeoutname: example.eigenstratgeno
snpoutname:      example.snp
indivoutname:    example.ind
familynames:    NO

产生三个pca所需的输入文件 example.eigenstrat example.snp example.ind

2. smartpca做PCA

smartpca -p runningpca.conf

##config file
genotypename: example.geno
snpname: example.snp
indivname: example.ind
evecoutname: example.pca.evec
evaloutname: example.eval
altnormstyle: NO
numoutevec: 10
numoutlieriter: 5
outliersigmathresh: 6.0
```
#### Admixture
群体结构分析的算法模型为：假设所有群体拥有K个祖先，每个群体的特征由其每个位点的等位基因频率决定，在哈代-温伯格平衡下，使用最大似然估计算法将实际群体中的每个个体以概率的形式，计算每个个体的基因组变异来源，用Q值表示，Q值越大，表明该位点来自这个祖先群体的可能性越大，从而将每个位点归类到不同的祖先成分。
```
# admixture
for K in 2 3 4 5 6 7 8 9 10 11 12 13 14 15; do /home/software/admixture_linux-1.3.0/admixture --cv QC.sample-northeast_Asia-geno005-maf003.bed $K | tee log${K}.out; done
# 提取CV值
CV error最小的为最佳K值
grep -h CV log*.out


Admixture.sh
touch admixture.sh
nohup bash admixture,sh &
## 提取：
bcftools view -S id.txt  common_89_cattle_851_ASIA.vcf  -Ov > sample-select.vcf
## 转格式：
plink --allow-extra-chr --chr-set 29 -vcf sample-select.vcf --make-bed --double-id --out sample-select
## 过滤： 
plink --allow-extra-chr --chr-set 29 -bfile sample-select --geno 0.05 --maf 0.03 --make-bed --out QC.sample-select-geno005-maf003
## admixture
for K in 2 3 4 5 6 7 8 9 10 11 12 13 14 15; do admixture --cv QC.sample-select-geno005-maf003.bed $K | tee log${K}.out; done
## 提取CV值
## CV error最小的为最佳K值
grep -h CV log*.out 
```
#### 系统发育树
##### 1 MEGA
```
# 计算genome
plink  --bfile QC.ld.cattle_204_hebing_Chr1_29_genotype.nchr-geno005-maf003-502502 \
       --allow-extra-chr --chr-set 29  \
       --genome
# 转换为.meg格式的矩阵形式

PLINK_genome_MEGA.pl
第一个open里改成自己的genome文件
第二个open里改成自己的fam文件
第三个open里将>后面的改成输出的文件名.meg
在下面的sample_size里，将数量改成自己使用的数量


#!usr/bin/perl
# define array of input and output files
open (AAA,"plink.genome") || die "can't open AAA";
open (BBB," QC.ld.cattle_204_hebing_Chr1_29_genotype.nchr-geno005-maf003-502502.fam") || die "can't open BBB"; 
open (CCC,"> cattle_204_hebing_Chr1_29.meg");
my @aa=<AAA>;
my @bb=<BBB>;
$sample_size=204; ###  涓綋鏁扮洰
print CCC "#mega\n!Title: $sample_size pigs;\n!Format DataType=Distance DataFormat=UpperRight NTaxa=$sample_size;\n\n"; 
foreach ($num1=0;$num1<=$#bb;$num1++){
	chomp $bb[$num1];
	@arraynum1=split(/\s+/,$bb[$num1]);
	print CCC "#$arraynum1[1]\n";       ##涓綋鐨処D鍚嶇О
	}
print CCC "\n";
@array=();
foreach ($num2=1;$num2<=$#aa;$num2++){
	chomp $aa[$num2];
	@arraynum1=split(/\s+/,$aa[$num2]);
	push(@array,1-$arraynum1[12]);
	}
	
@array2=(0);
$i=$sample_size;
while ($i>0){	
	push(@array2,$array2[$#array2]+$i);
	$i=$i-1;
	}
print "@array2";

for ($i=($sample_size-1); $i>=0; $i=$i-1){
	print CCC " " x ($sample_size-($i+1));
	    for ($j=$array2[$sample_size-$i-1]; $j<=$array2[$sample_size-$i]-1; $j++){
		                                                                          print CCC "$array[$j] ";	
			                                                                     }
	    print  CCC "\n";
	}
close AAA;
close BBB;
close CCC;
```
##### 2 RAxML(进化树):
```
1、转为phy格式：
python /home/sll/software/vcf2phylip.py --input FASN.vcf.recode.vcf

2、建树：
raxmlHPC-PTHREADS-SSE3 -f a -m GTRGAMMA \
                       -p 12345 -x 12345 \
                       -# 100 -s FASN.min4.phy \
                       -n raxml -T 30
-f a此参数用于选择 RAxML 运算的算法。可以设定的值非常之多。 a 表示执行快速 Bootstrap 分析并搜索最佳得分的 ML 树。
-x 12345指定一个 int 数作为随机种子，以启用快速 Bootstrap 算法。
-p 12345指定一个随机数作为 parsimony inferences 的种子。
-# 100指定 bootstrap 的次数。
-m PROTGAMMALGX 指定核苷酸或氨基酸替代模型。PROTGAMMALGX 的解释： "PROT" 表示氨基酸替代模型； GAMMA 表示使用 GAMMA 模型； X 表示使用最大似然法估计碱基频率。
-s ex.phy指定输入文件。phy 格式的多序列比对结果。软件包中包含一个程序来将 fasta 格式转换为 phy 格式。
-n ex输出文件的后缀为 .ex 。
-T 20指定多线程运行的 CPUs 。
```

##### 3 Treemix(群体树):
```
1.计算等位基因频率
# 转换为tped格式，生成sample.tped和sample.tfam文件。
vcftools --vcf QC.sample-select-geno005-maf003.vcf --plink-tped --out sample

# sample.tfam修改第一列数据为breed ID。

# 提取文件sample.pop.cov，格式为：共三列，前两列与修改后的sample.tfam前两列一样，为群体ID和样本ID，第三列和第一列一致，tab分隔。
cat sample.tfam |awk '{print $1"\t"$2"\t"$1}' >sample.pop.cov

# 计算等位基因组的频率，生成plink.frq.strat和plink.nosex文件
plink --threads 12 --tfile sample \
                   --freq --allow-extra-chr \
		   --chr-set 29 \
		   --within sample.pop.cov 
#压缩等位基因频率文件
gzip plink.frq.strat 

# 转换格式【耗时小时计】
#用treemix自带脚本进行格式转换，notes：输入输出都为压缩文件，plink2treemix.py使用python2并需要绝对路径（否则报错）。
python2 /home/sll/miniconda3/bin/plink2treemix.py plink.frq.strat.gz sample.treemix.in.gz 

2. treemix系统发育分析（ML）
群体间的系统发育树，每个分支代表一个群体。

treemix -i sample.treemix.in.gz -o sample.ML.tree -k 1000 -global

结果文件sample.treemix.treeout.gz解压后可用ITOL进行可视化
```
**注意：若使用NJ法构建进化树，则不能过滤LD，而使用ML法构建进化树时则可过滤LD！！！

### 基因流、群体历史、选择信号

#### Treemix推断基因流（加了外群）:
* TreeMix基于全基因组多态性模拟遗传漂变来推断种群之间的关系。首先估计样本数量之间关系的系统树图，然后来比较系统树模拟构建与观察到的群体之间的变异结构。当群体比通过分杈树建模表现出更密切的关系，则说明群体之间在历史上有过杂合过程。
* TreeMix在系统法语中增加边线使之成为一个系统网络，这些边缘的位置和方向都是有信息的；如果一个边缘产生更多的基底系统网络这表明杂交发生事件比较早或者来源于另一个分化的群体。
```
1.计算等位基因频率
# 转换为tped格式，生成sample.tped和sample.tfam文件。
vcftools --vcf QC.sample-select-geno005-maf003.vcf --plink-tped --out sample

# sample.tfam修改第一列数据为breed ID。

# 提取文件sample.pop.cov，格式为：共三列，前两列与修改后的sample.tfam前两列一样，为群体ID和样本ID，第三列和第一列一致，tab分隔。
cat sample.tfam |awk '{print $1"\t"$2"\t"$1}' >sample.pop.cov

# 计算等位基因组的频率，生成plink.frq.strat和plink.nosex文件
plink --threads 12 --tfile sample --freq --allow-extra-chr --chr-set 29 --within sample.pop.cov 
#压缩等位基因频率文件
gzip plink.frq.strat 

#转换格式【耗时小时计】
#用treemix自带脚本进行格式转换，notes：输入输出都为压缩文件，plink2treemix.py使用python2并需要绝对路径（否则报错）。
python2 /home/sll/miniconda3/bin/plink2treemix.py plink.frq.strat.gz sample.treemix.in.gz 
2.treemix推断基因流
# 多次分析以评估最佳m值
比如m取1-10(常用1-5,1-10)，每个m值重复5次(至少两次)
for m in {1..5}
do
	{
	for i in {1..5}
	do
		{
		   treemix -i sample.treemix.in.gz -o sample.${m}.${i} -bootstrap 100 -root wBGU -m ${m} -k 500 -noss
		}
	done
	}
done

-i 指定基因频率输入文件
-o 指定输出文件前缀
-tf      【可选】指定树文件，指定后就使用指定树的拓扑结构(最好把支持率和其他无关信息都删除，只留最简单的Newick格式)，否则treemix会推断拓扑
-root    指定外类群(指定的是居群名称)，多个用逗号分隔；最好指定，否则后面plot_tree画树没找到更换外类群的参数会很麻烦
-m       为the number of migration edges即基因渗入的次数
-k 1000  因为SNP之间不是独立位点，为了避免连锁不平衡，用k参数指定SNP数量有连锁，比如这里指定用1000个SNP组成的blocks评估协方差矩阵
-se      计算迁移权重的标准误差(计算成本高)，如果想省时间可以不用这个参数
-bootstrap 为了判断给定树拓扑的可信度，在blocks运行bootstrap重复
-global  在增加所有种群后做一轮全局重组。
-noss    关闭样本量校正。TreeMix计算协方差会考虑每个种群的样本量，有些情况(如果有种群的样本只有1个)会过度校正，可以关闭。
-g old.vertices.gz old.edges.gz #使用之前生成的树和图结果，用-g指定之前的两个结果文件
-cor_mig known_events and -climb #合并已知的迁移事件

2.2. 最佳迁移边缘个数选择
将生成的llik、cov、modelcov文件放置在同一文件夹，使用R包OptM分析：

library(OptM)
linear = optM("./data") ##文件夹名
plot_optM(linear)

生成图中，当Δm值最小时的migration edges为最佳迁移边缘个数
2.3.基因渗入作图
使用m=最佳迁移边缘个数结果文件做图

source("plotting_funcs.R") #treemix scr文件夹中R脚本
plot_tree("TreeMix") #TreeMix为结果文件前缀

##===== 绘制混合树====
prefix="sample.3"  ## treemix产生的结果文件前缀
library(RColorBrewer)
library(R.utils)
par(mfrow=c(2,3))
for(edge in 1:3){
  plot_tree(cex=0.8,paste0(prefix,".",edge))
  title(paste(edge,"repetition"))
} # 放的是m012345，则0:5
```
#### f3、f4检验以及D检验:
* D 统计量（Patterson’s D  statistic）是目前最广为使用的渗入检测方法之一。D 统计量需要四个群体，这里称之为 P1、P2、P3 和 O。他们的系统发育关系如图 1-15 所示，为(((P1,P2)P3,)O)。其中，P1 和 P2 为姐妹群，O 为外群。对于这四个群体中存在的双等位 SNP，以外群O 中的类型作为祖先型 A（即 ancestral  allele），另一种则为衍生型 B（即 derived allele）。在已知的系统发育关系的情况下，基因组上绝大多数 SNP 在这四个群体中的排布模式应该为 BBAA，即 P1 和 P2 同为衍生型等位基因，而 P3 和 O 同为祖先型的等位基因。BBAA 的模式是符合系统发育关系的，但如果 P2 和 P3 之间发生了基因流，那么则会产生大量 ABBA 模式的位点，即 P2 和 P3 共享了同一种等位基因。反之，如果 P1 和 P3 之间发生了渗入，则会产生大量 BABA 模式的位点。![image](https://user-images.githubusercontent.com/111029483/222377047-991108ca-381b-4e53-909b-f1cb211bcce6.png)
* 但实际上，由于 P1、P2 和 P3 的共同祖先在某些位点上可能是存在多态性的，这种多态性可能会随机分配给这三个群体，从而导致 ABBA 或者 BABA 的情况。这种现象通常被称为不完全谱系分选（incomplete  lineage  sorting，ILS）。因此单独去检测ABBA 或者 BABA 模式的位点数量无法判断它们是由于 ILS 还是渗入导致的。但由于ILS 是不受选择的，所以它产生的 ABBA 和 BABA 模式的位点数量应该是大致相当的。所以我们可以通过比较 ABBA 和 BABA 的数量是否有显著的差异来判断是单纯的 ILS还是发生了渗入。
* ![image](https://user-images.githubusercontent.com/111029483/222377311-d0c5c6f9-5d4b-4cbd-91a1-26dc15a9d9e3.png)













