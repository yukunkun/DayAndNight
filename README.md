### Android夜晚模式实现
[简书链接](http://www.jianshu.com/p/f1c09e483b11)

![](http://upload-images.jianshu.io/upload_images/3001453-8343773b38b1ab67.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)![S71031-13270753.jpg](http://upload-images.jianshu.io/upload_images/3001453-9309a743cbbfd2ef.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 超简单的实现白天夜晚模式，前提是都是Android高版本的系统。当然相信现在适配的大多数已经是高版本的系统了，4.4的一般也不会适配了吧
        compile 'com.android.support:appcompat-v7:25.+'
#### 相信你不会低于这个包吧'Activity'才会继承'AppCompatActivity'
#### 其实方法很简单就是一个方法就可以实现
    private void setNightMode() {
            //  获取当前模式
            int currentNightMode = getResources().getConfiguration().uiMode & Configuration.UI_MODE_NIGHT_MASK;
            //  将是否为夜间模式保存到SharedPreferences
            //  切换模式
            getDelegate().setDefaultNightMode(isNight ?
                    AppCompatDelegate.MODE_NIGHT_NO : AppCompatDelegate.MODE_NIGHT_YES);
    
            //  重启Activity
            recreate();
        }
    
        public void change(View view) {
            setNightMode();
            isNight=!isNight;
        }
#### 如上所示
    getDelegate().setDefaultNightMode(isNight ?
                        AppCompatDelegate.MODE_NIGHT_NO : AppCompatDelegate.MODE_NIGHT_YES);
#### 其实就是这个方法起的作用，当你调用这个方法的时候，系统就会相应的切换到你所需要的模式，前期你需要做一些简单的配置。如下：
#### 新建values-night目录，如下：
![](http://upload-images.jianshu.io/upload_images/3001453-9981c5d9d9cdb371?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
#### 在这里面新建一个values-night,在res 目录下。在values-night新建一个colors.xml
![](http://upload-images.jianshu.io/upload_images/3001453-e80ba1ea8ebbe24b?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
#### 接下来只需要在对应的colors文件下写不同的颜色值(夜间颜色值和白天颜色值)即可。至此关于实现夜间模式的配置已经基本完成。
#### 'AppCompatDelegate.setDefaultNightMode(mode)',其中mode有一下四个值：
     
     MODE_NIGHT_NO： 亮色(light)主题，不使用夜间模式
     MODE_NIGHT_YES：暗色(dark)主题，使用夜间模式
     MODE_NIGHT_AUTO：根据当前时间自动切换 亮色(light)/暗色(dark)主题（22：00-07：00时间段内自动切换为夜间模式）
     MODE_NIGHT_FOLLOW_SYSTEM(默认选项)：设置为跟随系统，通常为MODE_NIGHT_NO         
#### 这个时候，只需要你调用之前的方法就可以实现了。  'getDelegate().setDefaultNightMode(isNight ? AppCompatDelegate.MODE_NIGHT_NO : AppCompatDelegate.MODE_NIGHT_YES);'
#### 第二次进入这个页面就可以实现主题的更换了。
#### 有些手机自定义了Activity的进入方式的，不会出现闪动，但是有些手机就会出现闪动。可以使用'Activity'的转场动画，过度，点击之后finish掉
    Intent intent = new Intent(this, MainActivity.class);
                intent.putExtra("nightMode", true);
                startActivity(intent);
                overridePendingTransition(R.anim.animo_alph_close, R.anim.activity_close);
#### 这样实现不会闪动的效果         
#### 其次就是全局的夜间模式了，可以在自己的MyApplication里面实现，把我们的设置保存在SharePresference里面，在MyApplication里面调用
    public class MyApp extends Application {
        @Override
        public void onCreate() {
            super.onCreate();
    // 获取到SharePresference里面的boolean值，判断使用那种模式
    //        AppCompatDelegate.setDefaultNightMode(false ?
    //                AppCompatDelegate.MODE_NIGHT_YES : AppCompatDelegate.MODE_NIGHT_NO);
        }
    }
这里需要自己判断，这样你新开的页面就会是你需要的模式了。
#### 还可以写一个'BaseActivity',所有的'Activity'都要继承它，这里就自己慢慢琢磨吧。提供一种思路而已，应该也能实现全局的模式切换
