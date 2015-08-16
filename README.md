-----------------------------------
#Androidアプリのプログラムに挑戦  
ライブ壁紙  
-----------------------------------

ライブ壁紙を作ってみましょう  


背景にお化けが出現して  
タッチした指に憑いてきます！  

![00](https://github.com/miyake-yasunaga/06_WallPaper/blob/master/images/00.jpg)


新規プロジェクト作成  

Application name :KabeSample  
Company Domain   :applabo.sample.com  

![01](https://github.com/miyake-yasunaga/06_WallPaper/blob/master/images/01.jpg)
![02](https://github.com/miyake-yasunaga/06_WallPaper/blob/master/images/02.jpg)

「Add No Activity」を選択  

![03](https://github.com/miyake-yasunaga/06_WallPaper/blob/master/images/03.jpg)

壁紙に表示させる画像を格納するフォルダを作ります。  
「res」フォルダに「drawable-nodpi」というフォルダを新規作成  

![04](https://github.com/miyake-yasunaga/06_WallPaper/blob/master/images/04.jpg)
![05](https://github.com/miyake-yasunaga/06_WallPaper/blob/master/images/05.jpg)

壁紙用の画像を準備  
obake1.png  
![obake1](https://github.com/miyake-yasunaga/06_WallPaper/blob/master/image/obake1.png)

obake1.png  
![obake2](https://github.com/miyake-yasunaga/06_WallPaper/blob/master/image/obake2.png)

「obake1.png」、「obake2.png」をコピーして「drawable-nodpi」にpasteします。  
![06](https://github.com/miyake-yasunaga/06_WallPaper/blob/master/images/06.jpg)
![07](https://github.com/miyake-yasunaga/06_WallPaper/blob/master/images/07.jpg)
![08](https://github.com/miyake-yasunaga/06_WallPaper/blob/master/images/08.jpg)

壁紙用のプログラム作成  
「java」フォルダの下に「KabeMain」というJavaのクラスファイルを作成します。  

![09](https://github.com/miyake-yasunaga/06_WallPaper/blob/master/images/09.jpg)
![10](https://github.com/miyake-yasunaga/06_WallPaper/blob/master/images/10.jpg)

下記のソースを貼り付けます。  
  
KabeMain.java  
--------------------
        package com.sample.applabo.kabesample;
        
            import android.graphics.Bitmap;
            import android.graphics.BitmapFactory;
            import android.graphics.Canvas;
            import android.graphics.Color;
            import android.service.wallpaper.WallpaperService;
            import android.view.SurfaceHolder;
            import android.content.res.Resources;
            import android.view.MotionEvent;
        
        //ライブ壁紙
        public class KabeMain extends WallpaperService {
            @Override
            public Engine onCreateEngine() {
                return new WallpaperEngine(getResources());
            }
        
            public class WallpaperEngine extends Engine
                    implements Runnable {
                private SurfaceHolder holder;//サーフェイスホルダー
                private Thread        thread;//スレッド
        
                private Bitmap image1; //イメージ
                private Bitmap image2; //イメージ
                private int    width; //画面幅
                private int    height;//画面高さ
                private int   touchX =0;
                private int   touchY =0;
                private int   num = 0;
        
                //コンストラクタ
                public WallpaperEngine(Resources r) {
                    image1 = BitmapFactory.decodeResource(r, R.drawable.obake1);
                    image2 = BitmapFactory.decodeResource(r, R.drawable.obake2);
                }
        
                //サーフェイス生成
                @Override
                public void onSurfaceCreated(SurfaceHolder holder) {
                    super.onSurfaceCreated(holder);
                    this.holder = holder;
                }
        
                //サーフェイス変更
                @Override
                public void onSurfaceChanged(SurfaceHolder holder,
                                             int format, int width, int height) {
                    super.onSurfaceChanged(holder, format, width, height);
                    this.width = width;
                    this.height = height;
                }
        
                //表示状態変更
                @Override
                public void onVisibilityChanged(boolean visible) {
                    if (visible) {
                        //スレッドの開始
                        thread = new Thread(this);
                        thread.start();
                    } else {
                        //スレッドの停止
                        thread = null;
                    }
                }
        
                //サーフェイス破棄
                @Override
                public void onSurfaceDestroyed(SurfaceHolder holder) {
                    super.onSurfaceDestroyed(holder);
                    thread = null;
                }
        
                //ループ処理
                public void run() {
                    while(thread != null) {
                        long nextTime = System.currentTimeMillis()+1000;
                        repeat();
                        try {
                            Thread.sleep(nextTime-System.currentTimeMillis());
                        } catch (Exception e) {
                        }
                    }
                }
                //画面タッチ時の座標取得
                public void onTouchEvent(MotionEvent event) {
                    touchX = (int) event.getX();
                    touchY = (int) event.getY();
                }
        
                //繰り返し処理
                private void repeat() {
                    //ロック
                    Canvas canvas = holder.lockCanvas();
                    //背景を白に設定
                    canvas.drawColor(Color.WHITE);
                    //画像を切り替える
                    if(num == 0){
                        canvas.drawBitmap(image1,touchX,touchY,null);
                        num++;
                    } else {
                        canvas.drawBitmap(image2,touchX,touchY,null);
                        num = 0;
                    }
                    //アンロック
                    holder.unlockCanvasAndPost(canvas);
                }
            }
        }
---------------------


![11](https://github.com/miyake-yasunaga/06_WallPaper/blob/master/images/11.jpg)

壁紙設定用のxmlファイル作成  
「res」フォルダに「xml」というフォルダを新規作成して  
「wallpaper.xml」というファイルを作成。  

![12](https://github.com/miyake-yasunaga/06_WallPaper/blob/master/images/12.jpg)
![13](https://github.com/miyake-yasunaga/06_WallPaper/blob/master/images/13.jpg)
![14](https://github.com/miyake-yasunaga/06_WallPaper/blob/master/images/14.jpg)
![15](https://github.com/miyake-yasunaga/06_WallPaper/blob/master/images/15.jpg)

下記のソースを貼り付けます。  

wallpaper.xml  
---------------------
        <?xml version="1.0" encoding="utf-8"?>
        <wallpaper xmlns:android="http://schemas.android.com/apk/res/android"
            android:thumbnail="@drawable/obake1"
            android:description="@string/description"
            android:author="@string/author">
        </wallpaper>
---------------------


![16](https://github.com/miyake-yasunaga/06_WallPaper/blob/master/images/16.jpg)

文字列設定用xmlファイルを編集します。  
「res」-「values」フォルダの「strings.xml」に  
下記のソースを貼り付けます。  

strings.xml  
---------------------
        <resources>
            <string name="app_name">ばけがみ</string>
            <string name="description">お化けが指に憑いてくる</string>
            <string name="author">applabo</string>
        </resources>
---------------------


![17](https://github.com/miyake-yasunaga/06_WallPaper/blob/master/images/17.jpg)

アプリ全般の設定用ファイル  
AndroidManifest.xmlを編集します。  

下記のソースを貼り付けます。  

AndroidManifest.xml  
-----------------------------
        <manifest xmlns:android="http://schemas.android.com/apk/res/android"
            package="com.sample.applabo.kabeSample">
        
            <application android:allowBackup="true" android:label="@string/app_name"
                android:icon="@drawable/ic_launcher" android:theme="@style/AppTheme">
        
                <service
                    android:name=".KabeMain"
                    android:permission="android.permission.BIND_WALLPAPER" >
                    <intent-filter>
                        <action android:name="android.service.wallpaper.WallpaperService" />
                    </intent-filter>
                    <meta-data
                        android:name="android.service.wallpaper"
                        android:resource="@xml/wallpaper" />
                </service>
        
            </application>
        
        </manifest>
-----------------------------

![18](https://github.com/miyake-yasunaga/06_WallPaper/blob/master/images/18.jpg)

実機を接続して実行します。  
今回作ったのは壁紙アプリで  
起動する画面がないため  
「Edit Configuratiion」という画面がでてきますが  
そのまま「Run」を押します。  

![19](https://github.com/miyake-yasunaga/06_WallPaper/blob/master/images/19.jpg)

「Change Configuration Settings」という  
確認メッセージがでるので  
「Continue Anyway」を押します。  

![20](https://github.com/miyake-yasunaga/06_WallPaper/blob/master/images/20.jpg)

実行する機種の選択画面が出るので  
接続している実機を選択して「OK」ボタンを押下します。  

![21](https://github.com/miyake-yasunaga/06_WallPaper/blob/master/images/21.jpg)


ライブ壁紙の設定と削除  

ライブ壁紙は普通のアプリのようにインストールしても  
画面がアイコンに出てきたりしません。  

![a01](https://github.com/miyake-yasunaga/06_WallPaper/blob/master/images/a01.jpg)

画面のアイコンがない箇所を長押しします。  
「操作を選択」という画面が表示されるので  
「壁紙」を選択します。  

![a02](https://github.com/miyake-yasunaga/06_WallPaper/blob/master/images/a02.jpg)

「ライブ壁紙」を選択します。  

![a03](https://github.com/miyake-yasunaga/06_WallPaper/blob/master/images/a03.jpg)

インストールされているライブ壁紙が表示されます。  

![a04](https://github.com/miyake-yasunaga/06_WallPaper/blob/master/images/a04.jpg)

選択した壁紙がプレ表示されます。  
「壁紙に設定」を押下して決定します。  

![a05](https://github.com/miyake-yasunaga/06_WallPaper/blob/master/images/a05.jpg)

画面の背景に選択したライブ壁紙が表示されます。  
画面を指でタッチするとお化けがついてきます。  

![a06](https://github.com/miyake-yasunaga/06_WallPaper/blob/master/images/a06.jpg)
![a07](https://github.com/miyake-yasunaga/06_WallPaper/blob/master/images/a07.jpg)

「設定」⇒「ディスプレイ」⇒「壁紙」でも  
壁紙の設定ができます。  

![a08](https://github.com/miyake-yasunaga/06_WallPaper/blob/master/images/a08.jpg)
![a09](https://github.com/miyake-yasunaga/06_WallPaper/blob/master/images/a09.jpg)

ライブ壁紙の削除（アンインストール）  
壁紙の切り替えは上と同じ手順で別の壁紙やライブ壁紙を  
選択すればいいだけですが  

削除したい場合は  
「設定」⇒「アプリケーション」で  
アプリを選択すると「アプリ情報」画面が出てきて  
「アンインストール」ボタン押下で削除できます。  

![a08](https://github.com/miyake-yasunaga/06_WallPaper/blob/master/images/a08.jpg)
![a10](https://github.com/miyake-yasunaga/06_WallPaper/blob/master/images/a10.jpg)
![a11](https://github.com/miyake-yasunaga/06_WallPaper/blob/master/images/a11.jpg)

いかがでしょうか？  
皆さんも是非オリジナルのライブ壁紙を作ってみてください！  
