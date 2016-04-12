#Android Team 如何處理切圖

##簡述
因為Android系統特性關係，及裝置機種繁多，為了減輕設計師的負擔，讓設計師專注於流程與介面設計，而不浪費時間在處理切圖，Android工程師需要學習如何自己簡易的處理切圖。

##Android排版策略

基本上Android的切圖排版策略分為二種：

1.  只使用一套切圖，在各種裝置上以Scale的方式排版
2.  將切圖分為mdpi, hdpi, xhdpi, xxhdpi, xxxhdpi等數套圖

使用哪種策略端看設計師所繪的設計圖而定，當面臨的設計圖看起來在各種不同尺寸的配備當中，缺乏彈性調整的空間時，或是講求位置精確的設計圖當中，我們會選用一套切圖，並以縮放的方式將圖定位在正確的位置上。例如在Android Animation Desk Classic上的排版就較適合使用一套切圖縮放的方式處理。這樣做的好處是，只要準備一種圖，且size容易計算。這種方式會需要考慮所可能使用最大解析度的裝置，來作為切圖的範本，如果以小的裝置來做設計時，當圖片放大時會產生圖片模糊的問題。另一個問題是當排版複雜時，這可能會耗費額外的運算資源在處理排版的縮放。

在具有彈性空間配置的設計圖，則最好能使用第二種方法，將套圖分為不同dpi的套圖。例如在NoteLedge Cloud和PDF Reader當中使用此方法。既然已經分為不同解析度的圖，就不應該在把這種圖進行任何縮放，也不需要擔心圖片模糊的問題。但需要較耗費時間處理各種解析度的切圖。

並非一個專案只能使用1或2其中一種方式，而是應依照排版設計，來做適當的選擇。同一個畫面也可能需要同時使用兩種方法來進行排版，端看介面需求而定。

## 設計師著手設計前

###思考在不同裝置的樣子

設計師在開始著手設計介面之前，得先思考這個App在不同大小的裝置上，應該如何做介面適應，最好的設計是同一張設計圖得以運用在不同尺寸的平板或手機，或只要做行列數的調整，或只要版面位移就能做好不同裝置的適應。

考慮 48 rhythem，這是設計師應該了解的最重要概念，如果一個按鈕在手機上是48dp，在平板上也應該是48dp。另外48dp 是適合手指按壓的適切大小。大部分按鈕應大於48dp，不會因為螢幕小，按鈕就小。

參考資料：   
[UI Metrics & keylines](https://www.google.com/design/spec/layout/metrics-keylines.html#)

考慮哪個尺寸的裝置可能是最主要的使用者來源，選定介面設計的尺寸，並告知工程師，這個設計圖是使用哪個尺寸哪個density來設計的，例如選用Nexus 4為範本來設計時，其尺寸為768 x 1280 且density 為 xhdpi。裝置尺寸可以參考Device Matric網頁。

參考資料：  [Device Matric](https://design.google.com/devices/)

選定設計圖裝置後，依上例設計師應以768 x 1280 為底開始設計，此時設計師在此設計圖中所設計的元件大小為解析度xhdpi的大小，設計師可以利用工具計算出所繪的元件pixel，實際上在xhdpi裝置上dp是多少，然後提供給工程師。Android工程師通常在元件的大小會使用dp為單位而非px。如此一來工程師就能自己產生所需要的圖。

參考資料： [Supporting Multiple Screens](http://developer.android.com/intl/zh-tw/guide/practices/screens_support.html)

參考資料： [dp/px converter tool](http://pixplicity.com/dp-px-converter/)

## dp v.s. px


## 設計師切圖
###一般的按鈕切圖(單色，正方形)
設計師提供給工程師切圖時，需要考慮到所提供的圖，支援的最大解析度所需的還大，這樣一來工程師自己製作套圖時才不會有模糊的情況。譬如即使是在xhdpi上設計的圖，設計師要計算出在xxxhdpi上需要多大的圖。至少提供給工程師比xxxhdpi大的圖或相等的圖即可。只要這張圖是單色，工程師就可以利用這張圖來產生不同dpi的套圖。

如果是使用Material Icons當中的圖示，設計師甚至可以提供icon名稱請工程師自行下載或使用[Android Asset Studio for material design](http://shreyasachar.com/AndroidAssetStudio/)來製作就好。

[Material Icons](https://design.google.com/icons/)

###不同顏色的切圖
如果因點擊效果而需要不同顏色或透明度的切圖時，只要這張圖示是單色，則不需另外提供切圖，只要告知16進位色碼及透明度，工程師可以自行使用工具產生。


## 工程師的產生切圖工具
-  [Android Asset Studio](https://romannurik.github.io/AndroidAssetStudio/)
-  [Android Asset Studio for material design](http://shreyasachar.com/AndroidAssetStudio/)

上述兩項工具具有相同功能，第二項則內建了官方Material Design的圖示。有了上述工具，工程師能夠自己建立所需要的icon套圖，自定顏色、透明度和icon大小。

在Android Studio當中也有相同的工具，在module name按下右鍵 > New > Image asset 可以進行相同的切圖產生。

這個工具仍有其限制，就是當所要產生的切圖不是正方形時，上述工具就無法使用，目前仍未找到適當的工具，因此這時候仍然需要設計師幫忙。

## 程式當中直接變更Image顏色
執行期間動態變更image顏色

	imageView.setColorFilter(Color.argb(255, 255, 255, 255)); // White Tint

在XML當中設置變更圖片顏色

	<ImageView id="@+id/icon" 
	   android:layout_width="wrap_content" 
	   android:layout_height="wrap_content" 
	   android:tint="#FF000000" 
	   android:src="@drawable/chat_icon"/> 
   
依點擊事件處理圖片顏色

 [How to use selector to tint imageview in android](http://stackoverflow.com/questions/19500039/how-to-use-selector-to-tint-imageview-in-android)

## Vector assets 和 SVG圖示
仍然有一些情況，不適合以上面的方式來處理圖片，例如不是作為按鈕的較大張說明用圖，像是用在Splash 畫面的圖示、不是長方形且大張需隨畫面調整大小的圖、或一些功能介紹圖這些情況會較適合以向量圖示來處理。[Tip: 按鈕通常不會隨裝置大小而變更大小，但介紹用的圖示會需要，所以按鈕icon不需使用向量圖示，但介紹用圖示需要]。
###Vector assets 
Vector asset是以xml來定義向量圖型的格式，需注意<font color="red">只支援Android 4.4 (API level 20) 及以後的版本</font>。使用Android Studio中的Vector Asset Studio來製作出向量圖xml，並將xml放在drawable文件夾當中，使用時就和圖片資源使用方式一樣。

	<ImageButton
	  android:id="@+id/imageButton"
	  android:src="@drawable/ic_build_24dp"
	  android:tint="@color/colorAccent"
	  android:layout_width="wrap_content"
	  android:layout_height="wrap_content"
	  android:layout_below="@+id/textView2"
	  android:layout_marginTop="168dp" />
參考文件：[Vector Asset Studio](http://developer.android.com/intl/zh-tw/tools/help/vector-asset-studio.html)

###使用SVG
[Vector Asset Studio](http://developer.android.com/intl/zh-tw/tools/help/vector-asset-studio.html)也能夠直接將svg格式的文件轉換成Vector assets格式，因此設計師提供向量圖型時只需要提供常見的SVG給工程師即可。

###支援Android 4.4以前版本
若需要使用向量圖片支援4.4以前的版本則需使用到其他資源庫，例如[svg-android](https://code.google.com/archive/p/svg-android/)