# .Net MAUI Multi-Platform App UI 學習筆記
## 大致簡介

+ 使用C#和XMAL開發
+ 可以輸出Blazor => .Net Core 3.x 的網頁框架
	+ IE 11+ 只支援 Blazor Server 不支援Blazor WebAssembly

## 使用者介面

### 筆刷
官網參考資料:<https://learn.microsoft.com/zh-tw/dotnet/maui/user-interface/brushes/>
可以自己繪製區域，再用於各種地方，如控制項的背景

例:繪出漸層區域當作Resource，再替換掉按鈕的Background
，就可以得到有漸層圖案的按鈕了

### UI 元件 (控制項)
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