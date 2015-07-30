---
layout: post
title:  "Android toolbar"
date:   2015-01-21 14:06:05
categories: android
---

* content
{:toc}

#### toolbar介绍

toolbar 是Android API 21新推出的组件，用以替代ActionBar，比ActionBar使用更灵活

官方介绍 [https://developer.android.com/reference/android/widget/Toolbar.html](https://developer.android.com/reference/android/widget/Toolbar.html)

#### toolbar使用

theme：

隐藏原先的ActionBar

    <resources>
        <style name="AppTheme" parent="Theme.AppCompat.Light.DarkActionBar">
            <item name="windowActionBar">false</item>
            <item name="windowNoTitle">true</item>
        </style>
    </resources>

layout中：

    <android.support.v7.widget.Toolbar
        android:id="@+id/toolbar"
        android:layout_height="wrap_content"
        android:layout_width="match_parent"
        android:minHeight="?attr/actionBarSize"
        android:background="?attr/colorPrimary"
        >
    </android.support.v7.widget.Toolbar>

代码：

    import android.support.v7.app.ActionBarActivity;
    ......
    public class TemplateListActivity extends ActionBarActivity {
        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_template_list);

            Toolbar toolbar = (Toolbar) findViewById(R.id.toolbar);
            if (toolbar != null)
            {
                toolbar.setTitle(R.string.app_name);
                setSupportActionBar(toolbar);
                //getSupportActionBar().setDisplayHomeAsUpEnabled(true);
                //getSupportActionBar().setDisplayShowHomeEnabled(true);
            }
        }
        ......
    }