# android-lite-auto

lite your android, the code is on the way~ !

LiteAuto是一个代码生成框架，思路参考 JakeWharton 的开源项目 ButterKnife，在它的思路基础添加了一些自己的想法，从0到1设计并实现的。

LiteAuto设计思路的关键是约定大于配置，通过 **更简单的注解** 帮你自动生成 Android 中琐碎、冗长的功能代码。

目前可以自动生成 View 和 Event 相关的重复代码，还可以生成常用操作代码，而这些都是在编译时期自动生成的代码，几乎不影响性能，而且使得项目非常清晰简单。

和 ButterKnife 的不同点之一是 LiteAuto 只需要在 Activity 上添加一个 @LiteAuto 注解即可，框架自动分析 Activity 或者 Fragment 内部所有的 View 元素，判断是否在 layout 文件中有与之对应的 View 控件，比如一个 TextView 变量名为 “tvLabel”，框架会自动查找 layout 里面id 为 “tvLabel”的控件，并为之生成findViewById代码，如果查找不到那么探测下一个View，Event代码生成并关联的思路也是如此。

关于 @LiteAuto 注解，如果你有 View 变量不需要和 layout 文件关联，只需为变量添加 @UnBind 注解即可。

关于 @SemiAuto 注解，如果你需要向像 ButterKnife 一样，每个 View 或 Event 单独注解, 那么为Activity等添加 @SemiAuto 注解即可。

比如，之前的代码是这样：

```java
public class MainActivity extends Activity {

    TextView tvLabel;
    TextView tvLabel1;
    TextView tvLabel2;
    TextView tvLabel3;
    Button button;
    ImageView imageView;
    ProgressBar progressBar;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        tvLabel = (TextView) findViewById(R.id.tvLabel);
        tvLabel1 = (TextView) findViewById(R.id.tvLabel1);
        tvLabel2 = (TextView) findViewById(R.id.tvLabel2);
        tvLabel3 = (TextView) findViewById(R.id.tvLabel3);
        button = (Button) findViewById(R.id.button);
        imageView = (ImageView) findViewById(R.id.imageView);
        progressBar = (ProgressBar) findViewById(R.id.progressBar);

        tvLabel1.setOnClickListener(new View.OnClickListener() {

            @Override public void onClick(View v) {
                tvLabel1.setText("TvLabel Clicked!");
                button.setText("TvLabel Clicked!");
            }
        });

        tvLabel2.setOnClickListener(new View.OnClickListener() {

            @Override public void onClick(View v) {
                tvLabel2.setText("TvLabel 1 Clicked!");
                button.setText("TvLabel 1 Clicked!");
            }
        });
    }

}
```

那么，使用 LiteAuto 的代码是这样：

```java
@AutoLite
public class LiteMainActivity extends Activity {

    TextView tvLabel;
    TextView tvLabel1;
    TextView tvLabel2;
    TextView tvLabel3;
    Button button;
    ImageView imageView;
    ProgressBar progressBar;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        // 无需findViewById，也不需要为单个变量添加注解，Activity级别添加一个注解即可。
        AutoMan.activateThis(this);
    }

    public void clickTvLabel1(View view) {
        tvLabel1.setText("TvLabel Clicked!");
        button.setText("TvLabel Clicked!");
    }

    public void clickTvLabel2(View view) {
        tvLabel2.setText("TvLabel Clicked!");
        button.setText("TvLabel Clicked!");
    }

}
```
