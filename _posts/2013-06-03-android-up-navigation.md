---
layout: post
title:  "为Android应用提供up navigation"
date:   2013-05-18 14:06:05
categories: Android
---

* content
{:toc}

[http://developer.android.com/training/implementing-navigation/ancestral.html](http://developer.android.com/training/implementing-navigation/ancestral.html)

总结起来

在AndroidManifest.xml中

    <application ... >
        ...
        <!-- The main/home activity (it has no parent activity) -->
        <activity
            android:name="com.example.myfirstapp.MainActivity" ...>
            ...
        </activity>
        <!-- A child of the main activity -->
        <activity
            android:name="com.example.myfirstapp.DisplayMessageActivity"
            android:label="@string/title_activity_display_message"
            android:parentActivityName="com.example.myfirstapp.MainActivity" >
            <!-- Parent activity meta-data to support 4.0 and lower -->
            <meta-data
                android:name="android.support.PARENT_ACTIVITY"
                android:value="com.example.myfirstapp.MainActivity" />
        </activity>
    </application>
    
在代码中对ActionBar

    @Override
    public void onCreate(Bundle savedInstanceState) {
        ...
        getActionBar().setDisplayHomeAsUpEnabled(true);
    }
    
在代码中up

    @Override
    public boolean onOptionsItemSelected(MenuItem item) {
        switch (item.getItemId()) {
        // Respond to the action bar's Up/Home button
        case android.R.id.home:
            NavUtils.navigateUpFromSameTask(this);
            return true;
        }
        return super.onOptionsItemSelected(item);
    }
    
