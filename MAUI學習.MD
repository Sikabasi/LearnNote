# .Net MAUI Multi-Platform App UI 學習筆記
## 大致簡介

+ 使用C#和XMAL開發
+ 可以輸出Blazor => .Net Core 3.x 的網頁框架
	+ IE 11+ 只支援 Blazor Server 不支援Blazor WebAssembly

## 目錄
+ [使用者介面](#使用者介面)
	+ [筆刷](#筆刷)
	+ [控制項](#控制項)
	+ [頁面](#頁面)
	+ [檢視](#檢視)
+ [動態新增元件](#動態新增元件)
+ [啟動顯示畫面](#啟動顯示畫面)
+ [發布APK](#發布APK)
+ [功能](#功能)



## 使用者介面 <div id='使用者介面'/>

### 筆刷
官網參考資料:<https://learn.microsoft.com/zh-tw/dotnet/maui/user-interface/brushes/>
可以自己繪製區域，再用於各種地方，如控制項的背景

例:繪出漸層區域當作Resource，再替換掉按鈕的Background
，就可以得到有漸層圖案的按鈕了

### 控制項
UI元件(控制項)

官網參考資料:<https://learn.microsoft.com/zh-tw/dotnet/maui/user-interface/controls/>

#### 頁面
+ ContentPage

單一顯示頁面
+ FlyoutPage

管理兩個相關資訊頁面的頁面
+ NavigationPage

視需要瀏覽頁面、向前/後瀏覽
+ TabbedPage

由頁面頂端或底端的索引標籤巡覽

#### 版面配置
+ AbsoluteLayout

將子專案放在相對於父代的特定位置
+ BindableLayout
+ FlexLayout
+ Grid
+ HorizontalStackLayout
+ StackLayout
+ VerticalStackLayout

#### 檢視
##### 資料呈現
###### Border
可在另一個控制項周圍繪製框限

```
<Border>
</Border>
```
##### 命令類型元件
###### Button
最常用的按鈕

常搭配 Clicked ="函式名稱" 來控制按鈕按下後的動作

然後需要再這個xmal.cs的類別中添加相對應名稱的函式
```
private void OnButtonOnClicked(object sender,EventArgs args)
{
　//要執行的程式....
}
```
可以透過sender來獲得這顆按鈕
```
private void OnButtonOnClicked(object sender,EventArgs args)
{
　var btn = (Button)sender;
}
```


元件也會用到```x:name=""```
以方便使用程式碼來控制指定的元件
###### RefreshView
提供可捲動內容的重新整理功能
###### SearchBar
搜尋列
###### SwipeView
撥動手勢顯示內容功能表項目
##### 設定值

###### CheckBox

```<CheckBox/>```
###### DatePicker
開啟該平台的日期選擇器，可選取日期

```<DatePicker />```

###### Slider
滑桿元件
###### Stepper
顯示 **-|+** 選項的元件

```<Stepper/>```

###### Switch
切換按鈕

```<Switch/>```

Switch 元件在 目標平台為Windows時，有時候會一直感覺無法對齊，實際在外面包一層Border後就可以發現，在Windows平台的Switch左邊有一個非常大的預留空白，在此提供不要顯示那些空白的處理方法
1. 在Switch的外層加一層HorizontalStackLayout
2. 限制其寬度，我個人是覺得50還不錯，不過你可以自行決定

```
	<HorizontalStackLayout
		HorizontalOptions="Center"
		WidthRequest="50">
		<Switch VerticalStackLayout="Center">
	</HorizontalStackLayout>
```


###### TimePicker
開啟該平台的時間選擇器，可選取時間

```<TimePicker />```

##### 編輯文字
###### Editor
輸入及編輯多行文字

```<Editor/>```
###### Entry
輸入及編輯單行文字，也可當密碼欄

```<Entry/>```

##### 顯示活動
###### ActivityIndicator
顯示像是應用程式 像是Loading之類的需要等待的活動，並且可會撥放動畫

**不會顯示進度**

```<ActivityIndicator IsRunning="true" />```

##### 集合元件
用來表示像清單之類的有各種元素集合的元件

###### Carouse View

1. 新增 資料夾 ***Views***
2. 新增 資料夾 ***ViewModels***
3. 新增 資料夾 ***Models***
4. 在 Views 下新增 ContentPage 可以取名為 ***XXXView***
5. 在 ViewModels 下新增 Class 取名為 ***XXXViewModel*** ， Class 範圍 改為 public
6. 在 Models 下新增 Class 取名為 ***XXXModel*** ， Class 範圍改為 public
7. ***XXXModle.cs*** 加入一些 public的屬性，像是
```
	public string IntroTitle { get; set; }
	public string IntroDescription { get; set; }
```
8. 在 ***XXXViewModel.cs*** 新增 ***XXXModel*** 型別的 **ObservableCollection** 屬性 
```
	public ObservableCollection<XXXModel> XXXs { get; set; } =
            new ();
```
9. 在 ***XXXViewModel*** 建構子新增想要添加的模型資料
```
	public XXXViewModel()
        {
            XXXs.Add(new XXXModel
            {
                IntroTitle = "My Title 1",
                IntroDescription = "First page Description",
            });

            XXXs.Add(new XXXModel
            {
                IntroTitle = "My Title 2",
                IntroDescription = "Second page Description",
            });
        }
```
 

###### Picker 清單
就是MAUI中使用的清單元件

將清單中需要出現的項目加在Picker.Items中，
使用```x:string```來增加文字項目

由於預設的Picker沒有外框，所以可以在這外面再包一層Border元件來使Picker可以比較清楚的給使用者看見。

Picker有很多互動可以使用，最簡易可以使用```SelectedIndexChanged=""```來指定互動，當使用者選擇不同的項目之後進行觸發

```
<Border>
<Picker
 Title="My List" HorizontalOptions="Center"
 SelectedIndexChanged="SelectedChanged"
 >
	<Picker.Items>
	<x:String>Item1</x:String>
	<x:String>Item2</x:String>
	<x:String>Item3</x:String>
	</Picker.Items>
</Picker>
</Border>
```

而在程式碼中獲得這個清單

使用```SelectedItem```獲得選擇的這個項目，可以直接toString()來比對想要的項目

或是```SelectedIndex```或的index，可以在清單顯示項目和內部比較值不一樣的情框下使用
```
private void SelectedChanged(object sender,EventArgs e)
{
	var picker = (Picker)sender;
    Console.WriteLine(picker.SelectedItem.ToString());
	Console.WriteLine(picker.SelectedIndex);
}
```


#### ContentView
自訂的控制項，並且可重複使用，可跳至**動態新增元件**
觀看如何新增



## 動態新增元件

在MainPage.xaml.cs新增一個函式作為Loaded要執行的功能

```
	private void ContentPage_Loaded(object sender,EvetArgs e)
	{
		Label label = new();
		label.Text = "New Label!";
		label.HorizontalTextAlignment =TextAlignment.Center;
		label.BackgroundColor = Colors.AliceBlue;
		vsl.Children.Add(label);
	}


```

而在MainPage.xaml內 繫結 一個載入事件

並且建立一個ScrollView當作產生元件的物件


```
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="MauiApp1.MainPage"
             Loaded="ContentPage_Loaded"
             >

<AbsoluteLayout>
         <ScrollView AbsoluteLayout.LayoutFlags="All" AbsoluteLayout.LayoutBounds="0,0,1,1">
            <VerticalStackLayout x:Name="vsl"  />
        </ScrollView>    
    </AbsoluteLayout>


```

## 啟動顯示畫面
### Android的起始畫面設定
在Platforms/Android/Resources/values/colors.xml  設定顏色

在Platforms/Android/Resources/MainActivity.cs處理要啟動的圖片
## Android Emulator

如果報錯 :Another emulatro is still running

關閉背景執行的程式

CMD-> taskkill /F /IM "qemu-system-x86_64.exe" /T


## 發布APK

在方案總管視窗，對方案按右鍵/在終端機中開啟，使用以下命令
```
dotnet publish -f:net6.0-android -c:Release
```

他就會在 專案目錄/bin/Debug/net6.0-android下產生

或是在 專案目錄/bin/Release/net6.0-android下產生

全看你當下的方案組態

## 功能

### 儲存資料

在MAUI裡還是一樣可以像原本的C#一樣使用 File 類別來進行存讀檔

這裡是直接用Newtonsoft.json 來序列化資料(下載Newtonsoft.Json.Bson 的NuGet就可以了)


簡易的小程式:
```
  var jsonString = JsonConvert.SerializeObject(temp);
        File.WriteAllText(fileName, jsonString);//read
```

然後讀檔:

```
 var data = File.ReadAllText(fileName);
```

我的fileName 是使用

```
 string fileName = Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData), "temp.txt");
```

Environment.SpecialFolder列舉所提供的資料夾位址在官網上的說明:<https://learn.microsoft.com/zh-tw/dotnet/api/system.environment.specialfolder?view=net-6.0>


#### MAUI 使用 Google AdMob

#### 安裝nuget

這裡我們使用 Nuget Plugin : **Plugin.MauiMTAdmob** 
Github位置 :<https://github.com/marcojak/MauiMTAdmob>

當我們安裝NugGet的時候，可能會發生 找不到安裝包部分路徑的報錯，這是因為整個Nuget 路徑過長所導致的，一個比較簡單的解決方案是
1. 使用系統管理員身分打開命令提示字元
2. 將目前位置移動到 Maui 專案的資料夾位置
3. 輸入 ```dotnet add package Plugin.MauiMTAdmob --version 1.0.1```
4. 按下Enter 使用 終端機來進行nuget包的手動安裝
5. 如果出現 *已將套件'Plugin.MauiMTAdmob' 版本 '1.0.1' 的 PackageReference新增至專案* 就算安裝完成了

#### 出現 UWP 相關 資源找不到的問題

1. 請先下載 上面提供的 Github 庫 ，並把 **"SampleMMTAdmob"** 資料夾下的 **"Dependencies"**資料夾整個複製進專案資料夾下，並確保在Visual Studio 的 方案總管中 有加進 專案裡面
2. 雙擊 專案名稱打開 .csproj 檔
3.  在 專案空白處 並 在  <Project> </Project> 之中添加 
	```
	<ItemGroup Condition="'$(TargetFramework)' == 'net6.0-android'">      
    <AndroidLibrary Include="Dependencies\user-messaging-platform-2.0.0.aar" Bind="false" />
	</ItemGroup>
	```
4. 如此一來編譯應該就能成功了

#### 打開程式時發生報錯 *Java.Lang.RuntimeException: 'Unable to get provider com.google.android.gms.ads.MobileAdsInitProvider: java.lang.IllegalStateException:*

這是因為沒有正確與Google admob進行連結

1. 點開 Platforms 資料夾 下 Android 的 AndroidManifest.xml 
2. 原本的 application 標籤可能長這樣
   ``` <application android:allowBackup="true" android:icon="@mipmap/appicon" android:roundIcon="@mipmap/appicon_round" android:supportsRtl="true"/>```
3. 請將驗證程式ID的屬性加入 <application> 之中 
   
   程式碼: 
   ```
    <!-- Sample AdMob App ID: ca-app-pub-3940256099942544~3347511713 -->
  	<meta-data android:name="com.google.android.gms.ads.APPLICATION_ID"
             android:value="ca-app-pub-################~##########"/>
   ```
4. 因為目前只是要離線測試 ，所以使用 GoogleAdmob 所提供的測試用帳號 **ca-app-pub-3940256099942544~3347511713**,到時候如果想用自己的再更換app id即可
5. 應該會長得像以下
   ```
    <application android:allowBackup="true" android:icon="@mipmap/appicon" android:roundIcon="@mipmap/appicon_round" android:supportsRtl="true">
  	<!-- Sample AdMob App ID: ca-app-pub-3940256099942544~3347511713 -->
  	<meta-data android:name="com.google.android.gms.ads.APPLICATION_ID"
             android:value="ca-app-pub-3940256099942544~3347511713"/>
  	</application>
   ```
##### 程式打開時，廣告的初始化
1. 需要在CreateApp的時候，對AdMob進行初始化，且各平台的方式會不一樣，所以需要個別平台進行處理
2. Android
   1. 點開 Platforms 資料夾 下 Android 的 MainApplication.cs
   2. 將 CreateMauiApp() 中加入 ```MobileAds.Initialize(this)``` 
   3. 如果 原本是 使用箭頭函式的方式表達 ，可以依下面範例修改
   ```
    protected override MauiApp CreateMauiApp()
    {
        MobileAds.Initialize(this);
        return MauiProgram.CreateMauiApp();
    }
   ```
   


#### MAUI 發布Mac
當要封裝以供發布的話，有出現一個錯誤訊息
Unable to build chain to self-signed for signer
打開鑰匙圈會發現你的開發者憑證也許會寫 憑證不受信任
可以對其點擊右鍵->取得詳細資訊 
往下到簽發人名稱
查看他的
+ 一般名稱
+ 組織單位

再去手動下載相關的cer加入到你的鑰匙圈後就可以了
