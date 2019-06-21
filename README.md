# Android-Developer-Study
《第一行代码》学习
#使用SQL语言建立一个“男”会员的视图VSH，该视图包括信息：会员编号，会员名，性别，联系电话。
#    会员(会员编号，会员名，性别，所在区，联系电话)
CREATE VIEW VSH AS SELECT
	会员编号,
	会员名,
	性别,
	联系电话
FROM
	会员
WHERE
	性别 = '男';

CREATE TABLE 会员(
会员编号 INT(64)  NOT NULL ,
会员名 VARCHAR(64) DEFAULT NULL,
性别 VARCHAR(10) DEFAULT 'M',
所在区 VARCHAR(32) DEFAULT NULL,
联系电话 VARCHAR(32) DEFAULT NULL
);

DESCRIBE testdata
#201710

--     某火车站订票系统数据库表如下：
--     车次(车号，出发地，目的地，发车日期，开出时刻，剩余座位数，票价)
--     用户(身份证号，姓名，性别，电话)
--     订票(订单号，身份证号，车号，订购日期)
--     实现下列操作：
-- 36．使用关系代数查询“2017-01-01”从“沈阳站”出发终到“大连站”的剩余座位数。
       SELECT 剩余座数 FROM 车次 WHERE 发车日期='2017-01-01' AND 出发地='沈阳站' AND 目的地='大连站'
-- 37．使用SQL语言查询订票次数超过20次的身份证号及订票次数。
       SELECT 身份证号,COUNT(订单号) AS 订票次数 FROM 订票 WHERE  COUNT(订单号)>20 GROUP BY 身份证号
-- 38．使用SQL语言查询“杨鸣”订票信息，并按订购日期降序排序。(用嵌套查询做)
       SELECT * FROM 订票 WHERE 身份证号 IN (SELECT 身份证号 FROM 用户 WHERE 姓名='杨鸣') ORDER BY 订购日期 DESC 
-- 39．使用SQL语言将“T2567”车次的票价提高5元。
        UPDATE 车次 SET 票价=票价+5 WHERE 车号='12'
-- 40．使用SQL语言创建视图V_CYD，视图信息包括：车号、出发地、目的地、姓名、订购日期。
CREATE VIEW V_CYD (
	车号,
	出发地,
	目的地,
	姓名,
	订购日期
)  AS SELECT
	车次.车号,
	出发地,
	目的地,
	姓名,
	订购日期 FROM 车次,用户,订票 WHERE 车次.车号=订票.车号 AND 用户.身份证号=订票.身份证号

#201704
-- 某学生社团管理系统的数据库包含括如下关系表：
--     学生(学号，姓名，年龄，性别，所在系)
--     协会(协会编号，协会名，办公地点，负责人)
--     入会(学号，协会编号，入会日期)



SELECT
	姓名,
	所在系
FROM
	学生
WHERE
	学号 IN (
		SELECT
			学号
		FROM
			入会
		WHERE
			协会编号 IN (
				SELECT
					协会编号
				FROM
					协会
				WHERE
					协会名 = '科技协会'
			)
	) 
CREATE TABLE 学生 (

学号 VARCHAR(32) NOT NULL,
姓名 VARCHAR(32) NOT NULL,
年龄 INT(12) NOT NULL,
性别 VARCHAR(32) NOT NULL,
所在系 VARCHAR(32) NOT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

CREATE TABLE 协会(
协会编号 VARCHAR(32) NOT NULL,
协会名 VARCHAR(64) NOT NULL,
办公地点 VARCHAR(64) NOT NULL,
负责人 VARCHAR(64) NOT NULL
)ENGINE=InnoDB DEFAULT CHARSET=utf8;
#修改表名
ALTER TABLE 协会 RENAME TO 入会

CREATE TABLE 入会(
学号 VARCHAR(32) NOT NULL,
协会编号 VARCHAR(32) NOT NULL,
入会日期 VARCHAR(64) NOT NULL

)ENGINE=InnoDB DEFAULT CHARSET=utf8;

--     实现下列操作：
-- 36．使用关系代数语言查询加入“科技协会’’的学生姓名和所在系。
SELECT 姓名,所在系 FROM 学生 WHERE 学号 IN (SELECT 学号 FROM 入会 WHERE 协会编号 IN (SELECT 协会编号 FROM 协会 WHERE 协会名 = '科技协会')) 
--     学生(学号，姓名，年龄，性别，所在系)
-- --  协会(协会编号，协会名，办公地点，负责人)
--     入会(学号，协会编号，入会日期)
-- 37．使用SQL语句查询每个协会的协会编号和学生数，并按人数降序排列。
SELECT 协会.协会编号, COUNT(学号) AS 学生数 FROM 协会,入会 WHERE 协会.协会编号 IN  (SELECT 协会编号 FROM 入会) GROUP BY 协会.协会编号
答案:SELECT 协会编号, COUNT(学号) AS 学生数 FROM 入会  GROUP BY 协会编号 ORDER BY 学生数 DESC
-- SELECT 协会.协会编号,COUNT(入会.学号) AS 学生数 FROM 协会,入会 WHERE 协会编号 IN (SELECT 协会编号 FROM 入会)  ORDER BY 学生数 DESC
-- -- 
-- 
-- 
-- 38.使用SQL语句查询没有加入协会编号为XH4的学生的学号、姓名、所在系。
-- 
-- 
SELECT 学号,姓名,所在系 FROM 学生 WHERE 学号 NOT IN(SELECT 学号 FROM 入会 WHERE 协会编号='XH4')
-- 
-- 39．使用SQL语句将“篮球协会”办公地点改为‘‘综合楼lll”。
-- 
-- 
UPDATE 协会 SET 办公地点='综合楼lll' WHERE 协会名='篮球协会'
-- 
-- 40．使用SQL语句创建视图V～SA，视图包括学号、姓名、协会名、入会日期。
CREATE VIEW V～SA(学号,姓名,协会名,入会日期) AS SELECT 学生.学号,姓名,协会名,入会日期 FROM 学生,协会,入会  WHERE 协会.协会编号=入会.协会编号 AND 学生.学号=入会.学号

CREATE VIEW V～SA1(学号,姓名,协会名,入会日期) AS SELECT 学生.学号,姓名,协会名,入会日期 FROM 学生,协会,入会 

#删除视图
DROP VIEW  IF EXISTS V～SA1

DESCRIBE V～SA

CREATE TABLE Drug(
dID VARCHAR(32) NOT NULL,
dName VARCHAR(64) NOT NULL,
dPlace VARCHAR(64) NOT NULL,
dSpec VARCHAR(64) NOT NULL
)ENGINE=InnoDB DEFAULT CHARSET=utf8;

CREATE TABLE Purchase(
pDate VARCHAR(32) NOT NULL,
dID VARCHAR(64) NOT NULL,
pAmmo VARCHAR(64) NOT NULL,
pPrice VARCHAR(64) NOT NULL,
pProvider VARCHAR(64) NOT NULL
)ENGINE=InnoDB DEFAULT CHARSET=utf8;

CREATE TABLE Retail(
rDate VARCHAR(32) NOT NULL,
dID VARCHAR(64) NOT NULL,
rAmmo VARCHAR(64) NOT NULL,
rPrice VARCHAR(64) NOT NULL,
payStyle VARCHAR(64) NOT NULL
)ENGINE=InnoDB DEFAULT CHARSET=utf8;
#1504
-- 某药店管理系统的数据库包含如下关系表：
-- Drug(dID，dName，dPlace，dSpec)，药品目录表，分别表示(药品编号，药品名称，药品产地，规格)
-- Purchase(pDate，dID，pAmmo，pPrice，pProvider)，药品采购表，分别表示(采购日期，药品编号，采购数量，采购单价，供货商)
-- Retail(rDate，dID，rAmmo，rPrice，payStyle)，药品零售表，分别表示(销售日期，药品编号，销售数量，销售单价，付款方式)
-- 实现下列操作：
-- 36．用关系代数表达式查询单次采购数量大于1000的药品编号、药品名称和供货商名称。
SELECT Drug.dID,dName,pProvider FROM Drug,Purchase WHERE Drug.dID IN (SELECT Purchase.dID FROM Drug) AND pAmmo>1000
-- 37. 使用SQL语句修改药品编号为“l012”的药品名称为“头孢氨苄胶囊”，产地为“青岛”。
UPDATE Drug SET dPlace="青岛",dName="头孢氨苄胶囊" WHERE dID="1012" 
-- 38．使用SQL语句查询所有药品累计采购的情况，包括每种药品的药品编号、药品名称、总
--     数量和总金额。如果药品没有被采购过，数量和金额置为空值。
SELECT
	Drug.dID,
	dName,
	(
		SELECT
			SUM(pAmmo)
		FROM
			Purchase
		WHERE
			drug.dID = Purchase.dID
	) AS totalNum,
	(
		SELECT
			SUM(pPrice*pAmmo)
		FROM
			Purchase
		WHERE
			drug.dID = Purchase.dID
	) AS totalPrice
FROM
	Drug,
	Purchase
GROUP BY
	dName ORDER BY drug.dID



答案 : 
SELECT
	Drug.dID,
	dName,
	SUM(pAmmo),
	SUM(pAmmo * pPrice)
FROM
	Drug
LEFT OUTER JOIN Purchase ON Drug.dID = Purchase.dID
GROUP BY
	Drug.dID
-- 39. 使用SQL语句查询单笔销售金额低于l0元的药品编号、药品名称、销售金额(列名称改
--     为total)。


SELECT
	drug.dID,
	dName,
	(retail.rAmmo) * (retail.rPrice) AS total
FROM
	Drug,
	Retail
WHERE
	drug.dID IN (
		SELECT
			retail.dID
		FROM
			retail
		WHERE
			(retail.rAmmo) * (retail.rPrice) < 10
	) AND drug.dID=retail.dID

GROUP BY
	dName 

答案:
SELECT
	drug.dID,
	dName,
	(retail.rAmmo) * (retail.rPrice) AS total
FROM
	Drug,
	Retail
WHERE
	rAmmo * rPrice < 10
AND drug.dID = retail.dID
-- 40．使用SQL语句建立视图VWl：2015年3月1 13后药品的销售记录，包括药品编号、药品名称、销售日期、销售数量、销售单价、销售金额、付款方式。

DROP VIEW IF EXISTS VW1
CREATE VIEW VW1 (
	dID,
	dName,
	rDate,
	rAmmo,
	rPrice,
	total,
	payStyle
) AS SELECT
	drug.dID,
	dName,
	rDate,
	rAmmo,
	rPrice,
	(retail.rAmmo) * (retail.rPrice) AS total,
	payStyle
FROM
	drug,
	Retail
WHERE
	drug.dID = Retail.dID AND Retail.rDate>"2015年3月13"
#1410
-- 某宾馆有多间客房(Room)，顾客(Customer)可以入住到宾馆。其关系模式如下：
-- Room (rN0，rType，rPriee)，分别表示(房间编号，房间类型，价格)；
CREATE TABLE Room(
rN0 VARCHAR(32) NOT NULL,
rType VARCHAR(64) NOT NULL,
rPriee VARCHAR(64) NOT NULL
)ENGINE=InnoDB DEFAULT CHARSET=utf8;

-- Customer(eID，cName，cage，cPhone)，分别表示(顾客编号，顾客姓名，年龄，电话)；
CREATE TABLE Customer(
eID VARCHAR(32) NOT NULL,
cName VARCHAR(64) NOT NULL,
cage VARCHAR(64) NOT NULL,
cPhone VARCHAR(64) NOT NULL
)ENGINE=InnoDB DEFAULT CHARSET=utf8;
-- RC(rN0，eID，rcInDate，rcDays)，分别表示(房问号，顾客编号，入住日期，入住天数)。
CREATE TABLE RC(
rN0 VARCHAR(32) NOT NULL,
eID VARCHAR(64) NOT NULL,
rcInDate VARCHAR(64) NOT NULL,
rcDays VARCHAR(64) NOT NULL
)ENGINE=InnoDB DEFAULT CHARSET=utf8;
-- 实现下列操作：
-- 36．用关系代数表达式查询每次入住天数大于l0天的客户姓名和电话。
SELECT cName,cPhone FROM  Customer WHERE Customer.eID=RC.eID AND  RC.rcDays>10
-- 37．向Customer表插入一条新记录：客户编号为21011319，姓名为李四平，电话号码为 02478699876。
INSERT INTO Customer (eID,cName,cPhone) VALUES ("21011319","李四平","02478699876")
-- 38．使用SQL语句查询累计入住天数超过365天的客户编号、累计入住天数。
SELECT eID,rcDays FROM RC WHERE rcDays>365

答案:
#在 SQL 中增加 HAVING 子句原因是，WHERE 关键字无法与合计函数一起使用。

SELECT eID,SUM(rcDays) as sumdays FROM RC GROUP BY eID HAVING SUM(rcDays)>365
-- 39.使用SQL语言查询没有被使用过的房间编号。
SELECT Room.rN0 FROM  Room WHERE Room.rN0 NOT IN (SELECT RC.rN0 FROM RC WHERE Room.rN0=Rc.rN0)

答案:如果不是查多个表不会存在表字段冲突的问题
SELECT rN0 FROM Room WHERE rN0 NOT IN (SELECT DISTINCT rN0 FROM RC)
-- 40．使用SQL语言建立视图VWl：2014年10月1日后入住的所有客户的姓名、房间类别、
--   入住天数。     


CREATE VIEW VWl (cName, rType, rcDays) AS SELECT
	cName,
	rType,
	rcDays
FROM
	Customer,
	Room,
	RC
WHERE
	Room.rN0 = RC.rN0
AND Customer.eID = RC.eID
AND RC.rcInDate > "2014年10月1日"


#1404
-- 某工程项目管理系统的数据库包含如下关系表：
-- S(SNO，SNAME，SEX，DEPT，SCHOLARSHIP)；S为学生表，分别表示(学号，姓名，性别，专业，奖学金)
-- C(CNO，CNAME，CREDIT)；C为课程表，分别表示(课程号，课程名，学分)
-- SC(SNO，CNO，SCORE)；SC为选课表，分别表示(学号，课程号，分数)
-- 实现下列操作：
-- 36.用关系代数表达式查询选修了课号为C3或C4课程的学生学号。
SELECT SNO FROM SC WHERE CNO="C3" OR CNO="C4"
3分-- 37.使用SQL语句查询获得奖学金的所有学生所学课程的信息，包括学号、姓名、课程名和分数。
SELECT
	S.SNO,
	SNAME,
	C.CNO,
	SCORE
FROM
	S,
	C,
	SC
WHERE
	SCHOLARSHIP>0
AND S.SNO = SC.SNO
AND C.CNO = SC.CNO

答案:
SELECT
	S.SNO,
	SNAME,
	CNAME,
	SCORE
FROM
	S,
	C,
	SC
WHERE
	SCHOLARSHIP IS NOT NULL
AND S.SNO = SC.SNO
AND C.CNO = SC.CNO

***-- 38.使用SQL语句查询没有任何一门课程成绩超过90分的所有学生的信息，包括学号、姓名和专业。
SELECT COUNT(SCORE)  FROM SC WHERE SCORE<90 GROUP BY SNO  HAVING COUNT(SCORE>90)=0

答案:SELECT S.SNO,SNAME,DEPT FROM S WHERE SNO  NOT IN (SELECT SNO FROM SC WHERE SCORE>90) 

4分-- 39.使用SQL语言对成绩有过不及格的学生，如果已经获得奖学金的，将奖学金减半。
UPDATE S SET SCHOLARSHIP=SCHOLARSHIP/2 WHERE SNO IN (SELECT SNO FROM SC WHERE SCORE<60) AND SCHOLARSHIP IS NOT NULL
-- 40.使用SQL语言建立视图V—SC，视图包括学号、姓名、课程号、课程名、分数。
DROP VIEW IF  EXISTS V_SC
4分
CREATE VIEW V_SC(SNO,SNAME,CNO,CNAME,SCORE) AS SELECT S.SNO,SNAME,C.CNO,CNAME,SCORE FROM S,C,SC WHERE S.SNO=SC.SNO AND C.CNO=SC.CNO

-- 1310
-- 某设备管理系统的数据库包含如下关系表：
-- 设备（设备编号，设备名称，产地，购入日期，价值）
-- 人员（员工号，姓名，性别，出生日期，职位）
-- 设备使用（设备编号，员工号，借出日期，使用时间，收费金额）
-- 实现下列操作：
-- 36．使用关系代数查询所有设备价值大于6000元的设备的设备编号、员工号和借出日期。
-- 37．使用SQL语句查询王琦使用设备的信息。信息包括：姓名、设备名称、借出日期。---4'
SELECT
	姓名,
	设备名称,
	借出日期
FROM
	人员,
	设备,
	设备使用
WHERE
	姓名 = "王琦"
AND 设备.设备编号 = 设备使用.设备编号
AND 人员.员工号 = 设备使用.员工号 
-- 38．使用SQL语句查询每种设备使用的人数，输出列名为设备编号和使用人数（员工号不能重复计算）。2'
SELECT
	设备编号,
	SUM(DISTINCT 员工号) AS 使用人数
FROM
	设备使用
GROUP BY
	设备编号

答案:
SELECT
	设备编号,
	COUNT(DISTINCT 员工号) AS 使用人数
FROM
	设备使用
GROUP BY
	设备编号
-- 39．使用SQL语句将设备编号为130001的记录的收费金额减少10%。4'
UPDATE 设备使用 SET 收费金额=0.9*收费金额 WHERE 设备编号="130001"
-- 40．使用SQL语言创建视图V_SRS，视图按设备购人日期进行降序排列，包括设备编号、设备名称、购人日期。4'
CREATE VIEW V_SRS (
	设备编号,
	设备名称,
	购入日期
) AS SELECT
	设备编号,
	设备名称,
	购入日期
FROM
	设备
ORDER BY
	购入日期 DESC
#1301
-- 设学生管理数据库有3个关系：
-- 学生（学号，姓名，性别，年龄，系名）
-- 课程（课号，课名，学时）
-- 选课（学号，课号，成绩，考试时间）
-- 用SQL语言完成下面36-40题。
-- 36．查询不是信息系、数学系、物理系的学生姓名和性别（提示：使用NOT IN）。
SELECT 姓名,性别 FROM 学生 WHERE 系名 NOT IN(信息系,数学系,物理系)
-- 37．查询考试成绩有不及格（小于60分）的学生的学号（要求结果无重复）。
SELECT DISTINCT 学号 FROM 选课 WHERE 成绩<60
-- 38．查询各门课程的课号及其选课人数。
SELECT 课号,COUNT(DISTINCT 学号) FROM 选课
答案:SELECT 课号,COUNT(DISTINCT 学号) FROM 选课 GROUP BY 课号

-- 39．把学生“刘晨”所选修的课程的成绩加10分。
UPDATE 选课 SET 成绩=成绩+10 WHERE 学号 IN (SELECT 学号 FROM 学生 WHERE 姓名="刘晨")
-- 40．创建学生成绩表视图VW1，包括学号，姓名，课名，成绩，考试时间。
CREATE VIEW VW2(学号,姓名,课号,成绩,考试时间) AS SELECT 学号,姓名,课程.课号,成绩,考试时间 FROM 学生,课程,选课 WHERE 学生.学号=选课.学号 AND 课程.课号=选课.课号


-- --1210
-- -- 某农场有多名饲养员(Worker)，每名饲养员可以饲养多只动物(Animal)，每只动物都有一个动物编号，每只动物只由一名饲养员饲养，其关系模式如下：
-- Worker（wID,wName，wSex，wAge，wPhone），分别表示（编号，姓名，性别，年龄，电话）
-- Animal(aID，wID，aType，aAge)，分别表示（动物编号，饲养员编号，种类，年龄）
-- 实现下列操作：
-- 36．用关系代数语言查询没有饲养过牛的饲养员的姓名和年龄。
-- 37．根据题36给出的关系模式，实现下列操作：
-- 写出创建饲养员表的SQL语句，其中wID定义为主码。4'
CREATE TABLE Worker(
wID INT(10) PRIMARY KEY,
wName VARCHAR(32) NOT NULL,
wSex VARCHAR(32) NOT NULL, 
wAge INT(32) NOT NULL,
wPhone VARCHAR(32) NOT NULL
)
-- 38．根据题36给出的关系模式，实现下列操作：
-- 用SQL语言查询由姓吴的饲养员饲养的所有动物的个数。1'
SELECT COUNT(wID) FROM Worker WHERE wName like "吴%"
答案:SELECT COUNT(wID) FROM Worker,Animal WHERE wName like "吴%" AND Woker.wID=Animal.WId

-- 39．根据题36给出的关系模式，实现下列操作：
-- 用SQL语言查询饲养过牛或者年龄大于40岁的饲养员的编号。4'
SELECT wID FROM Worker WHERE wID IN (SELECT wID FROM Animal WHERE aType="牛") OR wAge>40
答案:(SELECT wID FROM Worker WHERE wAge>40) UNION (SELECT wID FROM Animal WHERE aType="牛")
-- 40．根据题36给出的关系模式，实现下列操作：
-- 用SQL语言创建视图VW：没有饲养过牛的饲养员的姓名和年龄。
CREATE VIEW VW AS SELECT wName,wAge FROM Worker WHERE  wID NOT IN (SELECT wID FROM Animal WHERE aType="牛") 
答案:CREATE VIEW VW AS SELECT wName,wAge FROM Worker WHERE  wID NOT IN (SELECT wID FROM Animal WHERE aType="牛" AND Woker.wID=Animal.WId ) 


#1201
-- 设有如下3个关系模式：
-- 职工(职工号，姓名，性别，年龄)
-- 工程(工程号，工程名称，预算)
-- 报酬(职工号，工程号，工资)
-- 用SQL语句完成下面36—40题。
-- 36.查询年龄不在19至55岁之间的职工姓名和性别。4’
SELECT 姓名,性别 FROM 职工 WHERE 年龄 NOT BETWEEN 19 AND 55
SELECT 姓名,性别 FROM 职工 WHERE 年龄<19 AND 年龄>55
-- 37.按照职工号统计每名职工的总收入。4'
 SELECT SUM(工资) FROM 报酬 WHERE 职工.职工号=报酬.职工号 GROUP BY 职工号
答案: SELECT SUM(工资) FROM 报酬 GROUP BY 职工号

-- 38.将预算额达到10万元及以上工程的职工工资提高10％。4'
UPDATE 报酬 SET 工资=1.1*工资 WHERE 工程号 IN (SELECT 工程号 FROM 工程 WHERE 预算>="10000")
-- 39.创建一个关于职工参加工程项目的视图VPS，该视图包括职工号，姓名，工程名称和工资。4'
CREATE VIEW VPS AS SELECT 职工.职工号,姓名,工程名称,工资 FROM 职工,工程,报酬 WHERE 职工.职工号=报酬.职工号 AND 工程.工程号=报酬.工程号
-- 40.查询参加过两个以上工程项目的职工号及项目数，并按项目数降序排列。
SELECT
	职工.职工号,
	COUNT(工程.工程号)
FROM
	职工,
	工程,
	报酬
WHERE
	职工.职工号 = 报酬.职工号
AND 工程.工程号 = 报酬.工程号
GROUP BY
	职工.职工号
HAVING
	COUNT(工程.工程号) > 2

答案: 
SELECT 职工号,COUNT(*) FROM 报酬 GROUP BY 职工号 HAVING COUNT(*)>=2 ORDER BY 2 DESC

#201110
-- 设一个图书借阅管理数据库中包括三个关系模式：
-- 图书(图书编号，书名，作者，出版社，单价)
-- 读者(借书证号，姓名，性别，单位，地址)
-- 借阅(借书证号，图书编号，借阅日期，归还日期，备注)
-- 用SQL语句完成下面36-39题。
-- 36．查询价格在50到60元之间的图书，结果按出版社及单价升序排列。
SELECT * FROM 图书 WHERE 单价 BETWEEN 50 AND 60  ORDER BY 出版社,单价 ASC
-- 38．查询各个出版社图书的最高价格、最低价格和平均价格。
SELECT MAX(单价),MIN(单价),AVG(单价) FROM 图书
答案:SELECT 出版社,MAX(单价),MIN(单价),AVG(单价) FROM 图书 GROUP BY 出版社

-- 39．建立“红星汽车厂”读者的视图RST。
#CREATE VIEW (借书证号,姓名,性别,单位,地址) 红星汽车厂 AS SELECT 借书证号,姓名,性别,单位,地址 FROM 读者,图书,借阅 WHERE 图书.图书编号=借阅.图书编号 AND 读者.借书证号=借阅.借书证号 
CREATE VIEW (借书证号,姓名,性别,单位,地址) 红星汽车厂 AS SELECT 借书证号,姓名,性别,单位,地址 FROM 读者
答案:CREATE VIEW RST(借书证号,姓名,性别,单位,地址)  AS SELECT 借书证号,姓名,性别,单位,地址 FROM 读者 WHERE 单位="红星汽车厂"

-- 40．依据36题的关系模式，用关系代数表达式检索借阅“高等数学”的读者姓名。



#201101
-- 36.设某数据库有三个关系：
-- 音像（音像编号，音像名，租金，类别）
-- 会员（会员编号，会员名，年龄，所在地区，联系电话）
-- 租借（音像编号，会员编号，租借日期，归还日期）
-- 试用SQL语言查询李扬租借过的音像制品的名称和类别。4'
SELECT 音像名,类别 FROM 音像 WHERE 音像编号 IN (SELECT 音像编号 FROM 租借 WHERE 会员编号 IN (SELECT 会员编号 FROM 会员 WHERE 会员名="李扬")) 
答案一:SELECT 音像名,类别 FROM 音像,会员,租借 WHERE 音像.音像编号=租借.音像编号 AND 会员.会员编号=租借.会员编号 AND 会员名="李扬"
-- 37.依据36题的关系模式，试用SQL语句查询2010年5月以前租借音像制品的会员编号。（注：租借日期为字符型，格式为'2010/01/01'）4'
SELECT 会员编号 FROM 租借 WHERE 租借日期<"2010/05/01"
-- 38.依据36题的关系模式，试用SQL语句建立一个有关科幻类音像制品的视图LM。2'
CREATE VIEW LM AS SELECT  音像.音像编号,音像名,租金,类别 FROM 音像,会员,租借 WHERE 类别="科幻" AND 音像.音像编号=租借.音像编号 AND 会员.会员编号=租借.会员编号
答案:CREATE VIEW(音像编号,音像名,租金,类别) LM AS SELECT  音像编号,音像名,租金,类别 FROM 音像 WHERE 类别="科幻" 

-- 39.依据36题的关系模式，试用SQL语句查询每一类音像制品的类别和被租借的次数。3’
SELECT 类别,COUNT(*) FROM 音像, 租借 WHERE 音像.音像编号=租借.音像编号 GROUP BY 音像.音像编号
注意审题！！每一类！！
答案:SELECT 类别,COUNT(*) FROM 音像, 租借 WHERE 音像.音像编号=租借.音像编号 GROUP BY 类别

-- 40.依据36题的关系模式，试用关系代数查询北京地区的会员名和联系电话。

#1010
-- 设学生社团管理数据库有三个关系：
-- S(Sno，Sname，Age，Sex，Dept)
-- A(Ano，Aname，Location，Manager)
-- SA(Sno，Ano，Date)
-- 其中表S的属性分别表示学号、姓名、年龄、性别和所在系；
-- 表A的属性分别表示会员编号、协会名、协会的办公地点和负责人(负责人为学号)；
-- 表SA描述了学生参加社团的情况，其属性分别表示学号、协会编号、加入协会时间。
--  36．试用SQL语言查询参加“篮球”协会的学生姓名和所在系。4'
SELECT Sname,Dept FROM S WHERE Sno IN (SELECT Sno FROM SA WHERE Ano IN(SELECT Ano FROM A WHERE Aname="篮球") )
SELECT Sname,Dept FROM S,A,SA WHERE S.Sno=SA.Sno AND A.Ano=SA.Ano and  Aname="篮球"
-- 37．依据36题的关系模式，建立一个包含Sno、Sname、Aname和Date的视图ST。4'
CREATE VIEW STD(Sno,Sname,Aname,Date) AS SELECT Sno,Sname,Aname,Date FROM S,A,SA WHERE S.Sno=SA.Sno AND A.Ano=SA.Ano 
-- 38．依据36题的关系模式，试用SQL语言查询每个协会的协会编号和学生数，并按人数降序排列。3'
SELECT Ano,COUNT(Sno) FROM SA ORDER BY 2 DESC
答案:注意审题！！SELECT Ano,COUNT(Sno) FROM SA GROUP BY Ano ORDER BY 2 DESC

-- 39．依据36题的关系模式，试用SQL语言查询没有参加任何协会的学生姓名和所在系。4'
SELECT Sname,Dept FROM S WHERE Sno NOT IN (SELECT Sno FROM SA WHERE Ano IN(SELECT Ano FROM A) )

答案:SELECT Sname,Dept FROM S WHERE Sno NOT IN (SELECT Sno FROM SA )


-- 40．依据36题的关系模式，试用关系代数查询计算机系的学生姓名和年龄。

#1001
-- 36．设有选课关系SC（学号，课号，成绩），试用SQL语句定义一个有关学生学号及其平均成绩的视图SV。3'
CREATE VIEW SV(学号,平均成绩) AS SELECT 学号,AVG(成绩) FROM SC
答案:CREATE VIEW SV(学号,平均成绩) AS SELECT 学号,AVG(成绩) FROM SC GROUP BY 学号

-- 37．设有两个关系：学生关系S（学号，姓名，年龄，性别）和选课关系SC（学号，课号，成绩），试用关系代数表达式检索没有选修B5课程的学生姓名。

-- 38.设有选课关系SC(学号，课号，成绩)，试用SQL语句检索选修B2或B5课程的学生学号。
SELECT 学号 FROM SC WHERE 课号="B2" OR 课号="B5"
-- 39.设有学生关系S(学号，姓名，性别，奖学金)，选课关系SC(学号，课号，成绩)，用SQL语句完成如下操作：对成绩得过满分(100)的学生，如果没有得过奖学金(NULL值)，将其奖学金设为1000元。3‘
UPDATE S SET 奖学金="1000" WHERE 学号 IN (SELECT 学号 FROM SC WHERE 成绩="100") AND 奖学金 IS NULL
答案:注意审题！！！UPDATE S SET 奖学金="1000" WHERE 学号 IN (SELECT 学号 FROM SC WHERE 成绩="100") 

-- 40.设有学生关系S(学号，姓名，性别，年龄)，课程关系C(课号，课名)，选课关系SC(学号，课号，成绩)，试用SQL语句检索选修课程名为BC的学生姓名和成绩。2'
注意审题！！！！
SELECT 姓名,成绩 FROM S,SC,C WHERE  S.学号 IN (SELECT SC.学号 FROM SC WHERE SC.课号 IN (SELECT C.课号 FROM C AND C.课号="BC"))


SELECT 姓名,成绩 FROM S,SC,C WHERE S.学号=SC.学号 AND SC.课号=C.课号   AND C.课号="BC" 

#0910
-- 设有两个关系模式：职工(职工号，姓名，性别，年龄，职务，工资，部门号)
-- 部门(部门号，部门名称，经理名，地址，电话)
-- 依据上述关系回答下面36～40题。
-- 用关系代数表达式写出下列查询：
-- 36.检索“采购部”女职工的职工号和姓名。
SELECT 职工号,姓名 FROM 职工 WHERE 部门号 IN (SELECT 部门号 FROM 部门 WHERE 部门名称="采购部");
-- 37.试用SQL语句删除年龄大于70岁的职工信息。4'
DELETE FROM 职工 WHERE 年龄>70
-- 38.试用SQL语句统计每个部门的人数。
SELECT SUM(*) FROM 职工 GROUP BY 部门号
这个不应该啊！！
答案:SELECT COUNT(*) FROM 职工 GROUP BY 部门号

-- 39.试用SQL语句检索人事部所有姓刘的职工姓名和年龄。4’
SELECT 姓名,年龄 FROM 职工 WHERE 部门号 IN (SELECT 部门号 FROM 部门 WHERE 部门名称="人事部") AND 姓名 LIKE "刘%";
-- 40.试用SQL语句定义一个包含姓名、性别、工资、职务和部门名称的视图ZBB。4'
CREATE VIEW ZBB(姓名,性别,工资,职务,部门名称) AS SELECT 姓名,性别,工资,职务,部门名称 FROM 职工,部门 WHERE 职工.部门号=部门.部门号

#0901
-- 设有关系S(S#，NAME，AGE，SEX)，其属性分别表示：学号，姓名，年龄和性别；
-- 关系SC(S#，C#，GRADE)，其属性分别表示：学号，课号和成绩。
-- 36．试用SQL语句完成统计每一年龄选修课程的学生人数。4'
 SELECT COUNT(S#) FROM S WHERE S# IN (SELECT S# FROM SC) GROUP BY AGE
参考答案: SELECT AGE,COUNT(DISTINCT S.S#) FROM S,SC WHERE S.S#=SC.S#  GROUP BY AGE

-- 37．设有学生表S(S#，NAME，AGE，SEX)，其属性分别表示：学号，姓名，年龄和性别；
-- 选课表SC(S#，C#，GRADE)，其属性分别表示：学号，课号和成绩。
-- 试用关系代数表达式表达下面查询：检索学习课号为C2课程的学号和姓名。

-- 38．设有职工基本表EMP(ENO，ENAME，AGE，SEX，SALARY)，其属性分别表示：职工号，姓名，年龄，性别，工资。试用SQL语句写出为每个工资低于1000元的女职工加薪200元。4'
UPDATE EMP SET SALARY=SALARY+200 WHERE SALARY<1000 AND SEX="女"
-- 39．设有科研项目表PROJ(项目编号，项目名称，金额，教师编号)。试用SQL语句写出下面查询：列出金额最高的项目编号和项目名称。
SELECT MAX(金额),项目编号,项目名称 FROM PROJ  GROUP BY 项目编号
-- 40．设有学生关系STU(SNO，SNAME，AGE，SEX)，其属性分别表示：学号，姓名，年龄和性别。试用SQL语句检索年龄为空值的学生姓名。4'
SELECT SNAME FROM STU WHERE AGE IS NULL
#0810
-- 设有三个关系
-- A（Anum，Aname, city），它们的属性分别是：商场号，商场名称，商场所在城市；
-- B（Bnum, Bname, price），它们的属性分别是：商品号，商品名称，价格；
-- AB（Anum, Bnum, qty），它们的属性分别是商场号，商品号，商品销售数量。
-- 36.用SQL语句创建一个基于A，B，AB三个表的视图（上海商场），其中包括城市为上海的商场名称及其销售的商品名称。4'
CREATE VIEW 上海商场(Aname,Bname) AS SELECT Aname,Bname FROM A,B,AB WHERE city="上海" AND A.Anum=AB.Anum AND B.Bnum=AB.Bnum
-- 37.对36题中的三个基本表，用SQL语句查询所有商品的名称及其销售总额。
SELECT Aname,price*qty FROM A,B,AB WHERE A.Anum=AB.Anum AND B.Bnum=AB.Bnum GROUP BY Aname
注意审题:所有商品名称及销售总额
答案:SELECT B.Bname,SUM(B.price*AB.qty) FROM B,AB WHERE B.Bnum=AB.Bnum GROUP BY B.Bname
-- 38.对36题中的三个基本表，用SQL语句查询共有多少家商场销售“长虹彩电”。
SELECT COUNT(A.Anum) FROM A,B,AB  WHERE Bname="长虹彩电"  AND A.Anum=AB.Anum AND B.Bnum=AB.Bnum 
答案:SELECT COUNT(DISTINCT AB.Anum) FROM B,AB  WHERE B.Bname="长虹彩电" AND B.Bnum=AB.Bnum 

-- 39.设有选课表SC（S#，C#，GRADE），它们的属性分别是：学号，课号，成绩。试用关系代数表达式检索学习课号为C2课程的学生学号和成绩。

-- 40.设有学生关系S（Sno, Sname, Sage, Sex），它们的属性分别是：学号，姓名，年龄，性别。试用SQL语句检索出年龄大于等于18小于等于20的学生姓名和性别。4'
SELECT Sname,Sex FROM S WHERE Sage>=18 AND Sage<=20

#0801
-- 已知有如下三个关系：
-- 学生(学号，姓名，系别号)
-- 项目(项目号，项目名称，报酬)
-- 参加(学号，项目号，工时)
-- 其中，报酬是指参加该项目每个工时所得报酬。
-- 依据此关系回答下面36～40题。
-- 36．试用关系代数表达式写出下列查询：
-- 列出“王明”同学所参加项目的名称。
SELECT 项目名称 FROM 学生,项目,参加 WHERE 学生.学号=参加.学号 AND 项目.项目号=参加.项目号 AND  姓名="王明"

SELECT 项目名称 FROM 项目 WHERE 项目号 IN (SELECT 项目号 FROM 参加 WHERE 学号 IN (SELECT 学号 FROM 学生 AND 姓名="王明"))
-- 37．试用SQL语句写出下列查询：
-- 列出报酬最高的项目编号。
SELECT 项目号,MAX(报酬) FROM 项目 GROUP BY 项目号
答案:SELECT 项目号 FROM 项目 WHERE 报酬=(SELECT MAX(报酬) FROM 项目号)

-- 38．试用SQL语句写出下列查询：
-- 列出每个系所有学生参加项目所获得的总报酬。
SELECT 系别名,SUM(报酬) FROM 学生,项目,参加 WHERE 学生.学号=参加.学号 AND 项目.项目号=参加.项目号  GROUP BY 系别名
答案:SELECT 系别名,SUM(报酬*工时) FROM 学生,项目,参加 WHERE 学生.学号=参加.学号 AND 项目.项目号=参加.项目号  GROUP BY 系别名

-- 39.试用SQL语句查询报酬大于800元（包括800元）的项目名称。
SELECT 项目名称 FROM 项目 WHERE 报酬>=800
-- 40．试用SQL命令创建一个学生_项目视图，该视图包含的属性名称为：学号，姓名和项目名称。4'

create VIEW 学生_项目(学号,姓名,项目名称) AS SELECT 学号,姓名,项目名称 FROM 学生,项目,参加 WHERE 学生.学号=参加.学号 AND 项目.项目号=参加.项目号 
#0710
-- 36.设教学数据库中有三个关系：
-- 学生关系S(S#，SNAME，AGE，SEX，DEPT)，其属性分别表示学号、姓名、年龄、性别、所在系。
-- 课程关系C(C#，CNAME，TEACHER)，其属性分别表示课程号、课程名、任课教师名。
-- 选课关系SC(S#，C#，GRADE}，其中GRADE表示成绩。
-- 请用关系代数表达式表达下面的查询。
-- 检索选修课程号为“C2”的学生的学号和姓名。
SELECT S#,SNAME FROM S WHERE S# IN (SELECT S# FROM SC WHERE C# IN (SELECT C# FROM C AND CNAME="C2"))
SELECT S.S#,SNAME FROM S,C,SC WHERE S.S#=SC.S# AND C.C#=SC.C# AND CNAME="C2"
-- 37.在36题的基本表中，试用SQL语句完成下面操作：
-- 查询与张明同一个系的学生信息。4'
SELECT * FROM S WHERE DEPT IN (SELECT DEPET FROM S WHERE SNAME="张明") 
-- 38.在36题的基本表中，试用SQL语句完成下面操作：
-- 删除学号为“95002”的学生选修的课程号为“C2”的记录。
DELETE FROM  S,C,SC WHERE S.S#=SC.S# AND C.C#=SC.C#  AND S.S#="95002" AND C.C#="C2"
答案:DELETE FROM SC WHERE S#="95002" AND C#="C2"

-- 39.在36题的基本表中，试用SQL语句完成下面的操作：
-- 建立数学系学生的视图C_STUDENT，并要求进行修改和插入数据时，仍需保证该视图只有数学系的学生。视图的属性名为：S#，SNAME，AGE，DEPT。1'
答案:CREATE VIEW C_STUDENT(S#，SNAME，AGE，DEPT) AS SELECT S#，SNAME，AGE，DEPT FROM S
WHERE DEPT="数学" WITH CHECK OPTION
-- 40.在36题的基本表中，试用SQL语句查询每个学生已选修课程的门数及平均成绩。

SELECT COUNT(C.C#),AVG(GRADE)FROM SC,S,C WHERE S.S#=SC.S# AND C.C#=SC.C#  GROUP BY S.S#
提示:能一个表查到的绝不查多个表！！！！
答案:SELECT S#，COUNT(C#),AVG(GRADE)FROM SC WHERE GRADE IS NOT NULL  GROUP BY S#
