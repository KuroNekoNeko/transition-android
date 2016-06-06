﻿# transition-android

起到和CSS中transition一样的效果，自动完成补间

  - 减少代码长度
  - 提高多动画执行效率

### 过去
做一个补间动画需要

        ValueAnimator s = ValueAnimator.ofFloat(0.9f,1.2f);
           s.setDuration(300);
           s.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
              @Override
             public void onAnimationUpdate(ValueAnimator animation) {
                 mView.setScaleX((float)animation.getAnimatedValue());        
              }
           });
       s.start();

如果走到一半要回到原状态则需要


        float current = 0f;
        ValueAnimator s = ValueAnimator.ofFloat(0.9f,1.2f);
        s.setDuration(300);
        s.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
            @Override
            public void onAnimationUpdate(ValueAnimator animation) {
                current = (float)animation.getAnimatedValue();
                mView.setScaleX(current);
            }
        });
        s.addListener(new AnimatorListenerAdapter() {
            @Override
            public void onAnimationCancel(Animator animation) {
                super.onAnimationCancel(animation);
                ValueAnimator s2 = ValueAnimator.ofFloat(current, 0.9f);
                s2.setDuration(300);
                s2.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
                    @Override
                    public void onAnimationUpdate(ValueAnimator animation) {
                        mView.setScaleX((float)animation.getAnimatedValue());
                    }
                });
                s2.start();
            }
        });
        s.start();
        
如果要折返三次，四次。。。

        无数的回调嵌套，而我们发现很多这样的动画回调是毫无意义的，因此我们引入了在CSS中用到的自动补间Transition
    
    
### 现在
我们只需要

        vd = new ViewContainer(t);
        vd.setProperty("scaleX",1.9f,3000);

如果需要在OnClick的时候让它到达另一个状态呢？

        vd = new ViewContainer(t);
        vd.setProperty("scaleX",1.9f,3000);
        t.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                vd.setProperty("scaleX",0.2f,3000);
            }
        });

它便会立刻从上一个动画当前状态转向0.2f进行变换 NO！need！回调！

### 当前支持变换属性
- scaleX
- scaleY
- TranslationX
- TranslationY
- RotationX
- RotationY
- Alpha
- X
- Y
- backgroundColor
    
And More in next version。。。

### TODO
- 直接解析xml应用动画
- 增加动画主线程稳定性以及进一步提升效率
- 增加动画线程池，对每一个ViewContainer产生一个独立线程

### 版本号
1.1.1

### 版本日志

#### 1.1.1
修复了多动画迭代时的ComodifyException，添加多种插值器，以及动画回调。

#### 1.1.0
修复了activity关闭时动画线程延时崩溃，添加BackgroundColor渐变，添加插值器

### 代码贡献
That's Great!
但是请在Pull Request时务必注明更改部分以及添加适当的注释


License
----

MIT

