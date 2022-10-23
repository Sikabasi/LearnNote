# .Net MAUI Multi-Platform App UI 學習筆記
## 大致簡介

+ 使用C#和XMAL開發
+ 可以輸出Blazor => .Net Core 3.x 的網頁框架
	+ IE 11+ 只支援 Blazor Server 不支援Blazor WebAssembly

## XAML元件


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
## Android Emulator 

如果報錯 :Another emulatro is still running

關閉背景執行的程式

CMD-> taskkill /F /IM "qemu-system-x86_64.exe" /T

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