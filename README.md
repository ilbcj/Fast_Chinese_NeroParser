# Fast-Chinese-NeroParser(快速神经网络分词包)
## 版本号：3.0.9，如果有志同道和的人指点和参与修正运算思维并提高版本的安全和性能，本人采用立即赠予现金形式， 并将协作者名字公布在项目中。
#### 1 首次采用《VPC架构》。
#### 2 支持海量并发运算，后端接口调用运算，纯全虚接口同步运算。
#### 3 经过SONAR 最高级认证（感知最高认证，语义最高认证，语法最高认证，行为最高认证，逻辑最高认证）。
#### 4 基于 《线性语料库》+《森林网络字典索引》+《欧基里德神经网络加权搜索》+《2分法位运算字符集处理》+《决策树树深度正向隐马可夫匹配》+《逆向马可夫词性修正策略》 的《VPC架构》 中文分词包。
#### 5 每秒高达1050万中文简体字准确分词。 因为通过国际SONAR最高认证，牺牲了程序执行时间十分之三的速度效率（自行修改去掉sonar认知模式可达1600万字分词每秒，性能比应该是世界第二，世界第一赠给高斯林先生，因为我用的是java，没办法）。
#### 5.1 每秒高达900万词语的中文词性索引速度。（Part Of Speech, POS），分词和词性分析可拆分使用。采用一次实例，多并发执行思想。
#### 5.2 多达8700+的中文语料库精确简体中文词汇，有效的辨别新词。
#### 6 大小55Kb。
#### 7 如果想用多核模式，可以自己写 parallelStream() 函数去实现，jdk8以上已经支持, CogsBinaryForestAnalyzer 支持海量多核多线程并发安全 。
#### 8 VPC架构采用纯虚函数做反向映射跳过IOC，效率增加，线程安全高度严格保障。

## 使用方法：
#### 1 支持 java JDK 8 以上。
#### 2 大家可以自由添加词汇，添加在 org/tinos/fhmm/imp/words.lyg文件里。
https://github.com/yaoguangluo/Fast_Chinese_NeroParser/blob/master/org/tinos/fhmm/imp/words.lyg
#### 2.1 大家可以自由添加 三连词 词汇的前后缀 动名与介词区分，添加在 org/tinos/fhmm/imp/ogld.lyg文件里。
#### 3 可以看下org/tinos/test里面的例子。
#
## 分词使用如下：
####   //1 实例化
   		//Analyzer analyzer = new CogsBinaryForestAnalyzerImp();  //哈希森林索引 多核多线程安全 支持并发
		Analyzer analyzer = new BinaryForestAnalyzerImp();  //哈希森林索引 单线程
		//Analyzer analyzer = new FastAnalyzerImp();        //快速线性索引 单线程
		//Analyzer analyzer = new PrettyAnalyzerImp();      //线性森林索引 单线程
		//Analyzer analyzer = new BaseAnalyzerImp();        //一元线性索引
		//Analyzer analyzer = new ScoreAnalyzerImp();       //森林打分索引
####   //2初始
    analyzer.init();
####   //3 创建字符串 utf 8
	String ss = "如果从容易开始于是从容不迫天下等于是非常识时务必为俊杰沿海南方向逃跑他说的确实在理结婚的和尚未结婚的提高产品质量中外科学名著内科学是临床医学的基础    内科学作为临床医学的基础学科，重点论述人体各个系统各种疾病的病因、发病机制、临床表现、诊断、治疗与预防";
####   //4 执行
    List<String> sets = analyzer.parserString(ss); 
####   //5 输出
    int j=0;
		for(int i = 0; i < sets.size(); i++){
			System.out.print(sets.get(i)+" | ");
			j++;
			if(j>25) {
				j=0;
				System.out.println("");
			}
		}
### 效果：
如果  |  从  |  容易  |  开始  |  于是  |  从容不迫  |  天下  |  等于  |  是非  |  常识  |  时务  |  必  |  为  |  俊杰  |  沿  |  海南  |  方向  |  逃跑  |  他  |  说的  |  确实  |  在理  |  结婚  |  的  |  和  |  尚未  |  结婚  |  的  |  提高  |  产品  |  质量  |  中外  |  
科学  |  名著  |  内科学  |  是  |  临床  |  医学  |  的  |  基础  |     |  内科学  |  作为  |  临床  |  医学  |  的  |  基础  |  学科  |  
，  |  重点  |  论述  |  人体  |  各个  |  系统  |  各种  |  疾病  |  的  |  病因  |  、  |  发病  |  机制  |  、  |  临床  |  表现  |  
、  |  诊断  |  、  |  治疗  |  与  |  预防  |   
## POS 词性分析如下：
	###   //1 实例化
   		//Analyzer analyzer = new CogsBinaryForestAnalyzerImp();  //哈希森林索引 多核多线程安全 支持并发
		Analyzer analyzer = new BinaryForestAnalyzerImp();  //哈希森林索引 单线程
		//Analyzer analyzer = new FastAnalyzerImp();        //快速线性索引 单线程
		//Analyzer analyzer = new PrettyAnalyzerImp();      //线性森林索引 单线程
		//Analyzer analyzer = new BaseAnalyzerImp();        //一元线性索引
		//Analyzer analyzer = new ScoreAnalyzerImp();       //森林打分索引
####   //2初始
    analyzer.init();
    Map<String, String> pos = analyzer.getWord();
####   //3 创建字符串 utf 8
	String ss = "如果从容易开始于是从容不迫天下等于是非常识时务必为俊杰沿海南方向逃跑他说的确实在理结婚的和尚未结婚的提高产品质量中外科学名著内科学是临床医学的基础    内科学作为临床医学的基础学科，重点论述人体各个系统各种疾病的病因、发病机制、临床表现、诊断、治疗与预防";
####   //4 执行
    List<String> sets = analyzer.parserString(ss); 
####   //5 输出
    int j=0;
		for(int i = 0; i < sets.size(); i++){
			System.out.print(sets.get(i)+"/"+pos.get(sets.get(i)) +"  ");
			j++;
			if(j>8) {
				j=0;
				System.out.println("");
			}
		}
#### 效果：
如果/从属连词  从/介词  容易/形谓词  开始/动词  于是/副词  从容不迫/副词  天下/名词  等于/动词  是非/名词  
常识/名词  时务/名词  必/副词  为/动词  俊杰/实体名  沿/null  海南/地名  方向/名词  逃跑/动词  
他/人称代词  说/动词 的  的确/副词  实在/副词  理/形谓词  结婚/动词  的/结构助词  和/连词  尚未/副词  
结婚/动词  的/结构助词  提高/动词  产品/名词  质量/名词  中外/名词  科学/名词  名著/名词  内科学/名词  
是/动词  临床/名词  医学/名词  的/结构助词  基础/名词  内科学/名词  作为/动词  临床/名词  医学/名词  
的/结构助词  基础/名词  学科/名词  ，/标点  重点/名词  论述/名词  人体/名词  各个/限定词  系统/名词  
各种/名词  疾病/名词  的/结构助词  病因/名词  、/标点  发病/动词  机制/名词  、/标点  临床/名词  
表现/名词  、/标点  诊断/名词  、/标点  治疗/动词  与/连词  预防/动词   
## 感谢声明
#### 1 感谢中国复旦大学的FNLP人工智能团队。 本人在设计数据字典扩充的时候 应用其新词识别函数 帮我节省了大量词语录入需花费的时间。
应用方法：本人用FNLP函数将文章中的词语将我分出词进行词性标注，得到的标注如果在我的词库里面没有出现，于是扩充在我的词库。特此声明。

## 代码协作贡献者
尚无

## 第三方开源包的引用和修改
尚无

## 有疑问联系313699483@qq.com 罗瑶光
#### 谢谢！
#### 2018/10/01
