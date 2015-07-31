---
layout: post
title:  "Android Butterknife框架"
date:   2014-08-21 14:06:05
categories: android
---

* content
{:toc}

#### Butterknife介绍

懒人框架，可以让你少写几行代码

官方网站 [http://jakewharton.github.io/butterknife//](http://jakewharton.github.io/butterknife/)

Annotate fields with @Bind and a view ID for Butter Knife to find and automatically cast the corresponding view in your layout.

#### Butterknife使用

    class ExampleActivity extends Activity {
      @Bind(R.id.user) EditText username;
      @Bind(R.id.pass) EditText password;

      @BindString(R.string.login_error)
      String loginErrorMessage;

      @OnClick(R.id.submit) void submit() {
        // TODO call server...
      }

      @Override public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.simple_activity);
        ButterKnife.bind(this);
        // TODO Use fields...
      }
    }
