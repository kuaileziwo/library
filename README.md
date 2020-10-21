# [mylib]() 简介

`mylib`包含WorkClass、WorkField等多个注解

>**添加依赖和配置**
```
第一步：添加依赖
dependencies {
    compile "com.library:mylib-api:[1.0.0,)"
    annotationProcessor "com.library:mylib-compiler:[1.0.0,)"
}

第二步：Application类添加如下配置
@Override
public void onCreate() {
    super.onCreate();
    if (BuildConfig.DEBUG) {
      MyLib.openDebug();
    }
    MyLib.init(this);
}
```

## 目录
- [WorkClass、WorkField、EWorkHandler](#WorkClass)
- [PropertiesConfiguration](#PropertiesConfiguration)
- [EnableTipMessage、ETipProcessor](#TipMessage)

---
[//]: #----------------------------------WorkClass
### **WorkClass**
WorkClass、WorkField、EWorkHandler 配合使用
>WorkClass属性说明
* __id__: layout资源id（必须）
* __idGone__: 隐藏的资源id（可选）
* __idVisible__: 显示的资源id（可选）
>WorkField属性说明
* __id__: 资源id（必须）
* __label__: 标签（可选）
* __inherited__: 是否可以被继承，默认false
>EWorkHandler属性说明
* __value__: 优先级，数值越小表示优先级越高（必须）

>**使用例子**

WorkHandler处理类全局配置一个即可
```
@EWorkHandler(0)
public class WorkHandler implements com.mylib.inf.WorkHandler {
  @Override
    public boolean setViewValue(View view, Object value, ExtraData extraData) {
      if (value instanceof Drawable) {
        ((ImageView) view).setImageDrawable((Drawable) value);
      } else if (value instanceof Bitmap) {
        ((ImageView) view).setImageBitmap((Bitmap) value);
      } else if (view instanceof SimpleTags) {
        ((SimpleTags) view).setValue(StrUtils.null2empty(value));
        if (extraData.getLabelId() != 0) {
          ((SimpleTags) view).setLabel(extraData.getLabelId());
        }
      } else {
        ViewUtils.setViewText(view, StrUtils.null2empty(value));
      }
      return false;
    }
}
```
```
@WorkClass(id = R.layout.activity_main)
public class TestEntity {

  @WorkField(id = R.id.basic, label = R.id.chains)
  private String aa;

  @WorkField(id = R.id.basic, label = R.id.chains)
  private Bitmap bb;

  @WorkField(id = R.id.basic)
  private List<String> list;

  @WorkField(id = R.id.basic)
  private List<Map<String, Object>> testList;

  private Drawable cc;

  @WorkField(id = R.id.basic)
  private Map<String,String> dd;

  @WorkField(id = R.id.basic, label = R.id.chains)
  public Drawable getCc() {
    return cc;
  }
  ...
}
```

---
[//]: ----------------------------------PropertiesConfiguration
### **PropertiesConfiguration**

>**使用例子**
```
public class PropertiesConfig {

  @PropertiesConfiguration
  protected boolean onSave(Properties props, StringBuilder comment) {
    int i = 1;
    comment.append(DateUtils.now("yyyy年MM月dd日 HH:mm:ss")).append(LINE_SEPARATOR);
    comment.append("#").append(i++).append("、xxx");
     return false;
  }

}
```

---
[//]: ----------------------------------TipMessage
### **TipMessage**

>**使用例子**
```
第一步：配置允许TipMessage
@EnableTipMessage
public class MyApplication extends Application {
}

第二步：代码使用
TipMessage.newBuilder(this)
        .setMessage("第1次有图")
        .setDrawable(getResources().getDrawable(R.drawable.lib_smart_image_view_load_failure))
        .setMsgType(TipMessage.MSG_TOAST)
        .show();
```
