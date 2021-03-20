###### tags: `Java基礎`
Java 基礎觀念
==
#### Simple 簡單
- **不允許**直接使用指標(pointer)去操控記憶體位置
- 只**允許**使用**物件參考**(Object reference)來操控物件，和**資源回收機制**(Garbage collection)來自動處理已不被參考的物件
- Java的布林資料型態可以有true或false的值，不像其他語言只有1與0的值

#### Object-Oriented 物件導向
- 相較於程序性語言利用撰寫程式的先後次序來解決問題，物件導向著重於**物件之間**可以**相互運用**的關係來解決問題
![](https://i.imgur.com/XHYzFRl.png)
- 將程式模組化，使得維護容易、易於大型專案分工

#### Distributed 分散式運算
- Java是一種分散式語言，支援許多分散式網路技術
- Java的動態類別載入功能允許部分的程式碼可由網路下載，並在個人電腦執行，如Applet
#### Platform-independent 跨平台
- Java使用者先使用java編譯器(**javac**)編譯成電腦能接收的位元碼
- 位元碼透過各系統專有的Java虛擬機器(**JVM**)上的硬體去執行
- JVM目前已有多種平台版本，如Linux、Mac、Windows、手機等..
- **JVM**即是Java能跨平台的主要原因

## java檔與class檔關係
![](https://i.imgur.com/QhJUXZy.png)
### java檔是什麼？
- 程式設計師將程式碼儲存的格式
- 儲存的內容為原始碼，又稱為**Source code**
### class檔是什麼？
- 藉由編譯器將指定的java檔編譯成功後的檔案格式
- 儲存的內容為位元碼，又稱**Java byte code**
- 與任何作業系統平台無關
:::warning
Java是直譯語言還編譯語言？混合，編譯 + 直譯
:::

### Java技術產品區分
> 在2017年之後更新週期從原本的兩年一版變更為六個月
> SE8為LTS版(長期支援)，是目前企業使用率最高的版本
#### Java SE
- 標準版
- 適用於開發用戶端程式
#### Java EE
- 企業版
- 適用於開發伺服器端程式
#### Java ME
- 手持裝置版
- 適用於開發手機、無線設備等程式

## JRE 
> Java Runtime Environment
#### Java程式執行的環境
> JVM + Java class libaries
1. 一個Java程式只需要一個JVM去執行
2. 一個Java程式也需要一套針對此平台設計的標準**Java class libraries**
#### Write once, run anwhere

## JDK
> JRE + 開發者工具

> Java Development Kit
> 一般的軟體開發套件通稱為SDK(Software Development Kit)，如Android SDK、Goole SDK，但為了特別突顯Java語言所以用J取代了Software的稱呼
- 提供JRE本身不包含的開發相關工具，滿足開發者需求
- 包含在JDK裡的主要開發相關工具(可在%JAVA_HOME%/bin路徑下找到)
    - **javac**：java編譯器
        - 須留意java檔的編碼方式，例如使用記事本寫java檔編碼方式為utf-8的話，需在javac後面加 -encoding
        ![](https://i.imgur.com/3kTOGLD.png)

    - **java**：執行java程式(啟動JVM)
    ![](https://i.imgur.com/lWH1AfG.png)

    - **jar**：打包(壓縮)編譯好的class檔
    - **javadoc**：產生java程式碼的說明文件
    - **javap**：反編譯(class檔 -> java檔)
    - **jdp**：除錯(debug)工具

![](https://i.imgur.com/SApcVBT.png =500x)

## 第一支Java程式
### class
![](https://i.imgur.com/JcSzzjV.png)
:::warning
Java的檔案名稱必須以**class**的名稱來命名
:::
![](https://i.imgur.com/pG9qEAu.png)
- class name和檔名一致
- <modifier> 存取修飾字

### main方法
:::warning
main 方法為程式執行的進入點(入口)，有固定格式
:::
```java=
public static void main(String[] args){
    System.out.println("Hello World!")
}
```

使用Eclipse
==
- Eclipse是project based
- 程式有亂碼時可能是因為與預設編碼方式不同，可以修改
![](https://i.imgur.com/J4bJ4Fw.png)


### Eclipse 操作技巧
![](https://i.imgur.com/17Hmb7j.png)

快捷鍵設定：
- Window->Preference->General->Keys
- Ctrl+Shift+L可查看快捷鍵組合

> [參考來源](https://sites.google.com/site/javaxuexizhi/eclipse-bian-ji-qi/eclipse-chang-yong-kuai-su-jian)

基本資料字面常數
==

整數/浮點數其實都是**數值**，所以Java語言對這兩種資料有做了處理，讓他們可以相容。\
資料的位階(排名)大小：
:::warning
double > float > long > int > short > byte
:::



### 整數型態
```java=
package module01_06;
public class TestPrimitiveType {
	public static void main(String[] args) {
		byte num1 = 1;
		short num2 = 2;
		int num3 = 3;
		long num4 = 4;
		int num5 = 0x8e;  // 十六進位整數表示
		int num6 = 0b0010;  // JDK 7以後可使用二進位整數表示
		int num7 = 1_000_000;  // JDK 7以後可使用底線將數值隔開
		
		System.out.println(num1);
		System.out.println(num2);
		System.out.println(num3);
		System.out.println(num4);
		System.out.println(num5); // java自動轉成10進位 
		System.out.println(num6); // java自動轉成10進位
		System.out.println(num7); 
		System.out.println("==============");
```
:::warning
- num5：十六進位整數用0x開頭表示，不可省略；java自動轉成10進位表示
- num6：JDK 7以後可使用二進位整數表示，用0b開頭表示，不可省略；java自動轉成10進位表示
- num7：JDK 7以後可使用底線將數值隔開
:::

### 浮點數型態
```java=+
		float num8 = 1.0f;  // f代表指定為float型態，預設為double
		double num9 = 2.0;
		float num10 = 1234567890;
		double num11 = 1234567890; // 位數過多，java自動轉為科學記號表示
		
		System.out.println(num8);
		System.out.println(num9);
		System.out.println(num10);
		System.out.println(num11);
		System.out.println("==============");
```
:::warning
- num8：f代表指定為float型態，預設為double
- num11：位數過多，java自動轉為科學記號表示
:::

### 其它型態
```java=+
		boolean b = true;
		char ch1 = 'A';
		char ch2 = '我';
		int i1 = ch1;
		int i2 = ch2;
		char ch3 = '\u0041';    //參考Unicode表：http://www.tamasoft.co.jp/en/general-info/unicode.html
		
		System.out.println(b);
		System.out.println(ch1);
		System.out.println(ch2);
		System.out.println(i1);
		System.out.println(i2);
		System.out.println(ch3);
	}
}
```
### 常數宣告的目的與規則
- 若程式裡所使用的資料不會有異動或修改的前提下，可宣告成常數來**保證資料的不變性**
- 常數 Constant 通常**全部大寫**，用**底線**_來分開自組\
    例如：MY_NAME
- 一個變數宣告為**final**，也就是常數\
    例如：final double PI = 3.14;

變數宣告與初始化
==
- 變數被使用前皆需要有初值，否則編譯時會有錯誤訊息
- 兩種宣告方式
    - 宣告之後，再指定初始值，**區域變數**才可以
    - 宣告同時指定初始值，但宣告的變數需為**同一類型**的資料型態
    ![](https://i.imgur.com/zPvCq3P.png)
:::warning
！java為嚴謹的語言，第10行的寫法不夠明確因此無法執行，能成功賦值的只有z
:::

變數種類與有效範圍
==
根據宣告的位置或是宣告時搭配的關鍵字，會有不同特性的變數。
:::warning
變數不能在同一範圍內重複命名，但在**不同**method內，可以宣告一樣的變數名稱
:::
#### 區域變數 Local variables
- 宣告在method或blcok內
- 只能在它們被宣告的同method內存取
- 又稱為automatic、temporary或stack variables
:::warning
宣告在main方法裡的變數也是區域變數
:::

#### 實體變數 Instance variables
- 宣告在method之外，class之內，且**沒有static修飾子**
- 可被class內任何非static方法存取
- 又稱member variaber(成員變數)、attribute variables(屬性變數)

#### 靜態變數 Static variables
- 加上static修飾子為類別變數，也稱為靜態變數


運算子
==
## 算術運算子
> Arithmetic Operators，又稱為二元運算子
- 若兩個運算元的位階不相等，則回傳值會與**位階高者**相同
- 若兩個運算元為基本型別，**至少會轉成int型別**，避免**溢位**(overflow)問題發生
- 宣告型別時要留意賦予的值是否超過該型態的範圍
![](https://i.imgur.com/bjZgz4P.png)

```java=
public class TestArithmeticOP {
	public static void main(String[] args) {
		int num1 = 5, num2 = 2;
		double numD = 2.0;
		System.out.println(num1 + num2);
		System.out.println(num1 - num2);
		System.out.println(num1 * num2 * numD);
		System.out.println(num1 / num2);
		System.out.println(num1 / numD);
		System.out.println(num1 % num2);
		System.out.println("===============");
		
		//以下注意字串相加"+"
		String s1 = "現在是上午";
		int num3 = 10;
		String s2 = "點";
		System.out.println(s1 + num3 + s2);
	}
}
```
:::warning
- 第8、10行運算後的回傳值為double
- 第17行**串接**的結果為字串
:::

```java=+
        byte s1 = 10;
        byte s2 = 20;
        
        int sum = s1 + s2;
        System.out.println(sum);
```
:::warning
- 第24行，運算完的結果至少會轉成int，因此如果24行宣告為byte、short會產生錯誤
:::

## 遞增遞減運算子
- 又稱為一元運算子
- 分為**後置型**與**前置型**
    - 後置型：先取值再遞增
    - 前置型：先遞增後再取值
![](https://i.imgur.com/tdJX3aH.png)
```java=
public class TestInDecrementOP {
	public static void main(String[] args) {
		int num1 = 3, num2 = 4;
		System.out.println(++num1); // 3
		System.out.println(num1++); // 5
		System.out.println(--num2); // 3
		System.out.println(num2--); // 3
		System.out.println(num1);   // 5
		System.out.println(num2);   // 2
	}
}
```

## 指定運算子
- 將**右邊**運算完成的結果**指定給左邊**的變數保存起來
- 右值的位階不可以高過左值的位階
![](https://i.imgur.com/x6y3iDZ.png)
```java=
public class TestAssignOP {
	public static void main(String[] args) {
		int num = 1;
		num += 2;         
		String s = "1";
		s += 2;
		System.out.println(num);    // 3
		System.out.println(s);     // 12
	}
}
```
:::warning
第8行，資料的型態對運算結果會有很大影響
:::

## 關係運算子
- 用來比較兩個變數的值，其回傳值為一個布林值**true**或**false**
![](https://i.imgur.com/drXLMvd.png)
```java=
public class TestRelationalOP {
	public static void main(String[] args) {
		int num1 = 5, num2 = 3;
		System.out.println(num1 < num2);  // false
		System.out.println(num1 <= num2); // false
		System.out.println(num1 == num2); // false
		System.out.println(num1 != num2); // true
		System.out.println(num1 >= num2); // true
		System.out.println(num1 > num2);  // true
	}
}
```
## 條件運算子
- 將兩個布林值合併起來的運算子，運算結果仍為布林值
- 使用&&、||進行合併，Java才會是以**短路/捷徑運算**的邏輯去執行 (short-circuited)
> 捷徑運算：例如，&&左側的運算結果若為false，就可以不用再去執行右側的計算，因為&&必須兩者皆為true。\
藉此減少執行次數，提升效率。

![](https://i.imgur.com/yZCpCiK.png)
```java=
public class TestConditionOP {
	public static void main(String[] args) {
		int num1 = 3, num2 = 4;
		System.out.println(num1 > num2 && num1 != num2); // false && true > false
		System.out.println(num1 > num2 || num1 != num2); // false || true > true
		System.out.println(!(num1 > num2)); // !(false) > true
	}
}
```
## 位元運算子
> 偏向硬體/底層的運算處理，java較少運用到
- &(ans)、|(or)、^(xor)，可用在整數的位元運算
- ~(位元反轉運算子)，用於整數，將位元1變成0，0變成1
![](https://i.imgur.com/tNSpgNb.png)

## 移位運算子
- 用在整數型態的位元移位運算
- 移位運算子的右邊參數，若超過型態本身的位元數，則以餘數作為移位的次數
![](https://i.imgur.com/Lr0ZJ9K.png)
```java=
public class TestShiftOP {

	public static void main(String[] args) {
		int num = 1;
		System.out.println("2的1次方 = " + (num << 1));
		System.out.println("2的2次方 = " + (num << 2));
		System.out.println("2的3次方 = " + (num << 3));
	}

}
```

## 三元運算子
- 是一個簡略的if-else敘述
![](https://i.imgur.com/Mv74dAD.png)

```java=
if(x + y)
    a = x + 100;
else
    a = y + 100;
    
// 相當於：a = ( x > y) ? x + 100  : y + 100;
```
```java=
public class TestTernaryOP {
	public static void main(String[] args) {
		int income = 10000, outcome = 12000;
		System.out.println((income > outcome) ? "有積蓄" : "入不敷出");
		// (false) > 執行 ? 右邊運算式 > 入不敷出
	}
}
```
:::warning
第4行，先判斷()裡的結果為false後，執行 ? 右邊運算式，輸出結果為入不敷出
:::

## 運算子優先順序
![](https://i.imgur.com/BjxTZmy.png)

晉升與型別轉換
==
- 晉升 Promotion：較小的資料型別(等號的右邊)自動晉升為較大的資料型別(等號的左邊)，語法預設OK不用作額外處理
- 型別轉換 Typecasting：較大的資料型別轉換成為較小的資料型別，予法預設不OK，必須**強制轉換**
:::danger
- 強制轉換會使資料遺失
- 強制轉換的兩個資料類型必須有關聯性，例如int、double皆為數值，而int、Str則會產生error
- int和char強制轉換，背後根據的是Unicode的結果
:::
```java=
int x = 1;
double y = 2.2;
y = x + 1; // 晉升
x = (int)y + 1 // 將y由double強制轉換為int後與int相加
```
```java=
public class TestCast {

	public static void main(String[] args) {
		int i = 1;
		double d = 11.1;
		double sum1 = i + d;
		int sum2 = (int)(i + d);
		System.out.println(sum1);
		System.out.println(sum2);
	}

}
```
:::warning
第7行，d為double，需強制轉型，但需注意，不可寫成(int)i+d，因為d在+號右邊，此寫法不會將d做轉換，應將(i+d)，兩者相加後為double再強制轉換為int
:::

流程控制
==
## 選擇結構
### if...else
![](https://i.imgur.com/outLVUz.png)
- 大括號內若只有一條敘述，則大括號可省略
- 通常會結合**關係運算子**：<、<=、>、>=、==、!= 一起使用
- **else (最後一個選擇)不能設條件**



```java=
// 計算BMI值，判斷結果
public class TestBMI {
	
	public static void main(String[] args) {
		double weight = 55;
		double hight = 162/100f; // 浮點數資料型態預設為doble，要加上f才會成為float類型資料
		double bmi = weight / Math.pow(hight, 2); //Math.pow(要計算平方的值, 平方次數)
		
		System.out.printf("bmi = %.2f%n", bmi);
		if(bmi < 18.5) {
			System.out.println(bmi + "你太瘦了，多吃點好嗎！");
		}else if(bmi >= 24) {
			System.out.println(bmi + "嗯...少吃點好嗎？");
		}else {
			System.out.println(bmi + "標準身材!");
		}
	}
	
}
```
### switch...case
![](https://i.imgur.com/dI82doz.png)
- 多選一的方法也可以用 switch...case
- switch case變數(n)只可為**整數**、**字元**，不可為浮點數、long
- **JDK 7以後可以比對字串**
- 只適用於條件**完全相等**的比較(例如上方BMI範例就不適合使用)
- 若省略break敘述，則會執行下一個case中的敘述
:::danger
老師建議：若情況允許的話最好以switch方式撰寫，因為對於要比較的變數來說，存取次數只需1次即可，**效率相對較佳**
:::



```java=
public class TestSwitchCase {

	public static void main(String[] args) {
		int n = 10; 

		switch (n) {
		case 10:
			System.out.println("n 的值是 10"); // 執行
		case 20:
			System.out.println("n 的值是 20"); // 執行
			break;
		default:
			System.out.println("n 的值不是 10 也不是 20");
		}

		System.out.println("我仍有執行到!"); // 執行
	}

}
```

:::warning
switch..case 有貫穿特性(fall through)，遇到符合條件的入口(case)後進入，如果沒有使用break，java程式會繼續往下走。
- 第8行，n符合第一個進入口，執行後沒有break，因此下方也會執行，直到遇見break
:::

### 巢狀選擇結構
![](https://i.imgur.com/6i9els1.png)
- 較複雜的情況，可以使用巢狀if...else敘述
- 但須檢視是否有使用巢狀結構的必要，避免程式碼難以閱讀，且降低執行效率

```java=
public class TestIfElse2 {

	public static void main(String[] args) {
		int age = 29;
		String sex = "女"; // "男"

		if (age <= 29) 
			if (sex.equals("女")) // equals是字串內容相等的比較(是不是一樣的字)
				System.out.println("我請妳看電影^_^");
			else
				System.out.println("謝謝再聯絡！");
		else
			System.out.println("謝謝再聯絡！");
```
:::warning
可以改成以下寫法
:::
```java=+
		if (age <= 29 && sex.equals("女"))
			System.out.println("我請妳看電影^_^");
		else
			System.out.println("謝謝再聯絡！");

	}
```
		


