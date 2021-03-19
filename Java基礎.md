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





## Java技術產品區分
> 在2017年之後更新週期從原本的兩年一版變更為六個月
> SE8為LTS版(長期支援)，是目前企業使用率最高的版本
### Java SE
- 標準版
- 適用於開發用戶端程式
### Java EE
- 企業版
- 適用於開發伺服器端程式
### Java ME
- 手持裝置版
- 適用於開發手機、無線設備等程式

JRE 
==
> Java Runtime Environment
#### Java程式執行的環境
> JVN + Java class libaries
1. 一個Java程式只需要一個JVM去執行
2. 一個Java程式也需要一套針對此平台設計的標準**Java class libraries**
#### Write once, run anwhere

JDK
==
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

第一支Java程式
==
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



基本資料字面常數
==

整數/浮點數其實都是**數值**，所以Java語言對這兩種資料有做了處理，讓他們可以相容。\
資料的位階(排名)大小：
:::warning
double > float > long > int > short > byte
:::



#### 整數型態
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
		System.out.println(num5);  
		System.out.println(num6); // java自動轉成10進位
		System.out.println(num7); // java自動轉成10進位
		System.out.println("==============");
```
:::warning
- num5：十六進位整數用0x開頭表示，不可省略；java自動轉成10進位表示
- num6：JDK 7以後可使用二進位整數表示，用0b開頭表示，不可省略；java自動轉成10進位表示
- num7：JDK 7以後可使用底線將數值隔開
:::

#### 浮點數型態
```java=+
		float num8 = 1.0f;  // f代表指定為float型態
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

- num11：位數過多，java自動轉為科學記號表示
:::

#### 其它型態
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

