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
