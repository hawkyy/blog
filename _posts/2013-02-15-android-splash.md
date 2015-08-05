---
layout: post
title:  "Android启动页面"
date:   2013-02-15 14:06:05
categories: Android
---

* content
{:toc}

不多废话，直接代码

    public class SplashActivity extends Activity {
        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);

            final View view = View.inflate(this, R.layout.activity_splash, null);
            setContentView(view);
            // 渐变展示启动屏
            AlphaAnimation aa = new AlphaAnimation(0.5f, 1.0f);
            aa.setDuration(800);
            view.startAnimation(aa);
            aa.setAnimationListener(new Animation.AnimationListener() {
                @Override
                public void onAnimationEnd(Animation arg0) {
                    Intent intent = new Intent(SplashActivity.this, BottomTabActivity.class);
                    startActivity(intent);
                    finish();
                }
                @Override
                public void onAnimationRepeat(Animation animation) {
                }

                @Override
                public void onAnimationStart(Animation animation) {
                }
            });
        }
    }
